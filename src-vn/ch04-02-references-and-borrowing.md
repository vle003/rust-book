## Tài liệu tham khảo và mượn

Vấn đề với bộ mã trong Liệt kê 4-5 là chúng ta phải trả lại `String` cho hàm gọi
để chúng ta vẫn có thể sử dụng `String` sau lệnh gọi đến `calculate_length`, bởi
vì `String` đã được chuyển vào `calculate_length`. Thay vào đó, chúng ta có thể
cung cấp một tham chiếu đến giá trị `String`. Một *tham chiếu* giống như một con
trỏ ở chỗ nó là một địa chỉ mà chúng ta có thể theo dõi để truy cập dữ liệu được
lưu trữ tại địa chỉ đó; dữ liệu đó được sở hữu bởi một số biến khác. Không giống
như một con trỏ, một tham chiếu được đảm bảo trỏ đến một giá trị hợp lệ của một
loại cụ thể trong vòng đời của tham chiếu đó.

Đây là cách bạn sẽ xác định và sử dụng hàm `calculate_length` có tham chiếu đến
một đối tượng dưới dạng tham số thay vì sở hữu giá trị:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-07-reference/src/main.rs:all}}
```

Trước tiên, hãy lưu ý rằng tất cả mã bộ trong khai báo biến và giá trị trả về
của hàm đã biến mất. Thứ hai, lưu ý rằng chúng ta chuyển `&s1` vào
`calculate_length` và, theo định nghĩa của nó, chúng ta lấy `&String` thay vì
`Chuỗi`. Các ký hiệu và này đại diện cho *tham chiếu* và chúng cho phép bạn tham
chiếu đến một số giá trị mà không cần sở hữu giá trị đó. Hình 4-5 mô tả khái
niệm này.

<img alt="Ba bảng: bảng của s chỉ chứa một con trỏ tới bảng của s1. Bảng của s1 chứa dữ liệu ngăn xếp của s1 và trỏ tới dữ liệu chuỗi trên heap." src="img/trpl04-05.svg" class="center" />

<span class="caption">Hình 4-5: Sơ đồ `&String s` trỏ vào `String s1`</span>

> Ghi chú: Ngược lại với tham chiếu bằng cách sử dụng `&` là *hủy tham chiếu*,
> được thực hiện với toán tử hủy tham chiếu, `*`. Chúng ta sẽ thấy một số công
> dụng của toán tử hủy tham chiếu trong Chương 8 và thảo luận chi tiết về bỏ 
> tham chiếu trong Chương 15.


Chúng ta hãy xem xét kỹ hơn về lệnh gọi hàm ở đây:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-07-reference/src/main.rs:here}}
```

Cú pháp `&s1` cho phép chúng ta tạo một tham chiếu *tham chiếu* đến giá trị của
`s1` nhưng không sở hữu nó. Bởi vì nó không sở hữu, nên giá trị mà nó trỏ tới sẽ
không bị loại bỏ khi tham chiếu không tiếp tục được sử dụng.

Tương tự như vậy, chữ ký của hàm sử dụng `&` để chỉ ra rằng loại tham số `s` là
một tham chiếu. Hãy thêm một số chú giải để giải thích:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-08-reference-with-annotations/src/main.rs:here}}
```

Phạm vi trong đó biến `s` hợp lệ giống như phạm vi của bất kỳ tham số chức năng
nào, nhưng giá trị được chỉ định bởi tham chiếu không bị loại bỏ khi `s` bị
ngừng sử dụng, vì `s` không có quyền sở hữu. Khi các hàm có tham chiếu dưới dạng
tham số thay vì tham trị, chúng ta sẽ không cần trả lại giá trị để trả lại quyền
sở hữu vì chúng ta chưa bao giờ có quyền sở hữu.

Chúng ta gọi hành động tạo tham chiếu là *mượn*. Như trong cuộc sống thực, nếu
một người sở hữu thứ gì đó, bạn có thể mượn thứ đó từ họ. Khi bạn dùng nó xong,
bạn phải trả lại. Bạn không sở hữu nó.

Vì vậy, điều gì sẽ xảy ra nếu chúng ta cố gắng sửa đổi thứ gì đó mà chúng ta
đang vay mượn? Hãy thử mã trong Liệt kê 4-6. Cảnh báo độc hại: nó không hoạt
động!

<span class="filename">Filename: src/main.rs</span>

```rust,bỏ qua,does_not_compile
{{#rustdoc_include ../listings/ch04-undering-ownership/listing-04-06/src/main.rs}}
```

<span class="caption">Danh sách 4-6: Cố gắng sửa đổi một giá trị mượn</span>

Đây là lỗi:

```console
{{#include ../listings/ch04-undering-ownership/listing-04-06/output.txt}}
```

Giống như các biến là bất biến theo mặc định, các tham chiếu cũng vậy. Chúng ta
không được phép sửa đổi thứ gì đó mà chúng tôi có tham chiếu đến.

### Tham chiếu có thể thay đổi

Chúng ta có thể sửa mã từ Liệt kê 4-6 để cho phép chúng ta sửa đổi một giá trị
mượn chỉ bằng một vài điều chỉnh nhỏ sử dụng, thay vào đó, một *tham chiếu có
thể thay đổi*:

<span class="filename">Tên tệp: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-09-fixes-listing-04-06/src/main.rs}}
```

Đầu tiên chúng ta đổi `s` thành `mut`. Sau đó, chúng tôi tạo một tham chiếu có
thể thay đổi với `&mut s` trong đó chúng tôi gọi hàm `change` và cập nhật chữ ký
hàm thành chấp nhận tham chiếu có thể thay đổi với `some_string: &mut String`.
Điều này cho thấy rất rõ ràng rằng chức năng `change` sẽ thay đổi giá trị mà nó
vay mượn.

Tham chiếu có thể thay đổi có một hạn chế lớn: nếu bạn có tham chiếu có thể thay
đổi đến một giá trị, thì bạn không thể có tham chiếu nào khác đến giá trị đó.
Đoạn mã cố gắng tạo hai tham chiếu có thể thay đổi tới `s` sẽ không thành công:

<span class="filename">Tên tệp: src/main.rs</span>

```rust,bỏ qua,does_not_compile
{{#rustdoc_include ../listings/ch04-undering-ownership/no-listing-10-multiple-mut-not-allowed/src/main.rs:here}}
```

Đây là lỗi:

```console
{{#include ../listings/ch04-undering-ownership/no-listing-10-multiple-mut-not-allowed/output.txt}}
```

Lỗi này nói rằng mã này không hợp lệ vì chúng ta không thể mượn `s` dưới dạng có
thể thay đổi nhiều lần tại một thời điểm. Lần mượn có thể thay đổi đầu tiên là
trong `r1` và phải kéo dài cho đến khi nó được sử dụng trong `println!`, nhưng
giữa việc tạo tham chiếu có thể thay đổi đó và việc sử dụng nó, chúng ta đã cố
gắng tạo một tham chiếu có thể thay đổi khác trong `r2` để mượn cùng một dữ liệu
như `r1`.

Hạn chế ngăn chặn nhiều tham chiếu có thể thay đổi đến cùng một dữ liệu cùng một
lúc cho phép thay đổi nhưng theo một cách rất có kiểm soát. Đó là điều mà những
tân binh Rustaceans phải vật lộn vì hầu hết các ngôn ngữ đều cho phép bạn biến
đổi bất cứ khi nào bạn muốn. Lợi ích của việc hạn chế này là Rust có thể ngăn
chặn các cuộc chạy đua dữ liệu tại thời điểm biên dịch. Một *cuộc đua dữ liệu*
tương tự như một cuộc đua có điều kiện và xảy ra khi có ba hành vi sau:

* Hai hoặc nhiều con trỏ truy cập cùng một dữ liệu cùng một lúc.
* Ít nhất một trong số các con trỏ đang được sử dụng để ghi dữ liệu.
* Không có cơ chế nào được sử dụng để đồng bộ hóa quyền truy cập vào dữ liệu.

Các cuộc chạy đua dữ liệu khiến chương trình tạo ra những hành vi không xác định
và khó chẩn đoán cũng như khắc phục khi bạn đang cố gắng theo dõi chúng trong
khi chạy; Rust ngăn chặn vấn đề này bằng cách từ chối biên dịch mã với các cuộc
đua dữ liệu!

Như thường lệ, chúng ta có thể sử dụng dấu ngoặc nhọn để tạo một phạm vi mới,
cho phép nhiều tham chiếu có thể thay đổi, chỉ không phải * đồng thời*:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-11-muts-in-separate-scopes/src/main.rs:here}}
```

Rust thực thi một quy tắc tương tự để kết hợp các tham chiếu có thể thay đổi và
không thể thay đổi. Mã này dẫn đến một lỗi:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-12-immutable-and-mutable-not-allowed/src/main.rs:here}}
```

Đây là lỗi: 

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-12-immutable-and-mutable-not-allowed/output.txt}}
```

Chà! Chúng ta cũng *không thể* có *một tham chiếu __khả biến__* (mutable
reference) trong khi chúng ta có *một tham chiếu __bất biến__* (immutable
reference) với cùng một giá trị.

Người dùng tham chiếu bất biến không mong muốn giá trị đột ngột thay đổi từ bên
dưới họ! Tuy nhiên, nhiều tham chiếu bất biến được cho phép vì không ai chỉ đọc
dữ liệu mà lại có thể ảnh hưởng đến việc đọc dữ liệu của người khác.

Lưu ý rằng phạm vi của tham chiếu bắt đầu từ nơi nó được khai báo và tiếp tục
cho đến lần cuối cùng tham chiếu đó được sử dụng. Chẳng hạn, mã này sẽ biên dịch
vì lần sử dụng cuối cùng của tham chiếu bất biến, `println!`, xảy ra trước khi
tham chiếu khả biến được giới thiệu:

```rust,edition2021
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-13-reference-scope-ends/src/main.rs:here}}
```

Phạm vi của các tham chiếu không thay đổi `r1` và `r2` kết thúc sau `println!`
nơi chúng được sử dụng lần cuối, tức là trước khi tham chiếu khả biến `r3` được
tạo. Các khối lệnh này không chồng lấn nhau, vì vậy mã này được cho phép: trình
biên dịch có thể cho biết rằng tham chiếu không còn được sử dụng tại một thời
điểm trước khi khối lệnh kết thúc .

Mặc dù lỗi vay mượn đôi khi có thể khiến bạn khó chịu, nhưng hãy nhớ rằng chính
trình biên dịch Rust đã sớm chỉ ra một lỗi tiềm ẩn (tại thời điểm biên dịch chứ
không phải trong thời gian chạy) và cho bạn biết chính xác vấn đề nằm ở đâu. Sau
đó, bạn không cần phải truy tìm lý do tại sao dữ liệu của bạn không như bạn
nghĩ.

### Các Tham khảo lơ lửng

Trong các ngôn ngữ có con trỏ, rất dễ tạo nhầm một *con trỏ lơ lửng*—một con trỏ
tham chiếu đến một vị trí trong bộ nhớ mà có thể đã được cấp cho người khác—bằng
cách giải phóng một vùng nhớ trong khi bảo toàn một con trỏ tới vùng nhớ đó.
Ngược lại, trong Rust, trình biên dịch đảm bảo rằng các tham chiếu sẽ không bao
giờ là tham chiếu lơ lửng: nếu bạn có tham chiếu đến một vài dữ liệu, trình biên
dịch sẽ đảm bảo rằng dữ liệu sẽ không nằm ngoài phạm vi trước khi tham chiếu đến
dữ liệu.

Hãy thử tạo một tham chiếu lơ lửng để xem cách Rust ngăn chặn chúng bằng lỗi
biên dịch:


<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-14-dangling-reference/src/main.rs}}
```

Đây là lỗi:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-14-dangling-reference/output.txt}}
```

Thông báo lỗi này đề cập đến một tính năng mà chúng tôi chưa đề cập đến: thời
gian tồn tại. Chúng ta sẽ thảo luận chi tiết về thời gian sống trong Chương 10.
Tuy nhiên, nếu bạn bỏ qua phần về thời gian sống, thông báo lỗi sẽ chứa chìa
khóa giải thích tại sao mã này lại có vấn đề:

```text
this function's return type contains a borrowed value, but there is no value
for it to be borrowed from
```

Chúng ta hãy xem xét kỹ hơn chính xác những gì đang xảy ra ở mỗi giai đoạn của
mã `dangle` của chúng ta:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-15-dangling-reference-annotated/src/main.rs:here}}
```

Vì `s` được tạo bên trong `dangle` nên khi mã của `dangle` kết thúc, `s` sẽ bị
hủy cấp phát. Nhưng chúng tôi đã cố gắng trả lại một tham chiếu đến nó. Điều đó
có nghĩa là tham chiếu này sẽ trỏ đến một `String` không hợp lệ. Điều đó không
tốt! Rust sẽ không cho phép chúng tôi làm điều này.

Giải pháp ở đây là  trực tiếp trả về `String`:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-16-no-dangle/src/main.rs:here}}
```

Điều này hoạt động mà không có bất kỳ vấn đề. Quyền sở hữu được chuyển ra, và
không có gì bị hủy cấp phát.


### Các Quy tắc của Tham chiếu

Hãy để tóm tắt lại những gì chúng tôi đã thảo luận về các tài liệu tham khảo:

* Tại bất kỳ thời điểm nào, bạn có thể có * hoặc * một tham chiếu có thể thay
  đổi * hoặc * bất kỳ số lượng tài liệu tham khảo bất biến.
* Tài liệu tham khảo phải luôn luôn hợp lệ.

Tiếp theo, chúng tôi sẽ xem xét một loại tài liệu tham khảo khác: lát cắt
(slice).