# Hướng dẫn mẫu

## Văn xuôi

* Ưu tiên viết hoa tiêu đề cho tiêu đề chương/phần, ví dụ: `## Tạo số bí mật` hơn là `## Tạo số bí mật`.
* Ưu tiên chữ in nghiêng hơn dấu nháy đơn khi gọi một thuật ngữ, ví dụ: `là một *hàm liên kết* của` thay vì `là một 'hàm liên kết' của`.
* Khi nói về một phương thức trong văn xuôi, KHÔNG bao gồm dấu ngoặc đơn, ví dụ: `read_line` thay vì `read_line()`.
* Bọc cứng ở 80 ký tự
* Không muốn trộn mã và không mã trong một từ, ví dụ: ``Hãy nhớ khi chúng ta viết `use std::io`?`` hơn là ``Hãy nhớ khi chúng ta `use`d `std::io`?` `

## Mã số

* Thêm tên tệp trước các khối đánh dấu để làm rõ chúng ta đang nói về tệp nào, khi áp dụng.
* Khi thay đổi mã, hãy làm rõ phần nào của mã đã thay đổi và phần nào giữ nguyên... chưa biết cách thực hiện
* Chia nhỏ các dòng dài phù hợp để giữ chúng dưới 80 ký tự nếu có thể
* Sử dụng đánh dấu cú pháp `bash` cho các khối mã đầu ra dòng lệnh

## Liên kết

Khi tất cả các tập lệnh được thực hiện:

* Nếu một liên kết không cần in, hãy đánh dấu nó để bỏ qua
   * Điều này bao gồm tất cả các liên kết nội bộ của "Chương XX", mà *nên* là các liên kết dành cho phiên bản HTML
* Đặt liên kết trong sách và liên kết tài liệu API stdlib tương đối để chúng hoạt động cho dù sách được đọc ngoại tuyến hay trên docs.rust-lang.org
* Sử dụng các liên kết đánh dấu và lưu ý rằng chúng sẽ được thay đổi thành `text at *url*` trong bản in, vì vậy hãy diễn đạt chúng theo cách mà nó đọc tốt ở định dạng đó