## Packaghe và Crate

Các phần đầu tiên của hệ thống mô-đun mà chúng tôi sẽ giới thiệu là các packacge
và crate. Một *crate* là lượng mã nhỏ nhất mà trình biên dịch Rust xem xét tại
một thời điểm. Ngay cả khi bạn chạy `rustc` chứ không phải `cargo` và chuyển một
tệp mã nguồn duy nhất (như chúng ta đã làm từ đầu trong phần *“Viết và chạy
chương trình Rust”* ở  Chương 1), trình biên dịch sẽ coi tệp đó là một *crate*.
Các crate (crate) có thể chứa các mô-đun và các mô-đun này có thể được xác định
trong các tệp khác được biên dịch cùng với crate, như chúng ta sẽ thấy trong các
phần tiếp theo.

Một crate có thể có một trong hai dạng: `nhị phân` hoặc `thư viện`. *Crate nhị
phân* là các chương trình bạn có thể biên dịch thành tệp thực thi mà bạn có thể
chạy, chẳng hạn như chương trình chạy từ dòng lệnh hoặc phần mềm máy chủ. Crate
nhị phân phải có một hàm có tên là `main` định nghĩa điều gì sẽ xảy ra khi tệp
thực thi chạy. Tất cả các crate chúng ta đã tạo cho đến nay đều là crate nhị
phân.

*Crate thư viện* không có hàm `main` và chúng không biên dịch thành tệp thực
thi. Thay vào đó, chúng chứa các dạng khai báo hàm được chia sẻ với nhiều dự án.
Ví dụ: crate `rand` mà chúng ta đã sử dụng trong [Chương 2][Rand]<!-- ignore -->
cung cấp chức năng tạo số ngẫu nhiên. Đa số khi các Rustaceans nói "crate", có
nghĩa là họ nói về crate thư viện và họ sử dụng "*crate*" thay thế cho khái niệm
chung về "*thư viện*" trong lập trình.

*Crate root* là một tệp nguồn mà trình biên dịch Rust dùng nó như nơi bắt đầu để
tạo nên mô-đun gốc cho crate của bạn (chúng tôi sẽ giải thích sâu về các mô-đun
trong phần [“Xác định mô-đun để kiểm soát phạm vi và quyền riêng
tư”][modules]<!-- ignore --> ).

Một *package* là một nhóm gồm một hoặc nhiều crate cung cấp một hay nhiều chức
năng. Một package chứa tệp *Cargo.toml* mô tả cách thức tạo các crate đó. Bản
chất Cargo chính là một package chứa crate nhị phân cho công cụ dòng lệnh mà bạn
đang sử dụng để xây dựng mã của mình. Package Cargo cũng chứa một crate thư viện
mà crate nhị phân phụ thuộc vào. Các dự án khác có thể phụ thuộc vào crate thư
viện Cargo để sử dụng logic tương tự mà Cargo sử dụng.

Một package có thể chứa bao nhiêu crate nhị phân tùy thích, nhưng nhiều nhất chỉ
có một crate thư viện. Một package phải chứa ít nhất một crate, cho dù đó là thư
viện hay crate nhị phân.

Hãy xem điều gì sẽ xảy ra khi chúng ta tạo một package. Đầu tiên, chúng ta nhập
lệnh `cargo new`:

```console
$ cargo new my-project
     Created binary (application) `my-project` package
$ ls my-project
Cargo.toml
src
$ ls my-project/src
main.rs
```

Sau khi chúng ta chạy `cargo new`, chúng ta sử dụng `ls` để xem những gì Cargo
tạo ra. Trong thư mục dự án, có một tệp *Cargo.toml*, cung cấp cho chúng ta một
package. Ngoài ra còn có một thư mục *src* chứa tệp *main.rs*. Mở *Cargo.toml*
trong trình soạn thảo văn bản của bạn và ta thấy nó không đề cập đến
*src/main.rs*. Cargo tuân theo một quy ước rằng *src/main.rs* là crate gốc của
một crate nhị phân có cùng tên với package. Tương tự như vậy, Cargo biết rằng,
nếu trong thư mục của package chứa *src/lib.rs*, thì package đó chứa một crate
thư viện có cùng tên với package và *src/lib.rs* là crate gốc. Cargo chuyển các
tệp crate gốc sang `rustc` để dựng thư viện hoặc tệp nhị phân.

Ở đây, chúng ta có một package chỉ chứa *src/main.rs*, nghĩa là nó chỉ chứa một
crate nhị phân có tên `my-project`. Nếu một package chứa *src/main.rs* và
*src/lib.rs*, package đó có hai crate: một nhị phân và một thư viện, cả hai đều
có cùng tên với package. Một package có thể có nhiều crate nhị phân bằng cách
đặt các tệp trong thư mục *src/bin*: mỗi tệp sẽ là một crate nhị phân riêng
biệt.

[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number