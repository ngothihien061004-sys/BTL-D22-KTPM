# Quản lý dữ liệu kiểm thử cho ứng dụng cơ sở dữ liệu
## 1. Thông tin sinh viên

* Sinh viên: Ngô Thị Hiền
* MSV: K225480106014
* Lớp: K58KTP
* Ngành: Kỹ thuật Phần mềm
* Bộ môn: Công nghệ Thông tin
* Giáo viên hướng dẫn: Nguyễn Tuấn Linh
* Tên đề tài: Quản lý dữ liệu kiểm thử cho ứng dụng cơ sở dữ liệu
## Link trình bày :

## 1. Giới thiệu đề tài

Dự án này được thực hiện cho đề tài:

**Quản lý dữ liệu kiểm thử cho ứng dụng cơ sở dữ liệu**

Yêu cầu chính của đề tài là đề xuất chiến lược:

1. Seed Data
2. Synthetic User
3. Refresh dữ liệu kiểm thử
4. Bảo vệ dữ liệu cá nhân

Dự án sử dụng Python để mô phỏng một hệ thống bán hàng có cơ sở dữ liệu, từ đó xây dựng và đánh giá các chiến lược quản lý dữ liệu kiểm thử.

---
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/0e5210f0-6b81-403d-8171-c3c5f6254564" />


## 2. Mục tiêu

* Xây dựng bộ Seed Data cho môi trường kiểm thử.
* Sinh Synthetic User phục vụ kiểm thử.
* Thiết kế quy trình Refresh dữ liệu kiểm thử.
* Áp dụng Data Masking để bảo vệ dữ liệu cá nhân.
* Xác định các điểm QA cần kiểm soát trong Test Data Management.

---

## 3. Công nghệ sử dụng

* Python 3.x
* Jupyter Notebook
* pandas
* numpy
* matplotlib
* pathlib
* random
* datetime

---

## 4. Cấu trúc thư mục

```text
btl KTPM/
│
├── main.ipynb
│
├── data/
│   ├── production_customers.csv
│   ├── production_products.csv
│   ├── production_orders.csv
│   ├── production_order_items.csv
│   ├── production_payments.csv
│   ├── seed_roles.csv
│   ├── seed_users.csv
│   ├── seed_products.csv
│   ├── seed_test_cases.csv
│   ├── synthetic_users.csv
│   └── test_customers_masked.csv
│
├── outputs_notebook/
│   ├── 01_chien_luoc_seed_data.csv
│   ├── 02_tong_hop_synthetic_user.csv
│   ├── 02_kiem_tra_chat_luong_synthetic_user.csv
│   ├── 03_ke_hoach_refresh_du_lieu.csv
│   ├── 03_kiem_tra_toan_ven_refresh.csv
│   ├── 04_bao_ve_du_lieu_ca_nhan.csv
│   ├── 05_chien_luoc_quan_ly_du_lieu_kiem_thu.csv
│   └── *.png
│
├── README.md
└── requirements.txt
```

---

## 5. Mô hình dữ liệu thực nghiệm

Hệ thống bán hàng mô phỏng gồm các bảng:

| Bảng        | Ý nghĩa                  |
| ----------- | ------------------------ |
| customers   | Lưu thông tin khách hàng |
| products    | Lưu thông tin sản phẩm   |
| orders      | Lưu thông tin đơn hàng   |
| order_items | Lưu chi tiết đơn hàng    |
| payments    | Lưu thông tin thanh toán |

Dữ liệu production mô phỏng gồm:

* 100 khách hàng
* 30 sản phẩm
* 250 đơn hàng
* Nhiều chi tiết đơn hàng
* 250 bản ghi thanh toán

---

## 6. Nội dung thực nghiệm

### 6.1. Seed Data

Seed Data được dùng để tạo trạng thái ban đầu ổn định cho môi trường kiểm thử.

Kết quả:

| Loại dữ liệu   | Số lượng |
| -------------- | -------: |
| Vai trò        |        3 |
| User Seed      |        4 |
| Sản phẩm Seed  |        4 |
| Test Case Seed |        4 |

Ý nghĩa:

* Giúp kiểm thử có khả năng tái lập.
* Giảm thao tác chuẩn bị dữ liệu thủ công.
* Hỗ trợ các test case cơ bản như đăng nhập, phân quyền, mua hàng.

---
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/6ea6aff6-3bda-438f-8912-89d8367ae1a9" />


### 6.2. Synthetic User

Synthetic User là người dùng giả lập được tạo tự động để phục vụ kiểm thử mà không dùng dữ liệu cá nhân thật.

Kết quả:

* Tổng số Synthetic User: 500

Phân bố theo hạng thành viên:

| Hạng thành viên | Số lượng |
| --------------- | -------: |
| Bronze          |      119 |
| Gold            |      116 |
| Platinum        |      136 |
| Silver          |      129 |

Ý nghĩa:

* Tạo dữ liệu người dùng đa dạng.
* Phục vụ kiểm thử báo cáo, phân quyền, phân nhóm khách hàng.
* Hạn chế sử dụng dữ liệu cá nhân thật.

---
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/1a2996df-1443-470b-bfdc-be050c69e778" />


### 6.3. Refresh dữ liệu kiểm thử

Refresh Data giúp làm mới môi trường kiểm thử và đảm bảo dữ liệu test gần với thực tế hơn.

Quy trình đề xuất:

1. Backup dữ liệu.
2. Masking dữ liệu nhạy cảm.
3. Import dữ liệu vào môi trường test.
4. Kiểm tra toàn vẹn dữ liệu.
5. Ghi log refresh.

Kết quả:

* 4/4 kiểm tra toàn vẹn dữ liệu đạt.

Các kiểm tra gồm:

* Đơn hàng tham chiếu khách hàng hợp lệ.
* Chi tiết đơn hàng tham chiếu đơn hàng hợp lệ.
* Chi tiết đơn hàng tham chiếu sản phẩm hợp lệ.
* Thanh toán tham chiếu đơn hàng hợp lệ.

---
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/41b59478-ad84-41f7-b8e6-630919e3379d" />


### 6.4. Bảo vệ dữ liệu cá nhân


Dữ liệu nhạy cảm được kiểm tra gồm:

* Email
* Số điện thoại
* CCCD

Kết quả:

| Loại dữ liệu       | Trước masking | Sau masking |
| ------------------ | ------------: | ----------: |
| Email thật         |           100 |           0 |
| Số điện thoại thật |           100 |           0 |
| CCCD thật          |           100 |           0 |

Ý nghĩa:

* Dữ liệu cá nhân đã được che giấu trước khi dùng trong môi trường kiểm thử.
* Giảm nguy cơ lộ thông tin cá nhân.
* Đảm bảo dữ liệu test an toàn hơn.

---
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/8c44ec67-5e5c-47d7-88e5-5dfdac99c2ac" />
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/443b50df-de2d-4d2e-a9e3-43cb5aad7ede" />



## 7. Chiến lược quản lý dữ liệu kiểm thử tổng thể

| Chiến lược     | Mục tiêu                | QA cần kiểm soát                |
| -------------- | ----------------------- | ------------------------------- |
| Seed Data      | Tạo dữ liệu nền ổn định | Khả năng tái lập                |
| Synthetic User | Sinh user giả lập       | Không chứa dữ liệu cá nhân thật |
| Refresh Data   | Làm mới dữ liệu test    | Toàn vẹn dữ liệu                |
| Data Masking   | Bảo vệ dữ liệu cá nhân  | Không còn dữ liệu nhạy cảm      |

---
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/c10d07b6-cdb1-489e-90ae-e3440fb3d829" />

## 8. Cách chạy dự án

### Bước 1. Cài thư viện

```bash
pip install pandas numpy matplotlib
```

Hoặc dùng:

```bash
pip install -r requirements.txt
```

### Bước 2. Mở notebook

```bash
jupyter notebook main.ipynb
```

Hoặc mở bằng VS Code.

### Bước 3. Chạy toàn bộ notebook

Trong Jupyter Notebook hoặc VS Code, chọn:

```text
Run All
```

Sau khi chạy, dữ liệu sẽ được lưu vào:

```text
data/
outputs_notebook/
```

---

## 9. Kết quả đầu ra

Các file kết quả chính:

| File                                       | Nội dung                           |
| ------------------------------------------ | ---------------------------------- |
| 01_chien_luoc_seed_data.csv                | Kết quả Seed Data                  |
| 02_tong_hop_synthetic_user.csv             | Tổng hợp Synthetic User            |
| 02_kiem_tra_chat_luong_synthetic_user.csv  | Kiểm tra chất lượng Synthetic User |
| 03_ke_hoach_refresh_du_lieu.csv            | Kế hoạch Refresh Data              |
| 03_kiem_tra_toan_ven_refresh.csv           | Kiểm tra toàn vẹn sau Refresh      |
| 04_bao_ve_du_lieu_ca_nhan.csv              | Kết quả Data Masking               |
| 05_chien_luoc_quan_ly_du_lieu_kiem_thu.csv | Chiến lược TDM tổng thể            |

---

## 10. Kết luận

Dự án đã mô phỏng thành công chiến lược quản lý dữ liệu kiểm thử cho ứng dụng cơ sở dữ liệu.

Các kết quả chính:

* Xây dựng được Seed Data ổn định.
* Sinh được 500 Synthetic User.
* Thiết kế được quy trình Refresh Data.
* Kiểm tra toàn vẹn dữ liệu đạt 4/4.
* Dữ liệu nhạy cảm sau masking giảm từ 100 xuống 0.

Qua đó, dự án cho thấy Test Data Management có vai trò quan trọng trong việc nâng cao chất lượng kiểm thử, đảm bảo khả năng tái lập và bảo vệ dữ liệu cá nhân trong môi trường kiểm thử phần mềm.

---
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/a3abae4e-e2b9-47ad-b18a-92130516807b" />


