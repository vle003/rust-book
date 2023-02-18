## Loại dữ liệu

Mỗi giá trị trong Rust thuộc về một *kiểu dữ liệu* nhất định, cho Rust biết kiểu dữ liệu nào đang được chỉ định để nó biết cách làm việc với dữ liệu đó. Chúng ta sẽ xem xét hai tập hợp con trong các kiểu dữ liệu: vô hướng và phức hợp.

Hãy nhớ rằng Rust là một ngôn ngữ *kiểu tĩnh*, có nghĩa là nó phải biết kiểu của tất cả các biến tại thời điểm biên dịch. Trình biên dịch thường có thể suy ra kiểu chúng ta muốn sử dụng dựa trên giá trị và cách chúng ta sử dụng nó. Có thể trong trường hợp có nhiều kiểu, chẳng hạn như khi chúng ta chuyển đổi `String` thành kiểu số bằng cách sử dụng `parse` trong [“So sánh Số Dự đoán với Số Bí Mật”][comparing-the-guess-to-the-secret-number]<!-- bỏ qua --> trong Chương 2, chúng ta phải thêm một chú thích kiểu, như thế này:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Nếu chúng ta không thêm chú thích kiểu `: u32` được hiển thị trong đoạn mã trước, Rust sẽ hiển thị lỗi như sau, điều đó có nghĩa là trình biên dịch cần thêm thông tin từ chúng ta để biết chúng ta muốn sử dụng kiểu nào:

```console
{{#include ../listings/ch03-common-programming-concepts/output-only-01-no-type-annotations/output.txt}}
```

Bạn sẽ thấy các loại chú thích khác cho các loại dữ liệu khác.

### Các loại vô hướng

Loại *scalar* đại diện cho một giá trị. Rust có bốn loại vô hướng chính:
số nguyên, số dấu phẩy động, Booleans và ký tự. Bạn có thể nhận ra chúng từ các ngôn ngữ lập trình khác. Hãy bắt đầu tìm hiểu cách chúng hoạt động trong Rust.

#### Các kiểu số nguyên

*Số nguyên* là một số không có thành phần phân số. Chúng tôi đã sử dụng một loại số nguyên trong Chương 2, loại `u32`. Khai báo loại này chỉ ra rằng giá trị được liên kết với nó phải là một số nguyên không dấu (các loại số nguyên có dấu bắt đầu bằng `i` thay vì `u`) chiếm 32 bit dung lượng. Bảng 3-1 cho thấy các kiểu số nguyên dựng sẵn trong Rust. Chúng ta có thể sử dụng bất kỳ biến thể nào trong số này để khai báo loại giá trị số nguyên.

<span class="caption">Bảng 3-1: Các kiểu số nguyên trong Rust</span>

| Length  | Signed  | Unsigned |
|---------|---------|----------|
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |

Mỗi biến thể có thể là số có dấu hoặc không dấu và có kích thước rõ ràng. *Signed* và *unsigned* đề cập đến việc liệu số đó có thể là số âm hay không—nói cách khác, liệu số đó có cần phải có dấu với nó (có dấu) hay liệu số đó sẽ chỉ là số dương và do đó có thể được biểu thị mà không cần một dấu (không dấu). Nó giống như viết số trên giấy: khi dấu quan trọng, một số được hiển thị bằng dấu cộng hoặc dấu trừ; tuy nhiên, khi có thể chắc chắn rằng số đó là số dương, thì nó sẽ không có dấu.
Các số có dấu được lưu trữ bằng cách sử dụng cách biểu diễn [phần bù hai][twos-complement] <!-- ignore -->.

Mỗi biến có dấu có thể lưu trữ các số từ -(2<sup>n - 1</sup>) đến 2<sup>n -1</sup> - 1, trong đó *n* là số bit mà biến thể sử dụng. Vì vậy, `i8` có thể lưu trữ các số từ -(2<sup>7</sup>) đến 2<sup>7</sup> - 1, tương đương với -128 đến 127. Các biến không dấu có thể lưu trữ các số từ 0 đến 2 <sup>n</sup> - 1, vì vậy `u8` có thể lưu trữ các số từ 0 đến 2<sup>8</sup> - 1, tương đương với 0 đến 255.

Ngoài ra, các loại `isize` và `usize` phụ thuộc vào kiến trúc của máy tính mà chương trình của bạn đang chạy trên đó, được biểu thị trong bảng là “arch”:
64 bit nếu bạn đang sử dụng kiến trúc 64 bit và 32 bit nếu bạn đang sử dụng kiến trúc 32 bit.

Bạn có thể viết các số nguyên theo bất kỳ dạng nào trong Bảng 3-2. Lưu ý rằng các chữ số có thể là nhiều kiẻu số cho phép có một hậu tố chỉ kiểu, chẳng hạn như `57u8`, để chỉ định kiểu. Chữ số cũng có thể sử dụng `_` làm dấu tách trực quan làm cho số dễ đọc hơn, chẳng hạn như `1_000`, sẽ có cùng giá trị như thể bạn đã viết `1000`.

<span class="caption">Bảng 3-2: Số nguyên trong Rust</span>

| Number literals  | Example       |
|------------------|---------------|
| Decimal          | `98_222`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1111_0000` |
| Byte (`u8` only) | `b'A'`        |

Vậy làm thế nào để bạn biết nên sử dụng loại số nguyên nào? Nếu bạn không chắc chắn, các giá trị mặc định của Rust thường là nơi tốt để bắt đầu: các loại số nguyên mặc định là `i32`.
Tình huống chính mà bạn sẽ sử dụng `isize` hoặc `usize` là khi lập chỉ mục một số loại bộ sưu tập (collection).

> ##### Tràn số nguyên
>
> Giả sử bạn có một biến loại `u8` có thể chứa các giá trị từ 0 đến 255. 
> Nếu bạn cố gắng thay đổi biến thành một giá trị nằm ngoài phạm vi đó, chẳng 
> hạn như 256, *tràn số nguyên* sẽ xảy ra, điều này có thể dẫn đến một trong 
> hai hành vi. Khi bạn đang biên dịch ở chế độ gỡ lỗi, Rust bao gồm kiểm tra 
> lỗi tràn số nguyên khiến chương trình của bạn *hoảng sợ* trong thời gian chạy 
> nếu hành vi này xảy ra. Rust sử dụng thuật ngữ *hoảng loạn* khi một chương 
> trình thoát ra có lỗi; chúng ta sẽ thảo luận hoảng loạn sâu hơn trong phần 
> [“Lỗi không thể khôi phục với `panic!`”][unrecoverable-errors-with-panic]<!-- > bỏ qua --> trong Chương 9.
>
> Khi bạn đang biên dịch ở chế độ phát hành với cờ `--release`, Rust sẽ
> *không* bao gồm kiểm tra lỗi tràn số nguyên gây hoảng loạn. Thay vào đó, nếu
> tràn xảy ra, Rust thực hiện *two's complement wrapping*. Tóm lại, các giá trị
> lớn hơn giá trị tối đa mà kiểu có thể chứa, sẽ được “bao quanh” ở mức tối 
> thiểu của các giá trị mà kiểu có thể chứa. Trong trường hợp `u8`, giá trị 256 
> trở thành 0, giá trị 257 trở thành 1, v.v. Chương trình sẽ không hoảng loạn, 
> nhưng biến sẽ có một giá trị có thể không phải là giá trị mà bạn mong đợi. 
> Việc dựa vào hành vi bao gói của tràn số nguyên được coi là một lỗi.
>
> Để xử lý rõ ràng khả năng tràn, bạn có thể sử dụng nhóm các phương thức do 
> thư viện chuẩn cung cấp cho các kiểu số nguyên thủy:
>
> * Bao trong tất cả các chế độ bằng các phương thức `wrapping_*`, chẳng hạn 
>   như `wrapping_add`.
> * Trả về giá trị `None` nếu có tràn với các phương thức `checked_*`.
> * Trả về giá trị và giá trị boolean cho biết liệu có bị tràn hay không
>   với các phương thức `overflowing_*`.
> * Bão hòa ở giá trị tối thiểu hoặc tối đa của giá trị với các phương thức 
>   `saturating_*`.

#### Các kiểu dấu phẩy động

Rust cũng có hai kiểu cơ bản cho *số dấu phẩy động*, đó là số có dấu thập phân. Các loại dấu phẩy động của Rust là `f32` và `f64`, có kích thước lần lượt là 32 bit và 64 bit. Loại mặc định là `f64`bởi vì trên các CPU hiện đại, nó có tốc độ gần bằng `f32` nhưng có khả năng chính xác hơn. Tất cả các loại dấu phẩy động đều có dấu.

Đây là một ví dụ hiển thị các số dấu phẩy động:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```

Các số dấu phẩy động được biểu diễn theo tiêu chuẩn IEEE-754. Loại `f32` là float có độ chính xác đơn và `f64` có độ chính xác gấp đôi.

#### Phép toán số

Rust hỗ trợ các phép toán cơ bản mà bạn mong muốn đối với tất cả các kiểu số: cộng, trừ, nhân, chia và phần dư. Phép chia số nguyên cắt ngắn dần về 0 đến số nguyên gần nhất. Đoạn mã sau cho biết cách bạn sử dụng từng phép toán số trong câu lệnh `let`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```

Mỗi biểu thức trong các câu lệnh này sử dụng một toán tử toán học và đánh giá thành một giá trị duy nhất, sau đó giá trị này được liên kết với một biến. [Ơhụ lục B][appendix_b]<!-- bỏ qua --> chứa danh sách tất cả các toán tử mà Rust cung cấp.

#### Kiểu Boolean

Như trong hầu hết các ngôn ngữ lập trình khác, kiểu Boolean trong Rust có hai giá trị có thể có: `true` và `false`. Booleans có kích thước một byte. Loại Boolean trong Rust được chỉ định bằng cách sử dụng `bool`. Ví dụ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```

Cách chính để sử dụng các giá trị Boolean là thông qua các điều kiện, chẳng hạn như biểu thức `if`. Chúng tôi sẽ đề cập đến cách các biểu thức `if` hoạt động trong Rust trong phần [“Luồng điều khiển”][control-flow]<!-- ignore -->.

#### Loại ký tự

Loại `char` của Rust là loại chữ cái nguyên thủy nhất của ngôn ngữ. Dưới đây là một số ví dụ về khai báo giá trị `char`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```

Lưu ý rằng chúng tôi chỉ định ký tự `char` bằng dấu nháy đơn, trái ngược với ký tự chuỗi sử dụng dấu ngoặc kép. Loại `char` của Rust có kích thước bốn byte và đại diện cho Giá trị vô hướng Unicode, có nghĩa là nó có thể đại diện cho nhiều thứ hơn là chỉ ASCII. chữ cái có dấu; Chữ Hán, Nhật, Hàn; biểu tượng cảm xúc; và không gian có chiều rộng bằng 0 đều là các giá trị `char` hợp lệ trong Rust. Các giá trị vô hướng Unicode bao gồm từ `U+0000` đến `U+D7FF` và `U+E000` đến `U+10FFFF`.
Tuy nhiên, một "ký tự" không thực sự là một khái niệm trong Unicode, vì vậy trực giác con người của bạn về "ký tự" là gì có thể không khớp với `char` là gì trong Rust. Chúng ta sẽ thảo luận chi tiết về chủ đề này trong phần ["Lưu trữ văn bản được mã hóa UTF-8 bằng chuỗi"][strings]<!-- ignore --> ở Chương 8.

### Loại Phức hợp (compound)

*Các loại phức hợp* có thể nhóm nhiều giá trị thành một loại. Rust có hai loại phức hợp nguyên thủy: bộ (tuple) dữ liệu và mảng.

#### Loại Tuple

*Tuple* là một cách chung để nhóm một số giá trị với nhiều kiểu khác nhau thành một kiểu hỗn hợp. Các bộ dữ liệu có độ dài cố định: một khi được khai báo, chúng không thể tăng hoặc giảm kích thước.

Chúng ta tạo một bộ bằng cách viết một danh sách các giá trị được phân tách bằng dấu phẩy bên trong dấu ngoặc đơn. Mỗi vị trí trong bộ có một loại và loại của các giá trị khác nhau trong bộ không nhất thiết phải giống nhau. Chúng ta đã thêm chú thích cho các kiểu tùy chọn trong ví dụ này:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

Biến `tup` liên kết với toàn bộ bộ vì một bộ được coi là một phần tử ghép đơn lẻ. Để lấy các giá trị riêng lẻ ra khỏi một bộ, chúng ta có thể sử dụng khớp mẫu để hủy cấu trúc một giá trị bộ, như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```
Chương trình này trước tiên tạo một bộ và liên kết nó với biến `tup`. Sau đó, nó sử dụng một mẫu có `let` để lấy `tup` và biến nó thành ba biến riêng biệt, `x`, `y` và `z`. Điều này được gọi là *phá hủy* vì nó bị phá vỡ bộ dữ liệu duy nhất thành ba phần. Cuối cùng, chương trình in ra giá trị của `y`, là `6.4`.

Chúng ta cũng có thể truy cập trực tiếp một phần tử bộ bằng cách sử dụng dấu chấm (`.`) theo sau là chỉ mục của giá trị mà chúng ta muốn truy cập. Ví dụ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

#### Kiểu mảng

Một cách khác để có một tập hợp nhiều giá trị là với một *mảng*. Không giống như một bộ, mọi phần tử của một mảng phải có cùng kiểu. Không giống như mảng trong một số ngôn ngữ khác, mảng trong Rust có độ dài cố định.

Chúng tôi viết các giá trị trong một mảng dưới dạng danh sách được phân tách bằng dấu phẩy bên trong dấu ngoặc vuông:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```

Mảng rất hữu ích khi bạn muốn dữ liệu của mình được phân bổ trên ngăn xếp chứ không phải trên đống (chúng ta sẽ thảo luận thêm về ngăn xếp và đống trong [Chương 4][stack-and-heap]<!-- bỏ qua -->) hoặc khi bạn muốn đảm bảo bạn luôn có một số phần tử cố định. Tuy nhiên, một mảng không linh hoạt như kiểu vectơ. *vector* là một kiểu bộ sưu tập tương tự được cung cấp bởi thư viện tiêu chuẩn mà *được phép* tăng hoặc giảm kích thước. Nếu bạn không chắc nên sử dụng mảng hay vectơ, rất có thể bạn nên sử dụng vectơ. [Chương 8][vector]<!-- bỏ qua --> thảo luận chi tiết hơn về vectơ.

Tuy nhiên, mảng hữu ích hơn khi bạn biết số phần tử sẽ không cần thay đổi. Ví dụ: nếu bạn đang sử dụng tên của tháng trong một chương trình, có thể bạn sẽ sử dụng một mảng thay vì một véc-tơ vì bạn biết rằng nó sẽ luôn chứa 12 phần tử:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

Bạn viết kiểu của mảng bằng cách sử dụng dấu ngoặc vuông với kiểu của từng phần tử, dấu chấm phẩy và sau đó là số lượng phần tử trong mảng, như sau:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Ở đây, `i32` là loại của từng phần tử. Sau dấu chấm phẩy, số `5` cho biết mảng có năm phần tử.

Bạn cũng có thể khởi tạo một mảng để chứa cùng một giá trị cho mỗi phần tử bằng cách chỉ định giá trị ban đầu, theo sau là dấu chấm phẩy, sau đó là độ dài của
mảng trong ngoặc vuông, như được hiển thị ở đây:

```rust
let a = [3; 5];
```

Mảng có tên `a` sẽ chứa`5`  phần tử, ban đầu sẽ được đặt thành giá trị `3`. Điều này giống như viết `let a = [3, 3, 3, 3, 3];` nhưng theo cách ngắn gọn hơn.

##### Truy cập phần tử mảng

Mảng là một đoạn bộ nhớ có kích thước cố định, đã biết có thể được phân bổ trên ngăn xếp. Bạn có thể truy cập các phần tử của một mảng bằng cách lập chỉ mục, như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```

Trong ví dụ này, biến có tên `first` sẽ nhận giá trị `1` vì đó là giá trị tại chỉ mục `[0]` trong mảng. Biến có tên `second` sẽ nhận giá trị `2` từ chỉ mục `[1]` trong mảng.

##### Truy cập phần tử mảng không hợp lệ

Hãy xem điều gì sẽ xảy ra nếu bạn cố gắng truy cập một phần tử của mảng vượt qua phần tử cuối của mảng. Giả sử bạn chạy mã này, tương tự như trò chơi đoán trong Chương 2, để lấy chỉ số mảng từ người dùng:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```

Mã này biên dịch thành công. Nếu bạn chạy mã này bằng cách sử dụng `cargo run` và nhập `0`, `1`, `2`, `3` hoặc `4`, thì chương trình sẽ in ra giá trị tương ứng tại chỉ mục đó trong mảng. Thay vào đó, nếu bạn nhập một số ở cuối mảng, chẳng hạn như `10`, thì bạn sẽ thấy kết quả như sau:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-->

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Chương trình dẫn đến lỗi *runtime* khi sử dụng giá trị không hợp lệ trong hoạt động lập chỉ mục. Chương trình đã thoát với một thông báo lỗi và không thực thi câu lệnh cuối cùng `println!`. Khi bạn cố gắng truy cập một phần tử bằng cách lập chỉ mục, Rust sẽ kiểm tra xem chỉ mục bạn đã chỉ định có nhỏ hơn độ dài mảng hay không. Nếu chỉ số lớn hơn hoặc bằng chiều dài. Rust sẽ hoảng sợ. Việc kiểm tra này phải diễn ra trong thời gian chạy, đặc biệt là trong trường hợp này, vì trình biên dịch không thể biết giá trị mà người dùng sẽ nhập khi họ chạy mã sau này.

Đây là một ví dụ về các nguyên tắc an toàn bộ nhớ của Rust khi đang hoạt động. Trong nhiều ngôn ngữ cấp thấp, loại kiểm tra này không được thực hiện và khi bạn cung cấp chỉ mục không chính xác, bộ nhớ không hợp lệ có thể được truy cập. Rust bảo vệ bạn khỏi loại lỗi này bằng cách thoát ngay lập tức thay vì cho phép truy cập bộ nhớ và tiếp tục. Chương 9 thảo luận thêm về cách xử lý lỗi của Rust và cách bạn có thể viết mã an toàn, dễ đọc, không gây hoảng loạn cũng như không cho phép truy cập bộ nhớ không hợp lệ.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[twos-complement]: https://en.wikipedia.org/wiki/Two%27s_complement
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[appendix_b]: appendix-02-operators.md
