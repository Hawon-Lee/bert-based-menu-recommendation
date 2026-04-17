# bert-based-menu-recommendation

자연어 문장을 입력하면 어울리는 메뉴 10개를 추천해주는 모델입니다. `klue/bert-base`를 153개 메뉴 클래스에 맞춰 파인튜닝했습니다.

## 데이터

- `input_data.csv`: 메뉴명(`menu`)과 설명(`content`) 쌍으로 구성된 한국어 데이터
- 전처리: 위키 주석(`[숫자]`) · 특수문자 · `\xa0` · 앞뒤 공백 제거 후 `LabelEncoder`로 라벨링
- 학습/검증: `train_test_split`으로 8:2 분할 (`stratify=label`)

## 학습

- 모델: `TFBertForSequenceClassification` (`klue/bert-base`, `num_labels=153`)
- 토크나이저: `BertTokenizerFast`
- Optimizer: Adam, lr=5e-5
- Batch size: 32, Epochs: 30

## 추론

```python
>>> recommend("비오는날 막걸리랑 먹기 좋은 음식 추천해줘")
['김치전', '파전', '덮밥', '해물파전', '빈대떡', '감자전', '감바스', '삼겹살', '오뎅', '꼬치']
```

입력 문장에 대한 클래스별 점수를 계산해 상위 10개 메뉴를 반환합니다.

## 파일

- `Menu_recommend.ipynb`: 전처리 → 파인튜닝 → 추론 전 과정
- `input_data.csv`: 학습 원본 데이터
