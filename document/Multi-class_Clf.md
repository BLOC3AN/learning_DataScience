## Multi-class Classification

### Tổng quan
Phân loại đa lớp liên quan đến việc dự đoán lớp của một mẫu từ nhiều hơn hai lớp. Trong notebook này, chúng ta khám phá các chiến lược khác nhau cho phân loại đa lớp sử dụng hồi quy logistic.

### Phương pháp

#### One-vs-All (OvA)
Trong phương pháp One-vs-All:
- Thuật toán huấn luyện một bộ phân loại nhị phân cho mỗi lớp.
- Mỗi bộ phân loại học cách phân biệt một lớp duy nhất với tất cả các lớp còn lại.
- Nếu có k lớp, sẽ có k bộ phân loại được huấn luyện.
- Trong quá trình dự đoán, thuật toán đánh giá tất cả các bộ phân loại trên mỗi đầu vào và chọn lớp có điểm tin cậy cao nhất làm lớp dự đoán.

**Ưu điểm:**
- Đơn giản hơn và hiệu quả hơn về số lượng bộ phân loại (k).
- Dễ dàng triển khai cho các thuật toán tự nhiên cung cấp điểm tin cậy (ví dụ: hồi quy logistic, SVM).

**Nhược điểm:**
- Các bộ phân loại có thể gặp khó khăn với sự mất cân bằng lớp vì mỗi bộ phân loại nhị phân phải phân biệt giữa một lớp và tất cả các lớp còn lại.
- Yêu cầu bộ phân loại hoạt động tốt ngay cả với các tập dữ liệu mất cân bằng cao, vì nhóm "tất cả" thường chứa nhiều mẫu hơn nhóm "một".

**Ví dụ:**
```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Chia dữ liệu
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42, stratify=y)

# Huấn luyện mô hình hồi quy logistic sử dụng One-vs-All
model_ova = LogisticRegression(multi_class='ovr', max_iter=1000)
model_ova.fit(X_train, y_train)

# Dự đoán
y_pred_ova = model_ova.predict(X_test)

# Đánh giá
print("Chiến lược One-vs-All (OvA)")
print(f"Độ chính xác: {accuracy_score(y_test, y_pred_ova) * 100:.2f}%")
```

#### One-vs-One (OvO)
Trong phương pháp One-vs-One:
- Thuật toán huấn luyện một bộ phân loại nhị phân cho mỗi cặp lớp trong tập dữ liệu.
- Nếu có k lớp, điều này dẫn đến $k(k-1)/2$ bộ phân loại.
- Mỗi bộ phân loại được huấn luyện để phân biệt giữa hai lớp cụ thể, bỏ qua các lớp còn lại.
- Trong quá trình dự đoán, mỗi bộ phân loại bỏ phiếu cho một trong hai lớp mà nó được huấn luyện, và lớp có nhiều phiếu nhất được chọn làm lớp dự đoán.

**Ưu điểm:**
- Mỗi bộ phân loại xử lý một vấn đề đơn giản hơn, phân biệt giữa chỉ hai lớp.
- Có thể chính xác hơn khi các lớp được phân tách rõ ràng.

**Nhược điểm:**
- Yêu cầu huấn luyện một số lượng lớn bộ phân loại, đặc biệt là đối với các tập dữ liệu có nhiều lớp.
- Tốn nhiều tài nguyên tính toán hơn trong cả quá trình huấn luyện và dự đoán.

**Ví dụ:**
```python
from sklearn.multiclass import OneVsOneClassifier

# Huấn luyện mô hình hồi quy logistic sử dụng One-vs-One
model_ovo = OneVsOneClassifier(LogisticRegression(max_iter=1000))
model_ovo.fit(X_train, y_train)

# Dự đoán
y_pred_ovo = model_ovo.predict(X_test)

# Đánh giá
print("Chiến lược One-vs-One (OvO)")
print(f"Độ chính xác: {accuracy_score(y_test, y_pred_ovo) * 100:.2f}%")
```

### So sánh
- **One-vs-All (OvA)** đơn giản hơn và yêu cầu ít bộ phân loại hơn nhưng có thể gặp khó khăn với sự mất cân bằng lớp.
- **One-vs-One (OvO)** có thể chính xác hơn cho các lớp được phân tách rõ ràng nhưng yêu cầu nhiều bộ phân loại và tài nguyên tính toán hơn.

### Kết luận
Cả hai phương pháp One-vs-All và One-vs-One đều là những chiến lược hiệu quả cho phân loại đa lớp, mỗi phương pháp có những điểm mạnh và điểm yếu riêng. Việc lựa chọn phương pháp phụ thuộc vào các đặc điểm cụ thể của tập dữ liệu và tài nguyên tính toán sẵn có.

### Tài liệu tham khảo
- [Tài liệu Scikit-learn](https://scikit-learn.org/stable/modules/multiclass.html)
- [Hồi quy Logistic](https://en.wikipedia.org/wiki/Logistic_regression)
- [One-vs-All và One-vs-One](https://machinelearningmastery.com/one-vs-rest-and-one-vs-one-for-multi-class-classification/)
