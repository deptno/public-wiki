# natural-language-processing-with-transformer|트랜스포머를 활용한 자연어 처리

## 생각해볼 문제
- 날짜에 대한 임베딩을 하게되는경우 토큰화가 어떻게 이루어지는에 맞춰서 서식을 정해야한다

## 개념
### one hot vector
|   | Bumblebee | Megatron | optimus Prime |
|---|-----------|----------|---------------|
| 0 | 1         | 0        | 0             |
| 0 | 0         | 0        | 0             |
| 0 | 0         | 0        | 1             |
- 종류만큼(여기선 3)의 길이를 가지며 하나만 1로 세팅

## dataset
- 훈련/테스트 분할 전에 *샘플링* 하지말 것
- 
### code snippet
```python
# set format to pandas
emotions.set_format(type="pandas")
df = emotions['train'][:]
df.head()

# reset format
emotions.reset_format()

# convert classlabel to string
df['label-name'] = emotions['train'].features['label'].int2str(row)

# draw bar h
df['label-name'].value_counts(ascending=True).plot.barh() 
plt.title('Frequency of Classes')
plt.show()

# create dataframe from scratch
categorical df = pd.DataFrame(
  {"Name": [''Bumblebee", "Optimus Prime", "Megatron''], "Label ID": [0,1,2]}
)

# p62, 문자 단위 토큰화 one hot encoding 예제
input_ids = [5,14,12,8,13,11,19,11,13,10,0,17,8,18, 17, 0, 11, 16, 0, 6, 0, 7, 14, 15, 8, 0, 17, 6, 16, 12, 0, 14, 9, 0, 3, 2, 4, 1] =
input_ids = torch.tensor(input_ids)
one_hot_encodings = F.one_hot(input_ids, num_classes=len(tokenZidx))
## `num_classes` 를 입력하지 않으면 `input_ids` 의 정수중 가장 큰수 + 1 로 설정되기 때문에 필수적으로 세팅이 필요하다

one_hot_encodings.shape # torch.Size([38, Z0])


```
## link
- [[index]]
