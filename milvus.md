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
      pageContent: "방금전 퍼즐버블 문제있나요?",
      metadata: {},
    }, Document {
      pageContent: "[티몬10분어택] 발가락교정기 1+1 외 다양 (6,125원/무료)",
      metadata: {},
    }
  ]
  ```

## link
- [[ai]]
- [[helm]]
- [[attu]]
