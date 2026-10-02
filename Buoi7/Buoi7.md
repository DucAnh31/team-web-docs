# Buổi 7: CRUD Cơ bản
---
- [Buổi 7: CRUD Cơ bản](#buổi-7-crud-cơ-bản)
  - [1. Triển khai RESTful API trong Spring Boot](#1-triển-khai-restful-api-trong-spring-boot)
    - [1.1. RESTful API là gì?](#11-restful-api-là-gì)
    - [1.2. Các HTTP Method cơ bản](#12-các-http-method-cơ-bản)
      - [1.2.1 GET](#121-get)
      - [1.2.2 POST](#122-post)
      - [1.2.3 PUT](#123-put)
      - [1.2.4 PATCH](#124-patch)
      - [1.2.5 DELETE](#125-delete)
    - [1.3. Tạo REST Controller trong Spring Boot](#13-tạo-rest-controller-trong-spring-boot)
    - [1.4. Nhận dữ liệu từ Request](#14-nhận-dữ-liệu-từ-request)
      - [1.4.1 `@PathVariable`](#141-pathvariable)
      - [1.4.2 `@RequestParam`](#142-requestparam)
      - [1.4.3 `@RequestBody`](#143-requestbody)
    - [1.5. HTTP Status Code](#15-http-status-code)
  - [2. JPA là gì? Hibernate là gì?](#2-jpa-là-gì-hibernate-là-gì)
    - [2.1. ORM là gì?](#21-orm-là-gì)
    - [2.3. JPA là gì?](#23-jpa-là-gì)
    - [2.4. Hibernate là gì?](#24-hibernate-là-gì)
  - [3. Kiến trúc 4 lớp trong Spring Boot](#3-kiến-trúc-4-lớp-trong-spring-boot)
    - [3.1. Controller Layer](#31-controller-layer)
    - [3.2. Service Layer](#32-service-layer)
    - [3.3. Repository Layer](#33-repository-layer)
    - [3.4. Entity Layer](#34-entity-layer)
  - [4. Request Flow](#4-request-flow)
  - [5. Cấu hình DataSource](#5-cấu-hình-datasource)
    - [5.1. DataSource là gì?](#51-datasource-là-gì)
    - [5.2. Dependency cần thiết](#52-dependency-cần-thiết)
    - [5.3. application.yml](#53-applicationyml)
    - [5.6. `ddl-auto`](#56-ddl-auto)
    - [5.7. `show-sql`](#57-show-sql)
  - [6. Các Annotation quan trọng](#6-các-annotation-quan-trọng)
    - [6.1. `@RestController`](#61-restcontroller)
    - [6.2. `@Service`](#62-service)
    - [6.3. `@Repository`](#63-repository)
    - [6.4. `@Entity`](#64-entity)
    - [6.5. `@Table`](#65-table)
    - [6.6. `@Id`](#66-id)
    - [6.7. `@GeneratedValue`](#67-generatedvalue)
    - [6.8. `@Column`](#68-column)
    - [6.9. `@Transactional`](#69-transactional)
    - [6.10. `@Query`](#610-query)

---
## 1. Triển khai RESTful API trong Spring Boot

### 1.1. RESTful API là gì?

REST là một phong cách kiến trúc để thiết kế API. Trong REST, chúng ta tập trung vào resource - tài nguyên.

Ví dụ các resource:

```text
Student
User
Event
Course
Product
```

Với resource `Student`, API nên thiết kế:

```text
GET    /api/students
GET    /api/students/{id}
POST   /api/students
PUT    /api/students/{id}
DELETE /api/students/{id}
```

Thay vì:

```text
/getStudents
/getStudentById
/createStudent
/updateStudent
/deleteStudent
```

Lý do là hành động đã được biểu diễn bằng HTTP Method.

### 1.2. Các HTTP Method cơ bản

#### 1.2.1 GET

Dùng để lấy dữ liệu.

```http
GET /api/students
```

Lấy danh sách sinh viên.

```http
GET /api/students/10
```

Lấy sinh viên có `id = 10`.

Ví dụ Spring Boot:

```java
@GetMapping("/{id}")
public StudentResponse getStudent(@PathVariable Long id) {
    return studentService.getStudent(id);
}
```

#### 1.2.2 POST

Dùng để tạo resource mới.

```http
POST /api/students
```

Request body:

```json
{
    "name": "Nguyen Duc Anh",
    "email": "ducanh@gmail.com"
}
```

Controller:

```java
@PostMapping
public StudentResponse createStudent(
        @RequestBody CreateStudentRequest request) {
    return studentService.createStudent(request);
}
```


#### 1.2.3 PUT

Thường dùng để cập nhật toàn bộ thông tin của một resource.

```http
PUT /api/students/10
```

Ví dụ:

```json
{
    "name": "Nguyen Duc Anh",
    "email": "newemail@gmail.com"
}
```


#### 1.2.4 PATCH

Dùng khi chỉ muốn cập nhật một phần resource.

Ví dụ:

```http
PATCH /api/students/10/status
```

```json
{
    "status": "INACTIVE"
}
```


#### 1.2.5 DELETE

Dùng để xóa resource.

```http
DELETE /api/students/10
```


### 1.3. Tạo REST Controller trong Spring Boot

Ví dụ:

```java
@RestController
@RequestMapping("/api/students")
public class StudentController {

    @GetMapping
    public String getStudents() {
        return "Student list";
    }

    @GetMapping("/{id}")
    public String getStudent(@PathVariable Long id) {
        return "Student " + id;
    }

    @PostMapping
    public String createStudent() {
        return "Student created";
    }

    @PutMapping("/{id}")
    public String updateStudent(@PathVariable Long id) {
        return "Student updated " + id;
    }

    @DeleteMapping("/{id}")
    public String deleteStudent(@PathVariable Long id) {
        return "Student deleted " + id;
    }
}
```


### 1.4. Nhận dữ liệu từ Request

#### 1.4.1 `@PathVariable`

Dùng để lấy dữ liệu từ URL.

Request:

```http
GET /api/students/10
```

Controller:

```java
@GetMapping("/{id}")
public StudentResponse getStudent(
        @PathVariable Long id) {

    return studentService.getStudent(id);
}
```

Khi đó:

```text
id = 10
```

#### 1.4.2 `@RequestParam`

Dùng để lấy query parameter.

Request:

```http
GET /api/students?page=0&size=10
```

Controller:

```java
@GetMapping
public List<StudentResponse> getStudents(
        @RequestParam int page,
        @RequestParam int size) {

    ...
}
```

Khi đó:

```text
page = 0
size = 10
```

Có thể đặt mặc định:

```java
@RequestParam(defaultValue = "0") int page
```


#### 1.4.3 `@RequestBody`

Dùng để lấy dữ liệu trong HTTP Request Body.

Request:

```json
{
    "name": "Nguyen Duc Anh",
    "email": "ducanh@gmail.com"
}
```

DTO:

```java
public class CreateStudentRequest {

    private String name;
    private String email;

}
```

Controller:

```java
@PostMapping
public StudentResponse createStudent(
        @RequestBody CreateStudentRequest request) {

    return studentService.createStudent(request);
}
```

Spring sẽ chuyển JSON thành Java Object.

Quá trình:

```text
JSON
 ↓
CreateStudentRequest
```

gọi là **deserialization**.

Ngược lại:

```text
StudentResponse
 ↓
JSON
```

gọi là **serialization**.

Spring Boot thường sử dụng Jackson để thực hiện việc này.


### 1.5. HTTP Status Code

Một REST API không chỉ trả dữ liệu mà còn nên trả HTTP Status phù hợp.

Một số status quan trọng:

| Status | Ý nghĩa |
|---|---|
| 200 OK | Request thành công |
| 201 Created | Tạo resource thành công |
| 204 No Content | Thành công nhưng không trả body |
| 400 Bad Request | Request không hợp lệ |
| 401 Unauthorized | Chưa được xác thực |
| 403 Forbidden | Không có quyền |
| 404 Not Found | Không tìm thấy resource |
| 409 Conflict | Dữ liệu xung đột |
| 500 Internal Server Error | Lỗi phía server |

Ví dụ khi tạo student:

```java
@PostMapping
public ResponseEntity<StudentResponse> createStudent(
        @RequestBody CreateStudentRequest request) {

    StudentResponse response =
            studentService.createStudent(request);

    return ResponseEntity
            .status(HttpStatus.CREATED)
            .body(response);
}
```

Kết quả:

```http
HTTP/1.1 201 Created
```

kèm JSON response.

---

## 2. JPA là gì? Hibernate là gì?

### 2.1. ORM là gì?

ORM là:

**Object Relational Mapping**

Ví dụ chúng ta có class:

```java
public class Student {

    private Long id;

    private String name;

    private String email;
}
```

Database:

```text
students

id
name
email
```

ORM thực hiện mapping:

```text
Student              students
--------------------------------
id        <------->  id
name      <------->  name
email     <------->  email
```

Từ đó chúng ta thao tác với Java Object thay vì phải viết JDBC thủ công cho mọi trường hợp.

### 2.3. JPA là gì?

JPA là (Jakarta Persistence API), là một specification - đặc tả, tức là tập hợp các quy tắc và API chuẩn quy định cách Java làm việc với database.

JPA định nghĩa những thứ như:

```java
@Entity
@Table
@Id
@Column

@OneToOne
@OneToMany
@ManyToOne
@ManyToMany

EntityManager
```

Nhưng JPA không phải là một ORM implementation hoàn chỉnh. Nó chủ yếu đưa ra chuẩn:
- Entity phải được định nghĩa như thế nào?

- Primary Key được đánh dấu thế nào?

- Relationship được mapping thế nào?

- EntityManager hoạt động theo giao diện nào?

=> Cần một framework thực hiện specification đó.

Một implementation phổ biến là: **Hibernate**

### 2.4. Hibernate là gì?

Hibernate là một ORM Framework, là implementation JPA mặc định khi sử dụng Spring

Hibernate có khả năng:

- Mapping Java Object ↔ Database Table
- Sinh SQL
- Thực thi SQL
- Quản lý Entity
- Quản lý Persistence Context
- Theo dõi sự thay đổi Entity


---

## 3. Kiến trúc 4 lớp trong Spring Boot

Một ứng dụng Spring Boot cơ bản thường được tổ chức:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Entity
```

### 3.1. Controller Layer

Controller là nơi tiếp nhận HTTP Request từ client.

Ví dụ:

```java
@RestController
@RequestMapping("/api/students")
@RequiredArgsConstructor
public class StudentController {

    private final StudentService studentService;

    @GetMapping("/{id}")
    public StudentResponse getStudent(
            @PathVariable Long id) {

        return studentService.getStudent(id);
    }
}
```

Controller có nhiệm vụ chính:

```text
Nhận Request
      ↓
Lấy dữ liệu từ Request
      ↓
Gọi Service
      ↓
Nhận kết quả
      ↓
Trả HTTP Response
```

Controller **không nên chứa quá nhiều business logic**.

Ví dụ không nên:

```java
@GetMapping("/{id}")
public StudentResponse getStudent(Long id) {

    Student student =
            studentRepository.findById(id)
                    .orElseThrow();

    // rất nhiều kiểm tra nghiệp vụ ở đây

    ...
}
```

Vì lúc đó Controller vừa xử lý HTTP vừa xử lý nghiệp vụ vừa truy cập database.

Các trách nhiệm bị trộn lẫn.


### 3.2. Service Layer

Service là nơi xử lý **business logic - logic nghiệp vụ**.

Ví dụ:

```java
@Service
@RequiredArgsConstructor
public class StudentService {

    private final StudentRepository studentRepository;

    public StudentResponse getStudent(Long id) {

        Student student =
                studentRepository.findById(id)
                        .orElseThrow();

        return new StudentResponse(
                student.getId(),
                student.getName(),
                student.getEmail()
        );
    }
}
```

Ví dụ business logic có thể là:

```text
Kiểm tra student có tồn tại không

Kiểm tra email có bị trùng không

Kiểm tra user có đủ quyền không

Kiểm tra event còn hạn đăng ký không

Kiểm tra event còn capacity không
```
**Lưu ý:**
- Service không nên quan tâm request được gửi từ: POST hay PUT
- Service tập trung vào: Nghiệp vụ này cần xử lý như thế nào?

### 3.3. Repository Layer

Repository chịu trách nhiệm truy cập dữ liệu.

Ví dụ:

```java
public class StudentRepository{
    private EntityManager entityManager;

    public StudentRepository(EntityManager entityManager)
    {
        this.entityManager = entityManager
    }

    public Student getById(int id) {
        return entityManager.find(Student.class, id);
    }
}

```

Service gọi:

```java
studentRepository.getById(id);
```

### 3.4. Entity Layer

Entity biểu diễn dữ liệu được JPA quản lý.

Ví dụ:

```java
@Entity
@Table(name = "students")
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;
}
```

Entity này mapping tới bảng:

```text
students
-----------------
id
name
email
```
---
## 4. Request Flow 

Flow có thể hình dung:

```text
Client
  ↓
Tomcat
  ↓
Filter
  ↓
DispatcherServlet
  ↓
HandlerMapping
  ↓
Controller
  ↓
Service
  ↓
Repository
  ↓
JPA / Hibernate
  ↓
JDBC
  ↓
Database
```

Sau đó kết quả đi ngược lại.


---

## 5. Cấu hình DataSource

### 5.1. DataSource là gì?

Muốn ứng dụng Spring Boot truy cập MySQL/PostgreSQL thì ứng dụng cần database connection.

`DataSource` là abstraction dùng để cung cấp và quản lý các connection tới database.


```text
Spring Boot
    ↓
DataSource
    ↓
Database Connection
    ↓
MySQL
```

Thông tin DataSource thường gồm:
- Database URL
- Username
- Password
- Driver


### 5.2. Dependency cần thiết

Dùng MySQL:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>
        spring-boot-starter-data-jpa
    </artifactId>
</dependency>

<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>
        mysql-connector-j
    </artifactId>
    <scope>runtime</scope>
</dependency>
```

`spring-boot-starter-data-jpa` cung cấp các thành phần cần thiết để sử dụng Spring Data JPA.

`mysql-connector-j` là JDBC Driver giúp Java giao tiếp với MySQL.

### 5.3. application.yml

Có thể viết:

```yaml
spring:

  datasource:
    url: jdbc:mysql://localhost:3306/student_db
    username: root
    password: 123456

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

### 5.6. `ddl-auto`

Thường gặp:

```properties
spring.jpa.hibernate.ddl-auto=update
```

Các giá trị phổ biến:
- none
- validate: Không thay đổi database, chỉ kiểm tra Entity mapping có phù hợp với schema hiện tại không.
- update: Hibernate cố cập nhật database schema theo Entity.
- create: Tạo schema mỗi lần application chạy.
- create-drop: Tạo khi application chạy và xóa khi application dừng.

### 5.7. `show-sql`

```properties
spring.jpa.show-sql=true
```

Giúp nhìn thấy SQL Hibernate sinh ra.

Ví dụ:

```sql
select
    s1_0.id,
    s1_0.email,
    s1_0.name
from students s1_0
where s1_0.id=?
```

---

## 6. Các Annotation quan trọng

### 6.1. `@RestController`

Ví dụ:

```java
@RestController
@RequestMapping("/api/students")
public class StudentController {

}
```

`@RestController` dùng để đánh dấu class xử lý REST API.

Có thể hiểu gần tương đương:

```java
@Controller
@ResponseBody
```

Khi method trả: ```StudentResponse```

Spring có thể serialize object thành JSON.

Ví dụ:

```java
@GetMapping("/{id}")
public StudentResponse getStudent(...) {
    ...
}
```

Response:

```json
{
    "id": 1,
    "name": "Nguyen Duc Anh"
}
```

### 6.2. `@Service`

```java
@Service
public class StudentService {

}
```

Đánh dấu class là một Spring Bean thuộc Service Layer.

Service thường chứa:

```text
Business Logic

Business Validation

Điều phối nhiều Repository

Transaction boundary
```

Ví dụ:

```java
@Service
@RequiredArgsConstructor
public class StudentService {

    private final StudentRepository repository;

    public Student getStudent(Long id) {
        return repository.findById(id)
                .orElseThrow(() -> new RuntimeError("");
    }
}
```

### 6.3. `@Repository`

```java
@Repository
public class StudentDaoImpl implements StudentDao{

    private EntityManager entityManager;

    public StudentDaoImpl(EntityManager entityManager)
    {
        this.entityManager = entityManager;
    }


    @Override
    public List<Student> getAll() {
        String jpql = "SELECT s FROM Student s";
        TypedQuery<Student> query = entityManager.createQuery(jpql, Student.class);
        return query.getResultList();
    }
}
```

Dùng để đánh dấu component thuộc Persistence/Data Access Layer.



---

### 6.4. `@Entity`

```java
@Entity
public class Student {

}
```

Dùng để đánh dấu class là một JPA Entity.

JPA/Hibernate sẽ quản lý object của class này.

Ví dụ:

```java
@Entity
public class Student {

    @Id
    private Long id;

    private String name;
}
```

---

### 6.5. `@Table`

```java
@Entity
@Table(name = "students")
public class Student {
}
```

Dùng để chỉ định table mà Entity mapping tới.

```text
Student
   ↓
students
```

Nếu không viết `@Table`, JPA sẽ sử dụng quy tắc đặt tên mặc định.

---

### 6.6. `@Id`

```java
@Id
private Long id;
```

Dùng để đánh dấu field là Primary Key của Entity.

Tương ứng database:

```sql
id BIGINT PRIMARY KEY
```

Mỗi Entity cần có identifier.

---

### 6.7. `@GeneratedValue`

Thường dùng cùng `@Id`:

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

`IDENTITY` thường để database tự sinh ID theo cơ chế AUTO_INCREMENT.

Ví dụ:

```java
Student student = new Student();

student.setName("Duc Anh");

repository.save(student);
```

Ta không cần tự:

```java
student.setId(1L);
```

Database sinh ID.

---

### 6.8. `@Column`

Ví dụ:

```java
@Column(
    name = "email",
    nullable = false,
    unique = true,
    length = 100
)
private String email;
```

Một số thuộc tính thường dùng:
- name: tên column
- nullable: có cho NULL không
- unique: có yêu cầu unique không
- length: chiều dài

Không bắt buộc mọi field đều phải có `@Column`.

Ví dụ:

```java
private String name;
```

vẫn được JPA mapping.

---

### 6.9. `@Transactional`

`@Transactional` dùng để xác định transaction boundary.

Ví dụ:

```java
@Transactional
public void registerStudent() {

    repositoryA.save(...);

    repositoryB.save(...);
}
```

Ví dụ chuyển tiền:

```java
@Transactional
public void transfer() {

    accountRepository.withdraw();

    accountRepository.deposit();
}
```

Nếu:

```text
withdraw thành công

deposit thất bại
```

thì transaction giúp tránh tình trạng chỉ thực hiện một nửa nghiệp vụ.

Thông thường transaction boundary nên nằm ở Service Layer, vì một nghiệp vụ có thể gọi nhiều Repository.

Ví dụ:

```java
@Service
public class RegistrationService {

    @Transactional
    public void register() {

        registrationRepository.save(...);

        eventRepository.update(...);
    }
}
```

Không nên mặc định đặt `@Transactional` lên tất cả Controller.

---

### 6.10. `@Query`

Spring Data JPA đã hỗ trợ query dựa trên method name.

Ví dụ:

```java
Optional<Student> findByEmail(String email);
```

Spring có thể tự tạo query dựa trên:

```text
find
By
Email
```

Nhưng khi query phức tạp hơn, có thể dùng:

```java
@Query
```

Ví dụ:

```java
@Query("""
    select s
    from Student s
    where s.email = :email
""")
Optional<Student> findStudentByEmail(
        @Param("email") String email);
```