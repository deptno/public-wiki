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
  - grpc 위에 올라가있고 에러가 많고 [[nextjs]] 와의 실패 경험이 있으니 참고 [[diary:2024-02-22]]

## 사용
### 주의사항
- insert 시에 **primary key** 중복이 허용된다
  - **조건을 넣어서** `query` 하면 마지막 입력 결과가 출력된다
  - **조건을 넣지않으면** 데이터가 존재하는것이 확인된다
  - **search** 를 하게되면 같은 아이디 데이터 여러개를 확인할 수 있다
  - 이를 피하려면 `upsert` 를 사용해야하고, `insert` 를 fail 시키는 방법은 없는 것으로 보인다
    + https://github.com/milvus-io/milvus/discussions/18201#discussioncomment-6658114

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
- index 는 데이터양에 따라 생성되는 시간이 소요됨, 검색 가능한 시간까지

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
      extra_params: {
        metric_type: "IP",
        index_type: "FLAT",
      },
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
- [ ] [[todo]] primary_key 를 주어도 insert 가 계속됨

### data type
- varchar, 한글은 3바이트로 계산

### partition
- partition 생성 안할 시 `_default` partition

## [[error]]
### "detail": "not support manually specifying the partition names if partition key mode is used"
```sh 
"detail": "not support manually specifying the partition names if partition key mode is used"
```
- `upsert` 시에 `partition_name` 지정시에 발생 `schema` 에 `is_partition_key` 를 지정했다고 발생하는 것으로 보임
- 문제는 `is_partition_key` 를 지정하더라도 파티션이 `_default_1` 형태로 생성됨 버전은 `2.3.5`
- schema 에서 제거한후 `upsert` 시에 파티션을 지정하니 아래 에러가 발생
```sh 
"reason": "Invalid partition name: 2024-02. Partition name can only contain numbers, letters and underscores.",
```
- `-`가 문제 `_` 로 변경
```json 
{
  "status": {
    "error_code": "MetaFailed",
    "reason": "partition not found[partition=2024_02]",
    "code": 200,
    "retriable": false,
    "detail": "partition not found[partition=2024_02]"
  }
}
```
- `is_partition_key` 를 주면 파티션 생성이 되지 않는다, 베타적
- 두가지 방법이 있다
  - `partition` 을 생성해서 `partition_name` 과 함께 `insert` 
  - 스키마에서 `is_partition_key` 를 true 로 주는 것, 하지만 생성은 `_default_1` 형태로 생성된다
- 파티션을 제거하려면 `unload` 후 제거해야한다



## link
- [[ai]]
- [[helm]]
- [[attu]]
