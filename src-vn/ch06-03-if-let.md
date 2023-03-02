## Điều khiển Luồng ngắn gọn với `if let`

Cú pháp `if let` cho phép bạn kết hợp `if` và `let` thành một cách ít dài dòng
hơn để xử lý các giá trị khớp với một mẫu trong khi bỏ qua phần còn lại. Hãy xem
xét chương trình trong Liệt kê 6-6 khớp với một giá trị `Option<u8>` trong biến
`config_max` nhưng chỉ muốn thực thi mã nếu giá trị đó là biến thể `Some`.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-06/src/main.rs:here}}
```

<span class="caption">Liệt kê 6-6: Một `match` chỉ quan tâm đến việc thực thi mã khi giá trị là `Some`</span>

Nếu giá trị là `Some`, thì chúng ta in ra giá trị trong biến thể `Some` bằng
cách liên kết giá trị với biến `max` trong mẫu. Chúng ta không muốn làm bất cứ
điều gì với giá trị `None`. Để đáp ứng biểu thức `match`, chúng ta phải thêm `_
=> ()` sau khi xử lý chỉ một biến thể, đây là mã soạn sẵn (boilerplate) gây khó
chịu khi thêm.

Thay vào đó, chúng ta có thể viết điều này theo cách ngắn hơn bằng cách sử dụng
`if let`. Đoạn mã sau hoạt động giống như `match` trong Liệt kê 6-6:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-12-if-let/src/main.rs:here}}
```

Cú pháp `if let` nhận một mẫu và một biểu thức được phân tách bằng dấu bằng. Nó
hoạt động theo cách giống như một `match`, trong đó biểu thức được cung cấp cho
`match` và mẫu là nhánh đầu tiên của nó. Trong trường hợp này, mẫu là
`Some(max)` và `max` liên kết với giá trị bên trong `Some`. Sau đó, chúng ta có
thể sử dụng `max` trong phần thân của khối `if let` giống như cách chúng ta đã
sử dụng `max` trong nhánh `match` tương ứng. Mã trong khối `if let` không chạy
nếu giá trị không khớp với mẫu.

Sử dụng `if let` có nghĩa là gõ phím ít hơn, ít thụt lề hơn và ít mã soạn sẵn
hơn. Tuy nhiên, bạn mất đi sự kiểm tra toàn diện mà `match` thực thi. Việc chọn
giữa `match` và `if let` tùy thuộc vào những gì bạn đang làm trong tình huống cụ
thể của mình và liệu đạt được sự ngắn gọn có phải là sự đánh đổi thích hợp để
mất đi việc kiểm tra toàn diện hay không.

Nói cách khác, bạn có thể coi `if let` là cú pháp ngọt ngào cho một `match` chạy
mã khi giá trị khớp với một mẫu và sau đó bỏ qua tất cả các giá trị khác.

Chúng ta có thể thêm `else` với `if let`. Khối mã đi kèm với `else` giống như
khối mã đi kèm với trường hợp `_` trong biểu thức `match` tương đương với `if
let` và `else`. Nhớ lại định nghĩa enum `Coin` trong Liệt kê 6-4, trong đó biến
thể `Quarter` cũng có giá trị `UsState`. Nếu chúng ta muốn đếm tất cả các đồng
xu không phải là 25 xu mà chúng ta thấy đồng thời thông báo *bang* của các đồng
25 xu, chúng ta có thể làm điều đó với biểu thức `match`, như sau:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-13-count-and-announce-match/src/main.rs:here}}
```

Hoặc chúng ta có thể sử dụng biểu thức `if let` và `else`, như sau:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-14-count-and-announce-if-let-else/src/main.rs:here}}
```

Nếu bạn gặp tình huống trong đó chương trình của bạn có logic quá dài dòng để
diễn đạt bằng `match`, hãy nhớ rằng `if let` cũng có trong bộ công cụ Rust của
bạn.

## Tổng kết chương

Bây giờ chúng ta đã biết cách sử dụng enums để tạo các kiểu dữ liệu tùy chỉnh có
thể là một trong các giá trị được liệt kê. Chúng ta đã chỉ ra cách mà kiểu
`Option<T>` của thư viện chuẩn giúp bạn sử dụng kiểu dữ liệu hệ thống để tránh
lỗi. Khi các giá trị enum có dữ liệu bên trong, bạn có thể sử dụng `match` hoặc
`if let` để trích xuất và sử dụng các giá trị đó, tùy thuộc vào số lượng trường
hợp bạn cần xử lý.

Các chương trình Rust của bạn hiện có thể diễn đạt các khái niệm trong miền dữ
liệu của bạn bằng cách sử dụng các cấu trúc và enum. Tạo các kiểu dữ liệu tùy
chỉnh để sử dụng trong API của bạn đảm bảo an toàn cho từng kiểu dữ liệu: trình
biên dịch sẽ đảm bảo rằng các hàm của bạn chỉ nhận được các giá trị thuộc đúng
kiểu mà mỗi hàm mong đợi.

Để cung cấp cho người dùng của bạn một API được tổ chức tốt, dễ sử dụng và chỉ
hiển thị chính xác những gì người dùng của bạn sẽ cần, bây giờ hãy chuyển sang
các mô-đun của Rust.