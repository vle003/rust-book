## Tách các mô-đun thành các tệp khác nhau

Cho đến nay, tất cả các ví dụ trong chương này đã khai báo nhiều mô-đun trong
một tệp. Khi các mô-đun trở nên lớn hơn, bạn có thể muốn di chuyển các khai báo
của chúng sang một tệp riêng để giúp mã dễ theo dõi hơn.

Ví dụ: hãy bắt đầu từ mã trong Liệt kê 7-17 có nhiều mô-đun nhà hàng. Chúng ta
sẽ trích xuất các mô-đun thành các tệp thay vì có tất cả các mô-đun được khai
báo trong tệp mã nguồn của crate gốc. Trong trường hợp này, tệp crate gốc là
*src/lib.rs*, nhưng quy trình này cũng hoạt động với các crate nhị phân có tệp
crate gốc là *src/main.rs*.

Trước tiên, chúng tôi sẽ tách mô-đun `front_of_house` vào một tệp riêng của nó.
Xóa mã bên trong dấu ngoặc nhọn cho mô-đun `front_of_house`, chỉ để lại phần
khai báo `mod front_of_house;`, sao cho *src/lib.rs* chứa mã được hiển thị trong
Liệt kê 7-21. Lưu ý rằng điều này sẽ khiến chương trình không biên dịch được cho
đến khi chúng ta tạo tệp *src/front_of_house.rs* như Liệt kê 7-22.

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/lib.rs}}
```

<span class="caption">Liệt kê 7-21: Khai báo mô-đun `front_of_house` có phần thân sẽ nằm trong *src/front_of_house.rs*</span>

Tiếp theo, đặt mã nằm trong dấu ngoặc nhọn vào một tệp mới có tên
*src/front_of_house.rs*, như trong Liệt kê 7-22. Trình biên dịch biết cách tìm
trong tệp này vì nó thấy trong phần khai báo mô-đun của crate gốc có tên
`front_of_house`.

<span class="filename">Filename: src/front_of_house.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-21-and-22/src/front_of_house.rs}}
```

<span class="caption">Liệt kê 7-22: Các định nghĩa bên trong mô-đun `front_of_house` trong *src/front_of_house.rs*</span>

Lưu ý rằng bạn chỉ cần tải tệp bằng cách sử dụng khai báo `mod` *một lần* trong
cây mô-đun của mình. Sau khi trình biên dịch biết tệp là một phần của dự án (và
biết mã nằm ở đâu trong cây mô-đun do bạn đã đặt câu lệnh `mod` ở đâu), các tệp
khác trong dự án của bạn sẽ tham chiếu đến mã của tệp đã tải bằng cách sử dụng
đường dẫn đến nơi nó đã được khai báo, như được đề cập trong phần [“Đường dẫn
Tham chiếu đến một Mục trong Cây Mô-đun”][đường dẫn]<!-- ignore -->. Nói cách
khác, `mod` *không phải* là thao tác “bao gồm” mà bạn có thể đã thấy trong các
ngôn ngữ lập trình khác.

Tiếp theo, chúng ta sẽ tách mô-đun `hosting` vào tệp riêng của nó. Quá trình này
hơi khác một chút vì `hosting` là mô-đun con của `front_of_house`, không phải
của mô-đun gốc. Chúng ta sẽ đặt tệp cho `hosting` trong một thư mục mới sẽ được
đặt tên theo mức trên của nó trong cây mô-đun, trong trường hợp này là
*src/front_of_house/*.

Để bắt đầu di chuyển `hosting`, chúng ta thay đổi *src/front_of_house.rs* để chỉ
chứa phần khai báo của mô-đun `hosting`:

<span class="filename">Filename: src/front_of_house.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house.rs}}
```

Sau đó, chúng tôi tạo một thư mục *src/front_of_house* và một tệp *hosting.rs*
để chứa các khai báo được tạo trong mô-đun `hosting`:

<span class="filename">Filename: src/front_of_house/hosting.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-02-extracting-hosting/src/front_of_house/hosting.rs}}
```

Thay vào đó, nếu chúng ta đặt *hosting.rs* trong thư mục *src*, thì trình biên
dịch sẽ yêu cầu mã *hosting.rs* nằm trong mô-đun `hosting` được khai báo trong
thư mục gốc và không được khai báo là con của mô-đun `front_of_house`. Các quy
tắc của trình biên dịch đối với các tệp cần kiểm tra mã của mô-đun nào có nghĩa
là các thư mục và tệp khớp với cây mô-đun.

> ### Đường dẫn Tệp thay thế
>
> Cho đến nay, chúng tôi đã đề cập đến các kiểu đường dẫn tệp tự nhiên nhất mà
> trình biên dịch Rust sử dụng, nhưng Rust cũng hỗ trợ kiểu đường dẫn tệp cũ
> hơn. Đối với một mô-đun có tên `front_of_house` được khai báo trong thư mục
> của crate gốc, trình biên dịch sẽ tìm mã của mô-đun trong:
>
> * *src/front_of_house.rs* (chúng ta đã nói về kiểu này)
> * *src/front_of_house/mod.rs* (kiểu cũ, vẫn được hỗ trợ)
>
> Đối với mô-đun có tên `hosting` là mô-đun con của `front_of_house`,
> trình biên dịch sẽ tìm mã của mô-đun trong:
>
> * *src/front_of_house/hosting.rs* (chúng ta đã nói về kiểu này)
> * *src/front_of_house/hosting/mod.rs* (kiểu cũ, vẫn được hỗ trợ)
>
> Nếu bạn sử dụng cả hai kiểu cho cùng một mô-đun, bạn sẽ gặp lỗi khi biên dịch.
> Ta được phép sử dụng kết hợp cả hai kiểu cho các mô-đun khác nhau trong cùng
> một dự án, nhưng có thể gây nhầm lẫn cho những người xem xét dự án của bạn.
> 
> Nhược điểm chính của phong cách sử dụng các tệp có tên *mod.rs* (kiểu củ) là
> dự án của bạn có thể kết thúc với nhiều tệp có tên *mod.rs*, điều này có thể
> gây nhầm lẫn khi bạn mở chúng trong chương trình soạn thảo của mình cùng một
> lúc.

Chúng ta đã chuyển mã của từng mô-đun sang từng tệp riêng biệt và cây mô-đun vẫn
giữ nguyên. Việc gọi Hàm trong `eat_at_restaurant`, mặc dù các định nghĩa nằm
trong các tệp khác nhau, vẫn hoạt động mà không cần bất kỳ sửa đổi nào. Kỹ thuật
này cho phép bạn di chuyển các mô-đun sang các tệp mới khi chúng tăng kích
thước.

Lưu ý rằng câu lệnh `pub use crate::front_of_house::hosting` trong *src/lib.rs*
cũng không thay đổi, `use` cũng không có bất kỳ tác động nào đối với các tệp
được biên dịch như một phần của crate. Từ khóa `mod` khai báo các mô-đun và Rust
tìm trong một tệp có cùng tên với mô-đun để tìm mã đi vào mô-đun đó.

## Tổng kết chương

Rust cho phép bạn chia một gói thành nhiều crate và một crate thành các mô-đun
để bạn có thể dùng các mục được khai báo trong một mô-đun từ một mô-đun khác.
Bạn có thể làm điều này bằng cách chỉ định đường dẫn tuyệt đối hoặc tương đối.
Các đường dẫn này có thể được đưa vào phạm vi lệnh bằng câu lệnh `use` để bạn có
thể sử dụng đường dẫn ngắn hơn cho nhiều mục đích sử dụng trong khối đó. Mã
mô-đun là riêng tư theo mặc định, nhưng bạn có thể công khai các định nghĩa bằng
cách thêm từ khóa `pub`.

Trong chương tiếp theo, chúng ta sẽ xem xét một số cấu trúc dữ liệu dạng
collection (bộ sưu tập) trong thư viện chuẩn mà bạn có thể sử dụng trong mã
nguồn đã sắp xếp gọn gàng của mình.

[paths]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
