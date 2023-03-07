# Các Collection chung

Thư viện tiêu chuẩn của Rust bao gồm một số cấu trúc dữ liệu rất hữu ích được
gọi là *collections(các bộ sưu tập)*. Hầu hết các kiểu dữ liệu khác đại diện cho
một giá trị cụ thể, nhưng collection có thể chứa nhiều giá trị. Không giống như
các kiểu mảng và tuple được tích hợp sẵn, dữ liệu mà các collection này trỏ tới
được lưu trữ trên heap, có nghĩa là không cần biết lượng dữ liệu tại thời điểm
biên dịch và có thể tăng hoặc giảm khi chương trình chạy. Mỗi loại collection có
các khả năng và chi phí khác nhau và việc chọn một collection thích hợp cho tình
huống hiện tại của bạn là một kỹ năng mà bạn sẽ phát triển theo thời gian. Trong
chương này, chúng ta sẽ thảo luận về ba collection được sử dụng rất thường xuyên
trong các chương trình Rust:

* Một *vector* cho phép bạn lưu trữ một số lượng giá trị thay đổi cạnh nhau.
* Một *chuỗi* là một tập hợp các ký tự. Chúng tôi đã đề cập đến loại `String`
  trước đây, nhưng trong chương này, chúng tôi sẽ nói sâu hơn về nó.
* Một *hash map* cho phép bạn liên kết một giá trị với một khóa cụ thể. Đó là
  một triển khai cụ thể của cấu trúc dữ liệu tổng quát hơn được gọi là *map*.

Để tìm hiểu về các loại collection khác do thư viện chuẩn cung cấp, hãy xem [tài
liệu][collections].

Chúng ta sẽ thảo luận về cách tạo và cập nhật vectơ, chuỗi và hash map, cũng như
điều gì làm cho chúng trở nên đặc biệt.