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

