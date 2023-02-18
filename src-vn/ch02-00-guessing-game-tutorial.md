# Viết một trò chơi Giải đoán

Hãy bắt đầu với Rust bằng cách cùng nhau thực hiện một dự án thực hành! Chương này giới thiệu cho bạn một số khái niệm Rust phổ biến bằng cách hướng dẫn bạn cách sử dụng chúng trong một chương trình thực tế. Bạn sẽ tìm hiểu về `let`, `match`, các phương thức, các hàm đi kèm, các crate, và nhiều hơn nữa! Trong các chương tiếp theo, chúng ta sẽ khám phá
những ý tưởng này một cách chi tiết hơn. Trong chương này, bạn sẽ chỉ thực hành các nguyên tắc cơ bản.

Chúng ta sẽ thực hiện một bài toán lập trình cổ điển dành cho người mới bắt đầu: một trò chơi Giải đoán. Đây là cách thức hoạt động: chương trình sẽ tạo ra một số nguyên ngẫu nhiên trong khoảng từ 1 đến 100. Sau đó nó sẽ nhắc người chơi nhập dự đoán. Sau khi dự đoán được nhập, chương trình sẽ cho biết dự đoán quá thấp hay quá cao. Nếu dự đoán là đúng, trò sẽ in thông báo chúc mừng và thoát ra.

## Thiết lập Dự án Mới

Để thiết lập một dự án mới, hãy chuyển đến thư mục *projects* mà bạn đã tạo trong Chương 1 và tạo một dự án mới bằng Cargo, như sau:

```console
$ cargo new guessing_game
$ cd guessing_game
```

Lệnh đầu tiên, `cargo new`, lấy tên của dự án (`guessing_game`)
như đối số đầu tiên. Lệnh thứ hai chuyển đến thư mục dự án mới.

Xem tệp *Cargo.toml* được tạo:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial
rm -rf no-listing-01-cargo-new
cargo new no-listing-01-cargo-new --name guessing_game
cd no-listing-01-cargo-new
cargo run > output.txt 2>&1
cd ../../..
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/Cargo.toml}}
```

Như bạn đã thấy trong Chương 1, `cargo new` tạo ra một chương trình "Hello, world!" cho bạn. Kiểm tra tệp *src/main.rs*:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/src/main.rs}}
```

Bây giờ hãy biên dịch chương trình "Hello, world!" và chạy nó trong cùng một bước sử dụng lệnh `cargo run`:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/output.txt}}
```

Lệnh `run` có ích khi bạn cần duyệt nhanh một dự án, như chúng ta sẽ làm trong trò chơi này, kiểm tra nhanh từng lần trước khi chuyển sang cái tiêp theo.

Mở lại tệp *src/main.rs*. Bạn sẽ viết tất cả mã trong tệp này.

## Xử lý trò chơi Dự Đoán

Phần đầu tiên của chương trình trò chơi đoán sẽ yêu cầu người dùng nhập liệu, xử lý đầu vào đó và kiểm tra xem đầu vào có ở dạng dự kiến hay không. Để bắt đầu, chúng ta sẽ cho phép người chơi nhập dự đoán. Nhập mã trong Liệt kê 2-1 vào tệp *src/main.rs*.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:all}}
```

<span class="caption">Listing 2-1: Chương trình sẽ nhận một dự đoán từ người dùng và in nó ra</span>

Mã này chứa rất nhiều thông tin, vì vậy hãy xem qua từng dòng một. Để lấy đầu vào từ người dùng và sau đó in kết quả dưới dạng đầu ra, chúng ta cần dùng thư viện xuất/nhập `io`. Thư viện `io` đến từ thư viện tiêu chuẩn, được gọi là `std`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:io}}
```

Theo mặc định, Rust có một tập hợp các mục được định nghĩa trong thư viện chuẩn mà nó đưa vào phạm vi của mọi chương trình. Bộ thư viện này được gọi là *prelude*, và bạn có thể thấy mọi thứ trong nó [in the standard library documentation][prelude].

Nếu loại bạn muốn sử dụng không có trong *prelude*, bạn phải mang nó vào phạm vi sử dụng một cách rõ ràng bằng câu lệnh `use`. Sử dụng thư viện `std::io` cung cấp cho bạn một số tính năng hữu ích, bao gồm khả năng chấp nhận đầu vào của người dùng.

Như bạn đã thấy trong Chương 1, hàm `main` là điểm bắt đầu thực thi của chương trình:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:main}}
```

Cú pháp `fn` khai báo một hàm mới; dấu ngoặc đơn, `()`, cho biết không có tham số nào; và dấu ngoặc nhọn, `{`, bắt đầu phần thân của hàm.

Như bạn đã học trong Chương 1, `println!` là một macro in một chuỗi ra màn hình:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print}}
```

Mã này đang in lời nhắc cho biết trò chơi là gì và yêu cầu nhập dữ liệu từ người dùng.

### Lưu các Giá trị với các Biến

Tiếp theo, chúng ta sẽ tạo một *variable* để lưu dữ liệu người dùng nhập vào, nó như sau:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:string}}
```

Bây giờ chương trình đang trở nên thú vị! Có rất nhiều điều đang diễn ra trong dòng lệnh nho nhỏ này. Chúng ta sử dụng câu lệnh `let` để tạo biến. Đây là một ví dụ khác:

```rust,ignore
let apples = 5;
```

Dòng này tạo một biến mới có tên `apples` và gắn nó với giá trị 5. Trong Rust, theo mặc định giá trị của các biến là bất biến, nghĩa là một khi chúng ta đặt giá trị cho biến khi khai báo thì giá trị đó sẽ không thay đổi. Chúng ta sẽ thảo luận chi tiết về khái niệm này trong phần [“Biến và khả năng thay đổi”][variables-and-mutability]<!-- bỏ qua --> trong Chương 3. Để tạo một biến với giá trị có thể thay đổi, chúng ta thêm `mut` trước tên biến:

```rust,ignore
let apples = 5; // immutable
let mut bananas = 5; // mutable
```

> Lưu ý: 
>   Cú pháp `//` bắt đầu một *comment* tiếp tục cho đến hết dòng. 
>   Rust bỏ qua mọi thứ trong các bình luận. 
>   Chúng ta sẽ thảo luận thêm về các bình luận
>   chi tiết trong [Chương 3][comments]<!-- bỏ qua -->.

Quay trở lại với chương trình trò chơi Dự đoán, bây giờ bạn đã biết rằng `let mut Guess` sẽ khai báo một biến có thể thay đổi giá trị có tên là `guess`. Dấu bằng (`=`) cho Rust biết rằng chúng tôi muốn gán thứ gì đó cho biến ngay lúc này. Ở bên phải của dấu bằng là giá trị mà `guess` được gán, nó là kết quả của việc gọi `String::new`, một hàm trả về một thể hiện mới của `String`. [`String`][string]<!-- bỏ qua --> là một kiểu chuỗi kí tự do thư viện chuẩn cung cấp, được mã hóa dạng UTF-8.

Cú pháp `::` trong dòng `::new` chỉ ra rằng `new` là một hàm liên kết của kiểu dữ liệu `String`. Một *hàm được liên kết* là một hàm được triển khai trên một kiểu, trong trường hợp này là `String`. Hàm `new` này tạo một chuỗi trống mới. Bạn sẽ tìm thấy hàm `new` trên nhiều kiểu dữ liệu khác vì đó là tên chung cho hàm tạo ra một giá trị mới thuộc một kiểu nào đó.

Nói một cách đầy đủ, dòng `let mut Guess = String::new();` đã tạo ra một biến có thể thay đổi được, nó liên kết với một thể hiện mới của một `String`. Chà!

### Nhận dữ liệu người dùng nhập vào

Nhớ lại rằng chúng ta đã thêm chức năng đầu vào/đầu ra từ thư viện chuẩn với `use std::io;` trên dòng đầu tiên của chương trình. Bây giờ chúng ta sẽ gọi hàm `stdin` từ mô-đun `io`, hàm này sẽ cho phép chúng ta xử lý thông tin nhập của người dùng:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:read}}
```

Nếu chúng ta chưa khai báo thư viện `io` với `use std::io;` khi bắt đầu chương trình, thì chúng ta vẫn có thể sử dụng hàm này bằng cách viết lời gọi hàm này là `std::io::stdin`. Hàm `stdin` trả về một thể hiện của [`std::io::Stdin`][iostdin]<!-- ignore -->, đó là một kiểu đại diện cho một điều khiển cho đầu vào tiêu chuẩn dùng cho thiết bị đầu cuối của bạn.

Tiếp theo, dòng `.read_line(&mut Guess)` gọi phương thức [`read_line`][read_line]<!--ignore--> trên bộ điều khiển đầu vào tiêu chuẩn để nhận thông tin nhập vào từ người dùng.
Chúng ta cũng đang truyền `&mut Guess` làm đối số cho `read_line` để cho nó biết chuỗi nào sẽ lưu trữ dữ liệu nhập của người dùng. Toàn bộ công việc của `read_line` là lấy bất cứ thứ gì người dùng nhập vào đầu vào tiêu chuẩn và nối nó vào một chuỗi (không ghi đè lên nội dung của chuỗi), vì vậy chúng tôi dùng chuỗi đó làm đối số. Đối số dạng chuỗi cần phải có thể thay đổi để phương thức có thể thay đổi nội dung của chuỗi.

Kí tự `&` cho biết rằng đối số này là một *tham chiếu* (*reference*), cung cấp cho bạn một cách để cho phép nhiều phần trong mã của bạn truy cập vào một phần dữ liệu mà không cần sao chép dữ liệu đó vào bộ nhớ nhiều lần. Tham chiếu là một tính năng phức tạp và một trong những lợi thế chính của Rust là mức độ an toàn và dễ dàng khi sử dụng tham chiếu. Bạn không cần phải biết nhiều chi tiết về điều đó để hoàn thành chương trình này. Hiện tại, tất cả những gì bạn cần là, giống như các biến, các tham chiếu là bất biến theo mặc định. Do đó, bạn cần viết `&mut Guess` thay vì `&guess` để làm cho nó có thể thay đổi được. (Chương 4 sẽ giải thích các tham chiếu kỹ lưỡng hơn.)

<!-- Old heading. Do not remove or links may break. -->
<a id="handling-potential-failure-with-the-result-type"></a>

### Xử lý lỗi có thể xảy ra với `Result`

Chúng tôi vẫn đang xử lý dòng mã này. Bây giờ chúng ta đang thảo luận về dòng văn bản thứ ba, nhưng lưu ý rằng nó vẫn là một phần của một dòng mã logic duy nhất. Phần kế tiếp là phương pháp này:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:expect}}
```

Chúng tôi có thể đã viết mã này như sau:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

Tuy nhiên, một dòng dài rất khó đọc, vì vậy tốt nhất bạn nên chia nó ra. Thông thường, bạn nên thêm một dòng mới và khoảng trắng khác để giúp ngắt các dòng dài khi bạn gọi một phương thức bằng cú pháp `.method_name()`. Bây giờ hãy thảo luận về những gì dòng này làm.

Như đã đề cập trước đó, `read_line` đặt bất kỳ thứ gì người dùng nhập vào chuỗi mà chúng ta chuyển đến nó, nhưng nó cũng trả về giá trị `Result`. [`Result`][result]<!--
ignore --> là một kiểu dữ liệu [*enumeration*][enums]<!-- bỏ qua -->, thường được gọi là *enum*, là kiểu có thể ở một trong nhiều trạng thái khả dụng. Chúng tôi gọi mỗi trạng thái ckhả dùng này là *biến thể*.

[Chapter 6][enums]<!-- ignore --> sẽ đề cập đến enums chi tiết hơn. Mục đích của kiểu `Result` này là để mã hóa thông tin xử lý lỗi.

Các biến thể của `Result` là `Ok` và `Err`. Biến thể `Ok` cho biết thao tác đã thành công và bên trong `Ok` là giá trị được tạo thành công.
Biến thể `Err` có nghĩa là thao tác không thành công và `Err` chứa thông tin về cách thức hoặc lý do thao tác không thành công.

Các giá trị của kiểu `Result`, cũng như các giá trị của bất kỳ kiểu dữ liệu nào, có các phương thức được định nghĩa trên chúng. Một phiên bản của `Result` có [`expect` method][expect]<!--ignover--> mà bạn có thể gọi. Nếu phiên bản `Result` này là một giá trị `Err`, thì `expect`
sẽ khiến chương trình gặp sự cố và hiển thị thông báo mà bạn đã chuyển làm đối số cho `expect`. Nếu phương thức `read_line` trả về `Err`, thì đó có thể là kết quả của một lỗi cơ bản đến từ hệ điều hành.
Nếu phiên bản `Result` này là một giá trị `Ok`, thì `expect` sẽ lấy giá trị trả về mà `Ok` đang giữ và chỉ trả lại giá trị đó cho bạn để bạn có thể sử dụng nó. 
Trong trường hợp này, giá trị đó là số byte trong dữ liệu người dùng nhập vào.

Nếu bạn không gọi `expect`, chương trình sẽ biên dịch, nhưng bạn sẽ nhận được một cảnh báo:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-02-without-expect/output.txt}}
```

Rust cảnh báo rằng bạn chưa sử dụng giá trị `Result` được trả về từ `read_line`, cho biết rằng chương trình chưa xử lý một lỗi có thể xảy ra.

Cách đúng đắn để loại bỏ cảnh báo là viết mã xử lý lỗi, nhưng trong trường hợp của chúng ta, chúng ta chỉ muốn làm xảy ra sự cố để làm hỏng chương trình này, để chúng ta
có thể sử dụng `expect`. Bạn sẽ tìm hiểu về cách khắc phục lỗi trong [Chương 9][recover]<!-- ignore -->.

### In giá trị với phần giữ chỗ của `println!` 

Ngoài dấu ngoặc nhọn đóng, chỉ còn một dòng nữa để thảo luận trong mã cho đến nay:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print_guess}}
```

Dòng này in chuỗi hiện chứa dữ liệu người dùng nhập vào. Tập hợp `{}` của các dấu ngoặc nhọn là một giữ chỗ: hãy coi `{}` như những chiếc càng cua nhỏ giữ một giá trị tại vị trí đó. Khi in giá trị của một biến, tên biến có thể nằm trong dấu ngoặc nhọn. Khi in kết quả đánh giá một biểu thức, hãy đặt các dấu ngoặc nhọn trống trong chuỗi định dạng, theo sau chuỗi định dạng với danh sách các biểu thức được phân tách bằng dấu phẩy để in trong từng chỗ dành sẵn cho dấu ngoặc nhọn trống theo cùng một thứ tự. Việc in một biến và kết quả của một biểu thức trong một lệnh gọi `println!` sẽ như sau:

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
```

Mã này sẽ in ra `x = 5 và y + 2 = 12`.

### Kiểm tra phần đầu tiên

Hãy kiểm tra phần đầu tiên của trò chơi. Chạy nó bằng `cargo run`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-01/
cargo clean
cargo run
input 6 -->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 6.44s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

Tại thời điểm này, phần đầu tiên của trò chơi đã hoàn thành: chúng ta đang nhận thông tin đầu vào từ
bàn phím và sau đó in nó.

## Tạo số bí mật

Tiếp theo, chúng ta cần tạo một số bí mật mà người dùng sẽ cố gắng đoán. Số bí mật phải khác nhau mỗi lần để trò chơi thú vị khi chơi nhiều lần. Chúng tôi sẽ sử dụng một số ngẫu nhiên từ 1 đến 100 để trò chơi không quá khó. Rust chưa bao gồm chức năng tạo số ngẫu nhiên trong thư viện tiêu chuẩn của nó. Tuy nhiên, nhóm phát triển Rust cung cấp một [crate `rand`][randcrate] cho chức năng này.

### Sử dụng Crate để có thêm chức năng

Hãy nhớ rằng một crate là một tập hợp các tệp mã nguồn Rust. Dự án mà chúng tôi đang xây dựng là một *binary crate*, là một dự án có thể thực thi được. Crate `rand` là một *crate thư viện*, chứa mã nhằm mục đích sử dụng trong các chương trình khác và không thể tự thực thi.

Sự phối hợp giữa các crate bên ngoài của Cargo là điểm mà Cargo thực sự tỏa sáng. Trước khi có thể viết mã sử dụng `rand`, chúng ta cần sửa đổi tệp *Cargo.toml* để bao gồm crate `rand` làm phần phụ thuộc. Mở tệp đó và thêm dòng sau vào dưới cùng, bên dưới tiêu đề phần `[dependencies]` mà Cargo đã tạo cho bạn. Đảm bảo chỉ định chính xác `rand` như chúng tôi có ở đây, với số phiên bản này, nếu không các ví dụ về mã trong hướng dẫn này có thể không hoạt động:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:8:}}
```

Trong tệp *Cargo.toml*, mọi thứ theo sau tiêu đề là một phần của phân đoạn (*section*) phân đoạn tiếp tục cho đến khi phân đoạn khác bắt đầu. Trong `[dependencies]`, bạn cho Cargo biết dự án của bạn phụ thuộc vào loại crate  bên ngoài nào và bạn yêu cầu phiên bản nào của những crate đó. Trong trường hợp này, chúng tôi chỉ định crate `rand` với trình xác định phiên bản ngữ nghĩa `0.8.5`. Cargo hiểu [Semantic Versioning][semver]<!-- ignore -->  (đôi khi được gọi là *SemVer*), đây là tiêu chuẩn để đánh số phiên bản. Nó xác định `0.8.5` thực ra là viết tắt của `^0.8.5`, có nghĩa là bất kỳ phiên bản nào ít nhất là 0.8.5 nhưng thấp hơn 0.9.0.

Cargo coi các phiên bản này có API công khai tương thích với phiên bản 0.8.5 và thông số kỹ thuật này đảm bảo bạn sẽ nhận được bản bản vá lỗi mới nhất vẫn sẽ biên dịch với mã trong chương này. Mọi phiên bản 0.9.0 trở lên đều không được đảm bảo có cùng API như các ví dụ sau sử dụng.

Bây giờ, không thay đổi bất kỳ mã nào, hãy build dự án, như trong Listing 2-2.

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
rm Cargo.lock
cargo clean
cargo build -->

```console
$ cargo build
    Updating crates.io index
  Downloaded rand v0.8.5
  Downloaded libc v0.2.127
  Downloaded getrandom v0.2.7
  Downloaded cfg-if v1.0.0
  Downloaded ppv-lite86 v0.2.16
  Downloaded rand_chacha v0.3.1
  Downloaded rand_core v0.6.3
   Compiling libc v0.2.127
   Compiling getrandom v0.2.7
   Compiling cfg-if v1.0.0
   Compiling ppv-lite86 v0.2.16
   Compiling rand_core v0.6.3
   Compiling rand_chacha v0.3.1
   Compiling rand v0.8.5
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
```

<span class="caption">Listing 2-2: Kết quả từ việc chạy `cargo build` sau khi thêm crate rand làm phụ thuộc</span>

Bạn có thể thấy các số phiên bản khác nhau (nhưng tất cả chúng sẽ tương thích với mã, nhờ SemVer!) và các dòng khác nhau (tùy thuộc vào hệ điều hành) và các dòng có thể theo thứ tự khác.

Khi chúng tôi đưa vào phần phụ thuộc bên ngoài, Cargo sẽ tìm và tải về các phiên bản mới nhất của mọi thứ mà phần phụ thuộc đó cần từ *registry*, đây là bản sao dữ liệu từ [Crates.io][cratesio]. Crates.io là nơi mọi người trong hệ sinh thái Rust đăng các dự án Rust nguồn mở của họ để cho người khác sử dụng.

Sau khi cập nhật registry, Cargo sẽ kiểm tra phần `[dependencies]` và tải xuống mọi crate chưa được tải xuống trong danh sách. Trong trường hợp này,
mặc dù chúng tôi chỉ liệt kê `rand` là phụ thuộc, Cargo cũng lấy các crate khác mà `rand` phụ thuộc để hoạt động. Sau khi tải xuống các crate, Rust biên dịch
chúng và sau đó biên dịch dự án với các phụ thuộc có sẵn.

Nếu bạn ngay lập tức chạy lại `cargo build` mà không thực hiện bất kỳ thay đổi nào, thì bạn sẽ không nhận được bất kỳ kết quả nào ngoài dòng `Finished`. Cargo biết đã tải xuống và biên dịch các phụ thuộc và bạn chưa thay đổi bất kỳ điều gì về chúng trong tệp *Cargo.toml* của mình. Cargo cũng biết rằng bạn chưa thay đổi bất kỳ điều gì trong mã của mình nên nó cũng không biên dịch lại mã nguồn. Không có gì để làm, nó chỉ đơn giản là kết thúc.

Nếu bạn mở tệp *src/main.rs*, thực hiện một thay đổi nhỏ, sau đó lưu và xây dựng lại, bạn sẽ chỉ thấy hai dòng đầu ra:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
touch src/main.rs
cargo build -->

```console
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
```

Những dòng này cho thấy Cargo chỉ cập nhật bản dựng với thay đổi nhỏ của bạn đối với tệp *src/main.rs*. Các thành phần phụ thuộc của bạn không thay đổi nên Cargo biết rằng nó có thể sử dụng lại những gì nó đã tải xuống và biên dịch cho chúng.

#### Đảm bảo các bản dựng có thể tái sản xuất bằng tệp *Cargo.lock*

Cargo có một cơ chế đảm bảo rằng bạn có thể xây dựng lại cùng một thành phần phần mềm mỗi khi bạn hoặc bất kỳ ai khác xây dựng mã của bạn: Cargo sẽ chỉ sử dụng các phiên bản của các thành phần phụ thuộc mà bạn đã chỉ định cho đến khi bạn chỉ định khác. Ví dụ: giả sử rằng phiên bản 0.8.6 của thùng `rand` sẽ ra mắt vào tuần tới và phiên bản đó
chứa một sửa lỗi quan trọng, nhưng nó cũng chứa một hồi quy sẽ phá vỡ mã của bạn. Để giải quyết vấn đề này, Rust tạo tệp *Cargo.lock* trong lần đầu tiên bạn chạy `cargo build`, vì vậy chúng tôi hiện có tệp này trong *guessing_game*
danh mục.

Khi bạn xây dựng dự án lần đầu tiên, Cargo tìm ra tất cả các phiên bản của các thành phần phụ thuộc phù hợp với tiêu chí rồi ghi chúng vào tệp *Cargo.lock*. Khi bạn xây dựng dự án của mình trong tương lai, Cargo sẽ thấy
rằng tệp *Cargo.lock* tồn tại và sẽ sử dụng các phiên bản được chỉ định ở đó thay vì thực hiện lại tất cả công việc tìm ra các phiên bản. Điều này cho phép bạn có một bản dựng có thể lặp lại tự động. Nói cách khác, dự án của bạn sẽ
duy trì ở mức 0.8.5 cho đến khi bạn nâng cấp rõ ràng, nhờ vào tệp *Cargo.lock*.
Vì tệp *Cargo.lock* rất quan trọng đối với các bản dựng có thể tái sản xuất nên tệp này thường được kiểm tra trong kiểm soát nguồn cùng với phần mã còn lại trong dự án của bạn.

#### Đang cập nhật thùng để có phiên bản mới

Khi bạn *muốn* cập nhật các crate, Cargo cung cấp lệnh `update`, lệnh này sẽ bỏ qua tệp *Cargo.lock* và tìm ra tất cả các phiên bản mới nhất phù hợp với thông số kỹ thuật của bạn trong *Cargo.toml*. Sau đó, Cargo sẽ ghi các phiên bản đó vào tệp *Cargo.lock*. Nếu không, theo mặc định, Cargo sẽ chỉ xem xét các phiên bản lớn hơn 0.8.5 và nhỏ hơn 0.9.0. Nếu crate `rand` đã phát hành hai phiên bản mới 0.8.6 và 0.9.0, bạn sẽ thấy thông tin sau nếu
bạn đã chạy `cargo update`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
cargo update
assuming there is a new 0.8.x version of rand; otherwise use another update
as a guide to creating the hypothetical output shown here -->

```console
$ cargo update
    Updating crates.io index
    Updating rand v0.8.5 -> v0.8.6
```

Cargo bỏ qua bản phát hành 0.9.0. Tại thời điểm này, bạn cũng sẽ nhận thấy sự thay đổi trong tệp *Cargo.lock* của mình, lưu ý rằng phiên bản của thùng `rand` mà bạn hiện đang sử dụng là 0.8.6. Để sử dụng `rand` phiên bản 0.9.0 hoặc bất kỳ phiên bản nào trong sê-ri 0.9.*x*, bạn phải cập nhật tệp *Cargo.toml* để trông giống như sau:

```toml
[dependencies]
rand = "0.9.0"
```

Lần tới khi bạn chạy `cargo build`, Cargo sẽ cập nhật registry các crate có sẵn và đánh giá lại các yêu cầu phiên bản `rand` của bạn theo phiên bản mới bạn đã chỉ định.

Còn rất nhiều điều để nói về [Cargo][doccargo]<!-- ignore --> và [hệ sinh thái crate][doccratesio]<!-- ignore -->, mà chúng ta sẽ thảo luận trong Chương 14, nhưng bây giờ, đó là tất cả những gì bạn cần biết. Cargo làm cho việc tái sử dụng các thư viện rất dễ dàng, vì vậy Rustaceans có thể viết các dự án nhỏ hơn được tập hợp từ nhiều gói.

### Tạo số ngẫu nhiên

Hãy bắt đầu sử dụng `rand` để tạo một số để đoán. Bước tiếp theo là cập nhật *src/main.rs*, như trong Listing 2-3.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:all}}
```

<span class="caption">Listing 2-3: Thêm mã lệnh để tạo số ngẫu nhiên</span>

Đầu tiên chúng ta thêm dòng `use rand::Rng;`. Đặc trưng (traits) `Rng` xác định các phương thức mà trình tạo số ngẫu nhiên triển khai và đặc trưng này phải nằm trong phạm vi để chúng ta sử dụng các phương pháp đó. Chương 10 sẽ đề cập chi tiết đến các đặc trưng.

Tiếp theo, chúng ta sẽ thêm hai dòng ở giữa. Trong dòng đầu tiên, chúng ta gọi hàm `rand::thread_rng` cung cấp cho chúng ta bộ tạo số ngẫu nhiên cụ thể mà chúng ta sẽ sử dụng: một bộ tạo nằm trong phạm vi của luồng thực thi hiện tại và được tạo bởi hệ điều hành. Sau đó, chúng ta gọi phương thức `gen_range` trên trình tạo số ngẫu nhiên. Phương pháp này được xác định bởi đặc trưng `Rng` mà chúng ta đã đưa vào phạm vi với câu lệnh `use rand::Rng;`. Các
Phương thức `gen_range` nhận một biểu thức phạm vi làm đối số và tạo một số ngẫu nhiên trong phạm vi. Loại biểu thức phạm vi chúng ta đang sử dụng ở đây có dạng `start..=end` và bao gồm cả giới hạn trên và dưới, vì vậy chúng ta cần chỉ định `1..=100` để yêu cầu một số từ 1 đến 100.

> Lưu ý: 
> Bạn sẽ không chỉ biết nên sử dụng đặc điểm nào, phương pháp và chức năng nào để gọi từ crate,
> vì vậy mỗi crate đều có tài liệu hướng dẫn sử dụng. Một tính năng thú vị khác của Cargo là 
> chạy `cargo doc > --open` lệnh sẽ xây dựng tài liệu được cung cấp bởi tất cả các phụ thuộc
> ngay trên máy tính của bạn và mở nó trong trình duyệt. Nếu bạn quan tâm đến các chức năng của
> crate `rand`, bạn có thể chạy `cargo doc --open` và nhấp vào `rand` trong khung bên trái.

Dòng thêm mới thứ hai in số bí mật. Điều này hữu ích khi chúng ta đang phát triển chương trình để có thể kiểm tra, nhưng chúng ta sẽ xóa nó khỏi phiên bản cuối cùng. Chương trình sẽ không phải là trò chơi nếu in câu trả lời ngay khi nó bắt đầu!

Hãy thử chạy chương trình một vài lần:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-03/
cargo run
4
cargo run
5
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4

$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

Bạn sẽ nhận được các số ngẫu nhiên khác nhau và tất cả chúng phải là các số nằm giữa 1 và 100. Bạn đã làm rất tốt !

## So sánh số dự đoán với số bí mật

Bây giờ chúng ta có đầu vào của người dùng và một số ngẫu nhiên, chúng ta có thể so sánh chúng. Bước này được hiển thị trong Listing 2-4. Lưu ý rằng mã này sẽ chưa biên dịch được, chúng ta sẽ giải thích vì sao.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-04/src/main.rs:here}}
```

<span class="caption">Listing 2-4: Thao tác với các giá trị trả về và thực hiện so sánh hai số</span>

Trước tiên, chúng ta thêm một câu lệnh `use` khác, đưa một kiểu có tên là `std::cmp::Ordering` vào sử dụng từ thư viện chuẩn. Kiểu `Ordering` là một enum khác và có các biến thể `Less`, `Greater` và `Equal`. Đây là ba kết quả có thể xảy ra khi bạn so sánh hai giá trị.

Sau đó, chúng ta thêm năm dòng mới ở dưới cùng sử dụng kiểu `Ordering`. Phương thức `cmp` so sánh hai giá trị và có thể được dùng để so sánh bất kỳ thứ gì có thể so. Nó tham chiếu đến bất cứ thứ gì bạn muốn so sánh: ở đây nó đang so sánh `guess` với `secret_number`. Sau đó, nó trả về một biến thể của enum `Ordering` chúng ta đã đưa vào chương trình với câu lệnh `use`. Chúng ta sử dụng biểu thức [`match`][match]<!-- ignore --> để quyết định bước tiếp theo dựa trên giá trị trả về của `Ordering` từ lệnh gọi tới `cmp` với các giá trị trong `guess` và `secret_number`.

Biểu thức `match` được tạo thành từ *các rẽ nhánh (arms)*. Một nhánh bao gồm một *mẫu (pattern)* để so khớp và mã sẽ được chạy nếu giá trị được cung cấp cho `match` phù hợp với mẫu của nhánh đó. Rust lấy giá trị được cung cấp cho `match` và lần lượt xem qua mẫu của từng nhánh. Các mẫu và cấu trúc `match` là các tính năng mạnh mẽ của Rust: chúng cho phép bạn thể hiện nhiều tình huống mà mã của bạn có thể gặp phải và chúng đảm bảo bạn xử lý tất cả chúng. Các tính năng này sẽ được đề cập chi tiết tương ứng trong Chương 6 và Chương 18.

Hãy cùng xem một ví dụ với biểu thức `match` mà chúng tôi sử dụng ở đây. Giả sử người dùng đã đoán 50 và số bí mật được tạo ngẫu nhiên lần này là 38.

Khi mã so sánh 50 với 38, vì 50 lớn hơn 38 nên phương thức `cmp` sẽ trả về `Ordering::Greater`. Biểu thức `match` nhận giá trị `Ordering::Greater` và bắt đầu kiểm tra mẫu của từng nhánh. Nó tìm ở mẫu của nhánh đầu tiên, `Ordering::Less`, và thấy rằng giá trị `Ordering::Greater` không khớp với `Ordering::Less`, vì vậy nó bỏ qua mã trong nhánh đó và chuyển sang nhánh tiếp theo. Mẫu của nhánh tiếp theo là `Ordering::Greater`, mà mẫu này *khớp* với `Ordering::Greater`! Mã được liên kết trong nhánh đó sẽ thực thi và in chuỗi `Too big!` ra màn hình. Biểu thức `match` kết thúc sau lần so thành công đầu tiên, do đó trong trường hợp này, nó sẽ không xem xét nhánh cuối cùng.

Tuy nhiên, mã trong Listing 2-4 vẫn chưa biên dịch được. Hãy thử xem nào:

<!--
The error numbers in this output should be that of the code **WITHOUT** the
anchor or snip comments
-->

```console
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-04/output.txt}}
```

Nội dung chính của lỗi nói rằng *mismatched types*. Rust có một hệ thống kiểu dữ liệu tĩnh, mạnh mẽ. Tuy nhiên, nó cũng có kiểu suy luận. Khi chúng ta viết `let mut guess = String::new()`, Rust có thể suy luận rằng `guess` phải là một `String` và không bắt chúng ta khai báo kiểu dữ liệu trước. Mặt khác, `secret_number` là một biến kiểu số. Một số kiểu dữ liệu dạng số của Rust có thể có giá trị từ 1 đến 100, `i32`: số 32 bit; `u32`: một số 32 bit không dấu; `i64`: một số 64 bit; cũng như những người khác. Trừ khi được quy định khác, Rust mặc định là `secret_number` là kiểu `i32`, trừ khi bạn thêm thông tin kiểu dữ liệu ở nơi khác sẽ khiến Rust suy ra một kiểu dữ liệu số khác. Nguyên nhân của lỗi là Rust không thực hiện phép so sánh giữa một dữ liệu kiểu chuỗi kí tự và một dữ liệu kiểu số.

Cuối cùng, chúng ta chuyển đổi `String` mà chương trình đọc dưới dạng đầu vào thành một kiểu số để chúng ta có thể so sánh nó với số bí mật. Chúng ta làm điều đó bằng cách thêm dòng này vào thân hàm `main`:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/src/main.rs:here}}
```

Dòng cần thêm:

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

Chúng ta tạo một biến có tên `guess`. Mà khoan đã, chẳng phải chương trình đã có biến tên `guess` rồi sao? Nó có, nhưng thật hữu ích, Rust cho phép chúng ta che khuất giá trị trước đó của `guess` bằng một giá trị mới. *Che khuất (Shadowing)* cho phép chúng ta sử dụng lại tên biến `guess` thay vì buộc chúng tôi phải tạo hai biến riêng biệt, chẳng hạn như: `guess_str` và `guess`. Chúng ta sẽ trình bày chi tiết hơn về vấn đề này trong [Chương 3][shadowing]<!-- ignore -->, nhưng hiện tại, hãy biết rằng tính năng này thường được sử dụng khi bạn muốn chuyển đổi một giá trị từ kiểu này sang kiểu khác.

Chúng ta gắn biến mới này với biểu thức `guess.trim().parse()`. Biến `guess` trong biểu thức đề cập đến biến `guess` ban đầu chứa dữ liệu được nhập vào dưới dạng một chuỗi. Phương thức `trim` của `String` sẽ loại bỏ mọi khoảng trắng ở đầu và cuối, điều mà chúng ta phải làm để có thể so sánh chuỗi với `u32`, chuỗi này chỉ có thể chứa dữ liệu dạng số. Người dùng phải nhập dự đoán của họ và nhấn <span class="keystroke">enter</span> để cho hàm `read_line` nhận dữ liệu, thao tác này sẽ thêm một ký tự xuống dòng vào chuỗi. Ví dụ: nếu người dùng nhập <span class="keystroke">5</span> và nhấn <span class="keystroke">enter</span>, `guess` trông như thế này: `5\n`. Kí tự `\n` đại diện cho “dòng mới”. (Trên Windows, nhấn <span class="keystroke">enter</span> dẫn đến xuống dòng và đừa con trỏ về đầu dòng, `\r\n`.) Phương thức `trim` loại bỏ `\n` hoặc `\r\ n`, làm cho kết quả chỉ còn duy nhất giá trị `5`.

[Phương thức `parse` trên chuỗi][parse] chuyển đổi một chuỗi thành một kiểu khác. Ở đây, chúng ta sử dụng nó để chuyển đổi từ một chuỗi kí tự thành một số. Chúng ta cần phải cho Rust biết lkiểu số chính xác mà chúng ta muốn bằng cách sử dụng `let guess: u32`. Dấu hai chấm (`:`) sau `guess` cho Rust biết rằng chúng ta sẽ khai báo kiểu dữ liệu. Rust có một vài kiểu số tích hợp sẵn; `u32` ở đây là một số nguyên 32 bit không dấu.
Đó là lựa chọn mặc định phù hợp với một số dương nhỏ. Bạn sẽ tìm hiểu về các loại số khác trong [Chương 3][integers]<!-- ignore -->.

Ngoài ra, khai báo `u32` trong chương trình ví dụ này và sự so sánh với `secret_number` có nghĩa là Rust sẽ suy ra rằng, như vậy `secret_number` phải là một biến có kiểu `u32`. Vì vậy, bây giờ phép so sánh sẽ là so sánh giữa hai giá trị của cùng một kiểu !

Phương thức `parse` sẽ chỉ hoạt động trên các ký tự có thể được chuyển đổi thành số một cách hợp lý và do đó có thể dễ dàng gây ra lỗi. Ví dụ, nếu chuỗi chứa `A👍%`, sẽ không có cách nào để chuyển đổi nó thành một số. Vì phép so sánh có thể không thành công, cho nên phương thức `parse` trả về kiểu `Result`, giống như cách mà phương thức `read_line` đã làm (đã thảo luận trước đó trong [“Handling Potential Failure with `Result`”] (#handling-potential-failure-with-result)<!-- ignore-->). 

Chúng ta sẽ xử lý với `Result` theo cách tương tự bằng cách sử dụng lại phương thức `expect`. Nếu `parse` trả về một kết quả `Err` của `Result` vì nó không thể tạo một số từ chuỗi, thì lệnh gọi `expect` sẽ làm chương trình bị sập và in thông báo mà chúng ta cung cấp.
Nếu `parse` thành công cho việc chuyển đổi chuỗi thành số, nó sẽ trả về kết quả `Ok` của `Result` và `expect` sẽ trả về số mà chúng ta muốn từ giá trị của `Ok`.

Bây giờ hãy chạy chương trình:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/
cargo run
  76
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
    76
You guessed: 76
Too big!
```

Tốt! Mặc dù khoảng trắng đã được thêm vào trước khi đoán, nhưng chương trình vẫn nhận ra rằng người dùng đã đoán là 76. Chạy chương trình một vài lần để xác minh các hành vi khác nhau với các loại đầu vào khác nhau: đoán đúng số, đoán một số quá cao, và đoán một con số quá thấp.

Hiện tại chương trình của chúng ta đã gần như là hoạt động, nhưng người dùng chỉ có thể đoán một lần.
Hãy thay đổi điều đó bằng cách thêm một vòng lặp!

## Cho phép nhiều lần đoán với vòng lặp

Từ khóa `loop` tạo ra một vòng lặp vĩnh cửu. Chúng ta sẽ thêm một vòng lặp để cung cấp cho người dùng nhiều cơ hội đoán số hơn:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-04-looping/src/main.rs:here}}
```

Như bạn có thể thấy, chúng ta đã chuyển mọi thứ từ lời nhắc nhập dữ liệu trở đi vào một vòng lặp. Hãy chắc chắn thụt lề các dòng bên trong vòng lặp thêm bốn khoảng trắng và chạy lại chương trình. Bây giờ, chương trình sẽ liên tục yêu cầu một lần đoán khác, điều này thực sự đưa ra một vấn đề mới. Có vẻ như người dùng không thể bỏ cuộc!

Người dùng sẽ luôn có thể ngắt chương trình bằng cách sử dụng phím tắt <span class="keystroke">ctrl-c</span>. Nhưng có một cách khác để thoát khỏi con quái vật vô độ này, như đã đề cập trong cuộc thảo luận về 'phân tích cú pháp' ở [“So sánh số đoán với số bí mật”](#comparing-the-guess-to-the-secret-number)<!-- ignore -->: nếu người dùng nhập một câu trả lời không phải là số, chương trình sẽ bị lỗi. Chúng ta có thể tận dụng lợi thế đó để cho phép người dùng thoát chương trình, như minh họa ở đây:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-04-looping/
cargo run
(too small guess)
(too big guess)
(correct guess)
quit
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/main.rs:28:47
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Nhập `quit` sẽ thoát khỏi trò chơi, nhưng như bạn sẽ nhận thấy, việc nhập bất kỳ mục nhập không phải số nào khác cũng sẽ như vậy. Điều này là không tối ưu, có thể nói rằng ít nhất; chúng tôi muốn trò chơi cũng dừng lại khi đoán đúng số.

### Dừng trò chơi sau khi đoán đúng

Hãy lập trình trò chơi để thoát khi người dùng thắng bằng cách thêm câu lệnh `break`:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-05-quitting/src/main.rs:here}}
```

Việc thêm dòng `break` sau `You win!` làm cho chương trình thoát khỏi vòng lặp khi người dùng đoán đúng số bí mật. Thoát khỏi vòng lặp cũng có nghĩa là thoát khỏi chương trình, vì vòng lặp là phần cuối của hàm `main`.

### Xử lý dữ liệu nhập không hợp lệ

Để tiếp tục tinh chỉnh hành vi của trò chơi, thay vì chương trình gây lỗi khi người dùng nhập một số không phải là số, hãy làm cho trò chơi bỏ qua một số không phải là số người dùng có thể tiếp tục đoán. Chúng tôi có thể làm điều đó bằng cách thay đổi dòng mã nơi mà `guess` được chuyển đổi từ `String` thành `u32`, như trong Listing 2-5.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-05/src/main.rs:here}}
```

<span class="caption">Listing 2-5: Bỏ qua lời đoán không phải là kí tự số và cho nhập số mới thay vì gây lỗi dừng chương trình</span>

Chúng ta chuyển từ một lời gọi `expect` sang một biểu thức `match` để chuyển từ sự cố do lỗi sang xử lý lỗi. Hãy nhớ rằng `parse` trả về một kiểu `Result` và `Result` là một enum có các biến thể `Ok` và `Err`. Chúng ta đang sử dụng biểu thức `match` ở đây, giống như chúng ta đã làm với kết quả `Ordering` của phương thức `cmp`.

Nếu `parse` có thể thành công trong việc chuyển chuỗi thành số, thì nó sẽ trả về một giá trị `Ok` có chứa con số kết quả. Giá trị `Ok` đó sẽ khớp với mẫu của nhánh đầu tiên và biểu thức `match` sẽ chỉ trả về giá trị `num` mà `parse` đã tạo và đặt bên trong giá trị `Ok`. Con số đó sẽ kết thúc đúng nơi chúng ta muốn trong biến `guess` mới mà chúng ta đang tạo.

Nếu `parse` *không thể* chuyển chuỗi thành số, nó sẽ trả về giá trị `Err` chứa thêm thông tin về lỗi. Giá trị `Err` không khớp với mẫu `Ok(num)` trong nhánh `match` đầu tiên, nhưng nó khớp với mẫu `Err(_)` trong nhánh thứ hai. Dấu gạch dưới, `_`, là một giá trị tổng quát; trong ví dụ này, chúng ta đang nói rằng chúng ta muốn kiểm tra tất cả các giá trị `Err`, bất kể chúng có thông tin gì bên trong chúng. Vì vậy chương trình sẽ thực thi mã của nhánh thứ hai, `continue`, yêu cầu chương trình chuyển sang bước lặp tiếp theo của `vòng lặp` và yêu cầu một lần đoán khác. Vì vậy, chương trình bỏ qua một cách hiệu quả tất cả các lỗi mà `parse` có thể gặp phải !

Giờ mọi thứ trong chương trình sẽ hoạt động như mong đợi. Hãy thử xem:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-05/
cargo run
(too small guess)
(too big guess)
foo
(correct guess)
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 4.45s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

Tuyệt vời! Với một tinh chỉnh nhỏ cuối cùng, chúng ta sẽ kết thúc trò chơi Dự đoán. Nhớ lại rằng chương trình vẫn đang in ra số bí mật. Điều đó tốt để kiểm nghiệm, nhưng nó làm hỏng trò chơi. Hãy xóa lệnh `println!` xuất ra số bí mật. Listing 2-6 hiển thị bản hoàn thiện của chương trình.

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-06/src/main.rs}}
```

<span class="caption">Listing 2-6: Chương trình hoàn chỉnh</span>

Đến đây, bạn đã xây dựng thành công trò chơi Dự đoán. Xin chúc mừng!

## Tóm tắt

Dự án này là một cách thực tế để giới thiệu cho bạn nhiều khái niệm Rust mới:
`let`, `match`, các hàm, cách sử dụng các crate bên ngoài, v.v. Ở phần tiếp theo trong một vài chương, bạn sẽ tìm hiểu chi tiết hơn về các khái niệm này. Chương 3 bao gồm các khái niệm mà hầu hết các ngôn ngữ lập trình đều có, chẳng hạn như các biến, các kiểu dữ liệu và các hàm, đồng thời chỉ ra cách sử dụng chúng trong Rust. Chương 4 khám phá quyền sở hữu, một tính năng làm cho Rust khác biệt với các ngôn ngữ khác. Chương 5 thảo luận về cấu trúc và cú pháp của phương thức, và Chương 6 giải thích cách thức hoạt động của enums.


[prelude]: ../std/prelude/index.html
[variables-and-mutability]: ch03-01-variables-and-mutability.html#variables-and-mutability
[comments]: ch03-04-comments.html
[string]: ../std/string/struct.String.html
[iostdin]: ../std/io/struct.Stdin.html
[read_line]: ../std/io/struct.Stdin.html#method.read_line
[result]: ../std/result/enum.Result.html
[enums]: ch06-00-enums.html
[expect]: ../std/result/enum.Result.html#method.expect
[recover]: ch09-02-recoverable-errors-with-result.html
[randcrate]: https://crates.io/crates/rand
[semver]: http://semver.org
[cratesio]: https://crates.io/
[doccargo]: http://doc.crates.io
[doccratesio]: http://doc.crates.io/crates-io.html
[match]: ch06-02-match.html
[shadowing]: ch03-01-variables-and-mutability.html#shadowing
[parse]: ../std/primitive.str.html#method.parse
[integers]: ch03-02-data-types.html#integer-types
