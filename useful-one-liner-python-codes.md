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
 
 
