# Quản lý các Dự án đang phình ra bằng Package, Crate và Module (Mô-đun)

Khi bạn viết các chương trình lớn, việc tổ chức mã của bạn sẽ ngày càng trở nên
quan trọng. Bằng cách nhóm các chức năng liên quan và tách mã với các tính năng
riêng biệt, bạn sẽ làm rõ vị trí tìm mã triển khai một tính năng cụ thể và nơi
cần đến để thay đổi cách thức hoạt động của một tính năng.

Các chương trình chúng ta đã viết cho đến nay chỉ có một mô-đun nằm trong một
tệp. Khi một dự án phát triển, bạn nên tổ chức mã bằng cách chia nó thành nhiều
mô-đun và sau đó là nhiều tệp. Một *package* có thể chứa nhiều *crate* nhị phân
và một *crate* thư viện tùy chọn. Khi một *package* phình to ra, bạn có thể
trích các phần vào các *crate* riêng biệt để trở thành các phần phụ thuộc bên
ngoài. Chương này bao gồm tất cả các kỹ thuật này. Đối với các dự án rất lớn bao
gồm một tập hợp các *package* có liên quan với nhau và phát triển cùng nhau,
Cargo cung cấp *không gian làm việc* mà chúng tôi sẽ đề cập trong phần [“Không
gian làm việc Cargo”][workspaces]<!-- bỏ qua --> trong Chương 14.

Chúng ta cũng sẽ thảo luận chi tiết việc triển khai đóng gói, điều này cho phép
bạn tái sử dụng mã ở cấp độ cao hơn: sau khi bạn đã triển khai một thao tác, mã
khác có thể gọi mã của bạn thông qua giao diện chung của nó, mà nó không cần
biết chi tiết cách thức bạn triển khai hoạt động cho mã của mình như thế nào.
Cách bạn viết mã xác định phần nào là công khai để mã khác sử dụng và phần nào
là  triển khai chi tiết riêng tư mà bạn giữ quyền thay đổi. Đây là một cách khác
để hạn chế số lượng các chi tiết bạn phải ghi nhớ trong đầu.

Một khái niệm liên quan là phạm vi: trong một ngữ cảnh lồng nhau, trong đó mã
được viết có một tập hợp các tên được định nghĩa là có hiệu lực “trong phạm vi”.
Khi đọc, viết và biên dịch mã, lập trình viên và trình biên dịch cần biết liệu
một tên cụ thể tại một vị trí cụ thể có đề cập đến một biến, hàm, cấu trúc,
enum, mô-đun, hằng hoặc mục khác hay không và mục đó có nghĩa là gì. Bạn có thể
tạo phạm vi và thay đổi tên nào nằm trong hoặc ngoài phạm vi. Bạn không thể có
hai mục có cùng tên trong cùng một phạm vi;  có sẵn các công cụ để giải quyết
xung đột tên.

Rust có một số tính năng cho phép bạn quản lý tổ chức mã của mình, bao gồm chi
tiết nào được tiết lộ, chi tiết nào là riêng tư và tên nào nằm trong từng phạm
vi trong chương trình của bạn. Những tính năng này, đôi khi được gọi chung là
*hệ thống mô-đun*, bao gồm:

* **Packages:**  Một tính năng của Cargo cho phép bạn xây dựng, thử nghiệm và
  chia sẻ các *crate*
* **Crate:** Một cây của các mô-đun tạo ra một thư viện hoặc tệp thực thi
* **Mô-đun** và **use:** Cho phép bạn kiểm soát tổ chức, phạm vi (scope) và sự
  riêng biệt của các path (đường dẫn)
* **Path:** Cách đặt tên cho một mục, chẳng hạn như cấu trúc, hàm hoặc mô-đun

Trong chương này, chúng ta sẽ đề cập đến tất cả các tính năng này, thảo luận về
cách chúng tương tác và giải thích cách sử dụng chúng để quản lý phạm vi. Cuối
cùng, bạn sẽ có hiểu biết vững chắc về hệ thống mô-đun và có thể làm việc với
các phạm vi như một chuyên gia!

[workspaces]: ch14-03-cargo-workspaces.html