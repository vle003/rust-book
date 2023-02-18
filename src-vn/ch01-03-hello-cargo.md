## Chào Cargo !

Cargo là hệ thống xây dựng và quản lý gói của Rust. Hầu hết Rustaceans sử dụng công cụ này để quản lý các dự án Rust của họ vì Cargo xử lý rất nhiều việc cho bạn, chẳng hạn như xây dựng mã của bạn, tải xuống các thư viện mà mã của bạn sử dụng và xây dựng các thư viện đó. 
(Chúng tôi gọi các thư viện mà mã của bạn cần là *các phụ thuộc (dependencies)*.)

Chương trình Rust đơn giản nhất, giống như chương trình chúng ta đã viết, không có bất kỳ một phụ thuộc nào. Nếu chúng ta đã xây dựng "Hello, world!" dự án với Cargo, nó sẽ chỉ sử dụng phần Cargo xử lý việc xây dựng mã của bạn. Khi bạn viết các chương trình Rust phức tạp hơn, bạn sẽ thêm các phần phụ thuộc 
và nếu bạn bắt đầu một dự án bằng sử dụng Cargo, việc thêm các phụ thuộc sẽ dễ hơn nhiều.

Bởi vì phần lớn các dự án Rust sử dụng Cargo, phần còn lại của cuốn sách này giả định rằng bạn cũng sử dụng Cargo. Cargo được cài đặt với Rust nếu bạn đã sử dụng trình cài đặt chính thức được thảo luận trong phần [“Cài đặt”][cài đặt]<!-- bỏ qua -->. Nếu bạn đã cài đặt Rust thông qua một số phương thức khác, hãy kiểm tra xem Cargo đã được cài đặt hay chưa bằng cách nhập lệnh sau vào cửa sổ dòng lệnh của bạn:

```console
$ cargo --version
```

Nếu bạn thấy một số hiệu phiên bản, bạn đã có nó! Nếu bạn thấy lỗi, chẳng hạn như `command not found`, hãy xem tài liệu về cách cài đặt cho hệ thống của bạn để xác định cách cài Cargo riêng.

### Tạo một dự án với Cargo

Hãy tạo một dự án mới bằng cách sử dụng Cargo và xem nó khác với bản gốc dự án "Hello, world!" của chúng ta như thế nào. Hãy trở lại thư mục *projects* của bạn (hoặc bất cứ nơi nào bạn lưu trữ mã của mình). Sau đó, trên bất kỳ hệ điều hành nào, chạy các lệnh như sau:

```console
$ cargo new hello_cargo
$ cd hello_cargo
```
Lệnh đầu tiên tạo một thư mục và dự án mới có tên *hello_cargo*. Chúng ta đã đặt tên cho dự án của mình là *hello_cargo* và Cargo tạo các tệp của nó trong một thư mục cùng tên.

Vào thư mục *hello_cargo* và liệt kê các tệp. Bạn sẽ thấy Cargo đã tạo hai tệp và một thư mục cho chúng ta: tệp *Cargo.toml* và một thư mục *src* với tệp *main.rs* bên trong.

Nó cũng đã khởi tạo kho lưu trữ Git mới cùng với tệp *.gitignore*. Các tệp Git sẽ không được tạo nếu bạn chạy `cargo new` trong một kho lưu trữ Git hiện có; bạn có thể ghi đè hành vi này bằng cách sử dụng `cargo new --vcs=git`.

> Lưu ý: Git là một hệ thống kiểm soát phiên bản phổ biến. Bạn có thể 
> thay đổi `cargo new` thành sử dụng hệ thống kiểm soát phiên bản khác 
> hoặc không có hệ thống kiểm soát phiên bản nào bằng cách sử dụng tham
> số  `--vcs`. Chạy `cargo new --help` để xem các tùy chọn khả dụng.

Mở *Cargo.toml* trong trình soạn thảo văn bản của bạn. Nó sẽ trông giống như mã trong List 1-2.

<span class="filename">Filename: Cargo.toml</span>

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# Xem thêm các khóa và các định nghĩa của chúng tại https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

<span class="caption">Listing 1-2: Nội dung của *Cargo.toml* do `cargo new` tạo ra</span>

Tập tin này có định dạng [*TOML*][toml]<!-- ignore --> (*Tom’s Obvious, Minimal Language*), được sử dụng làm định dạng cấu hình của Cargo.

Dòng đâu tiên, `[package]`, là một đầu mục chỉ báo rằng các phát biểu theo là các cấu hình cho một gói. Khi chúng ta bổ sung thông tin vào tệp này, chúng ta sẽ bổ sung các thành các phân mục khác nhau.

Ba dòng kế tiếp là tập hợp các thông tin Cargo cần để biên dịch chương trình của bạn: tên chương trình, phiên bản chương trình, và phiên bản Rust được sử dụng để biên dịch. Chúng ta sẽ nói về khóa `edition` trong [Phụ lục E][appendix-e]<!-- ignore -->.

Dòng cuối cùng, `[dependencies]`, là nơi bắt đầu phân mục các thư viện phụ thuộc cho dự án của bạn. Trong Rust, các gói mã lệnh được tham chiếud đến như *crates*. Chúng ta không cần thêm crate nào cho dự án này, nhưng chúng ta sẽ cần đến khi làm dự án đầu tiên của Chương 2, khi đó chúng ta sẽ dùng đến phần khai báo các phụ thuộc này. Bây giờ hãy mở tệp *src/main.rs* và xem nội dung:

<span class="filename">Filename: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

Cargo đã tạo ra một chương trình "Hello, world!" cho bạn, giống như những gì chúng ta đã viết trong isting 1-1! Đến lúc này, sự khác biệt giữa hai dự án là Cargo đã đặt mã nguồn trong thư mục *src* và chúng ta có thêm một tệp tin *Cargo.toml* tại thư mục gốc của dự án.

Cargo muốn các tệp mã nguồn của bạn nằm trong thư mục *src*. Thư mục ở mức cao nhất của dự án chỉ chứa tệp README, thông tin bản quyền, các tệp tin cấu hình, và mọi thứ khác không phải mã lệnh. Sử dụng Cargo giúp bạn tổ chức các dự án của mình. Có một nơi cho mọi thứ, và mọi thứ luôn có ở cùng một nơi.

Nếu bạn bắt đầu một dự án mà không sử dụng Cargo, như chúng ta đã làm với dự án "Hello, world", bạn có thể chuyển nó thành dự án sử dụng Cargo. Chuyển mã lệnh vào thự mục con có tên *src* và tạo một tệp *Cargo.toml* tương ứng.

### Dựng và Chạy một dự án Cargo

Bây giờ hãy nhìn vào những gì khác biệt khi ta build và chạy chương trình "Hello, world!" với Cargo! Từ thư mục *hello_world* của bạn, build dự án bằng cách nhập lệnh sau vào cửa sổ dòng lệnh:

```console
$ cargo build
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 2.85 secs
```
Lệnh này tạo ra một tệp thực thi được và lưu vào thư mục *target/debug/hello_cargo* ) ( hoặc *target\debug\hello_cargo.exe* trên Windows ) thay vì lưu vào thự mục hiện hành. Vì việc dựng ngầm định là bản dựng có gỡ lỗi, do đó Cargo sẽ để tệp nhị phân trong thư mục con có tên *debug*. Bạn có thể chạy chương trình với lệnh:

```console
$ ./target/debug/hello_cargo 
Hello, world!

# hoặc trên Windows 
.\target\debug\hello_cargo.exe 
Hello, world!
```

Nếu mọi thứ hoạt động tốt, chuỗi `Hello, world!` sẽ hiện ra trên màn hình của sổ lệnh của bạn. Lần đầu tiên chạy `cargo build` sẽ tạo ra một tệp mới có tên: *Cargo.lock* tại thư mục gốc của dự án. Tệp tin này sẽ theo dõi và tách ra các phiên bản của các dependency trong dự án của bạn. Dự án này không có một dependency nào, vì vậy nội dung tệp này không có gì. Bạn không cần sửa tệp này thủ công; Cargo tự quản lý nội dung của tệp này cho bạn.

Chúng ta đã build một dự án với `cargo build` và chạy nó với lệnh `./target/debug/hello_cargo`, nhưng chúng ta cũng có thể dùng lệnh `cargo run` để biên dịch mã và chạy chương trình được tạo ra sau khi biên dịch

```console
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Việc sử dụng `cargo run` tiện lợi hơn việc sử dụng `cargo build` cùng toàn bộ đường dẫn của tệp nhị phân, vì thế hầu hết các lập trình viên thích sử dụng `cargo run`

Lưu ý rằng lần này chúng ta không thấy Cargo xuất ra kết quả thông báo rằng đang biên dịch `hello_cargo`. Cargo đã hiểu rằng các tệp tin không bị thay đổi, do đó nó không biên dịch lại mà chỉ chạy tệp nhị phân. Nếu bạn sửa đổi mã nguồn, Cargo sẽ biên dịch lại dự án trước khi chạy, và bạn sẽ thấy thông báo dạng như sau:

```console
$ cargo run
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.33 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Cargo cung cấp một lệnh là `cargo check`. Lệnh này kiểm tra nhanh mã lệnh của bạn để bảo đảm rằng mã không có lỗi biên dịch nhưng không tạo ra tệp nhị phân:

```console
$ cargo check
   Checking hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.32 secs
```

Tại sao bạn không muốn một tệp nhị phân thực thi được? Thông thường, `cargo check` nhanh hơn `cargo build` rất nhiều vì nó bỏ qua bước tạo file thực thi. Nếu bạn thường xuyên kiểm tra kết quả trong khi lập trình, dùng `cargo check` sẽ tăng tốc độ làm việc và bạn sẽ biết dự án của bạn không chứa lỗi biên dịch! Do đó, nhiều Rustaceans thường chạy `cargo check` khi họ viết chương trình để bảo đảm không có lỗi biên dịch. Họ sẽ chạy `cargo build` khi họ sẵn sàng dùng chương trình thực thi được.

Cùng tóm lược lại những gì chúng ta đã học về Cargo:

* Chúng ta có thể tạo một dự án bằng `cargo new`
* Chúng ta có thể build một dự án bằng `cargo build`
* Chúng ta có thể build và chạy mộ dự án với một động tác dùng `cargo run`
* Chúng ta có thể build một dự mà không kết sinh ra mã nhị phân để kiểm tra lỗi bằng cách sử dụng `cargo check`
* Thay vì lưu kết quả của build vào cùng một thư mục với mã nguồn, Cargo lưu kết quả vào thư mục *target/debug*.

Thêm một ích lợi của việc sử dụng Cargo là các lệnh giống nhau trên mọi hệ điều hành bạn sử dụng. Vì vậy, tại thời điểm này, chúng tôi sẽ không cung cấp các chỉ dẫn đạc thù cho từng hệ điều hành riêng biệt như Linux, MacOS hay Windows.

### Dựng để Xuất bản

Khi dự án của bạn đã sẵn sàng cho bước cuối cùng, bạn có thể sử dụng `cargo build --release` để biên dịch nó với các tối ưu hóa. Lệnh này sẽ tạo một tệp thực thi được trong thư mục *target/releas* thay vì *target/debug*. Việc tối ưu hóa mã sẽ làm chương trình Rust của bạn chạy nhanh hơn, nhưng sẽ khiến thời gian biên dịch tăng lên. Đây là nguyên do có hai cấu hình khác nhau: một cho quá trình phát triển, là quá trình mà bạn thường xuyên tái biên dịch nhanh, và một cho việc tạo ra chương trình đã hoàn thiện, là phiên bản chạy càng nhanh càng tốt, không thường xuyên biên dịch lại và bạn sẽ cung cấp cho người dùng. Nếu bạn kiểm định thời gian chạy của chương trình, hãy chắc chắn rằng bạn dùng lệnh `cargo run --release` và đoánh giá nó với phiên bản trong thư mục *target/release*

### Cargo như là Quy chuẩn

Với các dự án đơn giản, Cargo không mang lại quá nhiều giá trị so với việc chỉ sử dụng `rustc`, nhưng nó sẽ chứng minh giá trị của mình khi các chương trình của bạn phức tạp hơn. Một khi các chương trình phình to ra nhiều tệp tin hoặc cần thêm các phụ thuộc, nó sẽ dễ hơn rất nhiều khi để Cargo điều phối bản dựng.

Mặc dù dự án `hello_cargo` rất đơn giản, nó thực sự đang dùng các công cụ bạn sẽ dùng trong suốt hành trình Rust của bạn sau này. Trong thực tế, để làm việc trên bất kỳ dự án hiện hữu nào, bạn có thể dùng các lệnh sau để lấy mã nguồn từ Git, thay đổi thư mục của dự án và dựng nó:

```console
$ git clone example.org/someproject
$ cd someproject
$ cargo build
```

Để có thêm thông tin về Cargo, xin xem ở phần [tài liệu của nó][cargo].

## Tổng kết

Bạn đã sẵn sàng cho một bước tiến mạnh mẽ trên hành trình Rust của mình ! Trong chương này, bạn đã học cách:

* Cài đặt phiên bản ổn định mới nhất của Rust với `rustup`
* Cập nhật lên phiên bản Rust mới
* Mở các tài liệu hướng dẫn đi kèm
* Viết và chạy chương trình "Hello, world!" bằng sử dụng `rustc`
* Tạo và chạy một dự án mới bằng việc sử dụng Cargo quy chuẩn

Đây là lúc tuyệt vời để xây dựng nhiều chương trình hơn để thành thạo hơn trong việc đọc và viết mã lệnh Rust. Do đó, trong Chương 2, chúng ta sẽ xây dựng một trò chơi giải đố. Nếu bạn muốn bắt đầu bằng cách tìm hiểu cách các khái niệm lập trình phổ biến hoạt động trong Rust, hãy xem Chương 3 rồi quay lại Chương 2.

[installation]: ch01-01-installation.html#installation
[toml]: https://toml.io
[appendix-e]: appendix-05-editions.html
[cargo]: https://doc.rust-lang.org/cargo/
