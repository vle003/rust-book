## Kiểu Lát cắt (Slice)

*Slices* cho phép bạn tham chiếu các phần tử tuần tự liện tục trong một tập hợp
(collection) thay vì tất cả tập hợp. Một lát cắt là một loại tham chiếu, do đó
nó không có quyền sở hữu.

Đây là một yêu cầu lập trình nho nhỏ: viết một hàm lấy một chuỗi các từ được
phân tách bằng dấu cách và trả về từ đầu tiên nó tìm thấy trong chuỗi đó. Nếu
hàm không tìm thấy khoảng trắng trong chuỗi, thì toàn bộ chuỗi phải là một từ,
do đó, toàn bộ chuỗi sẽ được trả về.

Hãy tìm hiểu cách chúng ta khai báo hàm này mà không sử dụng các lát cắt, để
hiểu vấn đề mà các lát cắt sẽ giải quyết:

```rust,ignore
fn first_word(s: &String) -> ?
```

Hàm `first_word` có `&String` làm tham số. Chúng ta không muốn quyền sở hữu, vì
vậy điều này là tốt. Nhưng chúng ta nên trả lại cái gì? Chúng ta thực sự không
có cách nào để nói về *từng phần* của một chuỗi. Tuy nhiên, chúng ta có thể trả
về chỉ mục ở cuối từ, được biểu thị bằng khoảng trắng. Hãy thử điều đó, như
trong Liệt kê 4-7.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:here}}
```
<span class="caption">Liệt kê 4-7: Hàm `first_word` trả về một giá trị chỉ mục byte vào tham số `String`</span>

Bởi vì chúng ta cần xem qua từng phần tử trong `String` và kiểm tra xem một giá
trị có phải là khoảng trắng hay không, nên chúng ta sẽ chuyển đổi `String` thành
một mảng của các byte bằng cách sử dụng phương thức `as_bytes`.

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:as_bytes}}
```

Kế tiếp, chúng ta tạo một bộ xet duyệt (iterator) bằng cách sử dụng phương thức `iter` để duyệt toàn bộ các byte trong mảng:

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:iter}}
```

Chúng ta sẽ thảo luận chi tiết hơn về bộ xét duyệt (iterator) trong [Chương
13][ch13]<!-- ignore -->. Hiện tại, hãy biết rằng `iter` là một phương thức trả
về từng phần tử trong một tập hợp và `enumerate` sẽ gói kết quả của `iter` và
thay vào đó trả về mỗi phần tử như một phần của một bộ (tuple). Phần tử đầu tiên
của bộ được trả về từ `enumerate` là chỉ mục và phần tử thứ hai là một tham
chiếu đến phần tử. Điều này thuận tiện hơn một chút so với việc tự tính toán chỉ
số.

Bởi vì phương thức `enumerate` trả về một bộ dữ liệu, nên chúng ta có thể sử
dụng các mẫu để phân rã cấu trúc bộ dữ liệu đó. Chúng ta sẽ thảo luận nhiều hơn
về các mẫu trong [Chương 6][ch6]<!-- ignore -->. Trong vòng lặp `for`, chúng ta
chỉ định một mẫu có chỉ mục `i` trong bộ dữ liệu và `&item` cho một byte đơn độc
trong bộ dữ liệu. Bởi vì chúng ta nhận được tham chiếu đến phần tử từ
`.iter().enumerate()`, nên chúng ta sử dụng `&` trong mẫu.

Bên trong vòng lặp `for`, chúng ta tìm byte đại diện cho khoảng trắng bằng cách
sử dụng cú pháp ký tự byte. Nếu tìm thấy một khoảng trống, chúng ta sẽ trả lại
vị trí của nó. Nếu không, chúng ta trả về độ dài của chuỗi bằng cách sử dụng
`s.len()`.

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:inside_for}}
```

Bây giờ chúng ta có một cách để tìm ra chỉ số của vị trí kết thúc từ đầu tiên
trong chuỗi, nhưng có một vấn đề. Chúng ta đang trả về `usize`, nhưng đó chỉ là
một số có ý nghĩa trong ngữ cảnh của `&String`. Nói cách khác, vì đó là một giá
trị riêng biệt với `String` nên không có gì đảm bảo rằng giá trị đó sẽ vẫn hợp
lệ trong tương lai. Hãy xem xét chương trình trong Liệt kê 4-8 sử dụng hàm
`first_word` từ Liệt kê 4-7.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-08/src/main.rs:here}}
```

<span class="caption">Lưu trữ kết quả từ việc gọi hàm hàm `first_word` rồi thay đổi nội dung `String`</span>

Chương trình này biên dịch mà không có bất kỳ lỗi nào và cũng sẽ làm như vậy nếu
chúng ta sử dụng `word` sau khi gọi `s.clear()`. Vì `word` hoàn toàn không được
kết nối với trạng thái của `s` nên `word` vẫn chứa giá trị `5`. Chúng ta có thể
sử dụng giá trị `5` đó với biến `s` để cố trích xuất từ đầu tiên, nhưng đây sẽ
là một lỗi vì nội dung của `s` đã thay đổi kể từ khi chúng ta lưu giá trị `5`
vào biến `word`.

Việc phải quan tâm về chỉ mục trong `word` không phù hợp với dữ liệu trong `s`
thật tẻ nhạt và dễ bị lỗi! Việc quản lý các chỉ số này thậm chí còn dễ dàng hơn
nếu chúng ta viết một hàm `second_word`. Khai báo của nó sẽ trông như thế này:

```rust,ignore
fn second_word(s: &String) -> (usize, usize) {
```

Giờ đây, chúng tôi đang theo dõi chỉ mục bắt đầu *và* kết thúc và chúng tôi thậm
chí còn có nhiều giá trị hơn được tính toán từ dữ liệu ở một trạng thái cụ thể
nhưng hoàn toàn không bị ràng buộc với trạng thái đó. Chúng tôi có ba biến không
liên quan trôi nổi xung quanh cần được giữ đồng bộ.

May mắn thay, Rust có một giải pháp cho vấn đề này: các lát cắt chuỗi.

### Chuỗi lát

Một *string slice* là một tham chiếu đến một phần của `String`, và nó trông như
thế này:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-17-slice/src/main.rs:here}}
```

Thay vì tham chiếu đến toàn bộ `String`, `hello` là tham chiếu đến một phần của
`String`, được chỉ định trong bit bổ sung `[0..5]`. Chúng ta tạo các lát cắt
bằng cách sử dụng một giới hạn trong ngoặc với khai báo
`[starting_index..ending_index]`, trong đó `starting_index` là vị trí đầu tiên
trong lát và `ending_index` nhiều hơn một vị trí so với vị trí cuối cùng trong
lát. Bên trong nó, cấu trúc dữ liệu lát cắt lưu trữ vị trí bắt đầu và độ dài của
lát cắt, tương ứng với `ending_index` trừ đi `starting_index`. Vì vậy, trong
trường hợp `let world = &s[6..11];`, `world` sẽ là một lát cắt chứa con trỏ tới
byte tại chỉ mục 6 của `s` với giá trị độ dài là `5`.

Hình 4-6 cho thấy điều này trong một biểu đồ.

<img alt="Ba bảng: một bảng đại diện cho dữ liệu ngăn xếp của s, trỏ tới byte tại chỉ mục 0 trong một bảng có dữ liệu chuỗi &quot;hello world&quot; trên heap. Bảng thứ ba gửi lại dữ liệu ngăn xếp của thế giới lát cắt, có giá trị độ dài là 5 và trỏ tới byte 6 của bảng dữ liệu heap."
src="img/trpl04-06.svg" class="center" style="width: 50%;" />

<span class="caption">Hình 4-6: Lát cắt chuỗi đề cập đến một phần của `Chuỗi`</span>

Với cú pháp phạm vi `..` của Rust, nếu bạn muốn bắt đầu ở chỉ mục 0, bạn có thể
bỏ giá trị trước hai dấu chấm. Nói cách khác, chúng bằng nhau:

```rust
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
```

Tương tự như vậy, nếu lát cắt của bạn bao gồm byte cuối cùng của `String`, thì
bạn có thể bỏ số ở cuối. Điều đó có nghĩa là chúng bằng nhau:

```rust
let s = String::from("hello");

let len = s.len();

let slice = &s[0..len];
let slice = &s[..];
```

> Lưu ý: Chỉ số phạm vi lát cắt chuỗi phải xuất hiện ở các ký tự ranh giới  
> UTF-8 hợp lệ. Nếu bạn cố gắng tạo một lát cắt chuỗi ở giữa một ký tự nhiều 
> byte, chương trình của bạn sẽ thoát ra với một lỗi. Với mục đích giới thiệu 
> các lát cắt chuỗi, chúng tôi chỉ giả sử ASCII trong phần này; một cuộc thảo 
> luận kỹ lưỡng hơn về xử lý UTF-8 có trong phần [“Lưu trữ văn bản được mã hóa 
> UTF-8 bằng chuỗi”][strings]<!-- ignore --> của Chương 8.

Với tất cả thông tin này, hãy viết lại `first_word` để trả về một lát cắt. Kiểu
dữ liệu biểu thị “lát cắt chuỗi” được viết là `&str`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-18-first-word-slice/src/main.rs:here}}
```

Chúng ta lấy chỉ mục cho phần cuối của từ giống như cách chúng ta đã làm trong
Liệt kê 4-7, bằng cách tìm kiếm sự xuất hiện đầu tiên của khoảng trắng. Khi
chúng ta tìm thấy một khoảng trắng, chúng ta trả về một lát cắt chuỗi bằng cách
sử dụng phần đầu của chuỗi và chỉ mục của khoảng trắng làm chỉ số bắt đầu và kết
thúc.

Lúc này, khi gọi `first_word`, chúng ta nhận lại một giá trị duy nhất được liên
kết với dữ liệu cơ bản. Giá trị được tạo thành từ tham chiếu đến điểm bắt đầu
của lát cắt và số phần tử trong lát cắt.

Trả về một lát cắt cũng sẽ hoạt động đối với hàm `second_word`:

```rust,ignore
fn second_word(s: &String) -> &str {
```

Bây giờ chúng ta có một API đơn giản, khó *quậy* hơn nhiều vì trình biên dịch sẽ
đảm bảo các tham chiếu vào `String` vẫn hợp lệ. Bạn có nhớ lỗi trong chương
trình tại Liệt kê 4-8, khi chúng ta lấy chỉ mục đến cuối từ đầu tiên nhưng sau
đó xóa chuỗi nên chỉ mục của chúng ta không hợp lệ? Mã đó không chính xác về mặt
logic nhưng không hiển thị ngay lập tức bất kỳ lỗi nào. Các vấn đề sẽ xuất hiện
về sau nếu chúng ta tiếp tục cố sử dụng chỉ mục ngay từ ban đầu với một chuỗi
trống. Các lát cắt khiến lỗi này không thể xảy ra và cho chúng ta biết mã của
mình gặp sự cố sớm hơn nhiều. Sử dụng phiên bản lát cắt của `first_word` sẽ gây
ra lỗi biên dịch:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-19-slice-error/src/main.rs:here}}
```

Đây là lỗi trả về từ trình biên dịch:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-19-slice-error/output.txt}}
```

Hãy nhớ lại các quy tắc mượn (borrow rules), nếu chúng ta có *một tham chiếu bất
biến* đến một thứ gì đó, thì chúng ta cũng không thể có thêm *một tham chiếu khả
biến* được. Bởi vì `clear` cần cắt bớt `String`, nên nó cần dùng tới một tham
chiếu khả biến. Sau lệnh gọi `clear`, `println!` sử dụng tham chiếu trong `word`, vì vậy, tại thời điểm đó, tham chiếu bất biến vẫn phải hoạt động. 
Rust không cho phép tham chiếu khả biến trong `clear` và tham chiếu bất biến
trong `word` tồn tại cùng lúc, vì thế quá trình biên dịch sẽ không thành công. 
Rust không chỉ làm cho API của chúng ta dễ sử dụng hơn mà còn loại bỏ toàn bộ
lớp lỗi tại thời điểm biên dịch!

<!-- Old heading. Do not remove or links may break. -->
<a id="string-literals-are-slices"></a>

#### Chuỗi Tự nhiên dạng Slices

Nhớ lại rằng chúng ta đã nói về chuỗi ký tự được lưu trữ bên trong tệp nhị phân.
Bây giờ chúng ta đã biết về slice, chúng ta có thể hiểu đúng về chuỗi ký tự:

```rust
let s = "Hello, world!";
```

Kiểu của `s` ở đây là `&str`: đó là một lát cắt trỏ đến vị trí cụ thể đó của mã
nhị phân trong bộ nhớ. Đây cũng là lý do tại sao chuỗi ký tự là bất biến; `&str`
là một tham chiếu bất biến.

#### Dùng Chuỗi Slices làm Tham số

Việc biết rằng bạn có thể lấy các lát cắt kiểu kí tự và giá trị `String` dẫn
chúng tôi đến một cải tiến nữa trên `first_word`, và đây là khai báo của nó:

```rust,ignore
fn first_word(s: &String) -> &str {
```

Thay vào đó, một Rustacean có kinh nghiệm hơn sẽ viết khai báo hiển thị trong
Liệt kê 4-9 vì nó cho phép chúng ta sử dụng cùng một hàm trên cả hai giá trị
`&String` và các giá trị `&str`.

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-09/src/main.rs:here}}
```

<span class="caption">Liệt kê 4-9: Cải thiện chức năng `first_word` bằng cách sử dụng một lát cắt chuỗi cho loại tham số `s`</span>

Nếu chúng ta có một lát cắt chuỗi, chúng ta có thể chuyển nó trực tiếp. Nếu
chúng ta có `String`, chúng ta có thể chuyển một lát cắt của `String` hoặc
chuyển một tham chiếu đến `String`. Tính linh hoạt này tận dụng *sự ép buộc
deref*, một tính năng mà chúng ta sẽ đề cập trong phần [“Sự ép buộc Deref ngầm
định với Hàm và Phương thức”][deref-coercions]<!--ignore--> của Chương 15.

Việc xác định một hàm để lấy một lát cắt chuỗi thay vì tham chiếu đến một
`String` làm cho API của chúng ta trở nên tổng quát và hữu ích hơn mà không làm
mất đi bất kỳ chức năng nào:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-09/src/main.rs:usage}}
```

### Các Slice Khác

Các lát cắt chuỗi, như bạn có thể tưởng tượng, là dành riêng cho chuỗi. Nhưng
cũng có một loại lát cắt tổng quát hơn. Hãy xem xét mảng này:

```rust
let a = [1, 2, 3, 4, 5];
```

Giống như chúng ta có thể muốn tham chiếu đến một phần của chuỗi, chúng ta có
thể muốn tham chiếu đến một phần của mảng. Chúng ta sẽ làm điều đó như thế này:

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```

Lát cắt này có kiểu `&[i32]`. Nó hoạt động giống như các lát cắt chuỗi, bằng
cách lưu trữ tham chiếu đến phần tử đầu tiên và độ dài. Bạn sẽ sử dụng kiểu lát
cắt này cho tất cả các kiểu dạng tập hợp khác (collection types). Chúng ta sẽ
thảo luận chi tiết về các tập hợp này khi chúng ta nói về vectơ trong Chương 8.

## Tổng kết

Các khái niệm về __quyền sở hữu__, __mượn__ và __lát cắt__ đảm bảo an toàn cho
bộ nhớ trong các chương trình Rust tại thời điểm biên dịch. Ngôn ngữ Rust cung
cấp cho bạn quyền kiểm soát việc sử dụng bộ nhớ giống như các ngôn ngữ lập trình
hệ thống khác nhưng việc chủ sở hữu dữ liệu tự động dọn sạch dữ liệu đó khi chủ
sở hữu vượt quá phạm vi có nghĩa là bạn không phải viết và gỡ lỗi mã bổ sung để
có được quyền kiểm soát này.

Quyền sở hữu ảnh hưởng thế nào đến bao nhiêu phần hoạt động khác của Rust, vì
vậy chúng ta sẽ nói thêm về những khái niệm này trong suốt phần còn lại của cuốn
sách. Hãy chuyển sang Chương 5 và xem xét việc nhóm các phần dữ liệu lại với
nhau trong một `struct`.

[ch13]: ch13-02-iterators.html
[ch6]: ch06-02-match.html#patterns-that-bind-to-values
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[deref-coercions]: ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods