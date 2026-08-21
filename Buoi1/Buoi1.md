# **BUỔI 1: NHẬP MÔN CSDL**
---
- [**BUỔI 1: NHẬP MÔN CSDL**](#buổi-1-nhập-môn-csdl)
  - [1. CSDL là gì?](#1-csdl-là-gì)
  - [2. Hệ quản trị CSDL là gì ?](#2-hệ-quản-trị-csdl-là-gì-)
  - [3. Cài đặt MS SQL Server](#3-cài-đặt-ms-sql-server)
  - [4. Câu lệnh tạo database, table trong MS SQL Server](#4-câu-lệnh-tạo-database-table-trong-ms-sql-server)
---
## 1. CSDL là gì?
- Cơ sở dữ liệu (Database) là một tập hợp các dữ liệu được tổ chức, sắp xếp có cấu trúc và có liên quan với nhau. Chúng thường được lưu trữ và truy cập dưới dạng điện tử trong hệ thống máy tính.

- Mục đích chính của CSDL là giúp việc tìm kiếm, truy xuất, quản lý và cập nhật thông tin trở nên cực kỳ nhanh chóng và chính xác, thay vì phải lưu trữ dữ liệu rời rạc trong các file văn bản (Word, txt) hay bảng tính thông thường.
---
## 2. Hệ quản trị CSDL là gì ?
- Hệ quản trị CSDL (DBMS - Database Management System) là phần mềm đóng vai trò trung gian giữa người dùng, ứng dụng và hệ thống cơ sở dữ liệu vật lý. Nó cung cấp một môi trường và các công cụ để có thể định nghĩa, tạo lập, bảo trì và kiểm soát quyền truy cập vào dữ liệu.
- Các chức năng cốt lõi của một DBMS:
  - Mô hình hóa được dữ liệu
  - Lưu trữ và truy vấn dữ liệu
  - Hỗ trợ nhiều người dùng đồng thời
  - Bảo mật cho dữ liệu
  - Backup và Recovery


- **Ví dụ:** Microsoft SQL Server, MySQL, Oracle, PostgreSQL, MongoDB,...
---
## 3. Cài đặt MS SQL Server
- SQL Server
- SQL Server Management Studio (SSMS)
---
## 4. Câu lệnh tạo database, table trong MS SQL Server
- Tạo cơ sở dữ liệu:
``` sql
CREATE DATABASE ten_data_base;
```
- Ví dụ:
``` sql
CREATE DATABASE persons;
```

- Tạo table
``` sql
CREATE TABLE ten_bang (
    ten_cot_1 kieu_du_lieu rang_buoc,
    ten_cot_2 kieu_du_lieu rang_buoc,
    ten_cot_3 kieu_du_lieu rang_buoc,
    ...
    [Cac_rang_buoc_bang]
);
```
- Ví dụ:
``` sql
CREATE TABLE persons (
  id int PRIMARY KEY,
  lastname varchar(255),
  firstname varchar(255),
  address varchar(255),
);
```
- Kiểu dữ liêu:

| Kiễu dữ liệu  | Giá trị |
|---|---|
| TINYINT  | -128 đến 127  |
| INT  | -2,147,483,648 đến 2,147,483,647|
| BIGINT  | -9,223,372,036,854,775,808 đến 9,223,372,036,854,775,807|
| FLOAT  |  Giá trị dấu phẩy động chính xác thấp|
| DOUBLE  |  Giá trị dấu phẩy động chính xác cao hơn |
| DECIMAL  |  Giá trị số chính xác cao, phù hợp với lưu trữ tiền tệ |
| VARCHAR  |  Tối đa 65535 ký tự |
| TEXT  |  Tối đa 65535 ký tự |
| DATE  |  Lưu ngày theo định dạng YYYY-MM-DD |
| DATETIME  |  Lưu cả ngày và giờ theo định dạng YYYY-MM-DD HH:MM:SS |

- Ràng buộc:

| Ràng buộc | Miêu tả |
|--- |--- |
| NOT NULL | Đảm bảo một cột không được phép chứa giá trị `NULL` |
| UNIQUE | Đảm bảo tất cả các giá trị trong một cột (hoặc một nhóm cột) là hoàn toàn khác nhau, không bị trùng lặp |
| PRIMARY KEY | Kết hợp giữa `NOT NULL` và `UNIQUE`. Xác định duy nhất mỗi hàng (bản ghi) trong bảng. Mỗi bảng thường chỉ có một khóa chính |
| FOREIGN KEY | Dùng để liên kết dữ liệu giữa hai bảng với nhau. Đảm bảo tính toàn vẹn tham chiếu |
| CHECK | Đảm bảo giá trị trong một cột phải thỏa mãn một điều kiện logic cụ thể |
| DEFAULT | Gán một giá trị mặc định cho cột|
| AUTO_INCREMENT | Tự động tăng giá trị của cột số nguyên lên 1 mỗi khi có bản ghi mới được thêm vào (thường dùng cho cột khóa chính). |