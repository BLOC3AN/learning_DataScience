# Tiền xử lý và Xử lý Dữ liệu

## 1. Phương pháp Tính Toán Tầm Quan Trọng của Đặc Trưng

Có hai phương pháp phổ biến để tính toán tầm quan trọng của các đặc trưng trong mô hình học máy: sử dụng `RandomForestClassifier` và sử dụng các mô hình tuyến tính như hồi quy logistic với chiến lược one-vs-all (OvA).

### RandomForestClassifier

`feature_importances_` là một thuộc tính của mô hình `RandomForestClassifier` đã được huấn luyện, cung cấp tầm quan trọng của mỗi đặc trưng dựa trên mức độ cải thiện độ tinh khiết của các nút trong cây.

### Mô hình Tuyến tính (ví dụ: Hồi quy Logistic với OvA)

`np.mean(np.abs(model_ova.coef_), axis=0)` tính toán giá trị trung bình của các giá trị tuyệt đối của các hệ số qua tất cả các lớp. Phương pháp này được sử dụng để xác định tầm quan trọng của mỗi đặc trưng trong các mô hình tuyến tính.

Nếu bạn đang sử dụng `RandomForestClassifier`, bạn nên sử dụng `model.feature_importances_`. Nếu bạn đang sử dụng một mô hình tuyến tính, bạn nên sử dụng phương pháp dựa trên hệ số.

Dưới đây là cách bạn có thể sử dụng phương pháp dựa trên hệ số với một mô hình tuyến tính:

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler
from sklearn.datasets import load_iris

# Tải dữ liệu ví dụ
data = load_iris()
X = data.data
y = data.target

# Chuẩn hóa dữ liệu
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Huấn luyện mô hình hồi quy logistic với chiến lược one-vs-all
model_ova = LogisticRegression(multi_class='ovr')
model_ova.fit(X_scaled, y)

# Tính toán tầm quan trọng của các đặc trưng
feature_importance = np.mean(np.abs(model_ova.coef_), axis=0)

# Vẽ biểu đồ tầm quan trọng của các đặc trưng
plt.barh(data.feature_names, feature_importance)
plt.title("Tầm Quan Trọng của Đặc Trưng")
plt.xlabel("Tầm Quan Trọng")
plt.show()
```

Trong ví dụ này, chúng ta đã sử dụng hồi quy logistic với chiến
 lược one-vs-all để tính toán tầm quan trọng của các đặc trưng dựa trên các hệ số của mô hình.
