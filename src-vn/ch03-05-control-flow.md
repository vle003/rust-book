## Luồng Điều khiển

Việc chạy một số mã tùy thuộc vào việc một điều kiện có phải là `true` hay không
và khả năng chạy lặp lại một số mã trong khi một điều kiện là `true` là các khối
lệnh cơ bản trong hầu hết các ngôn ngữ lập trình. Các cấu trúc phổ biến nhất cho
phép bạn kiểm soát luồng thực thi mã Rust là các vòng lặp và biểu thức `if`.

### Biểu thức `if`

Biểu thức `if` cho phép bạn phân nhánh mã của mình tùy thuộc vào các điều kiện.
Bạn đưa ra một điều kiện và sau đó tuyên bố, “Nếu điều kiện này được đáp ứng,
hãy chạy khối mã này. Nếu điều kiện không được đáp ứng, đừng chạy khối mã này.”

Tạo một dự án mới có tên *branches* trong thư mục *projects* của bạn để khám phá
biểu thức `if`. Trong tệp *src/main.rs*, hãy nhập thông tin sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/src/main.rs}}
```

Tất cả các biểu thức `if` đều bắt đầu bằng từ khóa `if`, theo sau là một điều
kiện. Trong trường hợp này, điều kiện sẽ kiểm tra xem biến `number` có giá trị
nhỏ hơn 5 hay không. Chúng tôi đặt khối mã để thực thi nếu điều kiện là `true`
ngay sau điều kiện bên trong dấu ngoặc nhọn. Các khối mã được liên kết với các
điều kiện trong biểu thức `if` đôi khi được gọi là *arms*, giống như các nhánh
trong biểu thức `match` mà chúng ta đã thảo luận trong phần [“So sánh số đoán
với số bí mật”][comparing-the-guess-to-the-secret-number]<!-- bỏ qua --> của
Chương 2.

Theo tùy chọn, chúng ta cũng có thể bao gồm một biểu thức `else` mà chúng ta đã
chọn thực hiện ở đây, để cung cấp cho chương trình một khối mã thay thế để thực
thi nếu điều kiện đánh giá là `false`. Nếu bạn không cung cấp biểu thức `else`
và điều kiện là `false`, chương trình sẽ chỉ bỏ qua khối `if` và tiếp tục đến
đoạn mã tiếp theo.

Hãy thử chạy mã này; bạn sẽ thấy đầu ra sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/output.txt}}
```

Hãy thử thay đổi giá trị của `number` thành một giá trị làm cho điều kiện thành
`false` để xem điều gì sẽ xảy ra:

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/src/main.rs:here}}
```

Chạy lại chương trình và xem đầu ra:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/output.txt}}
```

Cũng cần lưu ý rằng điều kiện trong mã này *phải* là `bool`. Nếu điều kiện không
phải là `bool`, chúng ta sẽ gặp lỗi. Ví dụ: hãy thử chạy đoạn mã sau:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/src/main.rs}}
```

Lần này, điều kiện `if` đánh giá thành giá trị `3` và Rust đưa ra lỗi:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/output.txt}}
```

Lỗi chỉ ra rằng Rust mong đợi một `bool` nhưng nhận được một số nguyên. Không
giống như các ngôn ngữ như Ruby và JavaScript, Rust sẽ không tự động cố gắng
chuyển đổi các loại không phải Boolean thành Boolean. Bạn phải rõ ràng và luôn
cung cấp `if` với Boolean làm điều kiện. Ví dụ: nếu chúng ta muốn khối mã `if`
chỉ chạy khi một số khác `0`, thì chúng ta có thể thay đổi biểu thức `if` giống
như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-29-if-not-equal-0/src/main.rs}}
```

Chạy mã này sẽ in ra `number was something other than zero`.

#### Xử lý nhiều điều kiện với `else if`

Bạn có thể sử dụng nhiều điều kiện bằng cách kết hợp `if` và `else` trong một
biểu thức `else if`. Ví dụ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/src/main.rs}}
```

Chương trình này có bốn hướng có thể thực hiện. Sau khi chạy nó, bạn sẽ thấy đầu
ra như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/output.txt}}
```

Khi chương trình này thực thi, nó sẽ kiểm tra lần lượt từng biểu thức `if` và
thực thi phần thân đầu tiên mà điều kiện đánh giá là `true`. Lưu ý rằng mặc dù 6
chia hết cho 2, nhưng chúng tôi không thấy kết quả đầu ra `number is divisible
by 2`, cũng như không thấy văn bản `number is not divisible by 4, 3, or 2` từ
khối `else` . Đó là bởi vì Rust chỉ thực thi khối cho điều kiện `true` đầu tiên
và một khi nó tìm thấy một khối, nó thậm chí không kiểm tra phần còn lại.

Sử dụng quá nhiều biểu thức `else if` có thể làm lộn xộn mã của bạn, vì vậy nếu
có nhiều hơn một biểu thức, bạn có thể muốn cấu trúc lại mã của mình. Chương 6
mô tả cấu trúc phân nhánh mạnh mẽ của Rust được gọi là `match` cho những trường
hợp này.

#### Sử dụng `if` trong câu lệnh `let`

Vì `if` là một biểu thức nên chúng ta có thể sử dụng nó ở vế phải của câu lệnh
`let` để gán kết quả cho một biến, như trong Liệt kê 3-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-02/src/main.rs}}
```

<span class="caption">Listing 3-2: Assigning the result of an `if` expression to
a variable</span>

Biến `number` sẽ được liên kết với một giá trị dựa trên kết quả của biểu thức
`if`. Chạy mã này để xem điều gì xảy ra:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-02/output.txt}}
```

Hãy nhớ rằng các khối mã đánh giá biểu thức cuối cùng trong chúng và bản thân
các số cũng là biểu thức. Trong trường hợp này, giá trị của toàn bộ biểu thức
`if` phụ thuộc vào khối mã nào thực thi. Điều này có nghĩa là các giá trị có khả
năng là kết quả từ mỗi nhánh của `if` phải cùng kiểu; trong Liệt kê 3-2, kết quả
của cả nhánh `if` và nhánh `else` đều là các số nguyên `i32`. Nếu các kiểu không
khớp, như trong ví dụ sau, chúng ta sẽ gặp lỗi:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/src/main.rs}}
```

Khi chúng ta thử biên dịch mã này, chúng ta sẽ gặp lỗi. Các nhánh `if` và `else`
có các loại giá trị không tương thích và Rust chỉ ra chính xác nơi tìm thấy sự
cố trong chương trình:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/output.txt}}
```

Biểu thức trong khối `if` được đánh giá thành một số nguyên và biểu thức trong
khối `else` được đánh giá thành một chuỗi. Điều này sẽ không hoạt động vì các
biến phải có cùng một kiểu duy nhất và Rust cần biết tại thời điểm biên dịch,
chắc chắn loại biến `number` là gì. Biết kiểu `number` cho phép trình biên dịch
xác minh kiểu hợp lệ ở mọi nơi chúng ta sử dụng `number`. Rust sẽ không thể làm
điều đó nếu loại `number` chỉ được xác định trong thời gian chạy; trình biên
dịch sẽ phức tạp hơn và sẽ ít đảm bảo hơn về mã nếu nó phải theo dõi nhiều giả
thiết cho bất kỳ biến nào.

### Mã lặp lại với các Vòng lặp

Việc thực thi một khối mã nhiều lần thường hữu ích. Đối với tác vụ này, Rust
cung cấp một số kiểu *vòng lặp*, sẽ chạy qua mã bên trong thân vòng lặp cho đến
hết và sau đó bắt đầu quay lại ngay từ đầu. Để thử nghiệm với các vòng lặp, hãy
tạo một dự án mới có tên *loops*.

Rust có ba loại vòng lặp: `loop`, `while` và `for`. Hãy thử từng cái một.

#### Mã lặp lại với `loop`

Từ khóa `loop` yêu cầu Rust thực thi lặp đi lặp lại một khối mã mãi mãi hoặc cho
đến khi bạn yêu cầu nó dừng lại một cách rõ ràng.

Ví dụ: thay đổi tệp *src/main.rs* trong thư mục *loops* của bạn thành như sau:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-loop/src/main.rs}}
```

Khi chúng tôi chạy chương trình này, chúng tôi sẽ thấy `again!` được in liên tục
cho đến khi chúng tôi dừng chương trình theo cách thủ công. Hầu hết các thiết bị
đầu cuối đều hỗ trợ phím tắt <span class="keystroke">ctrl-c</span> để cưỡng bức
dừng chương trình bị kẹt trong một vòng lặp liên tục. Hãy thử một lần:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-32-loop
cargo run
CTRL-C
-->

```console
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```

Biểu tượng `^C` đại diện cho vị trí bạn đã nhấn <span
class="keystroke">ctrl-c</span>. Bạn có thể thấy hoặc không thấy từ `again!`
được in sau `^C`, tùy thuộc vào vị trí của mã trong vòng lặp khi nó nhận được
tín hiệu ngắt.

May mắn thay, Rust cũng cung cấp một cách để thoát khỏi vòng lặp bằng cách sử
dụng mã. Bạn có thể đặt từ khóa `break` trong vòng lặp để báo cho chương trình
biết khi nào ngừng thực hiện vòng lặp. Nhớ lại rằng chúng ta đã làm điều này
trong trò chơi đoán trong phần [“Thoát sau khi đoán
đúng”][quitting-after-a-correct-guess]<!-- bỏ qua --> của Chương 2 để thoát khỏi
chương trình khi người dùng đã thắng trò chơi bằng cách đoán đúng số.

Chúng ta cũng đã sử dụng `continue` trong trò chơi đoán, trò chơi này trong một
vòng lặp sẽ yêu cầu chương trình bỏ qua bất kỳ mã nào còn lại trong lần lặp này
của vòng lặp và chuyển sang lần lặp tiếp theo.

#### Trả về giá trị từ vòng lặp

Một trong những cách sử dụng `loop` là thử lại một thao tác mà bạn biết có thể
không thành công, chẳng hạn như kiểm tra xem một luồng đã hoàn thành công việc
của nó chưa. Bạn cũng có thể cần chuyển kết quả của thao tác đó ra khỏi vòng lặp
cho phần còn lại của mã. Để làm điều này, bạn có thể thêm giá trị bạn muốn trả
về sau biểu thức `break` mà bạn sử dụng để dừng vòng lặp; giá trị đó sẽ được trả
về ngoài vòng lặp để bạn có thể sử dụng nó, như được hiển thị ở đây:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-33-return-value-from-loop/src/main.rs}}
```

Trước vòng lặp, chúng ta khai báo một biến có tên `counter` và khởi tạo nó thành
`0`. Sau đó, chúng tôi khai báo một biến có tên `result` để giữ giá trị được trả
về từ vòng lặp. Trên mỗi lần lặp của vòng lặp, chúng tôi thêm `1` vào biến
`counter`, sau đó kiểm tra xem `counter` có bằng `10` hay không. Khi đó, chúng
tôi sử dụng từ khóa `break` với giá trị `counter * 2`. Sau vòng lặp, chúng ta sử
dụng dấu chấm phẩy để kết thúc câu lệnh gán giá trị cho `result`. Cuối cùng,
chúng tôi in giá trị trong `result`, trong trường hợp này là `20`.

#### Nhãn vòng lặp để phân biệt giữa nhiều vòng lặp

Nếu bạn có các vòng lặp bên trong các vòng lặp, `break` và `continue` sẽ áp dụng
cho vòng lặp trong cùng tại điểm đó. Bạn có thể tùy chọn chỉ định *nhãn vòng
lặp* trên vòng lặp mà sau đó bạn có thể sử dụng với `break` hoặc `continue` để
chỉ định rằng các từ khóa đó áp dụng cho vòng lặp được gắn nhãn thay vì vòng lặp
trong cùng. Nhãn vòng lặp phải bắt đầu bằng một trích dẫn. Đây là một ví dụ với
hai vòng lặp lồng nhau:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/src/main.rs}}
```

Vòng lặp bên ngoài có nhãn `'counting_up` và nó sẽ đếm từ 0 đến 2. Vòng lặp bên
trong không có nhãn sẽ đếm ngược từ 10 đến 9. `break` đầu tiên không chỉ định
nhãn sẽ chỉ thoát khỏi vòng lặp bên trong. Câu lệnh `break 'counting_up;` sẽ
thoát khỏi vòng lặp bên ngoài. Mã này in ra:

```console
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/output.txt}}
```

#### Vòng lặp có điều kiện với `while`

Một chương trình thường sẽ cần đánh giá một điều kiện trong một vòng lặp. Trong
khi điều kiện là `true`, vòng lặp sẽ chạy. Khi điều kiện không còn là `true`,
chương trình gọi `break`, dừng vòng lặp. Có thể triển khai hành vi như thế này
bằng cách sử dụng kết hợp `loop`, `if`, `else` và `break`; bạn có thể thử điều
đó ngay bây giờ trong một chương trình, nếu bạn muốn. Tuy nhiên, mẫu này phổ
biến đến mức Rust có một cấu trúc ngôn ngữ tích hợp cho nó, được gọi là vòng lặp
`while`. Trong Liệt kê 3-3, chúng ta sử dụng `while` để lặp chương trình ba lần,
đếm ngược mỗi lần và sau đó, sau vòng lặp, in một thông báo và thoát.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-03/src/main.rs}}
```

<span class="caption">Listing 3-3: Using a `while` loop to run code while a
condition holds true</span>

Cấu trúc này loại bỏ rất nhiều cách lồng ghép cần thiết nếu bạn sử dụng `loop`,
`if`, `else` và `break`, đồng thời nó rõ ràng hơn. Trong khi một điều kiện đánh
giá là `true`, thì mã sẽ chạy; nếu không, nó sẽ thoát khỏi vòng lặp.

#### Duyệt qua Bộ sưu tập với vòng lặp `for`

Bạn có thể chọn sử dụng cấu trúc `while` để lặp qua các phần tử của tập hợp,
chẳng hạn như một mảng. Ví dụ, vòng lặp trong Liệt kê 3-4 in mỗi phần tử trong
mảng `a`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-04/src/main.rs}}
```

<span class="caption">Liệt kê 3-4: Lặp qua từng phần tử của tập hợp bằng cách sử
dụng vòng lặp `while`</span>

Ở đây, mã đếm lên thông qua các phần tử trong mảng. Nó bắt đầu từ chỉ mục `0`,
sau đó lặp lại cho đến khi đạt đến chỉ mục cuối cùng trong mảng (nghĩa là khi
điều kiện `index < 5` không còn là `true`). Chạy mã này sẽ in mọi phần tử trong
mảng:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-04/output.txt}}
```

Tất cả giá trị năm phần tử của mảng xuất hiện trên thiết bị đầu cuối, như mong
đợi. Mặc dù `index` sẽ đạt đến giá trị `5` tại một thời điểm nào đó, nhưng vòng
lặp sẽ dừng thực thi trước khi tìm tới giá trị thứ sáu từ mảng.

Tuy nhiên, cách tiếp cận này dễ bị lỗi; chúng ta có thể khiến chương trình hoảng
loạn nếu giá trị chỉ mục hoặc điều kiện kiểm tra không chính xác. Ví dụ: nếu bạn
thay đổi định nghĩa của mảng `a` để có bốn phần tử nhưng quên cập nhật điều kiện
thành `trong khi chỉ số < 4`, mã sẽ bị hoảng loạn. Nó cũng chậm, vì trình biên
dịch thêm mã thời gian chạy để thực hiện kiểm tra có điều kiện xem chỉ mục có
nằm trong giới hạn của mảng trên mỗi lần lặp qua vòng lặp hay không.

Là một giải pháp thay thế ngắn gọn hơn, bạn có thể sử dụng vòng lặp `for` và
thực thi một số mã cho từng mục trong bộ sưu tập. Vòng lặp `for` trông giống như
mã trong Liệt kê 3-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-05/src/main.rs}}
```

<span class="caption">Liệt kê 3-5: Lặp qua từng phần tử của tập hợp bằng cách sử
dụng vòng lặp `for`</span>

Khi chạy đoạn mã này, chúng ta sẽ thấy kết quả giống như trong Liệt kê 3-4. Quan
trọng hơn, giờ đây chúng tôi đã tăng cường độ an toàn của mã và loại bỏ khả năng
xảy ra lỗi có thể xảy ra do vượt quá phần cuối của mảng hoặc không đi đủ xa và
thiếu một số mục.

Sử dụng vòng lặp `for`, bạn sẽ không cần phải nhớ thay đổi bất kỳ mã nào khác
nếu bạn thay đổi số lượng giá trị trong mảng, giống như cách bạn làm với phương
pháp được sử dụng trong Liệt kê 3-4.

Tính an toàn và ngắn gọn của các vòng lặp `for` khiến chúng trở thành cấu trúc
vòng lặp được sử dụng phổ biến nhất trong Rust. Ngay cả trong những tình huống
mà bạn muốn chạy một số mã trong một số lần nhất định, như trong ví dụ đếm ngược
đã sử dụng vòng lặp `while` trong Liệt kê 3-3, hầu hết những Rustaceans sẽ sử
dụng vòng lặp `for`. Cách để làm điều đó là sử dụng `Range`, được cung cấp bởi
thư viện tiêu chuẩn, tạo ra tất cả các số theo trình tự bắt đầu từ một số và kết
thúc trước một số khác.

Đây là cách đếm ngược trông ra sao khi sử dụng vòng lặp `for` và một phương pháp
khác mà chúng tôi chưa nói đến, `rev`, để đảo ngược phạm vi:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-34-for-range/src/main.rs}}
```

Mã này đẹp hơn một chút, phải vậy không?

## Tóm tắt

Bạn đã hoàn thành! Đây là một chương khá lớn: bạn đã học về biến, kiểu dữ liệu
vô hướng và phức hợp, hàm, chú thích, biểu thức `if` và vòng lặp! Để thực hành
với các khái niệm được thảo luận trong chương này, hãy thử xây dựng các chương
trình để thực hiện những việc sau:

* Chuyển đổi nhiệt độ giữa độ F và độ C.
* Tạo số Fibonacci thứ *n*.
* In lời bài hát mừng Giáng sinh “The Twelve Days of Christmas,” lợi dụng sự lặp
  lại trong bài hát.

Khi bạn đã sẵn sàng tiếp tục, chúng ta sẽ nói về một khái niệm trong Rust mà
thường *không* tồn tại trong các ngôn ngữ lập trình khác: quyền sở hữu.


[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[quitting-after-a-correct-guess]:
ch02-00-guessing-game-tutorial.html#quitting-after-a-correct-guess
