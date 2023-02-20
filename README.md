# Ngôn ngữ lập trình Rust

![Build Status](https://github.com/rust-lang/book/workflows/CI/badge.svg)

Kho lưu trữ này chứa mã nguồn của cuốn sách "Ngôn ngữ lập trình Rust".

[Cuốn sách có sẵn ở dạng dead-tree từ nhà xuất bản No Starch Press][nostarch].

[nostarch]: https://nostarch.com/rust-programming-language-2nd-edition

Bạn cũng có thể đọc trực tuyến miễn phí. Xin hãy đọc các ấn bản được phát hành
cùng với các bản phát hành Rust [stable] , [beta] hoặc [nightly] mới nhất. Xin
lưu ý rằng những vấn đề trong các phiên bản đó có thể đã được khắc phục trong
kho lưu trữ này vì các bản phát hành ít thường xuyên cập nhật hơn nội dung cuốn
sách này.

[stable]: https://doc.rust-lang.org/stable/book/
[beta]: https://doc.rust-lang.org/beta/book/
[nightly]: https://doc.rust-lang.org/nightly/book/

Xem [Release][releases] để tải xuống chỉ phần mã lệnh của tất cả các danh sách mã xuất hiện trong sách.

[releases]: https://github.com/rust-lang/book/releases

## Các yêu cầu để nhận được cuốn sách hoàn chỉnh

Dể hoàn thành việc dàn trang cho cuốn sách, bạn cần [mdBook], tốt nhất là phiên bản giống với phiên bản mà rust-lang/rust sử dụng trong [tệp này][rust-mdbook]. Để tải về, dùng đường dẫn dưới đây:

[mdBook]: https://github.com/rust-lang-nursery/mdBook
[rust-mdbook]: https://github.com/rust-lang/rust/blob/master/src/tools/rustbook/Cargo.toml

```bash
$ cargo install mdbook --version <version_num>
```

## Dàn trang

Để dàn trang hoàn chỉnh, gõ lệnh sau:

```bash
$ mdbook build
```

Kết quả sẽ được lưu trong thư mục con có tên `book`. Để kiểm tra, hãy mở nó trong trình duyệt của bạn.

_Firefox:_
```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_
```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

Để kiểm tra lỗi:

```bash
$ mdbook test
```

## Hỗ trợ

Chúng tôi cần sự giúp đỡ của các bạn! Vui lòng xem [CONTRIBUTING.md][contrib] để
tìm hiểu về các công tác hỗ trợ mà chúng tôi cần.

[contrib]: https://github.com/rust-lang/book/blob/main/CONTRIBUTING.md

Vì cuốn sách được [in ra giấy][nostarch] và vì chúng tôi muốn giữ phiên bản trực
tuyến của cuốn sách gần với ấn bản giấy nhất có thể nên chúng tôi có thể cần
nhiều thời gian hơn bình thường để giải quyết các vấn đề hoặc yêu cầu của bạn.

Cho đến nay, chúng tôi đã thực hiện một bản sửa đổi lớn để đồng bộ với [Phiên
bản Rust](https://doc.rust-lang.org/edition-guide/). Giữa các lần sửa đổi nội
dung lớn, chúng tôi sẽ chỉ sửa lỗi. Nếu vấn đề hoặc yêu cầu của bạn không được
khắc phục một cách triệt để, vấn đề đó có thể tồn tại cho đến lần tiếp theo khi
chúng tôi thực hiện một bản sửa đổi lớn: việc này dự kiến sẽ diễn ra theo thứ tự
hàng tháng hoặc hàng năm. Cảm ơn vì sự kiên nhẫn của bạn!

### Các bản dịch

Chúng tôi rất vui khi được giúp đỡ để dịch cuốn sách! Xem nhãn
[Translations][Translations] để tham gia vào các nỗ lực dịch thuật hiện đang
được tiến hành. Để bắt đầu dịch sang một ngôn ngữ mới, hãy tạo một vấn đề
mới! Chúng tôi đang chờ hỗ trợ của [mdbook][mdbook support] cho cơ chế đa ngôn
ngữ trước khi chúng tôi hợp nhất bất kỳ ngôn ngữ nào vào, nhưng bạn cứ thoải mái
mà bắt đầu!

[Translations]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations
[mdbook support]: https://github.com/rust-lang-nursery/mdBook/issues/5

## Kiểm tra chính tả

Để quét các tập tin nguồn để tìm lỗi chính tả, bạn có thể sử dụng tập lệnh
`spellcheck.sh` có sẵn trong thư mục `ci`. Nó cần một từ điển chứa các từ hợp
lệ, được cung cấp trong `ci/dictionary.txt`. Nếu tập lệnh tạo ra kết quả dương
tính giả (giả sử bạn đã sử dụng từ `BTreeMap` mà tập lệnh coi là không hợp lệ),
thì bạn cần thêm từ này vào `ci/dictionary.txt` (giữ nguyên thứ tự được sắp xếp
để thống nhất).