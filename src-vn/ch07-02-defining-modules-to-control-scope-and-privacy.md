## Định nghĩa các mô-đun để Kiểm soát Phạm vi và Quyền riêng tư

Trong phần này, chúng ta sẽ nói về các mô-đun và các phần khác của hệ thống
mô-đun, cụ thể là *đường dẫn* cho phép bạn đặt tên cho các mục; từ khóa `use`
đưa đường dẫn vào phạm vi; và từ khóa `pub` để công khai các mục. Chúng ta cũng
sẽ thảo luận về từ khóa `as`, các gói bên ngoài và toán tử toàn cục.

Đầu tiên, chúng ta sẽ bắt đầu với một danh sách các quy tắc để dễ dàng tham khảo
khi bạn sắp xếp mã của mình trong tương lai. Sau đó, chúng ta sẽ giải thích chi
tiết từng quy tắc.

### Mô-đun cheat sheet

Ở đây chúng tôi cung cấp tài liệu tham khảo nhanh về cách thức mô-đun, đường
dẫn, từ khóa `use` và từ khóa `pub` hoạt động trong trình biên dịch, và cách hầu
hết các lập trình viên tổ chức mã của họ. Chúng ta sẽ xem qua các ví dụ về từng
quy tắc này trong suốt chương này, nhưng đây là một nơi tuyệt vời để tham khảo
như một lời nhắc nhở về cách thức hoạt động của các mô-đun.

- **Bắt đầu từ crate gốc**: Khi biên dịch một crate, trước tiên trình biên dịch
    sẽ tìm trong tệp crate gốc (thường là *src/lib.rs* đối với crate thư viện
    hoặc *src/main.rs* đối với crate nhị phân ) để biên dịch mã.

- **Khai báo mô-đun**: Trong tệp gốc của crate, bạn có thể khai báo các mô-đun
    mới; giả sử, bạn khai báo một mô-đun "garden" với `mod garden;`. Trình biên
    dịch sẽ tìm mã của mô-đun ở những nơi sau đây:
    - Nội tuyến, trong dấu ngoặc nhọn thay thế dấu chấm phẩy sau `mod garden`
    - Trong tệp *src/garden.rs*
    - Trong tệp *src/garden/mod.rs*

- **Khai báo các mô-đun con**: Trong bất kỳ tệp nào khác ngoài thư mục crate
    gốc, bạn có thể khai báo các mô-đun con. Ví dụ: bạn có thể khai báo `mod
    vegetables` trong *src/garden.rs*. Trình biên dịch sẽ tìm mã của mô-đun con
    trong thư mục được đặt tên cho mô-đun mẹ ở những nơi sau:
    - Nội tuyến, ngay sau `mod vegetables`, trong dấu ngoặc nhọn thay vì dấu chấm phẩy
    - Trong tệp *src/garden/vegetables.rs*
    - Trong file *src/garden/vegetables/mod.rs*

- **Đường dẫn đến mã trong mô-đun**: Sau khi mô-đun là một phần trong crate của
    bạn, bạn có thể sử dụng mã trong mô-đun đó từ bất kỳ nơi nào khác trong cùng
    crate đó bằng cách sử dụng đường dẫn đến mã, miễn là các quy tắc về quyền
    riêng tư cho phép. Ví dụ: loại `Asparagus` trong mô-đun "garden vegetables" sẽ được tìm thấy tại `crate::garden::vegetables::Asparagus`.

- **Riêng tư so với Công khai**: Theo mặc định, mã trong một mô-đun là riêng tư
    với các mô-đun chính của nó. Để công khai một mô-đun, hãy khai báo mô-đun
    bằng `pub mod` thay vì `mod`. Để công khai các mục trong một mô-đun công
    khai, hãy sử dụng `pub` trước các khai báo của chúng.
    
- **Từ khóa `use`**: Trong một phạm vi, từ khóa `use` tạo lối tắt cho các mục để
    giảm sự lặp lại của các đường dẫn dài. Trong bất kỳ phạm vi nào có thể tham
    chiếu đến `crate::garden::vegetables::Asparagus`, bạn có thể tạo lối tắt với
    `use crate::garden::vegetables::Asparagus;` và từ đó trở đi, bạn chỉ cần
    viết `Asparagus` để sử dụng nó trong trong phạm vi mã.

Ở đây chúng tôi tạo một crate nhị phân có tên `backyard` để minh họa các quy tắc
này. Thư mục của crate, cũng có tên là `backyard`, chứa các tệp và thư mục sau:

```text
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

Tệp gốc crate trong trường hợp này là *src/main.rs* và chứa:

<span class="filename">Filename: src/main.rs</span>

```rust,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/main.rs}}
```

Dòng `pub mod garden;` yêu cầu trình biên dịch gộp mã mà nó tìm thấy trong
*src/garden.rs*, đó là:

<span class="filename">Filename: src/garden.rs</span>

```rust,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/garden.rs}}
```

Ở đây, `pub mod vegetables;` có nghĩa là mã trong *src/garden/vegetables.rs*
cũng được bao gồm. Mã đó là:

```rust,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/garden/vegetables.rs}}
```

Bây giờ, hãy đi vào chi tiết của các quy tắc này và chứng minh chúng bằng hành
động!

### Tạo Nhóm các mã liên quan trong các mô-đun

*Mô-đun* cho phép chúng ta tổ chức mã trong một crate để dễ đọc và tái sử dụng
dễ dàng. Các mô-đun cũng cho phép chúng ta kiểm soát *quyền riêng tư* của các
mục, vì mã trong một mô-đun là riêng tư theo mặc định. Các mục riêng là chi tiết
triển khai nội bộ không có sẵn để bên ngoài sử dụng. Chúng ta có thể chọn đặt
các mô-đun và các mục bên trong chúng ở chế độ công khai, điều này sẽ hiển thị
chúng để cho phép mã bên ngoài sử dụng và phụ thuộc vào chúng.

Ví dụ: hãy viết một crate thư viện cung cấp chức năng của một nhà hàng. Chúng
tôi sẽ định nghĩa các khai báo của các hàm nhưng để trống phần thân của chúng để
tập trung vào việc tổ chức mã, thay vì triển khai một nhà hàng.

Trong ngành nhà hàng, một số bộ phận của nhà hàng được gọi là *tiền sảnh* và
những bộ phận khác được gọi là *sân sau*. Trước nhà là nơi có khách hàng; điều
này bao gồm nơi người tổ chức sắp xếp chỗ ngồi cho khách hàng, người phục vụ
nhận đơn đặt hàng và thanh toán, và nhân viên pha chế đồ uống. Phía sau, là nơi
các đầu bếp và phụ bếp làm việc trong bếp, người rửa bát dọn dẹp và người quản
lý làm công việc hành chính.

Để cấu trúc crate của chúng ta theo cách này, chúng ta có thể tổ chức các chức
năng của nó thành các mô-đun lồng nhau. Tạo một thư viện mới có tên `restaurant`
bằng cách chạy `cargo new restaurant --lib`; sau đó nhập mã trong Liệt kê 7-1
vào *src/lib.rs* để xác định một số mô-đun và chữ ký hàm. Đây là *tiền sảnh* của
nhà hàng:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-01/src/lib.rs}}
```

<span class="caption">Liệt kê 7-1: Mô-đun `front_of_house` chứa các mô-đun khác mà sau đó chứa các hàm</span>

Chúng tôi xác định mô-đun bằng từ khóa `mod` theo sau là tên của mô-đun (trong
trường hợp này là `front_of_house`). Sau đó, phần thân của mô-đun sẽ nằm trong
dấu ngoặc nhọn. Bên trong các mô-đun, chúng ta có thể đặt các mô-đun khác, như
trong trường hợp này với các mô-đun `hosting` và `serving`. Các mô-đun cũng có
thể chứa các khai báo cho các mục khác, chẳng hạn như cấu trúc, enum, hằng số
và traits, như trong Liệt kê 7-1— các hàm.

Bằng cách sử dụng các mô-đun, chúng ta có thể nhóm các định nghĩa liên quan lại
với nhau và đặt tên cho lý do tại sao chúng lại liên quan. Các lập trình viên sử
dụng mã này có thể điều hướng mã dựa trên các nhóm thay vì phải đọc qua tất cả
các định nghĩa, giúp dễ dàng tìm thấy các định nghĩa liên quan đến chúng. Các
lập trình viên thêm chức năng mới vào mã này sẽ biết nơi đặt mã để giữ cho
chương trình có tổ chức.

Trước đó, chúng tôi đã đề cập rằng *src/main.rs* và *src/lib.rs* được gọi là
crate gốc. Lý do cho tên của chúng là nội dung của một trong hai tệp này tạo
thành một mô-đun có tên `crate` ở gốc cấu trúc mô-đun của crate, được gọi là
*cây mô-đun*.

Liệt kê 7-2 hiển thị cây mô-đun cho cấu trúc trong Liệt kê 7-1.

```text
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

<span class="caption">Danh sách 7-2: Cây mô-đun cho mã trong Liệt kê 7-1</span>

Cây này cho thấy cách một số mô-đun lồng vào nhau; ví dụ: `hosting` lồng bên
trong `front_of_house`. Cây cũng chỉ ra rằng một số mô-đun là *anh chị em* với
nhau, nghĩa là chúng được xác định trong cùng một mô-đun; `hosting` và `serving`
là anh em được xác định trong `front_of_house`. Nếu mô-đun A được chứa bên trong
mô-đun B, chúng ta nói rằng mô-đun A là *con* của mô-đun B và mô-đun B đó là
*cha* của mô-đun A. Lưu ý rằng toàn bộ cây mô-đun được bắt nguồn từ mô-đun ẩn có
tên `crate `.

Cây mô-đun có thể nhắc bạn về cây thư mục của hệ thống tập tin trên máy tính của
bạn; đây là một so sánh rất thích hợp! Giống như các thư mục trong một hệ thống
tệp, bạn sử dụng các mô-đun để sắp xếp mã của mình. Và giống như các tệp trong
một thư mục, chúng tôi cần một cách để tìm các mô-đun của mình.