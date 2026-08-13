# **BUỔI 1: NHẬP MÔN CSDL**
---
- [**BUỔI 1: NHẬP MÔN CSDL**](#buổi-1-nhập-môn-csdl)
  - [1. CSDL là gì?](#1-csdl-là-gì)
  - [2. Hệ quản trị CSDL là gì ?](#2-hệ-quản-trị-csdl-là-gì-)
  - [4. Câu lệnh tạo database, table trong MS SQL Server](#4-câu-lệnh-tạo-database-table-trong-ms-sql-server)
---
## 1. CSDL là gì?
- Cơ sở dữ liệu (Database) là một tập hợp các dữ liệu được tổ chức, sắp xếp có cấu trúc và có liên quan với nhau. Chúng thường được lưu trữ và truy cập dưới dạng điện tử trong hệ thống máy tính.

- Mục đích chính của CSDL là giúp việc tìm kiếm, truy xuất, quản lý và cập nhật thông tin trở nên cực kỳ nhanh chóng và chính xác, thay vì phải lưu trữ dữ liệu rời rạc trong các file văn bản (Word, txt) hay bảng tính thông thường.
---
## 2. Hệ quản trị CSDL là gì ?
- Hệ quản trị CSDL (DBMS - Database Management System) là phần mềm đóng vai trò trung gian giữa người dùng, ứng dụng và hệ thống cơ sở dữ liệu vật lý. Nó cung cấp một môi trường và các công cụ để có thể định nghĩa, tạo lập, bảo trì và kiểm soát quyền truy cập vào dữ liệu.
- Các chức năng cốt lõi của một DBMS:
  - Định nghĩa và quản lý: Tạo cấu trúc lưu trữ (bảng, cột, kiểu dữ liệu).
  - Thao tác dữ liệu: Thêm, sửa, xóa, và truy vấn dữ liệu thông qua ngôn ngữ như SQL.
  - Bảo mật & Toàn vẹn: Đảm bảo không ai truy cập trái phép, sao lưu (backup) và phục hồi dữ liệu khi có sự cố.

- Ví dụ phổ biến: Microsoft SQL Server, MySQL, Oracle, PostgreSQL, MongoDB,...
---
## 4. Câu lệnh tạo database, table trong MS SQL Server
- Tạo cơ sở dữ liệu:
``` sql
CREATE DATABASE ten_data_base;
```
- Tạo table
``` sql
CREATE TABLE persons (
  id int PRIMARY KEY,
  lastname varchar(255),
  firstname varchar(255),
  address varchar(255),
);
```