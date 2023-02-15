## Cài đặt

Bước đầu tiên là cài đặt Rust. Chúng ta sẽ tải xuống Rust thông qua `rustup`, một
công cụ dòng lệnh để quản lý các phiên bản Rust và các công cụ liên quan. Bạn sẽ cần
kết nối internet để tải xuống.

> Lưu ý: Nếu bạn không muốn sử dụng `rustup` vì lý do nào đó, vui lòng xem
> [trang Phương pháp cài đặt Rust khác][otherinstall] để có thêm tùy chọn.

Các bước sau đây cài đặt phiên bản ổn định mới nhất của trình biên dịch Rust.
Sự đảm bảo về độ ổn định của Rust đảm bảo rằng tất cả các ví dụ trong cuốn sách
biên dịch sẽ tiếp tục biên dịch với các phiên bản Rust mới hơn. Đầu ra có thể
hơi khác nhau giữa các phiên bản vì Rust thường cải thiện các thông báo lỗi và
cảnh báo. Nói cách khác, mọi phiên bản Rust mới hơn, ổn định hơn mà bạn cài đặt bằng
các bước này sẽ hoạt động như mong đợi trong nội dung của cuốn sách này.

> ### Ký hiệu dòng lệnh
>
> Trong chương này và xuyên suốt cuốn sách, chúng tôi sẽ chỉ ra một số lệnh được sử dụng trong
> thiết bị đầu cuối. Tất cả các dòng mà bạn nên nhập trong thiết bị đầu cuối đều bắt đầu bằng `$`. 
> Bạn không cần gõ ký tự `$`; đó là dấu nhắc dòng lệnh được hiển thị cho cho biết điểm bắt đầu 
> của mỗi lệnh. Các dòng không bắt đầu bằng `$` thường hiển thị kết xuất đầu ra của lệnh trước đó. 
> Ngoài ra, dành riêng cho PowerShell các ví dụ sẽ sử dụng `>` thay vì `$`.

### Cài đặt `rustup` trên Linux hoặc macOS

Nếu bạn đang sử dụng Linux hoặc macOS, hãy mở terminal và nhập lệnh sau:

```console
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

Lệnh này tải xuống một script và bắt đầu cài đặt công cụ `rustup`, 
nó sẽ cài đặt phiên bản ổn định mới nhất của Rust. Bạn có thể được nhắc
để nhập mật khẩu của bạn. Nếu cài đặt thành công sẽ hiện ra dòng sau:

```text
Lúc này Rust đã được cài đặt. Tuyệt vời!
```
Bạn cũng sẽ cần một *linker*, là chương trình mà Rust sử dụng để nối kết quả đầu ra 
của trình biên dịch vào một tập tin. Có khả năng bạn đã có chương trình này. Nếu bạn 
nhận được thông báo lỗi trình liên kết, bạn nên cài đặt trình biên dịch C, trình biên dịch
này thường bao gồm một trình liên kết. Trình biên dịch C cũng hữu ích vì một số gói
Rust phổ biến phụ thuộc vào mã C và sẽ cần một trình biên dịch C.

Trên macOS, bạn có thể tải trình biên dịch C bằng cách chạy:

```console
$ xcode-select --install
```

Người dùng Linux nói chung nên cài đặt GCC hoặc Clang, theo chỉ dẫn của
phiên bản Linux họ đang sử dụng. Ví dụ, nếu bạn sử dụng Ubuntu, bạn có thể
cài đặt gói `build-essential`.

### Cài đặt `rustup` trên Windows

Trên Windows, hãy truy cập [https://www.rust-lang.org/tools/install][install] và làm theo
hướng dẫn cài đặt Rust. Tại một số thời điểm trong quá trình cài đặt, bạn sẽ
nhận được thông báo rằng bạn sẽ cần các công cụ *build* MSVC cho Visual Studio 2013 trở lên.

Để có được các công cụ *build*, bạn cần cài đặt [Visual Studio 2022][studio trực quan]. 
Khi được hỏi khối lượng công việc nào sẽ cài đặt, bao gồm:

* “Desktop Development with C++”
* The Windows 10 or 11 SDK
* The English language pack component, along with any other 
  language pack of your choosing

Phần còn lại của cuốn sách này sử dụng các lệnh hoạt động trong cả *cmd.exe* và PowerShell.
Nếu có sự khác biệt cụ thể, chúng tôi sẽ giải thích nên sử dụng cái nào.

### Xử lý sự cố

Để kiểm tra xem bạn đã cài đặt Rust đúng cách chưa, hãy mở shell và nhập cái này
đường kẻ:

```console
$ rustc --version
```

Bạn sẽ thấy số phiên bản, hàm băm của bản cập nhật và ngày cập nhật mới nhất
của phiên bản ổn định đã được phát hành, ở định dạng sau:

```text
rustc x.y.z (abcabcabc yyyy-mm-dd)
```

Nếu bạn thấy thông tin này, bạn đã cài đặt Rust thành công! Nếu bạn không
xem thông tin này, hãy kiểm tra xem Rust có trong biến hệ thống `%PATH%` của bạn không
theo hướng dẫn sau đây.

Trong Windows CMD, sử dụng:

```console
> echo %PATH%
```

Trong PowerShell, sử dụng:

```powershell
> echo $env:Đường dẫn
```

Trong Linux và macOS, hãy sử dụng:

```console
$ echo $PATH
```

Nếu tất cả đều đúng và Rust vẫn không hoạt động, có những chỗ bạn có thể 
tìm sự giúp đỡ. Tìm hiểu cách liên lạc với những Rustacean khác (một
biệt danh ngớ ngẩn mà chúng tôi tự gọi mình) trên [trang cộng đồng][cộng đồng].

### Cập nhật và Gỡ cài đặt

Khi Rust được cài đặt qua `rustup`, rất dễ dàng cho việc cập nhật lên phiên bản mới được phát hành. 
Từ shell của bạn, hãy chạy tập lệnh cập nhật sau:

```console
$ rustup update
```

Để gỡ cài đặt Rust và `rustup`, hãy chạy tập lệnh gỡ cài đặt sau từ
vỏ bọc:

```console
$ rustup self uninstall
```
### Tài liệu đi kèm

Việc cài đặt Rust cũng bao gồm một bản sao cục bộ của tài liệu để
bạn có thể đọc nó khi không kết nối mạng internet. Chạy `rustup doc` 
để mở tài liệu cục bộ trong trình duyệt của bạn.

Bất cứ khi nào một loại hoặc chức năng được cung cấp bởi thư viện tiêu chuẩn
và bạn không chắc nó dùng làm gì thì hãy mở tài liệu giao diện lập trình
ứng dụng (API) để tìm hiểu!

[otherinstall]: https://forge.rust-lang.org/infra/other-installation-methods.html
[install]: https://www.rust-lang.org/tools/install
[visualstudio]: https://visualstudio.microsoft.com/downloads/
[community]: https://www.rust-lang.org/community
