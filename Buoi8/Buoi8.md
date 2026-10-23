# BUỔI 8: TỐI ƯU JPA, VALIDATION VÀ EXCEPTION HANDLING
---
- [BUỔI 8: TỐI ƯU JPA, VALIDATION VÀ EXCEPTION HANDLING](#buổi-8-tối-ưu-jpa-validation-và-exception-handling)
  - [1. Các phương pháp tối ưu JPA](#1-các-phương-pháp-tối-ưu-jpa)
    - [1.2. Lazy / Eager initialization](#12-lazy--eager-initialization)
      - [1.2.1 Lazy Loading](#121-lazy-loading)
      - [1.2.2 Eager Loading](#122-eager-loading)
    - [1.3. Pagination](#13-pagination)
      - [1.3.1 Tổng Quan về Phân trang và Sắp xếp với Spring Data JPA](#131-tổng-quan-về-phân-trang-và-sắp-xếp-với-spring-data-jpa)
    - [1.3.2 Triển Khai: Phân trang và Sắp xếp trong Spring Data JPA](#132-triển-khai-phân-trang-và-sắp-xếp-trong-spring-data-jpa)
    - [1.3.3. Cách sử dụng:](#133-cách-sử-dụng)
    - [1.4. Caching](#14-caching)
    - [1.5. Manual / Native query](#15-manual--native-query)
      - [1.5.1 Native query:](#151-native-query)
    - [1.6. Dynamic query](#16-dynamic-query)
    - [1.7. Phòng tránh N + 1 query](#17-phòng-tránh-n--1-query)
  - [2. Tìm hiểu về Validation](#2-tìm-hiểu-về-validation)
    - [2.1 Vì Sao Cần Validate Data Trong Spring Boot?](#21-vì-sao-cần-validate-data-trong-spring-boot)
    - [2.2 Thêm Dependency Validation](#22-thêm-dependency-validation)
    - [2.3 Các Annotation Validation Thường Dùng](#23-các-annotation-validation-thường-dùng)
    - [2.4 Validate Request Body (Payload)](#24-validate-request-body-payload)
    - [2.5. Validate PathVariable và RequestParam](#25-validate-pathvariable-và-requestparam)
  - [3. Tìm hiểu về Exception handling](#3-tìm-hiểu-về-exception-handling)
  - [4. Override default fetch plan với join fetch / entity graph](#4-override-default-fetch-plan-với-join-fetch--entity-graph)
    - [4.1 JOIN và JOIN FETCH khác nhau thế nào?](#41-join-và-join-fetch-khác-nhau-thế-nào)
    - [4.2 EntityGraph là gì?](#42-entitygraph-là-gì)

---
## 1. Các phương pháp tối ưu JPA
### 1.2. Lazy / Eager initialization
#### 1.2.1 Lazy Loading
Tải Tạm Hoãn có nghĩa là các thực thể liên quan chỉ được tìm nạp khi chúng được truy cập lần đầu tiên. Hibernate sử dụng các đối tượng proxy để đạt được hành vi này.
- Là mặc định cho các mối quan hệ ``@OneToMany`` và ``@ManyToMany``.
- Việc tìm nạp xảy ra theo yêu cầu, giảm bộ nhớ ban đầu và tải cơ sở dữ liệu.

**Lợi ích của Tải Tạm Hoãn**
- Chỉ dữ liệu cần thiết được tải, cải thiện hiệu suất khởi động của ứng dụng.
- Bộ nhớ chỉ được tiêu thụ khi dữ liệu được truy cập, làm cho nó tối ưu cho các bộ dữ liệu lớn.
- Chỉ dữ liệu cần thiết được truyền đi, giảm việc sử dụng băng thông.

**Hạn chế của Tải Tạm Hoãn**
- Khi dữ liệu cuối cùng được yêu cầu, nó có thể gây ra độ trễ vì dữ liệu bổ sung cần được tải.
- Có thể dẫn đến nhiều lệnh gọi cơ sở dữ liệu, làm tăng tổng số yêu cầu.
- Yêu cầu logic phức tạp hơn để xử lý việc tải dữ liệu bị trì hoãn.
xin chaào cacác banạn
#### 1.2.2 Eager Loading
Tải Nhanh là một chiến lược trong đó tất cả dữ liệu cần thiết, bao gồm cả dữ liệu liên quan, được tải ngay từ đầu trong một yêu cầu duy nhất, đảm bảo truy cập tức thì vào tất cả thông tin cần thiết.

- Là mặc định cho các mối quan hệ `@ManyToOne` và `@OneToOne`.

**Lợi ích của Tải Nhanh**
- Tất cả dữ liệu cần thiết được tải ngay từ đầu, đảm bảo không có độ trễ khi truy cập dữ liệu liên quan.
- Tất cả dữ liệu có sẵn ngay lập tức, không cần logic phức tạp để tải dữ liệu liên quan sau này.

**Hạn chế của Tải Nhanh**
- Việc tải tất cả dữ liệu cùng một lúc có thể dẫn đến thời gian khởi động ứng dụng hoặc tải trang chậm hơn.
- Việc tìm nạp tất cả dữ liệu liên quan cùng một lúc có thể tiêu thụ nhiều bộ nhớ hơn.
- Truy xuất nhiều dữ liệu hơn mức cần thiết, dẫn đến lãng phí trong xử lý dữ liệu và băng thông.

---
### 1.3. Pagination
#### 1.3.1 Tổng Quan về Phân trang và Sắp xếp với Spring Data JPA
Khi ứng dụng của bạn phải làm việc với các tập dữ liệu lớn, việc tải tất cả các bản ghi cùng một lúc là không hiệu quả và có thể gây ảnh hưởng tiêu cực đến hiệu suất và trải nghiệm người dùng. Spring Data JPA cung cấp các công cụ mạnh mẽ, tích hợp sẵn để xử lý Phân trang (Pagination) và Sắp xếp (Sorting) một cách dễ dàng.

**Phân trang (Pagination)**
Phân trang là kỹ thuật chia một tập dữ liệu lớn thành các "trang" nhỏ hơn, dễ quản lý hơn, chỉ lấy từng trang một khi cần.
- Ví dụ: Thay vì tải 1.000 hồ sơ khách hàng, bạn có thể tải 10 hồ sơ cho mỗi trang.
- Lợi ích: Giảm tải bộ nhớ, tăng tốc độ phản hồi API, và cải thiện trải nghiệm người dùng bằng cách hiển thị dữ liệu theo từng phần.

**Sắp xếp (Sorting)**
Sắp xếp cho phép bạn sắp xếp các bản ghi theo thứ tự tăng dần (ascending) hoặc giảm dần (descending) dựa trên một hoặc nhiều trường dữ liệu.
- Ví dụ: Sắp xếp danh sách sản phẩm theo tên (A-Z) hoặc theo giá (thấp đến cao).
- Lợi ích: Cải thiện khả năng trình bày dữ liệu và giúp người dùng tìm kiếm thông tin hiệu quả hơn.

**Các Class/Interface quan trọng trong Spring Data JPA**
- `Pageable:` Một interface đại diện cho thông tin phân trang (số trang, kích thước trang) và sắp xếp.
- `PageRequest:` Một lớp triển khai của `Pageable`, dùng để tạo đối tượng phân trang với các tham số cụ thể.
- `Sort`: Một lớp đại diện cho thông tin sắp xếp (trường sắp xếp, hướng sắp xếp).
- `Page<T>`: Một interface được trả về bởi các phương thức phân trang của Repository. Nó chứa danh sách các phần tử (dữ liệu của trang hiện tại), cùng với siêu dữ liệu (metadata) về phân trang như tổng số trang, tổng số phần tử, số trang hiện tại, v.v.

### 1.3.2 Triển Khai: Phân trang và Sắp xếp trong Spring Data JPA
**Bước 0: Thiết lập cấu hình và PageResponse cho phân trang**
```yml
spring:
  data:
    web:
      pageable:
        default-page-size: 10
        max-page-size: 100
        one-indexed-parameters: false
```
```java
@Getter
@AllArgsConstructor
@Builder
public class PageResponse<T> {
    private List<T> content;
    private Integer page;
    private Integer size;
    private Long totalElement;
    private Integer totalPage;
    private Boolean hasNext;

    public static <T> PageResponse<T> from(Page<T> result) {
        return PageResponse.<T>builder()
                .content(result.getContent())
                .page(result.getNumber())
                .size(result.getSize())
                .totalElement(result.getTotalElements())
                .totalPage(result.getTotalPages())
                .hasNext(result.hasNext())
                .build();
    }
}
```
**Bước 1: Tạo Lớp Entity**
```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class ScreeningRoomResponse {
    private Long id;
    private String name;
    private Integer seatCount;
    private String features;
}
```
**Bước 2: Tạo Interface Repository**
`JpaRepository` đã có sẵn các hàm có tích hợp phân trang với tham sô truyền vào là `Pageable` rồi hoặc ta hoàn toàn có thể tự định nghĩa như sau:
```java
public interface ScreeningRoomRepository extends JpaRepository<ScreeningRoom, Long> {
    public Page<ScreeningRoom> findAllByCinemaId(Long cinemaId, Pageable pageable);
}
```
**Bước 3: Tạo Lớp Service**
Ta có thể kiểm tra giá trị sort truyền vào có cho phép sort không và có thể tự thêm trường sort mặc định nếu như người dùng không truyền
```java
@Service
@RequiredArgsConstructor
public class ScreeningRoomService {
    private final ScreeningRoomRepository screeningRoomRepository;
    private final ScreeningRoomMapper screeningRoomMapper;
    private final Set<String> SCREENING_ROOM_SORT_FIELDS = Set.of("name");
    private final Sort SCREENING_ROOM_DEFAULT_SORT =
            Sort.by(Sort.Direction.DESC, "name");

    public Page<ScreeningRoomResponse> getRooms(Long cinemaId, Pageable pageable)
    {
        pageable = PageableUtils.validateAndApplyDefaultSort
                (pageable, SCREENING_ROOM_SORT_FIELDS, SCREENING_ROOM_DEFAULT_SORT);
        return screeningRoomRepository
                .findAllByCinemaId(cinemaId, pageable)
                .map(screeningRoom -> screeningRoomMapper
                        .toScreeningRoomResponse(screeningRoom));

    }
}

```
```java
   public static Pageable validateAndApplyDefaultSort(
            Pageable pageable,
            Set<String> allowedFields,
            Sort defaultSort
    ) {
        Sort sort = pageable.getSort();

        if (sort.isUnsorted()) {
            sort = defaultSort;
        }

        for(Sort.Order order : sort)
        {
            if(!allowedFields.contains(order.getProperty()))
                throw new ApiException(ErrorCode.VALIDATION_ERROR);
        }



        return PageRequest.of(
                pageable.getPageNumber(),
                pageable.getPageSize(),
                sort
        );
    }
}
```
**Bước 4: Tạo Lớp Controller với hàm**
```java
@GetMapping("/{cinemaId}/rooms")
    public ResponseEntity<ApiResponse<PageResponse<ScreeningRoomResponse>>> getRooms(
            @PathVariable Long cinemaId,
            Pageable pageable)
    {
        return ResponseEntity.status(HttpStatus.OK)
                .body(ApiResponse
                        .success(PageResponse.from(screeningRoomService.getRooms(cinemaId, pageable))));

    }

```
### 1.3.3. Cách sử dụng:
```http
http://localhost:8080/api/v1/cinemas/1/rooms?size=2&page=1&sort=name,asc
```
### 1.4. Caching

### 1.5. Manual / Native query
#### 1.5.1 Native query:
Native Queries (Truy vấn gốc) trong JPA là cách linh hoạt và mạnh mẽ nhất để truy xuất dữ liệu, cho phép chúng ta sử dụng tất cả các tính năng độc quyền được hỗ trợ bởi cơ sở dữ liệu (RDBMS) đang sử dụng. Spring Data JPA giúp việc định nghĩa và thực thi Native Queries trở nên dễ dàng hơn.

Trong JPA thuần hoặc Hibernate, việc định nghĩa và thực hiện một native query đòi hỏi nhiều bước cấu hình. Annotation ``@Query`` của Spring Data giúp loại bỏ hầu hết mã phức tạp đó.

Để định nghĩa một native query, chúng ta chú thích phương thức Repository với ``@Query``, thiết lập thuộc tính ``nativeQuery=true``, và cung cấp câu lệnh SQL gốc làm giá trị:
```java
//native
@Query(
    value = """
        SELECT *
        FROM movies m
        WHERE m.id IN (
            SELECT s.movie_id
            FROM showtimes s
            JOIN rooms r ON r.id = s.room_id
            WHERE r.cinema_id = :cinemaId
              AND s.start_time >= :from
              AND s.start_time < :to
              AND s.start_time > :currentTime
        )
        """,
    nativeQuery = true
)
List<Movie> findAvailableMovies(
    @Param("currentTime") Instant currentTime,
    @Param("cinemaId") Long cinemaId,
    @Param("from") Instant from,
    @Param("to") Instant to
);
```
```java
//jpql
@Query("""
            select m
            from Movie m
            where m.id in 
                (
                    select s.movie.id
                    from Showtime s
                    where s.room.cinema.id = :cinemaId and
                          s.startTime >= :from and
                          s.startTime < :to and
                          s.startTime > :currentTime                              
                )                                      
            """)
    public List<Movie> findAvailableMovies(
            @Param("currentTime")Instant currentTime,
            @Param("cinemaId") Long cinemaId,
            @Param("from") Instant from,
            @Param("to") Instant to);
```
**Hạn Chế của Native Queries:**
- Spring Data JPA và Persistence Provider (Hibernate) không tự động điều chỉnh truy vấn theo cú pháp SQL của một cơ sở dữ liệu cụ thể (ví dụ: cú pháp Oracle khác với MySQL).
- Yêu cầu: Chúng ta phải tự đảm bảo câu lệnh SQL được cung cấp tương thích với tất cả các RDBMS mà ứng dụng hỗ trợ.
### 1.6. Dynamic query
Truy vấn động (dynamic query) là truy vấn được xây dựng tùy theo dữ liệu đầu vào khi chương trình chạy, thay vì luôn có một cấu trúc cố định.

**Truy vấn cố định:** Dù movieId nhận giá trị 1, 2 hay 3, truy vấn vẫn có cùng cấu trúc: luôn lọc theo phim.
```sql
SELECT *
FROM showtimes
WHERE movie_id = :movieId;
```

**Truy vấn động:**
Nếu người dùng chỉ chọn phim:
```sql
SELECT *
FROM showtimes
WHERE movie_id = :movieId;
```

Nếu chọn thêm phòng:

```sql
SELECT *
FROM showtimes
WHERE movie_id = :movieId
  AND room_id = :roomId;`
```
Ở đây, số lượng và nội dung điều kiện thay đổi theo đầu vào.

**Specification** cho phép tạo các truy vấn động dựa trên một tập hợp các điều kiện bằng cách sử dụng ``interface Specification``.

**Triển khai**

**1. Mở rộng Repository:** Repository phải mở rộng interface  **JpaSpecificationExecutor** để kích hoạt chức năng này:
```java
public interface CinemaRepository extends JpaRepository<Cinema, Long>, JpaSpecificationExecutor<Cinema> {
}
```

**2. Định nghĩa các Specification:** Định nghĩa các phương thức tĩnh trả về ``Specification<Cinema>`` cho mỗi điều kiện lọc. Chúng ta sử dụng CriteriaBuilder để xây dựng các điều kiện lọc (Predicates).
```java
public class CinemaSpecification {

    public static Specification<Cinema> buildCinemaSpecification(CinemaFilterRequest filterRequest)
    {
        return hasName(filterRequest.getName()).and(hasAddress(filterRequest.getAddress()));
    }

    public static Specification<Cinema> hasName(String name) {
        return (root, query, criteriaBuilder) ->
        {
            if(name == null || name.isBlank())
                return null;
            else
                return criteriaBuilder.like(criteriaBuilder.lower(root.get("name")),
                        SpecificationUtils.toPattern(name), '!');
        };
    }

    public static Specification<Cinema> hasAddress(String address)
    {
        return (root, query, criteriaBuilder) ->
        {
            if(address == null || address.isBlank())
                return null;
            else
                return criteriaBuilder.like(criteriaBuilder.lower(root.get("address")),
                        SpecificationUtils.toPattern(address), '!');
        };
    }
}
```
```java
public class SpecificationUtils {
    public static String toPattern(String value)
    {
        String escaped = value.trim().toLowerCase()
                .replace("!", "!!")
                .replace("%", "!%")
                .replace("_", "!_");
        return "%" + escaped + "%";
    }
}
```
- ``root:`` entity gốc đang select, ở đây là Cinema.
- ``query:`` Criteria query hiện tại; có thể dùng cho distinct/subquery nếu cần.
- ``cb:`` CriteriaBuilder, dùng để tạo biểu thức.
### 1.7. Phòng tránh N + 1 query
Vấn đề N+1 (N+1 problem) là một vấn đề về hiệu năng trong Object Relational Mapping (ORM), nó kích hoạt nhiều câu lệnh Select Query (chính xác là N+1, với N = số lượng bản ghi trong bảng).
Ví dụ:
```java
@Entity
@Table(name = "seats",
    uniqueConstraints = @UniqueConstraint(
            name = "uk_seats_position_room",
            columnNames = {"position", "room_id"}
    ))
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Seat {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;

    @Column(name = "position", nullable = false)
    private String position;

    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "room_id", nullable = false)
    private ScreeningRoom room;

    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "seat_type_id", nullable = false)
    private SeatType seatType;
}
```
```java
public interface SeatRepository extends JpaRepository<Seat, Long> {
    
    public List<Seat> findAllByRoomIdOrderByPositionAsc(Long roomId);
```
```java
public List<SeatResponse> getSeats(Long showtimeId)
    {
        Showtime showtime = showtimeRepository.findById(showtimeId)
                .orElseThrow(() -> new ApiException(ErrorCode.SHOWTIME_NOT_FOUND));

        Set<Long> bookedSeatIds = ticketRepository.findBookedSeats(showtimeId);



        List<Seat> seats = seatRepository.findAllByRoomIdOrderByPositionAsc(showtime.getRoom().getId());

        List<SeatResponse> seatResponses = new ArrayList<>();

        for(Seat seat : seats)
        {
            SeatStatus status = bookedSeatIds.contains(seat.getId())
                    ? SeatStatus.BOOKED
                    : SeatStatus.AVAILABLE;
            BigDecimal price = seat.getSeatType().getSurcharge().add(showtime.getBasePrice());

            SeatResponse seatResponse = seatMapper.toSeatResponse(seat);
            seatResponse.setPrice(price);
            seatResponse.setStatus(status);

            seatResponses.add(seatResponse);
        }

        return seatResponses;

    }
```
```java
@Mapper(componentModel = "spring")
public interface SeatMapper {
    @Mapping(target = "seatTypeName", source = "seatType.name")
    public SeatResponse toSeatResponse(Seat seat);
}
```
``seatType`` trong entity ``Seat`` đang để lazy, khi repo lấy tất cả các ``Seat`` từ DB lên thì dữ liệu cho ``seatType`` chưa hề có và chỉ tồn tại dạng proxy, khi thực hiện việc mapping vào ``SeatResponse``, vì SeatResponse đang cần lấy seatTypeName nên nó phải thực hiện lệnh query select để lấy, khi đó duyệt qua các ghế thì nó sẽ select 1 lần tổng cộng N lần thêm 1 lần ở select gốc nữa đây là tình trạng N + 1

**Làm thế nào để nhận diện nó?**
- Bật SQL logging trong application.yml
```yml
spring:
  jpa:
    show-sql: true
```

**Giải quyết N+1**
- Fetch join
- EntityGraph
## 2. Tìm hiểu về Validation
### 2.1 Vì Sao Cần Validate Data Trong Spring Boot?
- Validate dữ liệu là bước quan trọng bắt buộc khi xây dựng API. Dù frontend đã validate trước đó, backend vẫn cần kiểm tra lại bởi:
- Backend là nơi làm việc trực tiếp với database. Nếu để dữ liệu không hợp lệ đi vào hệ thống, sẽ dẫn đến nhiều vấn đề nghiêm trọng.
- Tránh nguy cơ SQL Injection hoặc hacker chèn dữ liệu độc hại.
- Giúp hệ thống ổn định và an toàn hơn.
### 2.2 Thêm Dependency Validation
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```
### 2.3 Các Annotation Validation Thường Dùng
- `@NotNull` → không cho phép giá trị null.
- `@NotEmpty` → danh sách/collection không được trống.
- `@NotBlank` → chuỗi không được rỗng ("").
- `@Min`,`@Max` → giới hạn giá trị số.
- `@Pattern` → validate bằng regex.
- `@Email` → validate định dạng email.
### 2.4 Validate Request Body (Payload)
- Tạo CreateOrderRequest sử dụng các annotation để validate giá trị dữ liệu được truyền vào
- Sử dụng annotation ``@Valid`` để chỉ định kiểm duyệt giá trị Payload
```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class CreateOrderRequest {
    @NotNull
    @Positive
    private Long showtimeId;

    @NotEmpty
    private Set<@NotNull @Positive Long> seatIds;
}
```
```java
@RestController
@RequiredArgsConstructor
@RequestMapping("/api/v1/orders")
public class OrderController {
    private final OrderService orderService;
    @PostMapping()
    public ResponseEntity<ApiResponse<InvoiceResponse>> bookTickets(@Valid @RequestBody CreateOrderRequest request)
    {
        return ResponseEntity.status(HttpStatus.OK)
                .body(ApiResponse.success(orderService.createOrder(request)));
    }
}
```
### 2.5. Validate PathVariable và RequestParam
```java
@Validated
@RestController
@RequestMapping("/user")
public class UserController {

	@GetMapping("/detail/{userId}")
	public String getUserDetail(@PathVariable @Min(value = 1, message = "userId must be greater than 0") int userId) {
    	return "Hello";
	}
}
```
```java
@GetMapping("/list")
public List getAllUser(
        @RequestParam(defaultValue = "0") int pageNo, 
        @Min(10) @RequestParam(defaultValue = "20") int pageSize) {
        return ...;
    }
}
eê
```
## 3. Tìm hiểu về Exception handling
Exception handling là xử lý ngoại lệ xảy ra trong quá trình thực thi chương trình. Với REST API, việc này bao gồm chuyển ngoại lệ thành HTTP status và response body phù hợp, đồng thời ghi log để phục vụ kiểm tra lỗi.

Ví dụ khi bán vé:
- Không tìm thấy suất chiếu → trả 404 Not Found.
- Ghế đã được bán → trả 409 Conflict.
- Request thiếu mã suất chiếu → trả 400 Bad Request.
- Lỗi không dự kiến trong hệ thống → trả 500 Internal Server Error.

Nếu không xử lý tập trung, mỗi controller có thể tự trả một kiểu lỗi khác nhau, làm frontend khó xử lý và khiến code bị lặp.

Thay vì viết ``try-catch`` lặp lại trong từng ``controller``, ta gom xử lý lỗi tại một chỗ.

**Các annotation chính**
``@ExceptionHandler:`` Đánh dấu method xử lý một hoặc nhiều loại exception
``@ExceptionHandler:`` Áp dụng các xử lý dùng chung cho controller; có thể giới hạn phạm vi
``@RestControllerAdvice:`` Kết hợp @ControllerAdvice và @ResponseBody; phù hợp trả JSON cho REST API
``@ResponseStatus:`` Khai báo HTTP status cho method hoặc class exception

**Ví dụ:**
```java
public InvoiceResponse getInvoice(Long invoiceId)
    {
        Invoice invoice = invoiceRepository.findById(invoiceId)
                .orElseThrow(() -> new ApiException(ErrorCode.INVOICE_NOT_FOUND));

        List<Ticket> tickets = ticketRepository.findAllByInvoiceId(invoiceId);

        Showtime showtime = tickets.get(0).getShowtime();

        return invoiceMapper.toInvoiceResponse(
                invoice,
                showtime,
                ticketMapper.toListTicketResponse(tickets)
        );

    }
```
```java
@RestControllerAdvice
@Slf4j
public class GlobalExceptionHandler {
    @ExceptionHandler(ApiException.class)
    public ResponseEntity<ApiResponse<Void>> handleApiException(ApiException e) {
        log.error("ApiException exception: ", e);
        return ResponseEntity.status(e.getErrorCode().getStatus())
                .body(ApiResponse.error(
                        e.getMessage(), ErrorDetail.of(e.getErrorCode().name())));
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ApiResponse<Void>> handleValidation(MethodArgumentNotValidException e) {
        log.error("MethodArgumentNotValidException exception: ", e);
        List<ErrorDetail.FieldError> fieldErrors = new ArrayList<>();

        for (FieldError fieldError : e.getBindingResult().getFieldErrors()) {
            fieldErrors.add(toFieldError(fieldError));
        }

        ErrorDetail error = ErrorDetail.of(ErrorCode.VALIDATION_ERROR.name(), fieldErrors);
        return ResponseEntity.status(ErrorCode.VALIDATION_ERROR.getStatus())
                .body(ApiResponse.error(ErrorCode.VALIDATION_ERROR.getDefaultMessage(), error));
    }

    @ExceptionHandler(value = RuntimeException.class)
    public ResponseEntity<ApiResponse<Void>> handleRuntimeException(RuntimeException e) {
        log.error("Uncategorized exception: ", e);

        return ResponseEntity.status(ErrorCode.UNCATEGORIZED_EXCEPTION.getStatus())
                .body(ApiResponse.error(
                        ErrorCode.UNCATEGORIZED_EXCEPTION.getDefaultMessage(),
                        ErrorDetail.of(ErrorCode.UNCATEGORIZED_EXCEPTION.name())));
    }

    private ErrorDetail.FieldError toFieldError(FieldError fe) {
        return new ErrorDetail.FieldError(fe.getField(), fe.getDefaultMessage());
    }
}
```
## 4. Override default fetch plan với join fetch / entity graph
Override default fetch plan nghĩa là thay đổi cách nạp cho lần truy vấn cụ thể, không sửa mapping chung của mọi truy vấn.
### 4.1 JOIN và JOIN FETCH khác nhau thế nào?
``JOIN`` liên kết dữ liệu phục vụ lọc hoặc truy vấn:
```java
@Query("""
    select s
    from Showtime s
    join s.movie m
    where m.title like :title
    """)
List<Showtime> findByMovieTitle(@Param("title") String title);
```
Query trên chọn ``s``. Việc ``join`` ``movie`` để lọc không đảm bảo ``s.movie`` được khởi tạo sẵn.
``JOIN FETCH`` yêu cầu nạp quan hệ cùng entity được chọn:
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
Trong trường hợp này, Hibernate lấy suất chiếu, phim và phòng bằng query có join. Truy cập ``s.getMovie().getTitle()`` và ``s.getRoom().getName()`` không cần lazy select riêng cho hai quan hệ đã fetch.
### 4.2 EntityGraph là gì?
EntityGraph là cơ chế của JPA để mô tả các thuộc tính/quan hệ cần được nạp trong một lần truy xuất. Spring Data JPA cung cấp annotation `@EntityGraph` trên method repository.
```java
@EntityGraph(attributePaths = {"movie", "room"})
    @Query("""
           select s
           from Showtime s
           where s.movie.id = :movieId and
                 s.room.id = :roomId and
                 s.startTime >= :from and
                 s.startTime < :to and
                 s.startTime > :currentTime           
           """)
    public Page<Showtime> getAvailableShowtimes(
            @Param("movieId") Long movieId,
            @Param("roomId") Long roomId,
            @Param("from") Instant from,
            @Param("to") Instant to,
            @Param("currentTime") Instant currentTime,
            Pageable pageable);
```
**Lưu ý:** Graph trên nạp movie và room; không đảm bảo mọi quan hệ sâu hơn đều được nạp. Nếu DTO truy cập `room.cinema.name`, cần cân nhắc thêm đường dẫn tương ứng:
khi đó ta phải: `@EntityGraph(attributePaths = {"movie", "room", "room.cinema"})`