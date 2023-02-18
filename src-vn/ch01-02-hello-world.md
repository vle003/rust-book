## Hello, World!

Giờ đây, khi bạn đã cài đặt Rust, chính là lúc để viết chương trình Rust đầu tiên của bạn. Nó đã thành truyền thống khi học một ngôn ngữ lập trình mới là viết một chương trình nhỏ và in dòng chữ `Hello, world!` ra màn hình, do đó chúng ta cũng sẽ làm điều đó ở đây!

> Ghi chú: Cuốn sách này giả định sự quen thuộc cơ bản với dòng lệnh. Rust không có 
> yêu cầu cụ thể nào về bộ soạn thảo hay công cụ hay vị trí mã của bạn, vì vậy
> nếu bạn muốn sử dụng môi trường phát triển tích hợp (IDE) thay vì dòng lệnh,
> cứ thoải mái sử dụng IDE yêu thích của bạn. Nhiều IDE hiện có một số
> mức độ hỗ trợ Rust; kiểm tra tài liệu của IDE để biết chi tiết. Đội ngũ phát triển Rust
> đã và đang tập trung vào việc kích hoạt các IDE lớn hỗ trợ  thông qua `rust-analyzer`.
> Xem [Phụ lục D][devtools]<!-- ignore --> để biết thêm chi tiết.

### Tạo thư mục dự án

Bạn sẽ bắt đầu bằng cách tạo một thư mục để lưu mã Rust của mình. Rust không quan trọng nơi mã của bạn tồn tại, nhưng đối với các bài tập và dự án trong cuốn sách này, chúng tôi khuyên bạn nên tạo một thư mục *pojects* trong thư mục chính của bạn và giữ tất cả các dự án của bạn ở đó.

Mở terminal và nhập các lệnh sau để tạo thư mục *projects* và một thư mục cho dự án "Hello, world!" trong thư mục *projects*.

Đối với Linux, macOS và PowerShell trên Windows, hãy nhập:

```console
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

Đói với Windows CMD, hãy nhập:

```cmd
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

### Viết và chạy chương trình Rust

Tiếp theo, tạo một tệp nguồn mới và gọi nó là *main.rs*. Các tệp Rust luôn kết thúc bằng phần mở rộng *.rs*. Nếu bạn đang sử dụng nhiều hơn một từ trong tên tệp của mình, quy ước là sử dụng dấu gạch dưới để phân tách chúng. Ví dụ, sử dụng *hello_world.rs* thay vì *helloworld.rs*.

Bây giờ hãy mở tệp *main.rs* mà bạn vừa tạo và nhập mã vào như List 1-1.

<span class="filename">Filename: main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

<span class="caption">Listing 1-1: A program that prints `Hello, world!`</span>

Ghi lại thay đổi và trở về cửa sổ dòng lệnh trong thư mục *~/projects/hello_world*. Trong Linux hoặc macOS, nhập vào lệnh sau để biên dịch và chạy chương trình:

```console
$ rustc main.rs
$ ./main
Hello, world!
```

On Windows, enter the command `.\main.exe` instead of `./main`:

```powershell
> rustc main.rs
> .\main.exe
Hello, world!
```

Bất kể hệ điều hành của bạn là gì, chuỗi `Hello, world!` sẽ in ra cửa sổ dòng lệnh. Nếu bạn không thấy kết quả này, hãy tham khảo lại phần [“Khắc phục sự cố”][troubleshooting]<!-- bỏ qua --> trong chương Cài đặt
để tìm các sự trợ giúp.

Nếu `Hello, world!` hiển thị trên cửa sổ, xin chúc mừng! Bạn đã chính thức viết
một chương trình Rust. Điều đó khiến bạn trở thành một lập trình viên Rust—xin chào mừng!

### "Giải phẫu" một chương trình Rust

Hãy xem lại chương trình "Hello, world!" một cách chi tiết. Đây là phần đầu tiên của mảnh ghép:

```rust
fn main() {

}
```

Những dòng này khai báo một hàm có tên `main`. Chức năng của `main` rất đặc biệt: nó luôn là mã đầu tiên chạy trong mọi chương trình Rust có thể thực thi được. Ở đây, dòng đầu tiên khai báo một hàm có tên `main` không có tham số và không trả về kết quả gì. Nếu có tham số, chúng sẽ nằm trong dấu ngoặc đơn `()`.

Thân hàm được gói trong cặp kí tự `{}`. Rust yêu cầu dấu ngoặc nhọn xung quanh tất cả các hàm. Một phong cách tốt là đặt dấu ngoặc nhọn mở trên cùng một dòng với khai báo hàm, thêm một khoảng trắng ở giữa.

> Lưu ý: Nếu bạn muốn sử dụng một phong cách tiêu chuẩn trong các dự án Rust, bạn có thể
> sử dụng công cụ định dạng tự động có tên `rustfmt` để định dạng mã của bạn theo phong cách 
> định dạng cụ thể (xem thêm về `rustfmt` trong [Phụ lục D][devtools]<!-- bỏ qua -->). 
> Nhóm phát triển Rust đã bao gồm công cụ này với bản phân phối Rust tiêu chuẩn, cũng như `rustc`,
> vì vậy nó đã được cài đặt trên máy tính của bạn!

Phần thân của hàm `main` chứa đoạn mã sau:

```rust
    println!("Hello, world!");
```

Dòng này thực hiện mọi công việc trong chương trình nhỏ này: nó in văn bản ra màn hình. Có bốn chi tiết quan trọng cần chú ý ở đây.

Đầu tiên, phong cách Rust là thụt lề với bốn dấu cách, không phải tab.

Thứ hai, `println!` gọi một macro Rust. Thay vào đó, nếu nó đã gọi một hàm, thì nó sẽ được viết là `println` (không có `!`). Chúng ta sẽ thảo luận chi tiết hơn về các macro Rust trong Chương 19. Hiện tại, bạn chỉ cần biết rằng sử dụng `!` có nghĩa là bạn đang gọi một macro thay vì một hàm bình thường và các macro đó không phải lúc nào cũng tuân theo các quy tắc giống như hàm.

Thứ ba, bạn thấy chuỗi `"Hello, world!"`. Chúng tôi chuyển chuỗi này như đối số cho `println!`, và chuỗi được in ra màn hình.

Thứ tư, chúng tôi kết thúc dòng bằng dấu chấm phẩy (`;`), điều này biểu thị rằng biểu thức đã kết thúc và biểu thức tiếp theo đã sẵn sàng để bắt đầu. 
Hầu hết các dòng mã Rust kết thúc bằng dấu chấm phẩy

### Biên dịch và chạy là các bước riêng biệt

Bạn vừa chạy một chương trình mới tạo, vì vậy hãy xem xét từng bước trong quá trình.

Trước khi chạy một chương trình Rust, bạn phải biên dịch nó bằng trình biên dịch Rust bằng cách nhập lệnh `rustc` và chuyển tên tệp nguồn của bạn, chẳng hạn như thế này:

```console
$ rustc main.rs
```

Nếu bạn đã có kinh nghiệm với C hoặc C++, bạn sẽ nhận thấy rằng điều này tương tự như `gcc` hoặc `clang`. Sau khi biên dịch thành công, Rust xuất ra tệp thực thi nhị phân.

Trên Linux, macOS và PowerShell trên Windows, bạn có thể xem tệp thực thi bằng cách nhập lệnh `ls` trong shell của bạn:

```console
$ ls
main  main.rs
```
Trên Linux và macOS, bạn sẽ thấy hai tệp. Với PowerShell trên Windows, bạn sẽ xem ba tệp giống như bạn sẽ thấy bằng CMD. Với CMD trên Windows, bạn sẽ nhập như sau:

```cmd
> dir /B %= the /B option says to only show the file names =%
main.exe
main.pdb
main.rs
```

Điều này hiển thị tệp mã nguồn có phần mở rộng *.rs*, tệp thực thi (*main.exe* trên Windows, nhưng chỉ có *main* trên tất cả các nền tảng khác), và khi sử dụng Windows, có thêm một tệp chứa thông tin gỡ lỗi có phần mở rộng *.pdb*. Từ đây, bạn chạy tệp *main* hoặc *main.exe*, như sau:

```console
$ ./main # or .\main.exe on Windows
```
Nếu *main.rs* của bạn là chương trình “Hello, world!”, dòng này in ra `Hello, world!` trên cửa sổ dòng lệnh của bạn.

Nếu bạn quen thuộc hơn với một ngôn ngữ động, chẳng hạn như Ruby, Python hoặc JavaScript, bạn có thể không quen với việc biên dịch và chạy một chương trình như các bước riêng biệt. Rust là ngôn ngữ *được biên dịch trước*, nghĩa là bạn có thể biên dịch chương trình và đưa tệp thực thi cho người khác và họ có thể chạy 
chương trình đó ngay cả khi chưa cài đặt Rust. Nếu bạn cho ai đó *.rb*, *.py* hoặc *.js*, trước tiên họ cần cài Ruby, Python hoặc JavaScript (tương ứng). Nhưng trong những ngôn ngữ đó, bạn chỉ cần một lệnh để biên dịch và chạy chương trình của bạn. Tất cả những điều này là một sự đánh đổi trong thiết kế của ngôn ngữ.

Chỉ cần biên dịch với `rustc` là đủ cho các chương trình đơn giản, nhưng với dự án của bạn phình to lên, bạn sẽ muốn quản lý tất cả các tùy chọn và giúp dễ dàng chia sẻ mã. Tiếp theo, chúng tôi sẽ giới thiệu cho bạn công cụ Cargo, công cụ này sẽ giúp bạn viết các chương trình Rust thực thụ.

[troubleshooting]: ch01-01-installation.html#troubleshooting
[devtools]: appendix-04-useful-development-tools.md
