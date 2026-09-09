# BUỔI 5: CÁC KIẾN THỨC CƠ BẢN
---
- [BUỔI 5: CÁC KIẾN THỨC CƠ BẢN](#buổi-5-các-kiến-thức-cơ-bản)
  - [1. HTTP là gì](#1-http-là-gì)
    - [1.1 Các method trong HTTP](#11-các-method-trong-http)
    - [1.2 HTTP Request và HTTP Response](#12-http-request-và-http-response)
      - [1.2.1 HTTP Request](#121-http-request)
      - [1.2.2 HTTP Response](#122-http-response)
  - [2. API và RestAPI](#2-api-và-restapi)
    - [2.1 API](#21-api)
    - [2.2 RestAPI](#22-restapi)
  - [3. Design pattern DI, IOC](#3-design-pattern-di-ioc)
    - [3.1 IoC (Inversion of Control - Đảo ngược điều khiển)](#31-ioc-inversion-of-control---đảo-ngược-điều-khiển)
    - [3.2 Dependency Injection (DI) – Tiêm Phụ Thuộc](#32-dependency-injection-di--tiêm-phụ-thuộc)

---
## 1. HTTP là gì
**HTTP (HyperText Transfer Protocol - Giao thức truyền tải siêu văn bản)** là một trong các giao thức chuẩn về mạng Internet, được dùng để liên hệ thông tin giữa Máy cung cấp dịch vụ (Web server) và Máy sử dụng dịch vụ (Web client), là giao thức Client/Server dùng cho World Wide Web – WWW

**Sơ đồ hoạt động của HTTP:**
![alt text](image.png)

HTTP hoạt động dựa trên mô hình Client – Server. Trong mô hình này, các máy tính của người dùng sẽ đóng vai trò làm máy khách (Client). Sau một thao tác nào đó của người dùng, các máy khách sẽ gửi yêu cầu đến máy chủ (Server) và chờ đợi câu trả lời từ những máy chủ này.

**HTTP là một stateless protocol.** Hay nói cách khác, request hiện tại không biết những gì đã hoàn thành trong request trước đó.
### 1.1 Các method trong HTTP
- **GET:** Yêu cầu máy chủ trả về một tài nguyên cụ thể hoặc một danh sách (ví dụ: lấy thông tin một bài viết, lấy danh sách sản phẩm).
- **POST:** Gửi dữ liệu lên máy chủ để tiến hành xử lý, phổ biến nhất là để tạo mới một bản ghi vào cơ sở dữ liệu.
- **PUT:** Cập nhật một tài nguyên đã có bằng cách thay thế toàn bộ dữ liệu cũ bằng dữ liệu mới.
- **PATCH:** Áp dụng việc chỉnh sửa, cập nhật một phần nhỏ của tài nguyên hiện có.
- **DELETE:** Yêu cầu máy chủ tiêu hủy hoặc gỡ bỏ một tài nguyên xác định.
### 1.2 HTTP Request và HTTP Response
#### 1.2.1 HTTP Request
Là thông điệp do client gửi đến server để yêu cầu một dữ liệu hoặc hành động nào đó.

Cấu trúc chính của một Request:
- **Request Line:** Gồm phương thức (Method), đường dẫn tài nguyên (URL/Path) và phiên bản HTTP.
    - Các Method phổ biến: ``GET`` (lấy dữ liệu), ``POST`` (gửi/tạo dữ liệu mới), ``PUT/PATCH`` (cập nhật dữ liệu), ``DELETE`` (xóa dữ liệu)
- **Headers:** Các thông tin bổ sung (metadata) như loại trình duyệt, định dạng dữ liệu chấp nhận (Accept), token xác thực (Authorization), Content type,...
- **Body:** Phần chứa dữ liệu gửi kèm lên server (thường dùng trong POST hoặc PUT)

Ví dụ:
```http
POST /api/v1/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1Ni...
Accept: application/json

{
  "username": "nguyenvana",
  "email": "vana@example.com",
  "password": "secure_password_123"
}
```
#### 1.2.2 HTTP Response
Sau khi nhận Request, server xử lý và gửi lại một Response cho client.

Cấu trúc chính của một Response:
- **Status Line:** Gồm phiên bản HTTP, mã trạng thái (Status Code) và câu mô tả ngắn.
  - Các nhóm Status Code:
    - 2xx (Ví dụ 200 OK): Thành công.
    - 4xx (Ví dụ 404 Not Found, 401 Unauthorized): Lỗi từ phía client.
    - 5xx (Ví dụ 500 Internal Server Error): Lỗi từ phía server.
- **Headers:** Thông tin mô tả về phản hồi, ví dụ định dạng dữ liệu trả về Content-Type,...
- **Body:** Nội dung dữ liệu thực tế mà server gửi về

Ví dụ:
```http
HTTP/1.1 201 Created
Date: Wed, 09 Sep 2026 12:52:24 GMT
Server: Nginx/1.24.0
Content-Type: application/json; charset=utf-8
Content-Length: 145

{
  "success": true,
  "statusCode": 201,
  "message": "Tạo tài khoản thành công",
  "data": {
    "id": 105,
    "username": "nguyenvana",
    "email": "vana@example.com"
  }
}
```
---
## 2. API và RestAPI
### 2.1 API
Về mặt kỹ thuật, API là viết tắt của Giao diện lập trình ứng dụng (Application Programming Interface), là một tập hợp các quy tắc, giao thức và công cụ cho phép hai phần mềm hoặc hai hệ thống công nghệ khác nhau "trò chuyện" và trao đổi dữ liệu với nhau một cách an toàn.
### 2.2 RestAPI
API là một khái niệm rất rộng. Để các lập trình viên trên toàn thế giới không tạo ra các API một cách lộn xộn, người ta tạo ra các bộ tiêu chuẩn. REST là một trong những kiểu kiến trúc (architectural style) phổ biến nhất để thiết kế API trên môi trường mạng Internet (sử dụng giao thức HTTP).

Một API được gọi là RESTful (đạt chuẩn REST) khi nó tuân thủ nghiêm ngặt các nguyên tắc sau:
- **Kiến trúc Client - Server tách biệt** Giao diện người dùng (Frontend/Client) và lưu trữ dữ liệu (Backend/Server) phải hoàn toàn độc lập với nhau. Client chỉ quan tâm đến việc hiển thị, Server chỉ quan tâm đến việc truy xuất và xử lý dữ liệu. Nhờ đó, hai bên có thể nâng cấp và phát triển độc lập mà không ảnh hưởng tới nhau.
- **Không lưu trạng thái (Stateless)** Đây là nguyên tắc quan trọng nhất. Server sẽ không lưu giữ bất kỳ ngữ cảnh nào của Client giữa các lần gửi yêu cầu (requests).
- **Giao diện đồng nhất (Uniform Interface)** REST quy định cách đặt tên và gọi đường dẫn (URL) cực kỳ chuẩn mực, tập trung vào Tài nguyên (Resources) thay vì Hành động (Actions)
  - **Quy tắc 1:** Sử dụng Danh từ, tuyệt đối không dùng Động từ trong URL. 
    - Không chuẩn REST: /getAllUsers, /createNewUser, /deleteUser?id=5
    - chuẩn REST: /users 
  - **Quy tắc 2:** Tận dụng tối đa các HTTP Method để định nghĩa hành động.
    - GET /users: Lấy danh sách toàn bộ người dùng.
    - GET /users/5: Lấy thông tin chi tiết của người dùng có ID là 5.
    - POST /users: Tạo một người dùng mới.
    - PUT /users/5: Cập nhật toàn bộ thông tin của người dùng ID 5.
    - DELETE /users/5: Xóa người dùng ID 5.
  - **Quy tắc 3:** Định dạng dữ liệu trả về chuẩn hóa, dữ liệu được các REST API trao đổi thường được định dạng dưới chuẩn JSON
---
## 3. Design pattern DI, IOC
### 3.1 IoC (Inversion of Control - Đảo ngược điều khiển)
Trong cách lập trình truyền thống, một đối tượng sẽ tự chịu trách nhiệm trong việc tìm kiếm, khởi tạo và quản lý các đối tượng phụ thuộc mà nó cần.

Mô hình điều khiển truyền thống: Hãy hình dung một lớp OrderService cần sử dụng OrderRepository để tương tác với cơ sở dữ liệu. Theo cách tiếp cận thông thường, OrderService sẽ phải tự tạo ra instance của OrderRepository:
```java
public class OrderService {
    // OrderService tự tạo và quản lý dependency của nó
    private OrderRepository orderRepository = new OrderRepositoryImpl();

    public void processOrder() {
        // ... sử dụng orderRepository
    }
}
```
=> Cách làm này tạo ra một liên kết chặt chẽ (tight coupling). Lớp OrderService bị “buộc cứng” vào một triển khai cụ thể là OrderRepositoryImpl, muốn sửa đổi phải vào file này và sửa lại.

**Nguyên lý IoC**
IoC đảo ngược hoàn toàn luồng điều khiển này. Thay vì đối tượng tự đi tìm thứ nó cần, nó sẽ được cung cấp thứ nó cần bởi một bên thứ ba. Trách nhiệm khởi tạo và quản lý vòng đời của các dependency được chuyển giao cho một thực thể bên ngoài, đó chính là IoC Container (hay Spring Container).

Container sẽ đọc các cấu hình hoặc các chỉ dẫn (annotation) để biết OrderService cần một OrderRepository. Sau đó, nó sẽ tạo ra một instance của OrderRepositoryImpl và “trao” nó cho OrderService khi OrderService được tạo. Lớp OrderService giờ đây không cần biết OrderRepository được tạo ra như thế nào hay nó là implementation cụ thể nào.

Việc áp dụng IoC mang lại các ưu điểm kỹ thuật rõ rệt:
- Giảm thiểu sự phụ thuộc trực tiếp
- Linh hoạt trong việc thay thế
### 3.2 Dependency Injection (DI) – Tiêm Phụ Thuộc
Nếu IoC là một nguyên lý, một triết lý thiết kế, thì Dependency Injection (DI) là một trong những cách phổ biến nhất để hiện thực hóa nguyên lý đó.

DI là mẫu thiết kế mà trong đó IoC Container sẽ chủ động “tiêm” (inject) các đối tượng phụ thuộc vào một đối tượng, thay vì đối tượng đó phải tự tìm kiếm hay khởi tạo chúng. Hành động “tiêm” này chính là cách mà Container trao quyền điều khiển cho đối tượng.

Spring hỗ trợ ba phương thức DI chính:

- Constructor Injection (Tiêm qua hàm khởi tạo): Các dependency được cung cấp dưới dạng tham số của constructor. Đây là cách được khuyến khích cho các dependency bắt buộc, vì nó đảm bảo đối tượng luôn ở trạng thái hợp lệ ngay sau khi được khởi tạo.
- Setter Injection (Tiêm qua phương thức setter): Container sẽ gọi các phương thức setter của đối tượng sau khi nó được khởi tạo bằng constructor không tham số. Phương pháp này thường được dùng cho các dependency không bắt buộc (optional).
- Field Injection (Tiêm qua trường dữ liệu): Dependency được tiêm trực tiếp vào trường (field) của lớp, thường sử dụng annotation @Autowired. Cách này giúp code ngắn gọn nhưng có thể gây khó khăn cho việc viết unit test và che giấu các phụ thuộc của một lớp.