# 노트북 옵션 관련

- 경고 문구
  - ignore
  - default
```python
import warnings
# 경고 문구 무시하고 숨김
warnings.filterwarnings(action='ignore')

# 경고 문구 보이게
warnings.filterwarnings(action='default')
```

# 시각화 관련

- 라벨
```python
plt.xlabel("") # x축 라벨 없애기
```

- 사이즈 조정
```python
# Set the width and height of the figure
plt.figure(figsize=(10,10))
```

- 스타일 변경
```python
plt.style.use('seaborn-whitegrid')
```

- ```Matplotlib``` 설정
```python
plt.rc('figure', autolayout=True)
plt.rc('axes', labelweight='bold', labelsize='large',
       titleweight='bold', titlesize=18, titlepad=10)
```
- x축 회전
```python
plt.xticks(rotation=45)
```

- 눈금 
```python
plt.grid(True)
```


# 전처리 관련

- 숫자형 변수만 추출
```python
# To keep things simple, we'll use only numerical predictors
X = X_full.select_dtypes(exclude=['object'])
X_test = X_test_full.select_dtypes(exclude=['object'])
```

- 범주형 변수 확인
```python
# Get list of categorical variables
s = (X_train.dtypes == 'object')
object_cols = list(s[s].index)
```

- 결측이 하나라도 있는 변수 목록 저장
```python
# Drop columns with missing values (simplest approach)
cols_with_missing = [col for col in X_train_full.columns if X_train_full[col].isnull().any()] 
```

- ```factor``` 형 변수 목록 저장
```python
# "Cardinality" means the number of unique values in a column
# Select categorical columns with relatively low cardinality (convenient but arbitrary)
low_cardinality_cols = [cname for cname in X_train_full.columns if X_train_full[cname].nunique() < 10 and 
                        X_train_full[cname].dtype == "object"]
```

- 트레인 데이터로만 ```onehot encoder``` 
```python
from sklearn.preprocessing import OneHotEncoder

# Apply one-hot encoder to each column with categorical data
OH_encoder = OneHotEncoder(handle_unknown='ignore', sparse=False)
OH_cols_train = pd.DataFrame(OH_encoder.fit_transform(X_train[object_cols]))
OH_cols_valid = pd.DataFrame(OH_encoder.transform(X_valid[object_cols]))
```

- 데이터 프레임 합치기
```pd.concat()```  
```axis=0``` : 위아래로  
```axis=1``` : 옆으로  
 ```python
 OH_X_train = pd.concat([num_X_train, OH_cols_train], axis=1)
 ```
 
- 병렬처리 함수 ```map```  
```python
object_cols = X_train.columns

# Get number of unique entries in each column with categorical data
object_nunique = list(map(lambda col: X_train[col].nunique(), object_cols))
d = dict(zip(object_cols, object_nunique))

# Print number of unique entries by column, in ascending order
sorted(d.items(), key=lambda x: x[1])
```

- ```dictionary``` 형식 변수에서 ```value``` 값이 최소인 ```key``` 추출하기
```python
results
'''
{50: 18353.8393511688,
 100: 18395.2151680032,
 150: 18288.730020956387,
 200: 18248.345889801505,
 250: 18255.26922247291,
 300: 18275.241922621914,
 350: 18270.29183308043,
 400: 18270.197974402367}
'''

## 최소값 추출
n_estimators_best = min(results, key=results.get)
```

- ```pandas``` ```train```, ```valid``` 나누기
```python
df_train = df.sample(frac=0.7, random_state=0)
df_valid = df.drop(df_train.index)
```

- 매핑하기
```python
df['Class'] = df['Class'].map({'good': 0, 'bad': 1}) # 'good'을 0으로, 'bad'를 1로
```

- min_max 정규화
```python
max_ = df_train.max(axis=0)
min_ = df_train.min(axis=0)

df_train = (df_train - min_) / (max_ - min_)
df_valid = (df_valid - min_) / (max_ - min_)
```

- X,y split
```python
X = data.copy()
y = X.pop('col')
```

- ```query```
```python
# Drop live projects
df = df.query('state != "live"')
```

- ```assign```
```python
# Add outcome column, "successful" == 1, others are 0
df = df.assign(outcome=(df['state'] == 'successful').astype(int))
```

- 날짜 변수 ```assign```
```python
df = df.assign(hour=df.launched.dt.hour,
               day=df.launched.dt.day,
               month=df.launched.dt.month,
               year=df.launched.dt.year)
```

- ```LabelEncoder()```
```python
from sklearn.preprocessing import LabelEncoder

cat_features = ['category', 'currency', 'country']
encoder = LabelEncoder()

# Apply the label encoder to each column
encoded = df[cat_features].apply(encoder.fit_transform)
```

- ```pandas``` 요일 추출하기
```python
df['date'].dt.weekday_name
```
