# milvus
vector store, 검색 대상을 벡터화하여 유사도 검색

## 설치
- standalone 과 cluster 설치가 있다 한대로 운영하려 standalone 을 선택
+ https://milvus.io/docs/v2.0.x/install_standalone-helm.md

```sh 
helm repo add milvus https://zilliztech.github.io/milvus-helm/
helm repo update
helm pull milvus/milvus --untar
mv milvus milvus@[VERSION]
helm upgrade -n [NAMESPACE] --install milvus milvus@[VERSION]
# standalone 설치를 위해서는 아래와 같으 옵션이 values.yaml 에 반영되어야한다
# helm install my-release milvus/milvus --set cluster.enabled=false --set etcd.replicaCount=1 --set minio.mode=standalone --set pulsar.enabled=false
```

### sdk 설치
- [[node]]
  + https://github.com/milvus-io/milvus-sdk-node

## 사용
### [[langchain]]
+ https://github.com/milvus-io/milvus-sdk-node/tree/main/examples/LangChain
```javascript 
const address = `localhost:19530`
const collectionName = 'test'

// text sample from Godel, Escher, Bach
const vectorStore = await Milvus.fromTexts(
  [
    '[티몬] GS25 모바일상품권 9%할인',
    '칠성사이다 350ml x 24개',
    '제스프리 골드키위 점보 2.6kg',
    'LG 올레드 evo 게이밍 TV OLED42C2ENA',
    '[11마존] 쿠폰소진용 Kraft 마요 리얼 마요네즈 354ml (5,560)',
    '[롯데온] 오뚜기 3분 짜장 200g 24개 (KB/삼성카드 15,240)',
    '[티몬10분어택] 발가락교정기 1+1 외 다양 (6,125원/무료)',
    '[품절/닌텐도 스위치] 퍼즐버블 에브리버블 + 팬더 컨트롤러 40500/3000',
    '웰치스 오렌지 355ml 48캔',
    '방금전 퍼즐버블 문제있나요?',
    '[인터파크] 5600g 완본체 ssd512/램16g 309,370/무배',
    '[11마존] Thermalright TL-C12CW-S X3 CPU 팬 120mm 화이트 [3팩]',
    '[11마존] Thermalright TL-C12C X3 CPU 팬 120mm [3팩]',
  ],
  [],
  new OpenAIEmbeddings({}),
  {
    collectionName,
    vectorField: 'vectors',
    url: address,
  },
)
console.log('inserted')
const response = await vectorStore.similaritySearch('음료수', 3)
console.log('response', response)
```
- **게임** 검색 결과
  ```sh 
  response [
    Document {
      pageContent: "방금전 퍼즐버블 문제있나요?",
      metadata: {},
    }, Document {
      pageContent: "[티몬10분어택] 발가락교정기 1+1 외 다양 (6,125원/무료)",
      metadata: {},
    }
  ]
  ```
- **음료수** 검색 결과
  ```sh 
  response [
    Document {
      pageContent: "칠성사이다 350ml x 24개",
      metadata: {},
    }, Document {
      pageContent: "웰치스 오렌지 355ml 48캔",
      metadata: {},
    }, Document {
      pageContent: "[11마존] 쿠폰소진용 Kraft 마요 리얼 마요네즈 354ml (5,560)",
      metadata: {},
    }
  ]
  ```

### 검색
- load 후 사용, load 전에 index 필요

#### query
```javascript
const expr = `id == 'ID'` || `id in ['ID_01', 'ID_02']`
const results = await milvus.query({
  collection_name,
  expr,
  output_fields: ["id", "title"],
  limit: rowCount,
  offset: 0,
})
```

#### search
```javascript
const results2 = await milvus.search({
  collection_name,
  vector,
});
```

## 예제
```typescript
import OpenAI from "openai"
import _ from "lodash"
import { DataType, MetricType, MilvusClient } from "@zilliz/milvus2-sdk-node"

const main = async () => {
  try {
    const rows = [] // data
    const rowCount = rows.length
    const collectionName = "collectionName"
    const milvus = new MilvusClient({ address: "localhost:19530" })
    const hasCollection = await milvus.hasCollection({
      collection_name: collectionName,
    })
    console.log({ hasCollection })

    if (hasCollection.value) {
      await milvus.client.dropCollection({
        collection_name: collectionName
      })
    }
    
    // 스미카 생성
    await tryToCreateSaljiroDealSchema(milvus, collectionName)
    // 인덱스 생성
    await milvus.createIndex({
      collection_name: collectionName,
      field_name: "title",
      extra_params: index_params,
    })

    const openai = new OpenAI()
    const chunked = _.chunk<T>(rows, 100)
    const chunkedFieldsData = await Promise.all(
      chunked.map(async (rows) => {
        const result = await openai.embeddings.create({
          model: "text-embedding-3-small",
          input: rows.map((r) => r.title), // title 을 embeddings (벡터화)
        })
        console.log("embedding", result.usage)

        return rows.map((r: T, index) => {
          const { id, category, brand, published_date, published_month } = r

          return {
            id,
            title: result.data[index].embedding,
            published_month,
            model: result.model,
          }
        })
      }),
    )
    const fieldsData = chunkedFieldsData.flat()
    const inserted = await milvus.insert({
      collection_name: collectionName,
      fields_data: fieldsData,
    })

    const loaded = await milvus.loadCollection({
      collection_name: collectionName,
    })
    const index_params = {
      metric_type: "IP",
      index_type: "FLAT",
    }
    
    // ID 로 검색
    const expr = `id == '${rows[0].id}'`
    // const expr = `id in ['${rows[0].id}', '${rows[1].id}']`
    const results = await milvus.query({
      collection_name: collectionName,
      expr,
      output_fields: ["id", "title"],
      limit: rowCount,
      offset: 0,
    })
    console.log("result", results.status, results.data.length)
    
    // 유사도 검색
    const results2 = await milvus.search({
      collection_name: collectionName,
      vector: results.data[0].title,
    })
    console.log("result2", results2)

    milvus.closeConnection()
  } catch (err) {
    console.log({ file, err })

    throw new Error(err)
  }
}

type T = {
  id: string
  title: string
  category: string
  brand: string
  published_date: string
  published_month: string
}


async function tryToCreateSaljiroDealSchema(
  client: MilvusClient,
  collectionName: string,
) {
  await client.createCollection({
    collection_name: collectionName,
    description: "saljiro deal",
    metric_type: MetricType.IP,
    fields: [
      {
        name: "id",
        description: "ID",
        data_type: DataType.VarChar,
        is_primary_key: true,
        max_length: "20",
      },
      {
        name: "title",
        description: "OpenAI embedding(title)",
        data_type: DataType.FloatVector,
        dim: 1536,
      },
      {
        name: "model",
        description: "model",
        data_type: DataType.VarChar,
        max_length: 40,
      },
      {
        name: "published_month",
        description: "Published month(partition key)",
        data_type: DataType.VarChar,
        max_length: 7,
        is_partition_key: true,
      },
    ],
  })
}
```

## 개념
> database > collection > partition > data

### database
- 사용 유저의 권한을 여기서 구분
- 접속시 선택
- `default`

### collection
- 사용 유저의 권한을 여기서 구분
- 생성하면서 `field` 와 data-type 을 지정할 수 있다.
- [ ] [[@todo]] primary_key 를 주어도 insert 가 계속됨

### data type
- varchar, 한글은 3바이트로 계산

### partition
- partition 생성 안할 시 `_default` partition

## link
- [[ai]]
- [[helm]]
- [[attu]]
