# [BUỔI 9] Thao tác với DATABASE
---
- [\[BUỔI 9\] Thao tác với DATABASE](#buổi-9-thao-tác-với-database)
  - [1. Các mối quan hệ(1..N, N..N) trong JPA (@ManyToMany, @OneToMany,...)](#1-các-mối-quan-hệ1n-nn-trong-jpa-manytomany-onetomany)
    - [1.1. Quan hệ N:1 với `@ManyToOne`](#11-quan-hệ-n1-với-manytoone)
      - [1.1.1. Định nghĩa](#111-định-nghĩa)
      - [1.1.2. Ví dụ: Phòng chiếu thuộc một rạp](#112-ví-dụ-phòng-chiếu-thuộc-một-rạp)
    - [1.2. Quan hệ 1:N với `@OneToMany`](#12-quan-hệ-1n-với-onetomany)
      - [1.2.1. Định nghĩa](#121-định-nghĩa)
      - [1.2.2. Quan hệ một chiều và hai chiều](#122-quan-hệ-một-chiều-và-hai-chiều)
      - [1.2.3. Owning side, inverse side](#123-owning-side-inverse-side)
    - [1.3. Quan hệ 1:1 với `@OneToOne`](#13-quan-hệ-11-với-onetoone)
      - [1.3.1. Định nghĩa](#131-định-nghĩa)
      - [1.3.2. Ví dụ: Nhân viên và hồ sơ nhân viên](#132-ví-dụ-nhân-viên-và-hồ-sơ-nhân-viên)
    - [1.4. Quan hệ N:N với `@ManyToMany`](#14-quan-hệ-nn-với-manytomany)
      - [1.4.1. Định nghĩa](#141-định-nghĩa)
      - [1.4.2. Mapping bằng `@JoinTable`](#142-mapping-bằng-jointable)
      - [1.4.3. Khi nào nên tạo Entity cho bảng trung gian?](#143-khi-nào-nên-tạo-entity-cho-bảng-trung-gian)
    - [1.5. Cascade](#15-cascade)
      - [1.5.1. Định nghĩa và các loại cascade](#151-định-nghĩa-và-các-loại-cascade)
      - [1.5.2. Nguyên tắc sử dụng](#152-nguyên-tắc-sử-dụng)
    - [1.6. `orphanRemoval` và sự khác nhau với cascade xóa](#16-orphanremoval-và-sự-khác-nhau-với-cascade-xóa)
      - [1.6.1. Định nghĩa](#161-định-nghĩa)
      - [1.6.2. Phân biệt với `CascadeType.REMOVE`](#162-phân-biệt-với-cascadetyperemove)
      - [1.6.3. Ví dụ: Bỏ vé khỏi hóa đơn nháp](#163-ví-dụ-bỏ-vé-khỏi-hóa-đơn-nháp)
  - [2.Derived query methods (truy vấn theo tên hàm)](#2derived-query-methods-truy-vấn-theo-tên-hàm)
    - [2.1. Derived query method là gì?](#21-derived-query-method-là-gì)
    - [2.2. Cấu trúc tên phương thức](#22-cấu-trúc-tên-phương-thức)
    - [2.3. Tên thuộc tính và truy cập thuộc tính lồng nhau](#23-tên-thuộc-tính-và-truy-cập-thuộc-tính-lồng-nhau)
      - [2.3.1. Dùng tên thuộc tính Java](#231-dùng-tên-thuộc-tính-java)
      - [2.3.2. Truy cập qua quan hệ](#232-truy-cập-qua-quan-hệ)
    - [2.4. Các từ khóa điều kiện thường dùng](#24-các-từ-khóa-điều-kiện-thường-dùng)
    - [2.5. Kiểu dữ liệu trả về](#25-kiểu-dữ-liệu-trả-về)
    - [2.6. Phân trang với `Pageable`](#26-phân-trang-với-pageable)
    - [2.7. Xóa dữ liệu bằng derived query method](#27-xóa-dữ-liệu-bằng-derived-query-method)
  - [3. JPQL với @Query](#3-jpql-với-query)
    - [3.1. JPQL là gì?](#31-jpql-là-gì)
    - [3.2. Khai báo JPQL bằng `@Query`](#32-khai-báo-jpql-bằng-query)
    - [3.3. Truyền tham số vào truy vấn](#33-truyền-tham-số-vào-truy-vấn)
      - [3.3.1. Tham số có tên](#331-tham-số-có-tên)
      - [3.3.2. Tham số theo vị trí](#332-tham-số-theo-vị-trí)
    - [3.4. Lọc dữ liệu với `WHERE`](#34-lọc-dữ-liệu-với-where)
    - [3.5. Truy vấn qua quan hệ và `JOIN`](#35-truy-vấn-qua-quan-hệ-và-join)
      - [3.5.1. Điều hướng bằng đường dẫn thuộc tính](#351-điều-hướng-bằng-đường-dẫn-thuộc-tính)
      - [3.5.2. `JOIN` và `LEFT JOIN`](#352-join-và-left-join)
      - [3.5.3. `JOIN FETCH`](#353-join-fetch)
    - [3.6. Truy vấn con — Subquery](#36-truy-vấn-con--subquery)
    - [3.7. Chọn dữ liệu và thống kê](#37-chọn-dữ-liệu-và-thống-kê)
      - [3.7.1. Chọn Entity hoặc một thuộc tính](#371-chọn-entity-hoặc-một-thuộc-tính)
      - [3.7.2. Hàm tổng hợp, `GROUP BY` và `HAVING`](#372-hàm-tổng-hợp-group-by-và-having)
  - [4. Native query (SQL thuần)](#4-native-query-sql-thuần)
    - [4.1. Native query là gì?](#41-native-query-là-gì)
    - [4.2. Khai báo bằng `@Query(nativeQuery = true)`](#42-khai-báo-bằng-querynativequery--true)
    - [4.3. Truyền tham số](#43-truyền-tham-số)
    - [4.4. `JOIN` và truy vấn con trong native query](#44-join-và-truy-vấn-con-trong-native-query)

---
## 1. Các mối quan hệ(1..N, N..N) trong JPA (@ManyToMany, @OneToMany,...)

Quan hệ trong JPA là cách ánh xạ sự liên kết giữa các Entity với các quan hệ giữa bảng trong cơ sở dữ liệu. Trong Java, liên kết được biểu diễn bằng thuộc tính tham chiếu đến một Entity hoặc một collection Entity; trong database, liên kết thường được biểu diễn bằng khóa ngoại hoặc bảng trung gian.

| Loại quan hệ | Ý nghĩa | Annotation |
|---|---|---|
| N:1 | Nhiều thực thể phía này liên kết với một thực thể phía kia | `@ManyToOne` |
| 1:N | Một thực thể liên kết với nhiều thực thể phía kia | `@OneToMany` |
| 1:1 | Mỗi thực thể liên kết với tối đa một thực thể phía kia | `@OneToOne` |
| N:N | Mỗi thực thể ở cả hai phía có thể liên kết với nhiều thực thể phía còn lại | `@ManyToMany` |

### 1.1. Quan hệ N:1 với `@ManyToOne`

#### 1.1.1. Định nghĩa

Quan hệ nhiều–một (many-to-one) là quan hệ trong đó nhiều thực thể của một loại có thể cùng tham chiếu đến một thực thể của loại khác.

`@ManyToOne` được đặt trên thuộc tính tham chiếu đơn, không phải collection. Với cách mapping bằng khóa ngoại thông thường, khóa ngoại nằm ở bảng tương ứng với Entity khai báo thuộc tính này.

**Các cấu hình thường dùng:**

| Cấu hình | Ý nghĩa |
|---|---|
| `fetch` | Quy định cách nạp Entity liên quan; mặc định là `EAGER` |
| `optional` | Cho biết quan hệ có được phép không tồn tại hay không; mặc định là `true` |
| `cascade` | Quy định các thao tác được lan truyền sang Entity liên quan; mặc định không có |

`@JoinColumn` chỉ định cột dùng để liên kết:

- `name`: Tên cột khóa ngoại.
- `referencedColumnName`: Tên cột được tham chiếu; mặc định tham chiếu khóa chính của Entity đích.
- `nullable`: Khai báo cột có cho phép null khi sinh schema hay không.

**Lưu ý:**`optional = false` diễn tả quan hệ bắt buộc ở mức JPA; `nullable = false` diễn tả ràng buộc cột khi sinh schema. Nếu quản lý schema bằng SQL hoặc migration, cần tạo các ràng buộc database tương ứng.

Không dùng `@Column` để ánh xạ thuộc tính tham chiếu đến một Entity.

#### 1.1.2. Ví dụ: Phòng chiếu thuộc một rạp

Nhiều `ScreeningRoom` có thể cùng thuộc một `Cinema`. Mỗi phòng bắt buộc thuộc về một rạp.

```text
cinemas                         screening_rooms
id (PK) | name                  id (PK) | name     | cinema_id (FK, NOT NULL)
1       | CGV Vincom            10      | Phòng 01 | 1
2       | Lotte Cinema          11      | Phòng 02 | 1
                                12      | Phòng 01 | 2
```

Class `ScreeningRoom`:

```java
@Entity
@Table(name = "screening_rooms")
@Getter
@Setter
@NoArgsConstructor
public class ScreeningRoom {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(name = "seat_count", nullable = false)
    private Integer seatCount;

    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "cinema_id", nullable = false)
    private Cinema cinema;
}
```

`cinema_id` nằm ở bảng `screening_rooms` và tham chiếu `cinemas.id`. Thuộc tính `cinema` cho phép truy cập rạp bằng `room.getCinema()`.

**Ví dụ** tạo phòng cho rạp đã tồn tại, giả sử Service đã inject hai repository:

```java
@Transactional
public Long createRoom(Long cinemaId, String name, Integer seatCount) {
    Cinema cinema = cinemaRepository.findById(cinemaId)
            .orElseThrow(() -> new IllegalArgumentException("Không tìm thấy rạp"));

    ScreeningRoom room = new ScreeningRoom();
    room.setName(name);
    room.setSeatCount(seatCount);
    room.setCinema(cinema);

    return screeningRoomRepository.save(room).getId();
}
```

SQL insert có dạng minh họa:

```sql
INSERT INTO screening_rooms (name, seat_count, cinema_id)
VALUES ('Phòng 03', 80, 1);
```

### 1.2. Quan hệ 1:N với `@OneToMany`

#### 1.2.1. Định nghĩa

Quan hệ một–nhiều (one-to-many) là quan hệ trong đó một thực thể có thể liên kết với nhiều thực thể của loại khác.

`@OneToMany` được đặt trên thuộc tính collection, thường là `List` hoặc `Set`. Quan hệ 1:N và N:1 có thể là hai cách nhìn của cùng một liên kết, tùy phía đang xét.

Các cấu hình thường dùng gồm `mappedBy`, `cascade`, `orphanRemoval` và `fetch`. Fetch mặc định là `LAZY`.

#### 1.2.2. Quan hệ một chiều và hai chiều

- **Một chiều (unidirectional):** Chỉ một Entity khai báo thuộc tính tham chiếu đến phía còn lại.
- **Hai chiều (bidirectional):** Cả hai Entity đều khai báo thuộc tính tham chiếu đến nhau.

Chiều của quan hệ mô tả khả năng điều hướng giữa các object Java. Khai báo hai chiều không có nghĩa phải tạo hai khóa ngoại.

Không bắt buộc mọi quan hệ đều phải hai chiều. Chỉ thêm chiều truy cập khi cần thiết cho việc sử dụng mô hình Entity.

#### 1.2.3. Owning side, inverse side

| Khái niệm | Vai trò |
|---|---|
| Owning side — phía sở hữu | Quyết định việc ghi hoặc cập nhật liên kết xuống database |
| Inverse side — phía đối ứng | Tham chiếu lại mapping của owning side bằng `mappedBy` |


Với cặp `@OneToMany` / `@ManyToOne` hai chiều, phía `@ManyToOne` là owning side. Phía `@OneToMany` khai báo `mappedBy`.


**Ví dụ: Một rạp có nhiều phòng chiếu**
```java
@OneToMany(mappedBy = "cinema")
private List<ScreeningRoom> rooms = new ArrayList<>();
```

### 1.3. Quan hệ 1:1 với `@OneToOne`

#### 1.3.1. Định nghĩa

Quan hệ một–một (one-to-one) là quan hệ trong đó mỗi thực thể ở một phía liên kết với tối đa một thực thể ở phía còn lại.

`@OneToOne` được đặt trên thuộc tính tham chiếu đơn. Có thể biểu diễn quan hệ bằng khóa ngoại.

Trong quan hệ hai chiều dùng khóa ngoại, phía chứa mapping khóa ngoại là owning side; phía còn lại dùng `mappedBy`.


#### 1.3.2. Ví dụ: Nhân viên và hồ sơ nhân viên

Mỗi `Employee` có tối đa một `EmployeeProfile`. 

```text
employees.id ← employee_profiles.employee_id (FK, UNIQUE, NOT NULL)
```

Các thuộc tính quan hệ cần thêm:

```java
// Trong Employee
@OneToOne(mappedBy = "employee", fetch = FetchType.LAZY)
private EmployeeProfile profile;

// Trong EmployeeProfile
@OneToOne(fetch = FetchType.LAZY, optional = false)
@JoinColumn(name = "employee_id", nullable = false, unique = true)
private Employee employee;
```

`EmployeeProfile` là Entity riêng, có ID và các thông tin hồ sơ. `EmployeeProfile.employee` sở hữu mapping; `Employee.profile` là phía đối ứng.

### 1.4. Quan hệ N:N với `@ManyToMany`

#### 1.4.1. Định nghĩa

Quan hệ nhiều–nhiều (many-to-many) là quan hệ trong đó mỗi thực thể ở cả hai phía đều có thể liên kết với nhiều thực thể phía còn lại.

`@ManyToMany` được đặt trên thuộc tính collection. Trong database quan hệ, liên kết N:N được biểu diễn bằng bảng trung gian chứa khóa ngoại tham chiếu đến hai bảng.

Mỗi dòng của bảng trung gian biểu diễn một liên kết. Cặp khóa ngoại thường được dùng làm khóa chính ghép hoặc có ràng buộc `UNIQUE` để tránh liên kết trùng.

#### 1.4.2. Mapping bằng `@JoinTable`

`@JoinTable` mô tả bảng trung gian:

| Thuộc tính | Ý nghĩa |
|---|---|
| `name` | Tên bảng trung gian |
| `joinColumns` | Cột tham chiếu Entity đang khai báo `@JoinTable` |
| `inverseJoinColumns` | Cột tham chiếu Entity phía còn lại |

Với quan hệ hai chiều, có thể chọn một trong hai phía làm owning side. Phía đó khai báo `@JoinTable`; phía đối ứng dùng `mappedBy`. Fetch mặc định là `LAZY`.

**Ví dụ: Phim và thể loại**

```text
movies.id ← movie_genres.movie_id
            movie_genres.genre_id → genres.id
```

```java
// Trong Movie, thay cho String genre
@ManyToMany(fetch = FetchType.LAZY)
@JoinTable(
    name = "movie_genres",
    joinColumns = @JoinColumn(name = "movie_id"),
    inverseJoinColumns = @JoinColumn(name = "genre_id"),
    uniqueConstraints = @UniqueConstraint(
        name = "uk_movie_genre",
        columnNames = {"movie_id", "genre_id"}
    )
)
private Set<Genre> genres = new HashSet<>();

// Trong Entity Genre được bổ sung
@ManyToMany(mappedBy = "genres")
private Set<Movie> movies = new HashSet<>();
```

#### 1.4.3. Khi nào nên tạo Entity cho bảng trung gian?

Nên tạo Entity trung gian khi liên kết cần có dữ liệu riêng, được truy vấn độc lập hoặc có vòng đời nghiệp vụ riêng. Khi đó, thay `@ManyToMany` trực tiếp bằng hai quan hệ `@ManyToOne` từ Entity trung gian đến hai Entity liên quan.

**Ví dụ: Vé liên kết suất chiếu và ghế**

```text
Showtime (1) ← (N) Ticket (N) → (1) Seat
```

`Ticket.java`:

```java
@ManyToOne(fetch = FetchType.LAZY, optional = false)
@JoinColumn(name = "showtime_id", nullable = false)
private Showtime showtime;

@ManyToOne(fetch = FetchType.LAZY, optional = false)
@JoinColumn(name = "seat_id", nullable = false)
private Seat seat;

@ManyToOne(fetch = FetchType.LAZY, optional = false)
@JoinColumn(name = "invoice_id", nullable = false)
private Invoice invoice;

@Column(name = "final_price", nullable = false)
private BigDecimal finalPrice;
```

### 1.5. Cascade

#### 1.5.1. Định nghĩa và các loại cascade

Cascade là cơ chế lan truyền một thao tác trên Entity sang các Entity liên quan theo cấu hình của quan hệ.

| CascadeType | Ý nghĩa |
|---|---|
| `PERSIST` | Lan truyền thao tác persist Entity mới |
| `MERGE` | Lan truyền thao tác merge trạng thái Entity vào persistence context |
| `REMOVE` | Lan truyền thao tác xóa Entity |
| `REFRESH` | Lan truyền thao tác nạp lại trạng thái từ database |
| `DETACH` | Lan truyền thao tác tách Entity khỏi persistence context |
| `ALL` | Bao gồm tất cả thao tác trên |

#### 1.5.2. Nguyên tắc sử dụng

- Chọn cascade theo vòng đời của các Entity liên quan.
- Chỉ lan truyền xóa khi Entity liên quan thực sự cần bị xóa cùng Entity nguồn.
- Cascade không tự gán owning side hoặc đồng bộ quan hệ hai chiều.



### 1.6. `orphanRemoval` và sự khác nhau với cascade xóa

#### 1.6.1. Định nghĩa

`orphanRemoval` là cơ chế xóa Entity con khi Entity đó bị loại khỏi quan hệ với Entity cha. Cơ chế này áp dụng cho `@OneToMany` và `@OneToOne`, mặc định là `false`.

Khi bật `orphanRemoval = true`, loại một Entity con đang managed khỏi collection hoặc bỏ tham chiếu quan hệ có thể dẫn đến việc xóa Entity con khi flush.

Cơ chế này phù hợp khi Entity con thuộc riêng một Entity cha và không cần tồn tại độc lập sau khi bị tách khỏi cha. Không dùng nó cho thao tác chuyển Entity con sang cha khác.

#### 1.6.2. Phân biệt với `CascadeType.REMOVE`

| Cấu hình | Hành vi |
|---|---|
| `cascade = REMOVE` | Khi xóa Entity nguồn, lan truyền xóa Entity liên quan; chỉ bỏ phần tử khỏi collection chưa đủ để xóa phần tử đó |
| `orphanRemoval = true` | Xóa Entity con khi bị loại khỏi quan hệ; khi xóa cha cũng lan truyền xóa con, không cần thêm `REMOVE` chỉ cho mục đích này |

#### 1.6.3. Ví dụ: Bỏ vé khỏi hóa đơn nháp

```java
@OneToMany(mappedBy = "invoice", orphanRemoval = true)
private List<Ticket> tickets = new ArrayList<>();

public void removeTicket(Ticket ticket) {
    tickets.remove(ticket)
}
```
---

## 2.Derived query methods (truy vấn theo tên hàm)

### 2.1. Derived query method là gì?

Derived query method là phương thức Repository mà Spring Data JPA suy ra truy vấn từ tên phương thức theo một quy tắc đặt tên xác định.

Chúng ta khai báo tên hàm, tham số và kiểu trả về trong interface Repository. Spring Data JPA phân tích tên hàm để xác định điều kiện truy vấn; JPA provider như Hibernate thực thi truy vấn và tạo SQL phù hợp với database.

Không cần tự viết phần triển khai method hoặc câu truy vấn cho những trường hợp được quy tắc đặt tên hỗ trợ.

**Ví dụ:**

```java
public interface MovieRepository extends JpaRepository<Movie, Long> {
    List<Movie> findByGenre(String genre);
}
```

Gọi từ Service:

```java
List<Movie> movies = movieRepository.findByGenre("Hành động");
```

Điều kiện SQL tương đương về ý nghĩa:

```sql
SELECT * FROM movies WHERE genre = ?;
```

Giá trị truyền vào được bind qua tham số. SQL thực tế có thể khác về alias, danh sách cột và cú pháp tùy provider/database.

### 2.2. Cấu trúc tên phương thức

Tên method thường có dạng:

```text
[Thao tác][Tùy chọn]By[Điều kiện][OrderBy...]
```

- **Thao tác:** Tìm dữ liệu, kiểm tra tồn tại, đếm hoặc xóa.
- **Tùy chọn:** Có thể giới hạn số kết quả bằng `First`, `Top` hoặc loại trùng bằng `Distinct`.
- **`By`:** Phân tách phần thao tác với phần điều kiện.
- **Điều kiện:** Tên thuộc tính, toán tử so sánh và các từ nối `And`, `Or`.
- **`OrderBy`:** Khai báo sắp xếp cố định nếu cần.

| Tiền tố thường dùng | Mục đích |
|---|---|
| `find…By`, `read…By`, `get…By` | Lấy dữ liệu |
| `exists…By` | Kiểm tra có dữ liệu phù hợp hay không |
| `count…By` | Đếm số bản ghi phù hợp |
| `delete…By`, `remove…By` | Xóa các Entity phù hợp |

**Ví dụ phân tích tên hàm:** Trong `SeatRepository`:

```java
List<Seat> findAllByRoomIdOrderByPositionAsc(Long roomId);
```

```text
findAll       By       RoomId       OrderBy       Position       Asc
   ↓           ↓          ↓            ↓             ↓           ↓
Lấy danh sách  Lọc     room.id       Sắp xếp       position     Tăng dần
```

Từ `All` trong `findAllBy` giúp tên dễ đọc; `findByRoomId` cũng có thể trả danh sách. Số lượng kết quả không được quyết định chỉ bởi việc có từ `All` hay không, mà còn bởi kiểu trả về và các tùy chọn giới hạn.

### 2.3. Tên thuộc tính và truy cập thuộc tính lồng nhau

#### 2.3.1. Dùng tên thuộc tính Java

Phần điều kiện phải dùng tên thuộc tính của Entity, không dùng tên cột SQL. Tên thuộc tính được viết theo dạng PascalCase khi ghép vào tên method.

**Ví dụ:** `Movie` có:

```java
@Column(name = "production_year")
private Integer productionYear;
```

Method tương ứng trong `MovieRepository`:

```java
List<Movie> findByProductionYear(Integer productionYear);
```

Không viết `findByProduction_year(...)` theo tên cột SQL.

#### 2.3.2. Truy cập qua quan hệ

Spring Data có thể phân tích đường dẫn thuộc tính qua nhiều Entity. Dấu `_` có thể dùng để chỉ rõ điểm phân tách khi tên ghép dễ gây nhầm lẫn.

**Ví dụ:** `ScreeningRoom` có `Cinema cinema`, còn `Cinema` có `Long id`:

```java
Page<ScreeningRoom> findAllByCinemaId(Long cinemaId, Pageable pageable);
```

`CinemaId` được hiểu là `cinema.id` vì Entity hiện không có thuộc tính trực tiếp tên `cinemaId`. Có thể viết rõ đường dẫn thành:

```java
Page<ScreeningRoom> findAllByCinema_Id(Long cinemaId, Pageable pageable);
```

Hai cách trên là hai phương án đặt tên cho cùng nhu cầu, không cần khai báo cả hai.

Một ví dụ đường dẫn sâu hơn, đặt trong `ShowtimeRepository`:

```java
List<Showtime> findByRoom_Cinema_Id(Long cinemaId);
```

### 2.4. Các từ khóa điều kiện thường dùng

Trong ví dụ dưới đây, `Movie` có các thuộc tính `title`, `genre`, `productionYear`, `durationMinutes`, `description`; `Showtime` có `startTime` như trong project. Các chữ ký method được đặt ở Repository của Entity tương ứng.

| Từ khóa | Ví dụ tên method | Ý nghĩa điều kiện |
|---|---|---|
| Không thêm toán tử | `findByGenre` | Bằng giá trị truyền vào |
| `Not` | `findByGenreNot` | Khác giá trị truyền vào |
| `LessThan`, `LessThanEqual` | `findByDurationMinutesLessThan` | Nhỏ hơn; thêm `Equal` để bao gồm bằng |
| `GreaterThan`, `GreaterThanEqual` | `findByDurationMinutesGreaterThanEqual` | Lớn hơn hoặc bằng |
| `Between` | `findByProductionYearBetween` | Nằm trong khoảng, bao gồm hai đầu mút |
| `In`, `NotIn` | `findByGenreIn` | Thuộc hoặc không thuộc một tập giá trị |
| `IsNull`, `IsNotNull` | `findByDescriptionIsNull` | Thuộc tính null hoặc không null |
| `Containing` | `findByTitleContaining` | Chuỗi có chứa nội dung truyền vào |
| `StartingWith`, `EndingWith` | `findByTitleStartingWith` | Chuỗi bắt đầu hoặc kết thúc bằng nội dung truyền vào |
| `Like` | `findByTitleLike` | So khớp mẫu do người gọi cung cấp |
| `IgnoreCase` | `findByTitleContainingIgnoreCase` | Yêu cầu so sánh chuỗi không phân biệt hoa thường |
| `Before`, `After` | `findByStartTimeAfter` | Trước hoặc sau mốc truyền vào, không bao gồm mốc đó |

**Ví dụ khai báo:**

```java
// Trong MovieRepository
List<Movie> findByProductionYearBetween(Integer fromYear, Integer toYear);

List<Movie> findByGenreIn(Collection<String> genres);

List<Movie> findByDescriptionIsNull();

List<Movie> findByTitleContainingIgnoreCase(String keyword);

List<Movie> findByTitleLike(String pattern);

// Trong ShowtimeRepository
List<Showtime> findByStartTimeAfter(Instant time);
```

Số tham số phụ thuộc toán tử: `Between` nhận hai giá trị, `In` nhận collection, còn `IsNull` không nhận giá trị so sánh.

Với `Containing`, truyền `"Avengers"` để tìm chuỗi chứa nội dung đó. Với `Like`, người gọi tự truyền mẫu, chẳng hạn `"%Avengers%"`; `%` khớp một chuỗi ký tự, `_` khớp một ký tự. Không cần tự thêm `%` khi dùng `Containing`.

**Kết hợp điều kiện với `And`, `Or`**

`And` yêu cầu các điều kiện cùng đúng; `Or` yêu cầu ít nhất một điều kiện đúng.

**Ví dụ:** Tìm phim theo thể loại và năm sản xuất:

```java
List<Movie> findByGenreAndProductionYear(String genre, Integer productionYear);
```

Thứ tự tham số tương ứng với thứ tự điều kiện trong tên method.

Khi kết hợp cả `And` và `Or`, `And` có độ ưu tiên cao hơn. Ví dụ điều kiện `AOrBAndC` được hiểu là `A OR (B AND C)`. Tên method không có cú pháp đặt dấu ngoặc tùy ý; nên chuyển sang truy vấn tường minh khi biểu thức trở nên khó đọc.

### 2.5. Kiểu dữ liệu trả về

Chọn kiểu trả về theo số lượng kết quả dự kiến và nhu cầu sử dụng.

| Kiểu trả về | Mục đích |
|---|---|
| `List<T>` | Trả nhiều kết quả; không có kết quả thì trả collection rỗng |
| `Optional<T>` | Trả tối đa một kết quả; không có thì `Optional.empty()` |
| `boolean` | Kết quả kiểm tra tồn tại với `exists…By` |
| `long` | Số lượng bản ghi với `count…By` |
| `Page<T>` | Dữ liệu một trang và thông tin tổng số phần tử/trang |

**Ví dụ:**

```java
// Trong MovieRepository
List<Movie> findByGenre(String genre);

long countByGenre(String genre);

// Trong ScreeningRoomRepository
Optional<ScreeningRoom> findByCinema_IdAndName(Long cinemaId, String name);

boolean existsByCinema_IdAndName(Long cinemaId, String name);
```



### 2.6. Phân trang với `Pageable`

`Pageable` mang thông tin số trang, kích thước trang và sắp xếp. Không cần đưa từ `Pageable` vào tên method.

**Ví dụ:**

```java
Page<ScreeningRoom> findAllByCinemaId(Long cinemaId, Pageable pageable);
```

Gọi từ Service:

```java
Pageable pageable = PageRequest.of(0, 10, Sort.by("name"));
Page<ScreeningRoom> rooms =
        screeningRoomRepository.findAllByCinemaId(1L, pageable);
```

### 2.7. Xóa dữ liệu bằng derived query method

Tiền tố `delete…By` hoặc `remove…By` cho phép khai báo thao tác xóa theo điều kiện. Thao tác xóa cần chạy trong transaction.


**Ví dụ:** 

```java
// Trong ScreeningRoomRepository
long deleteByCinema_IdAndName(Long cinemaId, String name);
```

```java
// Trong Service đã inject screeningRoomRepository
@Transactional
public long deleteRoom(Long cinemaId, String name) {
    return screeningRoomRepository.deleteByCinema_IdAndName(cinemaId, name);
}
```

---

## 3. JPQL với @Query

### 3.1. JPQL là gì?

**JPQL (Jakarta Persistence Query Language)** là ngôn ngữ truy vấn của JPA, dùng để truy vấn dữ liệu thông qua Entity và các thuộc tính được mapping.

JPQL có cú pháp gần SQL, nhưng sử dụng **tên Entity và thuộc tính Java** thay cho tên bảng và cột. JPA provider như Hibernate chuyển truy vấn thành SQL phù hợp với database.

| Thành phần | Trong JPQL |
|---|---|
| Đối tượng truy vấn | Entity |
| Trường dữ liệu | Thuộc tính được mapping của Entity |
| Quan hệ | Thuộc tính tham chiếu hoặc collection đã được mapping |
| Kết quả | Có thể là Entity, giá trị đơn hoặc dữ liệu được chọn theo nhu cầu |

**Ví dụ:** Entity `Movie` mapping đến bảng `movies`, có thuộc tính `productionYear` mapping đến cột `production_year`:

```jpql
select m
from Movie m
where m.productionYear >= :year
```

- `Movie` là tên Entity; mặc định lấy tên class nếu không cấu hình tên khác trong `@Entity`.
- `m` là alias đại diện cho Entity trong truy vấn.
- `m.productionYear` là thuộc tính Java.
- `:year` là tham số cần truyền vào.
- `select m` chọn Entity `Movie`; không viết `select *` như SQL.

### 3.2. Khai báo JPQL bằng `@Query`

`@Query` là annotation của Spring Data JPA, dùng để khai báo truy vấn trực tiếp trên method Repository. Mặc định `nativeQuery = false`, chuỗi truy vấn được xử lý theo ngôn ngữ truy vấn JPA.

Khi dùng `@Query`, nội dung annotation xác định truy vấn; không cần diễn đạt toàn bộ điều kiện bằng tên method như derived query.

**Ví dụ trong `MovieRepository`:**

```java
@Query("""
    select m
    from Movie m
    where m.genre = :genre
      and m.productionYear >= :year
    """)
List<Movie> searchMovies(
        @Param("genre") String genre,
        @Param("year") Integer year);
```

Gọi từ Service:

```java
List<Movie> movies = movieRepository.searchMovies("Hành động", 2024);
```

### 3.3. Truyền tham số vào truy vấn

#### 3.3.1. Tham số có tên

Tham số có tên được viết dưới dạng `:parameterName` và liên kết với tham số Java bằng `@Param("parameterName")`.

Cách này giúp đọc truy vấn rõ ràng và tránh phụ thuộc vào thứ tự tham số Java khi đã khai báo `@Param` đầy đủ.

**Ví dụ:**

```java
@Query("select m from Movie m where m.genre = :genre")
List<Movie> searchByGenre(@Param("genre") String value);
```

#### 3.3.2. Tham số theo vị trí

Tham số theo vị trí dùng `?1`, `?2`, ... tương ứng với thứ tự tham số được bind của method, bắt đầu từ `1`.

**Ví dụ:**

```java
@Query("""
    select m from Movie m
    where m.genre = ?1 and m.productionYear >= ?2
    """)
List<Movie> searchMovies(String genre, Integer year);
```

### 3.4. Lọc dữ liệu với `WHERE`

`WHERE` giới hạn các kết quả theo biểu thức điều kiện. Có thể kết hợp các phép so sánh bằng `AND`, `OR`, `NOT` và dấu ngoặc để xác định độ ưu tiên.

| Biểu thức | Ý nghĩa |
|---|---|
| `=`, `<>`, `>`, `>=`, `<`, `<=` | So sánh giá trị |
| `BETWEEN ... AND ...` | Trong khoảng, bao gồm hai đầu mút |
| `IN (...)` | Thuộc một tập giá trị hoặc kết quả truy vấn con |
| `LIKE` | So khớp mẫu chuỗi |
| `IS NULL`, `IS NOT NULL` | Kiểm tra null |

**Ví dụ tìm phim theo từ khóa:**

```java
@Query("""
    select m
    from Movie m
    where lower(m.title) like lower(concat('%', :keyword, '%'))
    """)
List<Movie> searchByTitle(@Param("keyword") String keyword);
```

### 3.5. Truy vấn qua quan hệ và `JOIN`

#### 3.5.1. Điều hướng bằng đường dẫn thuộc tính

JPQL cho phép đi qua quan hệ tham chiếu đơn bằng dấu chấm. Provider sử dụng mapping để xác định cách liên kết các bảng.

**Ví dụ rút gọn từ `ShowtimeRepository`:**

```java
@Query("""
    select s
    from Showtime s
    where s.room.cinema.id = :cinemaId
      and s.startTime >= :from
      and s.startTime < :to
    """)
List<Showtime> findByCinemaAndTimeRange(
        @Param("cinemaId") Long cinemaId,
        @Param("from") Instant from,
        @Param("to") Instant to);
```

`s.room.cinema.id` đi từ suất chiếu đến phòng, đến rạp, rồi lấy ID rạp. Khoảng thời gian `[from, to)` bao gồm mốc đầu và loại mốc cuối, giúp các khoảng liên tiếp không trùng mốc biên.

#### 3.5.2. `JOIN` và `LEFT JOIN`

- **`JOIN` / `INNER JOIN`:** Chỉ giữ các kết quả có phần tử liên quan thỏa điều kiện join.
- **`LEFT JOIN`:** Giữ Entity bên trái kể cả khi không có phần tử liên quan; các giá trị phía phải khi đó là null.

JPQL thường join theo thuộc tính quan hệ, thay vì tự viết điều kiện khóa ngoại như SQL.

**Ví dụ lọc suất chiếu theo tên rạp:**

```java
@Query("""
    select s
    from Showtime s
    join s.room r
    join r.cinema c
    where c.name = :cinemaName
    """)
List<Showtime> findByCinemaName(@Param("cinemaName") String cinemaName);
```

#### 3.5.3. `JOIN FETCH`

`JOIN` phục vụ liên kết dữ liệu để lọc hoặc chọn kết quả; `JOIN FETCH` yêu cầu nạp quan hệ cùng Entity được chọn.

**Ví dụ:**

```java
@Query("""
    select s
    from Showtime s
    join fetch s.movie
    join fetch s.room
    where s.id = :id
    """)
Optional<Showtime> findDetailById(@Param("id") Long id);
```

Hai quan hệ `movie`, `room` được fetch để sử dụng sau khi lấy suất chiếu.

### 3.6. Truy vấn con — Subquery

Subquery là truy vấn nằm trong một truy vấn khác, thường dùng trong `WHERE` hoặc `HAVING` với các biểu thức như `IN`, `EXISTS` hoặc phép so sánh.

**Ví dụ: `MovieRepository`:** Tìm các phim có suất chiếu tại một rạp trong khoảng thời gian:

```java
@Query("""
    select m
    from Movie m
    where m.id in (
        select s.movie.id
        from Showtime s
        where s.room.cinema.id = :cinemaId
          and s.startTime >= :from
          and s.startTime < :to
    )
    """)
List<Movie> findMoviesByCinemaAndTimeRange(
        @Param("cinemaId") Long cinemaId,
        @Param("from") Instant from,
        @Param("to") Instant to);
```

Truy vấn con chọn ID các phim có suất chiếu phù hợp. Truy vấn ngoài trả về các Entity `Movie` có ID nằm trong kết quả đó. Một phim có nhiều suất chiếu phù hợp vẫn chỉ được chọn một lần ở truy vấn ngoài này.

### 3.7. Chọn dữ liệu và thống kê

#### 3.7.1. Chọn Entity hoặc một thuộc tính

Biểu thức sau `SELECT` quyết định hình dạng kết quả. Kiểu trả về của method cần tương thích với dữ liệu được chọn.

**Ví dụ từ `TicketRepository`:**

```java
@Query("""
    select t.seat.id
    from Ticket t
    where t.showtime.id = :showtimeId
    """)
Set<Long> findBookedSeats(@Param("showtimeId") Long showtimeId);
```

Truy vấn chỉ chọn ID ghế đã có vé trong suất chiếu, nên trả `Set<Long>` thay vì `List<Ticket>`.

#### 3.7.2. Hàm tổng hợp, `GROUP BY` và `HAVING`

Các hàm thường dùng gồm `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.

- `WHERE`: Lọc dữ liệu trước khi nhóm.
- `GROUP BY`: Nhóm dữ liệu theo các thuộc tính.
- `HAVING`: Lọc các nhóm theo điều kiện, thường liên quan đến giá trị tổng hợp.

**Ví dụ đếm vé của từng hóa đơn và chỉ lấy hóa đơn có ít nhất một số lượng vé nhất định:**

```java
@Query("""
    select t.invoice.id, count(t)
    from Ticket t
    group by t.invoice.id
    having count(t) >= :minimum
    """)
List<Object[]> countTicketsByInvoice(@Param("minimum") Long minimum);
```

---

## 4. Native query (SQL thuần)

### 4.1. Native query là gì?

Native query là truy vấn viết bằng SQL của hệ quản trị cơ sở dữ liệu đang sử dụng và được thực thi thông qua JPA.

Khác với JPQL, native query làm việc trực tiếp với **tên bảng, tên cột và các phép liên kết trong database**, thay vì tên Entity và thuộc tính Java.

Native query phù hợp khi cần dùng cú pháp hoặc tính năng riêng của database, tái sử dụng SQL có sẵn, hoặc diễn đạt truy vấn khó viết bằng JPQL.

| Tiêu chí | JPQL | Native query |
|---|---|---|
| Tên đối tượng | Tên Entity | Tên bảng |
| Tên trường | Thuộc tính Java | Cột database |
| Liên kết dữ liệu | Thường đi qua quan hệ đã mapping | Viết điều kiện liên kết giữa các bảng |
| Cú pháp | Theo ngôn ngữ truy vấn JPA | Theo database đang sử dụng |
| Khả năng chuyển database | Ít phụ thuộc cú pháp SQL cụ thể hơn | Cần kiểm tra và có thể phải sửa SQL |

**Ví dụ:** Tìm phim từ một năm sản xuất:

```jpql
-- JPQL
select m from Movie m where m.productionYear >= :year
```

```sql
-- SQL thuần
SELECT m.* FROM movies m WHERE m.production_year >= :year
```

Trong ví dụ SQL trên, `:year` là placeholder do framework xử lý trước khi gửi câu lệnh đến database, không phải cú pháp tham số để dán trực tiếp vào mọi công cụ SQL.

### 4.2. Khai báo bằng `@Query(nativeQuery = true)`

Đặt `nativeQuery = true` để Spring Data JPA xử lý nội dung `value` là SQL thuần. Tên method có thể đặt theo nghiệp vụ, không cần tuân theo cấu trúc derived query.

**Ví dụ trong `MovieRepository`:**

```java
@Query(
    value = """
        SELECT m.*
        FROM movies m
        WHERE m.production_year >= :year
        """,
    nativeQuery = true
)
List<Movie> findMoviesFromYear(@Param("year") Integer year);
```

Gọi từ Service:

```java
List<Movie> movies = movieRepository.findMoviesFromYear(2024);
```

### 4.3. Truyền tham số

Có thể dùng tham số có tên qua `@Param` hoặc tham số theo vị trí như `?1`, `?2`.

**Ví dụ tìm phim theo từ khóa:**

```java
@Query(
    value = """
        SELECT m.*
        FROM movies m
        WHERE LOWER(m.title) LIKE LOWER(CONCAT('%', :keyword, '%'))
        """,
    nativeQuery = true
)
List<Movie> searchMoviesByTitle(@Param("keyword") String keyword);
```

### 4.4. `JOIN` và truy vấn con trong native query

SQL thuần sử dụng các phép `JOIN`, `LEFT JOIN`, subquery, `GROUP BY`, `HAVING`, ... theo cú pháp của database.

Điều kiện liên kết được viết tường minh bằng các cột tương ứng. Không sử dụng đường dẫn Entity như `s.room.cinema.id` trong SQL thuần.

**Ví dụ chuyển truy vấn tìm phim có suất chiếu trong project từ JPQL sang SQL:**

```java
@Query(
    value = """
        SELECT m.*
        FROM movies m
        WHERE m.id IN (
            SELECT s.movie_id
            FROM showtimes s
            JOIN screening_rooms r ON r.id = s.room_id
            WHERE r.cinema_id = :cinemaId
              AND s.start_time >= :from
              AND s.start_time < :to
        )
        """,
    nativeQuery = true
)
List<Movie> findMoviesByCinemaAndTimeRangeNative(
        @Param("cinemaId") Long cinemaId,
        @Param("from") Instant from,
        @Param("to") Instant to);
```
