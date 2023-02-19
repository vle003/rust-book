## Hàm

Các chức năng phổ biến trong mã Rust. Bạn đã thấy một trong những hàm quan trọng
nhất trong ngôn ngữ này: hàm `main`, là điểm vào của nhiều chương trình.Bạn cũng
đã thấy từ khóa `fn`, cho phép bạn khai báo các hàm mới.

Mã Rust sử dụng *snake case* làm phong cách quy ước cho tên hàm và tên biến,
trong đó tất cả các chữ cái đều là chữ thường và gạch dưới giữa các từ riêng
biệt. Đây là một chương trình chứa định nghĩa hàm ví dụ:

<span class="filename">Tên tệp: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

Chúng ta định nghĩa một hàm trong Rust bằng cách nhập `fn` theo sau là tên hàm
và một cặp dấu ngoặc đơn. Các dấu ngoặc nhọn cho trình biên dịch biết nơi bắt
đầu và kết thúc thân hàm.

Chúng ta có thể gọi bất kỳ hàm nào chúng ta đã định nghĩa bằng cách nhập tên của
nó theo sau là một cặp ngoặc đơn. Bởi vì `another_function` được định nghĩa
trong chương trình nên nó có thể được gọi từ bên trong hàm `chính`. Lưu ý rằng
chúng tôi đã định nghĩa `another_function` *sau* hàm `main` trong mã nguồn;
chúng ta cũng có thể đã định nghĩa nó trước đây. Rust không quan tâm đến việc
bạn định nghĩa hàm ở đâu, chỉ quan tâm đến việc chúng được định nghĩa ở đâu đó
trong phạm vi mà người gọi có thể nhìn thấy.

Hãy bắt đầu một dự án nhị phân mới có tên *functions* để khám phá thêm các hàm.
Đặt ví dụ `another_function` vào *src/main.rs* và chạy nó. Bạn sẽ thấy đầu ra
sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-16-functions/output.txt}}
```

Các dòng thực thi theo thứ tự xuất hiện trong hàm `main`. Đầu tiên là in thông
báo “Hello, world!”, sau đó `another_function` được gọi và in ra thông báo của
nó.

### Thông số

Chúng ta có thể định nghĩa các hàm với *tham số*, là các biến đặc biệt nằm trong
chữ ký của hàm. Khi một hàm có các tham số, bạn có thể cung cấp các giá trị cụ
thể cho các tham số đó. Về mặt kỹ thuật, các giá trị cụ thể được gọi là *đối
số*, nhưng trong cuộc trò chuyện thông thường, mọi người có xu hướng sử dụng các
từ *tham số* và *đối số* thay thế cho các biến trong định nghĩa của hàm hoặc các
giá trị cụ thể được truyền vào khi bạn gọi một hàm.

Trong phiên bản `another_function` này, chúng tôi thêm một thông số:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

Hãy thử chạy chương trình này; bạn sẽ nhận được kết quả như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/output.txt}}
```

Khai báo của `another_function` có một tham số tên là `x`. Kiểu của `x` được chỉ
định là `i32`. Khi chúng ta chuyển `5` vào `another_function`, macro `println!`
sẽ đặt `5` ở vị trí cặp dấu ngoặc nhọn chứa `x` nằm trong chuỗi định dạng.

Trong chữ ký hàm, bạn *phải* khai báo loại của từng tham số. Đây là một quyết
định có chủ ý trong thiết kế của Rust: yêu cầu chú thích kiểu trong định nghĩa
hàm có nghĩa là trình biên dịch hầu như không bao giờ cần bạn sử dụng chúng ở
nơi khác trong mã để tìm ra kiểu bạn muốn nói. Trình biên dịch cũng có thể đưa
ra các thông báo lỗi hữu ích hơn nếu nó biết loại mà hàm mong đợi.

Khi xác định nhiều tham số, hãy phân tách các khai báo tham số bằng dấu phẩy,
như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```

Ví dụ này tạo một hàm có tên là `print_labeled_measurement` với hai tham số.
Tham số đầu tiên được đặt tên là `value` và là kiểu `i32`. Hàm thứ hai được đặt
tên là `unit_label` và là kiểu `char`. Sau đó, hàm in văn bản chứa cả `value` và
`unit_label`.

Hãy thử chạy mã này. Thay thế chương trình hiện có trong tệp *src/main.rs* của
dự án *functions* bằng ví dụ trước và chạy chương trình bằng `cargo run`:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/output.txt}}
```

Bởi vì chúng ta đã gọi hàm với `5` là giá trị cho `value` và `'h'` là giá trị
của `unit_label`, nên đầu ra của chương trình chứa các giá trị đó.

### Câu lệnh và Biểu thức

Các thân hàm được tạo thành từ một loạt các câu lệnh tùy ý kết thúc bằng một
biểu thức. Cho đến giờ, các hàm mà chúng ta đã đề cập chưa bao gồm biểu thức kết
thúc, nhưng bạn đã thấy biểu thức là một phần của câu lệnh. Vì Rust là một ngôn
ngữ dựa trên biểu thức nên đây là một điểm khác biệt quan trọng cần hiểu. Các
ngôn ngữ khác không có sự khác biệt giống như vậy, vì vậy hãy xem câu lệnh và
biểu thức là gì và sự khác biệt của chúng ảnh hưởng đến phần thân của hàm như
thế nào.

* **Câu lệnh** là các câu lệnh thực hiện một số hành động và không trả về giá
  trị.
* **Biểu thức** đánh giá một giá trị kết quả. Hãy xem xét một số ví dụ.

Chúng ta thực sự đã sử dụng các câu lệnh và biểu thức. Tạo một biến và gán giá
trị cho nó bằng từ khóa `let` là một câu lệnh. Trong Liệt kê 3-1, `let y = 6;`
là một câu lệnh.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

<span class="caption">Liệt kê 3-1: Một khai báo hàm `main` chứa một câu
lệnh</span>

Các định nghĩa hàm cũng là các câu lệnh; trong ví dụ trước, bản thân nó chính là
một câu lệnh.

Các câu lệnh không trả về giá trị. Do đó, bạn không thể gán câu lệnh `let` cho
một biến khác, như đoạn mã sau cố gắng thực hiện; bạn sẽ gặp lỗi:

<span class="filename">Tên tệp: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

Khi bạn chạy chương trình này, bạn sẽ nhận được lỗi như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/output.txt}}
```

Câu lệnh `let y = 6` không trả về giá trị, vì vậy không có gì để `x` liên kết
với nó. Điều này khác với những gì xảy ra trong các ngôn ngữ khác, chẳng hạn như
C và Ruby, trong đó phép gán trả về giá trị của phép gán. Trong các ngôn ngữ đó,
bạn có thể viết `x = y = 6` và cả `x` và `y` đều có giá trị `6`; điều đó không
hoạt động trong Rust.

Các biểu thức đánh giá một giá trị và chiếm phần lớn phần còn lại của mã mà bạn
sẽ viết trong Rust. Hãy xem xét một phép toán, chẳng hạn như `5 + 6`, là một
biểu thức có giá trị bằng `11`. Các biểu thức có thể là một phần của câu lệnh:
trong Liệt kê 3-1, `6` trong câu lệnh `let y = 6;` là một biểu thức đánh giá giá
trị `6`. Gọi một hàm là một biểu thức. Gọi một macro là một biểu thức. Một khối
phạm vi mới được tạo bằng dấu ngoặc nhọn là một biểu thức, ví dụ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

Biểu thức này:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

là một khối, trong trường hợp này, được đánh giá là `4`. Giá trị đó được liên
kết với `y` như một phần của câu lệnh `let`. Lưu ý rằng dòng `x + 1` không có
dấu chấm phẩy ở cuối, điều này không giống với hầu hết các dòng bạn đã thấy cho
đến nay. Biểu thức không kết thúc bằng dấu chấm phẩy. Nếu bạn thêm dấu chấm phẩy
vào cuối một biểu thức, bạn sẽ biến nó thành một câu lệnh và sau đó nó sẽ không
trả về giá trị. Hãy ghi nhớ điều này khi bạn khai thác các giá trịvà biểu thức
được trả về của hàm tiếp theo.

### Các hàm với các giá trị trả về

Các hàm có thể trả về các giá trị cho mã gọi chúng. Chúng ta không đặt tên cho
các giá trị trả về, nhưng chúng ta phải khai báo kiểu của chúng sau một mũi tên
(`->`). Trong Rust, giá trị trả về của hàm đồng nghĩa với giá trị của biểu thức
cuối cùng trong khối phần thân của hàm. Bạn có thể trả về sớm từ một hàm bằng
cách sử dụng từ khóa `return` và chỉ định một giá trị, nhưng hầu hết các hàm đều
hoàn trả về biểu thức cuối cùng. Đây là một ví dụ về một hàm trả về một giá trị:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

Không có lời gọi hàm, macro hay thậm chí là câu lệnh `let` nào trong hàm `
five`—chỉ có số `5`. Đó là một chức năng hoàn toàn hợp lệ trong Rust. Lưu ý rằng
kiểu trả về của hàm cũng được chỉ định, là `-> i32`. Thử chạy mã này; đầu ra sẽ
trông như thế này:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/output.txt}}
```

`5` trong `five` là giá trị trả về của hàm, đó là lý do tại sao kiểu trả về là
`i32`. Hãy xem xét điều này chi tiết hơn. Có hai bit quan trọng: đầu tiên, dòng
`let x = five();` cho thấy rằng chúng ta đang sử dụng giá trị trả về của một hàm
để khởi tạo một biến. Bởi vì hàm ` five` trả về `5`, nên dòng đó giống như sau:

```rust
let x = 5;
```

Thứ hai, hàm ` five` không có tham số và xác định loại giá trị trả về, nhưng
phần thân của hàm là một `5` lẻ loi không có dấu chấm phẩy vì đó là một biểu
thức có giá trị mà chúng ta muốn trả về.

Hãy xem xét một ví dụ khác:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

Chạy mã này sẽ in ra `The value of x is: 6`. Nhưng nếu chúng ta đặt dấu chấm
phẩy ở cuối dòng chứa `x + 1`, thay đổi nó từ một biểu thức thành một câu lệnh,
thì chúng ta sẽ gặp lỗi:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

Biên dịch mã này tạo ra một lỗi, như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/output.txt}}
```

Thông báo lỗi chính, `mismatched types`, tiết lộ vấn đề cốt lõi với mã này. Định
nghĩa của hàm `plus_one` nói rằng nó sẽ trả về kiểu `i32`, nhưng các câu lệnh
không đánh giá thành một giá trị, được biểu thị bằng `()`, kiểu đơn vị. Do đó,
không có gì được trả về, điều này mâu thuẫn với định nghĩa hàm và dẫn đến lỗi.
Trong kết quả này, Rust cung cấp một thông báo có thể giúp khắc phục sự cố này:
thông báo đề xuất xóa dấu chấm phẩy, điều này sẽ khắc phục lỗi.