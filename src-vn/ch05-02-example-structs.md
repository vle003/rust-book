## Ví dụ một Chương trình sử dụng Cấu trúc

Để hiểu khi nào chúng ta có thể muốn sử dụng các cấu trúc, hãy viết một chương
trình tính diện tích hình chữ nhật. Chúng ta sẽ bắt đầu bằng cách sử dụng các
biến đơn lẻ, sau đó cấu trúc lại chương trình cho đến khi chúng ta sử dụng các
cấu trúc thay thế.

Hãy tạo một dự án nhị phân mới với Cargo đặt tên là *rectangles*, nó sẽ lấy
chiều rộng và chiều cao của hình chữ nhật được chỉ định bằng điểm ảnh và tính
diện tích của hình chữ nhật. Liệt kê 5-8 cho thấy một chương trình ngắn với một
cách thực hiện chính xác điều đó trong tệp *src/main.rs* của dự án của chúng ta.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-08/src/main.rs:all}}
```

<span class="caption">Liệt kê 5-8: Tính diện tích hình chữ nhật được chỉ định bởi các biến chiều rộng và chiều cao riêng biệt</span>

Bây giờ, hãy chạy chương trình này bằng lệnh `cargo run`:

```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-08/output.txt}}
```
Mã này thành công trong việc tìm ra diện tích của hình chữ nhật bằng cách gọi
hàm `area` với mỗi kích thước, nhưng chúng ta có thể làm nhiều hơn để làm cho mã
này rõ ràng và dễ đọc.

Vấn đề với mã này là hiển nhiên trong khai báo của hàm `area`:

```rust,ignore
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-08/src/main.rs:here}}
```

Hàm `area` được cho là để tính diện tích của một hình chữ nhật, nhưng hàm chúng
ta đã viết trong chương trình có hai tham số và không rõ ràng ở bất kỳ đâu rằng
các tham số có liên quan với nhau. Nó sẽ dễ đọc hơn và dễ quản lý hơn khi nhóm
chiều rộng và chiều cao lại với nhau. Chúng ta đã thảo luận về một cách chúng ta
có thể làm điều đó trong phần [“Loại Tuple”][the-tuple-type]<!-- ignore --> của
Chương 3: bằng cách sử dụng các bộ.

### Tái cấu trúc với Tuples

Liệt kê 5-9 cho thấy một phiên bản khác của chương trình sử dụng các bộ dữ liệu.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-09/src/main.rs}}
```

<span class="caption">Liệt kê 5-9: Chỉ định chiều rộng và chiều cao của hình chữ nhật bằng một bộ</span>

Theo một cách nào đó, chương trình này tốt hơn. Các Bộ dữ liệu (tuples) cho phép
chúng ta thêm một chút cấu trúc và hiện tại chúng ta chỉ chuyển một đối số.
Nhưng theo một cách khác, phiên bản này kém trong sáng hơn: các bộ dữ liệu không
đặt tên cho các phần tử của chúng, vì vậy chúng ta phải lập chỉ mục cho các phần
của bộ dữ liệu, khiến cho việc tính toán của chúng ta trở nên kém rõ ràng.

Trộn lẫn chiều rộng và chiều cao sẽ không thành vấn đề đối với phép tính diện
tích, nhưng nếu chúng ta muốn vẽ hình chữ nhật trên màn hình, điều đó sẽ quan
trọng! Chúng ta phải ghi nhớ rằng `width` có chỉ số bộ là `0` và `height` có chỉ
số bộ là `1`. Điều này thậm chí còn khó hơn đối với người khác để tìm ra và ghi
nhớ nếu họ sử dụng mã của chúng ta. Bởi vì chúng ta chưa truyền đạt ý nghĩa của
dữ liệu trong mã của mình nên giờ đây dễ bị gặp lỗi hơn.

### Tái cấu trúc với cấu trúc: Thêm ý nghĩa

Chúng ta sử dụng các cấu trúc để thêm ý nghĩa bằng cách gắn nhãn dữ liệu. Chúng
ta có thể chuyển đổi bộ dữ liệu mà chúng ta đang sử dụng thành một cấu trúc có
tên cho toàn bộ cũng như tên cho các bộ phận, như trong Liệt kê 5-10.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-10/src/main.rs}}
```

<span class="caption">Liệt kê 5-10: Định nghĩa cấu trúc `Rectangle`</span>

Ở đây chúng ta đã định nghĩa một cấu trúc và đặt tên nó là `Rectangle`. Bên
trong dấu ngoặc nhọn, chúng ta đã xác định các trường là `width` và `height`, cả
hai đều có loại `u32`. Sau đó, trong `main`, chúng ta đã tạo một phiên bản cụ
thể của `Rectangle` có chiều rộng là `30` và chiều cao là `50`.

Hàm `area` của chúng ta hiện được xác định bằng một tham số, mà chúng ta đã đặt
tên là `rectangle`, có kiểu là phần mượn bất biến của một thực thể cấu trúc
`Rectangle`. Như đã đề cập trong Chương 4, chúng ta muốn mượn cấu trúc hơn là sở
hữu nó. Bằng cách này, `main` vẫn giữ quyền sở hữu của nó và có thể tiếp tục sử
dụng `rect1`, đó là lý do chúng ta sử dụng `&` trong lời khai báo hàm và gọi
hàm.

Hàm `area` truy cập các trường `width` và `height` của thực thể `Rectangle` (lưu
ý rằng việc truy cập các trường của một phiên bản cấu trúc mượn không di chuyển
các giá trị trường, đó là lý do tại sao bạn thường thấy các cấu trúc mượn). Khai
báo hàm của chúng ta cho `area` giờ đây thể hiện chính xác ý nghĩa điều chúng ta
muốn: tính diện tích của `Rectangle`, sử dụng các trường `width` và `height` của
nó. Điều này cho thấy rằng chiều rộng và chiều cao có liên quan với nhau và nó
cung cấp các tên mô tả cho các giá trị thay vì sử dụng các giá trị chỉ mục `0`
và `1` của tuple. Đây là một màn ghi điểm cho sự rõ ràng.

### Thêm chức năng hữu ích với các đặc điểm có nguồn gốc

Sẽ rất hữu ích nếu có thể in một thực thể của `Rectangle` trong khi chúng ta
đang gỡ lỗi chương trình của mình và xem các giá trị cho tất cả các trường của
nó. Liệt kê 5-11 thử sử dụng macro [`println!`][println]<!-- ignore --> như
chúng ta đã sử dụng trong các chương trước. Tuy nhiên, điều này sẽ không hoạt
động.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-11/src/main.rs}}
```

<span class="caption">Liệt kê 5-11: Đang cố gắng in một `Hình chữ nhật`
ví dụ</span>

Khi chúng ta biên dịch mã này, chúng ta gặp lỗi với thông báo lỗi như sau:

```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-11/output.txt:3}}
```

Macro `println!` có thể thực hiện nhiều loại định dạng và theo mặc định, dấu
ngoặc nhọn cho `println!` sử dụng định dạng được gọi là `Display`: đầu ra dành
cho người dùng cuối  sử dụng trực tiếp. Các kiểu nguyên thủy mà chúng ta đã thấy
cho đến nay triển khai `Display` theo mặc định vì chỉ có một cách duy nhất bạn
muốn hiển thị `1` hoặc bất kỳ loại nguyên thủy nào khác cho người dùng. Nhưng
với các cấu trúc, cách `println!` nên định dạng đầu ra ít rõ ràng hơn vì có
nhiều khả năng hiển thị hơn: Bạn có muốn dấu phẩy hay không? Bạn có muốn in dấu
ngoặc nhọn không? Có nên hiển thị tất cả các trường không? Do sự mơ hồ này, Rust
không cố gắng đoán những gì chúng ta muốn và các cấu trúc không cài đặt
`Display` để sử dụng với `println!` và cặp kí tự giữ chỗ `{}`.

Nếu chúng ta tiếp tục đọc các lỗi, chúng ta sẽ tìm thấy ghi chú hữu ích này:

```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-11/output.txt:9:10}}
```

Hãy thử nó! Lệnh gọi macro `println!` bây giờ sẽ giống như `println!("rect1 is
{:?}", rect1);`. Đặt thông số xác định `:?` bên trong dấu ngoặc nhọn cho biết
`println!` chúng ta muốn sử dụng định dạng đầu ra có tên là `Debug`. Đặc trưng
`Debug` cho phép chúng ta in cấu trúc của mình theo cách hữu ích cho các nhà
phát triển để chúng ta có thể thấy giá trị của nó trong khi chúng ta đang gỡ lỗi
mã của mình.

Biên dịch mã với thay đổi này. chết tiệt! Chúng ta vẫn gặp lỗi:

```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/output-only-01-debug/output.txt:3}}
```

Nhưng một lần nữa, trình biên dịch cung cấp cho chúng ta một lưu ý hữu ích:

```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/output-only-01-debug/output.txt:9:10}}
```

Rust *có* bao gồm chức năng in ra thông tin gỡ lỗi, nhưng chúng ta phải chọn bật
gỡ lỗi một cách tường minh để cung cấp chức năng đó cho cấu trúc của mình. Để
làm điều đó, chúng ta thêm thuộc tính bên ngoài `#[derive(Debug)]` ngay trước
định nghĩa cấu trúc, như trong Liệt kê 5-12.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-12/src/main.rs}}
```

<span class="caption">Liệt kê 5-12: Thêm thuộc tính để lấy đặc điểm `Debug` và in thể hiện `Rectangle` bằng cách sử dụng định dạng gỡ lỗi</span>

Bây giờ khi chúng tôi chạy chương trình, chúng ta sẽ không gặp bất kỳ lỗi nào và
chúng ta sẽ thấy đầu ra sau:

```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-12/output.txt}}
```

Tuyệt! Nó không phải là kết quả đẹp nhất, nhưng nó hiển thị giá trị của tất cả
các trường cho trường hợp này, điều này chắc chắn sẽ hữu ích trong quá trình gỡ
lỗi. Khi chúng ta có các cấu trúc lớn hơn, sẽ rất hữu ích khi có đầu ra dễ đọc
hơn một chút; trong những trường hợp đó, chúng ta có thể sử dụng `{:#?}` thay vì
`{:?}` trong chuỗi `println!`. Trong ví dụ này, sử dụng kiểu `{:#?}` sẽ xuất ra
kết quả như sau:

```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/output-only-02-pretty-debug/output.txt}}
```

Một cách khác để in ra một giá trị bằng cách sử dụng định dạng `Debug` là sử
dụng macro [`dbg!`][dbg]<!-- ignore -->, chiếm quyền sở hữu của một biểu thức
(trái ngược với `println!` , lấy một tham chiếu), in tệp và số dòng nơi lệnh gọi
macro `dbg!` đó xảy ra trong mã của bạn cùng với giá trị kết quả của biểu thức
đó và trả về quyền sở hữu giá trị.

> Lưu ý: Việc gọi macro `dbg!` sẽ in ra luồng bảng điều khiển lỗi tiêu chuẩn
> (`stderr`), trái ngược với `println!`, in ra luồng bảng điều khiển đầu ra tiêu
> chuẩn (`stdout`). Chúng ta sẽ nói nhiều hơn về `stderr` và `stdout` trong phần
> [“Viết thông báo lỗi thành lỗi tiêu chuẩn thay vì đầu ra tiêu chuẩn” trong
> Chương 12][err]<!-- bỏ qua -->.

Đây là một ví dụ mà chúng tôi quan tâm đến giá trị được gán cho trường `width`,
cũng như giá trị của toàn bộ cấu trúc trong `rect1`:

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-05-dbg-macro/src/main.rs}}
```

Chúng ta có thể đặt `dbg!` xung quanh biểu thức `30 * scale` và vì `dbg!` trả về
quyền sở hữu giá trị của biểu thức, trường `width` sẽ nhận cùng giá trị như thể
chúng ta không có `dbg! ` gọi đến đó. Chúng tôi không muốn `dbg!` sở hữu
`rect1`, vì vậy chúng tôi sử dụng tham chiếu đến `rect1` trong lệnh gọi tiếp
theo. Đây là kết quả đầu ra của ví dụ này:

```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/no-listing-05-dbg-macro/output.txt}}
```

Chúng ta có thể thấy bit đầu ra đầu tiên đến từ *src/main.rs* dòng 10 nơi chúng
ta đang gỡ lỗi biểu thức `30 * scale` và giá trị kết quả của nó là `60` (định
dạng `Debug` được triển khai cho các số nguyên là để chỉ in giá trị của chúng).
Lệnh gọi `dbg!` trên dòng 14 của *src/main.rs* xuất giá trị của `&rect1`, là cấu
trúc `Rectangle`. Đầu ra này sử dụng định dạng `Debug` đẹp mắt của kiểu
`Rectangle`. Macro `dbg!` có thể thực sự hữu ích khi bạn đang cố tìm hiểu mã của
mình đang làm gì!

Ngoài đặc trưng `Debug`, Rust đã cung cấp một số đặc trưng để chúng ta sử dụng
với thuộc tính `derive` có thể thêm hành vi hữu ích vào các loại tùy chỉnh của
chúng tôi. Những đặc trưng đó và hành vi của chúng được liệt kê trong [Phụ lục
C][app-c]<!-- ignore -->. Chúng ta sẽ đề cập đến cách triển khai các đặc trưng
này với hành vi tùy chỉnh cũng như cách tạo các đặc trưng của riêng bạn trong
Chương 10. Ngoài ra còn có nhiều thuộc tính khác ngoài `derive`; để biết thêm
thông tin, hãy xem [phần “Thuộc tính” của Tham chiếu Rust][attributes].

Hàm `area` của chúng ta rất cụ thể: hàm này chỉ tính diện tích hình chữ nhật. Sẽ
rất hữu ích nếu liên kết hành vi này chặt chẽ hơn với cấu trúc `Rectangle` của
chúng ta vì nó sẽ không hoạt động với bất kỳ loại nào khác. Hãy xem cách chúng
ta có thể tiếp tục cấu trúc lại mã này bằng cách chuyển hàm `area` thành một
*phương thức* `area`  được định nghĩa trên kiểu `Rectangle` của chúng ta.

[the-tuple-type]: ch03-02-data-types.html#the-tuple-type
[app-c]: appendix-03-derivable-traits.md
[println]: ../std/macro.println.html
[dbg]: ../std/macro.dbg.html
[err]: ch12-06-writing-to-stderr-instead-of-stdout.html
[attributes]: ../reference/attributes.html