<!-- Old heading. Do not remove or links may break. -->
<a id="the-match-control-flow-operator"></a>
## Cấu trúc Điều khiển Luồng `match`

Rust có cấu trúc điều khiển luồng cực kỳ mạnh mẽ được gọi là `match` cho phép
bạn so sánh một giá trị với một loạt mẫu và sau đó thực thi mã dựa trên mẫu nào
khớp. Các mẫu có thể được tạo thành từ các giá trị bằng chữ, tên biến, ký tự đại
diện và nhiều thứ khác; [Chương 18][ch18-00-patterns]<!-- ignore --> bao gồm tất
cả các loại mẫu khác nhau và chức năng của chúng. Sức mạnh của `match` đến từ
tính biểu đạt của các mẫu và thực tế là trình biên dịch xác nhận rằng tất cả các
trường hợp đều được xử lý.

Hãy nghĩ về biểu thức `match` giống như một chiếc máy phân loại đồng xu: các
đồng xu trượt xuống một đường ray có các lỗ với kích thước khác nhau dọc theo nó
và mỗi đồng xu rơi qua lỗ đầu tiên mà nó gặp phải mà nó lọt vào. Theo cách tương
tự, các giá trị đi qua từng mẫu trong một `match` và ở mẫu đầu tiên, giá trị
"phù hợp", giá trị rơi vào khối mã được liên kết sẽ được sử dụng trong quá trình
thực thi.

Nói về tiền xu, hãy lấy chúng làm ví dụ bằng cách sử dụng `match`! Chúng ta có
thể viết một hàm lấy một đồng xu Mỹ chưa biết và theo cách tương tự như máy đếm,
xác định đó là đồng xu nào và trả về giá trị của nó bằng xu, như trong Liệt kê
6-3.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-03/src/main.rs:here}}
```

<span class="caption">Liệt kê 6-3: Một enum và một biểu thức `match` có các biến thể của enum làm mẫu của nó</span>

Hãy phân tích `match` trong hàm `value_in_cents`. Trước tiên, chúng ta liệt kê
từ khóa `match` theo sau là một biểu thức, trong trường hợp này là giá trị
`coin`. Điều này có vẻ rất giống với một biểu thức điều kiện được sử dụng với
`if`, nhưng có một sự khác biệt lớn: với `if`, điều kiện được xem xét như giá
trị Boolean, nhưng ở đây nó có thể là bất kỳ kiểu dữ liệu gì. Loại `coin` trong
ví dụ này là enum `Coin` mà chúng ta đã định nghĩa ở dòng đầu tiên.

Tiếp theo là các rẽ nhánh `match`. Một nhánh có hai phần: một mẫu và một số mã.
Nhánh đầu tiên ở đây có một mẫu là giá trị `Coin::Penny` và sau đó là toán tử
`=>` phân tách mẫu và mã để chạy. Mã trong trường hợp này chỉ có giá trị là `1`.
Mỗi nhánh được ngăn cách với nhánh tiếp theo bằng dấu phẩy.

Khi biểu thức `match` được thực thi, nó sẽ so sánh giá trị kết quả với mẫu của
mỗi nhánh theo thứ tự. Nếu một mẫu khớp với giá trị, mã được liên kết với mẫu đó
sẽ được thực thi. Nếu mẫu đó không khớp với giá trị, quá trình thực thi sẽ tiếp
tục sang nhánh tiếp theo, giống như trong máy phân loại tiền xu. Chúng ta có thể
có bao nhiêu nhánh tùy thích: trong Liệt kê 6-3, `match` của chúng ta có bốn
nhánh.

Mã được liên kết với mỗi nhánh là một biểu thức và giá trị kết quả của biểu thức
trong nhánh phù hợp là giá trị được trả về cho toàn bộ biểu thức `match`.

Chúng ta thường không sử dụng dấu ngoặc nhọn nếu mã nhánh ngắn, như trong Liệt
kê 6-3, nơi mỗi nhánh chỉ trả về một giá trị. Nếu bạn muốn chạy nhiều dòng mã
trong một nhánh so khớp, bạn phải sử dụng dấu ngoặc nhọn và dấu phẩy sau nhánh
đó là tùy chọn. Ví dụ: đoạn mã sau in ra "Lucky penny!" mỗi khi phương thức được
gọi với `Coin::Penny`, nhưng vẫn trả về giá trị cuối cùng của khối là `1`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-08-match-arm-multiple-lines/src/main.rs:here}}
```

### Các Mẫu Liên kết với các Giá trị

Một tính năng hữu ích khác của các nhánh so khớp (match arms) là chúng có thể
liên kết với các phần của giá trị khớp với mẫu. Đây là cách chúng ta có thể
trích xuất các giá trị từ các biến thể enum.

Ví dụ: hãy thay đổi một trong các biến thể enum của chúng ta để chứa dữ liệu bên
trong nó. Từ năm 1999 đến năm 2008, Hoa Kỳ đã đúc các đồng 25 xu (quarter) với
các thiết kế khác nhau cho từng bang trong số 50 bang ở một bên. Không có đồng
xu nào khác có thiết kế chung toàn quốc, vì vậy chỉ đồng 25 xu có giá trị bổ
sung này. Chúng ta có thể thêm thông tin này vào `enum` của mình bằng cách thay
đổi biến thể `Quarter` để bao gồm giá trị `UsState` được lưu trữ bên trong nó,
điều chúng ta đã thực hiện trong Liệt kê 6-4.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-04/src/main.rs:here}}
```

<span class="caption">Liệt kê 6-4: Một enum `Coin` trong đó biến thể `Quarter` cũng chứa giá trị `UsState`</span>

Hãy tưởng tượng rằng một người bạn đang cố gắng thu thập tất cả đồng 25 xu của
50 bang. Trong khi chúng ta sắp xếp tiền lẻ theo loại tiền xu, chúng ta cũng sẽ
gọi tên của tiểu bang tương ứng với mỗi đồng 25 xu để nếu đó là tiểu bang mà bạn
của chúng ta không có, họ có thể thêm tiểu bang đó vào bộ sưu tập của mình.

Trong biểu thức so khớp cho mã này, chúng ta thêm một biến có tên `state` vào
mẫu khớp với các giá trị của biến thể `Coin::Quarter`. Khi `Coin::Quarter` khớp
với mẫu, biến `state` sẽ liên kết với giá trị là tên *Bang* của đồng 25 xu đó.
Sau đó, chúng ta có thể sử dụng `state` trong mã cho nhánh đó, như sau:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-09-variable-in-pattern/src/main.rs:here}}
```

Nếu chúng ta gọi `value_in_cents(Coin::Quarter(UsState::Alaska))`, `coin` sẽ là
`Coin::Quarter(UsState::Alaska)`. Khi chúng ta so sánh giá trị đó với từng nhánh
đối sánh, không có nhánh nào khớp cho đến khi chúng ta đạt đến
`Coin::Quarter(state)`. Tại thời điểm đó, liên kết cho `state` sẽ là giá trị
`UsState::Alaska`. Sau đó, chúng ta có thể sử dụng ràng buộc đó trong biểu thức
`println!`, do đó lấy giá trị *Bang* bên trong ra khỏi biến thể enum `Coin` cho
`Quarter`.

### So khớp với `Option<T>`

Trong phần trước, chúng ta muốn lấy giá trị `T` bên trong ra khỏi `Some` trong
trường hợp sử dụng `Option<T>`; chúng ta cũng có thể xử lý `Option<T>` bằng cách
sử dụng `match`, như chúng ta đã làm với enum `Coin`! Thay vì so sánh các đồng
xu, chúng ta sẽ so sánh các biến thể của `Option<T>`, nhưng cách hoạt động của
biểu thức `match` vẫn giữ nguyên.

Giả sử chúng ta muốn viết một hàm nhận `Option<i32>` và nếu có một giá trị bên
trong, hãy thêm 1 vào giá trị đó. Nếu không có giá trị bên trong, hàm sẽ trả về
giá trị `None` và không thực hiện bất kỳ thao tác nào.

Hàm này rất dễ viết, nhờ có `match`, nó sẽ giống như Liệt kê 6-5.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:here}}
```

<span class="caption">Liệt kê 6-5: Một hàm sử dụng biểu thức `match` trên
một `Option<i32>`</span>

Hãy xem xét lần thực thi đầu tiên của `plus_one` một cách chi tiết hơn. Khi
chúng ta gọi `plus_one( five)`, biến `x` trong phần thân của `plus_one` sẽ có
giá trị `Some(5)`. Sau đó, chúng ta so sánh điều đó với từng nhánh:

```rust, ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:first_arm}}
```

Giá trị `Some(5)` không khớp với mẫu `None`, vì vậy chúng ta tiếp tục với nhánh tiếp theo:

```rust, ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:second_arm}}
```

`Some(5)` có khớp với `Some(i)` không? Có! Chúng ta có cùng một biến thể. `i`
liên kết với giá trị có trong `Some`, vì vậy `i` nhận giá trị `5`. Sau đó, mã
trong nhánh so khớp được thực thi, vì vậy chúng ta thêm 1 vào giá trị của `i` và
tạo một giá trị `Some` mới với tổng số `6` bên trong.

Bây giờ, hãy xem xét lệnh gọi thứ hai của `plus_one` trong Liệt kê 6-5, trong đó
`x` là `None`. Chúng ta nhập `match` và so sánh với nhánh đầu tiên:

```rust,ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:first_arm}}
```

Nó phù hợp! Không có giá trị nào để thêm vào, vì vậy chương trình dừng lại và
trả về giá trị `None` ở phía bên phải của `=>`. Vì nhánh đầu tiên khớp nên không
có nhánh nào khác được so sánh.

Kết hợp `match` và enums rất hữu ích trong nhiều trường hợp. Bạn sẽ thấy rất
nhiều dạng này trong mã Rust: `match` với một enum, liên kết một biến với dữ
liệu bên trong, rồi thực thi mã dựa trên dữ liệu đó. Ban đầu sẽ hơi phức tạp,
nhưng một khi bạn đã quen với nó, bạn sẽ ước mình có nó trong tất cả các ngôn
ngữ. Nó luôn được người dùng yêu thích.

### Các Match là kín kẽ

Có một khía cạnh khác của `match` mà chúng ta cần thảo luận: các mẫu của nhánh
phải bao gồm tất cả các khả năng. Hãy xem xét phiên bản này của hàm `plus_one`,
phiên bản này có lỗi và sẽ không biên dịch được:


```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-10-non-exhaustive-match/src/main.rs:here}}
```

Chúng ta không xử lý trường hợp `None`, vì vậy mã này sẽ gây ra lỗi. May mắn
thay, đó là một lỗi mà Rust biết cách bắt. Nếu chúng ta cố gắng biên dịch mã
này, chúng ta sẽ gặp lỗi này:

```console
{{#include ../listings/ch06-enums-and-pattern-matching/no-listing-10-non-exhaustive-match/output.txt}}
```

Rust biết rằng chúng ta đã không bao quát hết mọi trường hợp có thể xảy ra và
thậm chí còn biết chúng ta đã quên mẫu nào! Các so sánh trong Rust là *kín kẽ
(exhaustive)*: chúng ta phải bao quát hết mọi khả năng để mã hợp lệ. Đặc biệt là
trong trường hợp `Option<T>`, khi Rust ngăn chúng ta quên xử lý rõ ràng trường
hợp `None`, nó bảo vệ chúng ta khỏi giả định rằng chúng ta có giá trị trong khi
chúng ta có thể có null, do đó không thể gây ra *sai lầm hàng tỷ đô la* mà ta đã
thảo luận.

### Các mẫu Vét cạn và Trình giữ chỗ `_`

Sử dụng enums, chúng ta cũng có thể thực hiện các hành động đặc biệt đối với một
vài giá trị cụ thể, nhưng đối với tất cả các giá trị khác ta thực hiện một hành
động mặc định. Hãy tưởng tượng chúng ta đang triển khai một trò chơi trong đó,
nếu bạn tung 3 con xúc xắc, người chơi của bạn sẽ không di chuyển mà thay vào đó
nhận được một chiếc mũ lạ mắt mới. Nếu bạn tung được số 7, người chơi của bạn sẽ
mất một chiếc mũ lạ mắt. Đối với tất cả các giá trị khác, người chơi của bạn sẽ
di chuyển số ô đó trên bảng trò chơi. Đây là một `match` triển khai logic đó,
với kết quả của việc tung xúc xắc được mã hóa cứng thay vì một giá trị ngẫu
nhiên và tất cả logic khác được biểu thị bằng các hàm không có phần thân vì thực
tế việc triển khai chúng nằm ngoài phạm vi của ví dụ này:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-15-binding-catchall/src/main.rs:here}}
```

Đối với hai nhánh đầu tiên, các mẫu là các giá trị bằng chữ `3` và `7`. Đối với
nhánh cuối cùng bao gồm mọi giá trị có thể khác, mẫu là biến mà chúng ta đã chọn
đặt tên là `other`. Mã chạy cho nhánh `other` sử dụng biến bằng cách chuyển nó
tới hàm `move_player`.

Mã này biên dịch được, mặc dù chúng ta chưa liệt kê tất cả các giá trị có thể có
mà `u8` có thể có, vì mẫu cuối cùng sẽ khớp với tất cả các giá trị không được
liệt kê cụ thể. Mẫu tổng hợp này đáp ứng yêu cầu là `match` phải kín kẽ. Lưu ý
rằng chúng ta phải đặt nhánh vét cạn ở cuối cùng vì các mẫu được đánh giá theo
thứ tự. Nếu chúng ta đặt nhánh vét cạn sớm hơn, các nhánh khác sẽ không bao giờ
chạy, vì vậy Rust sẽ cảnh báo chúng ta nếu chúng ta thêm nhánh mới sau nhánh vét
cạn!

Rust cũng có một mẫu mà chúng ta có thể sử dụng khi chúng ta muốn vét cạn nhưng
không muốn *sử dụng* giá trị trong mẫu vét cạn: `_` là một mẫu đặc biệt khớp với
bất kỳ giá trị nào và không liên kết với giá trị đó giá trị. Điều này cho Rust
biết rằng chúng ta sẽ không sử dụng giá trị, vì vậy Rust sẽ không cảnh báo chúng
ta về một biến không được sử dụng.

Hãy thay đổi luật chơi: bây giờ, nếu bạn tung bất kỳ thứ gì khác ngoài số 3 hoặc
7, bạn phải tung lại. Chúng ta không cần sử dụng giá trị bắt tất cả nữa, vì vậy
chúng ta có thể thay đổi mã của mình để sử dụng `_` thay vì biến có tên `other`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-16-underscore-catchall/src/main.rs:here}}
```

Ví dụ này cũng đáp ứng yêu cầu về tính toàn diện vì chúng ta rõ ràng đang bỏ qua
tất cả các giá trị khác trong nhánh cuối cùng; chúng ta đã không quên bất cứ
điều gì.

Cuối cùng, chúng ta sẽ thay đổi quy tắc của trò chơi một lần nữa để không có gì
khác xảy ra trong lượt của bạn nếu bạn tung bất kỳ thứ gì khác ngoài 3 hoặc 7.
Chúng ta có thể diễn đạt điều đó bằng cách sử dụng giá trị đơn vị (loại bộ trống
mà chúng ta đã đề cập trong phần [“Loại Tuple”][tuples]<!-- ignore -->) dưới
dạng mã đi kèm với nhánh `_`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-17-underscore-unit/src/main.rs:here}}
```

Ở đây, chúng ta nói rõ với Rust rằng chúng ta sẽ không sử dụng bất kỳ giá trị
nào khác không khớp với mẫu trong nhánh trước đó và chúng ta không muốn chạy bất
kỳ mã nào trong trường hợp này.

Chúng ta sẽ đề cập thêm về các mẫu và cách so khớp trong [Chương
18][ch18-00-patterns]<!-- ignore -->. Bây giờ, chúng ta sẽ chuyển sang cú pháp
`if let`, cú pháp này có thể hữu ích trong các tình huống mà biểu thức `match`
có một chút dài dòng.

[tuples]: ch03-02-data-types.html#the-tuple-type
[ch18-00-patterns]: ch18-00-patterns.html