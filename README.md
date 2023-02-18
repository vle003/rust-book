# Ngôn ngữ lập trình Rust

![Build Status](https://github.com/rust-lang/book/workflows/CI/badge.svg)

Kho lưu trữ này chứa mã nguồn của cuốn sách "Ngôn ngữ lập trình Rust".

[Cuốn sách có sẵn ở dạng dead-tree từ nhà xuất bản No Starch Press][nostarch].

[nostarch]: https://nostarch.com/rust-programming-language-2nd-edition

Bạn cũng có thể đọc cuốn sách miễn phí trực tuyến. Vui lòng xem sách được phát hành cùng với các bản phát hành Rust [stable] , [beta] hoặc [nightly] mới nhất. Xin lưu ý rằng các sự cố trong các phiên bản đó có thể đã được khắc phục trong kho lưu trữ này vì các bản phát hành đó được cập nhật ít thường xuyên hơn.

[stable]: https://doc.rust-lang.org/stable/book/
[beta]: https://doc.rust-lang.org/beta/book/
[nightly]: https://doc.rust-lang.org/nightly/book/

Xem [Release] để tải xuống chỉ phần mã lệnh của tất cả các danh sách mã xuất hiện trong sách.

[releases]: https://github.com/rust-lang/book/releases

## Requirements

Dể dựng sách hoàn chỉnh, cần có [mdBook], tốt nhất là phiên giống với phiên bản mà 
rust-lang/rust sử dụng trong tệp này [this file][rust-mdbook]. Để tải về:

[mdBook]: https://github.com/rust-lang-nursery/mdBook
[rust-mdbook]: https://github.com/rust-lang/rust/blob/master/src/tools/rustbook/Cargo.toml

```bash
$ cargo install mdbook --version <version_num>
```

## Dựng

Để dựng sách hoàn chỉnh, gõ lệnh sau:

```bash
$ mdbook build
```

Kết quả sẽ được lưu trong thư mục con có tên `book`. Để kiểm tra, mở nó trong trình duyệt của bạn.

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

Để kiểm tra thử :

```bash
$ mdbook test
```

## Đóng góp

Chúng tôi muốn sự giúp đỡ của bạn! Vui lòng xem [CONTRIBUTING.md][contrib] để tìm hiểu về các loại đóng góp mà chúng tôi đang tìm kiếm.

[contrib]: https://github.com/rust-lang/book/blob/main/CONTRIBUTING.md

Vì cuốn sách được [in][nostarch] và vì chúng tôi muốn giữ phiên bản trực tuyến của cuốn sách gần với phiên bản in nhất có thể nên chúng tôi có thể mất nhiều thời gian hơn bình thường để giải quyết vấn đề hoặc yêu cầu của bạn.

Cho đến nay, chúng tôi đã thực hiện một bản sửa đổi lớn hơn để trùng với [Phiên bản Rust](https://doc.rust-lang.org/edition-guide/). Giữa các bản sửa đổi lớn hơn đó, chúng tôi sẽ chỉ sửa lỗi. Nếu vấn đề hoặc yêu cầu của bạn không khắc phục được lỗi một cách triệt để, lỗi đó có thể tồn tại cho đến lần tiếp theo khi chúng tôi thực hiện một bản sửa đổi lớn: dự kiến sẽ diễn ra theo thứ tự hàng tháng hoặc hàng năm. Cảm ơn vì sự kiên nhẫn của bạn!

### Các bản dịch

We'd love help translating the book! See the [Translations] label to join in
efforts that are currently in progress. Open a new issue to start working on
a new language! We're waiting on [mdbook support] for multiple languages
before we merge any in, but feel free to start!

Chúng tôi rất muốn được giúp đỡ dịch cuốn sách! Xem nhãn [Translation] để tham gia vào các nỗ lực hiện đang được tiến hành. Mở một vấn đề mới để bắt đầu làm việc với một ngôn ngữ mới! Chúng tôi đang chờ hỗ trợ của [mdbook][mdbook support] cho nhiều ngôn ngữ trước khi chúng tôi hợp nhất bất kỳ ngôn ngữ nào vào, nhưng bạn cứ thoải mái bắt đầu!

[Translations]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations
[mdbook support]: https://github.com/rust-lang-nursery/mdBook/issues/5

## Kiểm tra chính tả

Để quét các tập tin nguồn để tìm lỗi chính tả, bạn có thể sử dụng tập lệnh `spellcheck.sh` có sẵn trong thư mục `ci`. Nó cần một từ điển chứa các từ hợp lệ, được cung cấp trong `ci/dictionary.txt`. Nếu tập lệnh tạo ra kết quả dương tính giả (giả sử bạn đã sử dụng từ `BTreeMap` mà tập lệnh coi là không hợp lệ), thì bạn cần thêm từ này vào `ci/dictionary.txt` (giữ nguyên thứ tự được sắp xếp để thống nhất).
