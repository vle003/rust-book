## Đường dẫn Tham chiếu đến một Mục trong Cây Mô-đun

Để hiển thị cho Rust vị trí tìm một mục trong cây mô-đun, chúng ta sử dụng đường
dẫn giống như cách sử dụng đường dẫn khi duyệt hệ thống tệp. Để gọi một hàm,
chúng ta cần biết đường dẫn của nó.

Một đường dẫn có thể có hai dạng:

* Một *đường dẫn tuyệt đối* là đường dẫn đầy đủ bắt đầu từ crate gốc; đối với mã
  từ một crate bên ngoài, đường dẫn tuyệt đối bắt đầu bằng tên crate và đối với
  mã từ crate hiện tại, nó bắt đầu bằng chữ `crate`.
* Một *đường dẫn tương đối* bắt đầu từ mô-đun hiện tại và sử dụng `self`,
  `super` hoặc một mã định danh trong mô-đun hiện tại.

Cả đường dẫn tuyệt đối và tương đối đều được theo sau bởi một hoặc nhiều mã định
danh được phân tách bằng dấu hai chấm (`::`).

Quay trở lại Liệt kê 7-1, giả sử chúng ta muốn gọi hàm `add_to_waitlist`. Điều
này giống như hỏi: đường dẫn của hàm `add_to_waitlist` là gì? Liệt kê 7-3 chứa
Liệt kê 7-1 với một số mô-đun và hàm đã bị loại bỏ.

Chúng ta sẽ chỉ ra hai cách để gọi hàm `add_to_waitlist` từ một hàm mới
`eat_at_restaurant` được xác định trong thư mục crate gốc. Những đường dẫn này
là chính xác, nhưng còn một vấn đề khác sẽ ngăn ví dụ này biên dịch như nó vốn
phải thế. Chúng ta sẽ giải thích  một chút lý do tại sao.

Hàm `eat_at_restaurant` là một phần trong API công khai của thư viện, vì vậy
chúng ta đánh dấu nó bằng từ khóa `pub`. Chúng ta sẽ đi vào chi tiết hơn về
`pub` trong phần [“Hiển thị đường dẫn với từ khóa `pub`”][pub]<!-- ignore -->.


<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-03/src/lib.rs}}
```

<span class="caption">Liệt kê 7-3: Gọi hàm `add_to_waitlist` bằng đường dẫn tuyệt đối và tương đối</span>

Lần đầu tiên chúng ta gọi hàm `add_to_waitlist` trong `eat_at_restaurant`, chúng
ta sử dụng một đường dẫn tuyệt đối. Hàm `add_to_waitlist` được khai báo trong
cùng một crate với `eat_at_restaurant`, có nghĩa là chúng ta có thể sử dụng từ
khóa `crate` để bắt đầu một đường dẫn tuyệt đối. Sau đó, chúng ta gộp từng
mô-đun kế tiếp cho đến khi chúng ta tìm ra đường đến `add_to_waitlist`. Bạn có
thể tưởng tượng một hệ thống tệp có cùng cấu trúc: chúng ta sẽ chỉ định đường
dẫn `/front_of_house/hosting/add_to_waitlist` để chạy chương trình
`add_to_waitlist`; sử dụng tên `crate` để bắt đầu từ crate gốc giống như sử dụng
`/` để bắt đầu từ gốc hệ thống tệp trong shell trên hệ điều hành của bạn.

Lần thứ hai chúng ta gọi `add_to_waitlist` trong `eat_at_restaurant`, chúng ta
sử dụng một đường dẫn tương đối. Đường dẫn bắt đầu bằng `front_of_house`, tên
của mô-đun được xác định ở cùng cấp độ của cây mô-đun với tên
`eat_at_restaurant`. Ở đây, hệ thống tệp tương đương sẽ sử dụng đường dẫn
`front_of_house/hosting/add_to_waitlist`. Bắt đầu bằng tên mô-đun có nghĩa là
đường dẫn là tương đối.

Việc chọn sử dụng đường dẫn tương đối hay tuyệt đối là quyết định mà bạn sẽ đưa
ra dựa trên dự án của mình, và phụ thuộc vào việc bạn có nhiều khả năng di
chuyển mã khai báo đề mục riêng biệt, hoặc cùng với mã sử dụng đề mục đó hay
không. Ví dụ: nếu chúng ta di chuyển mô-đun `front_of_house` và chức năng
`eat_at_restaurant` vào một mô-đun có tên `customer_experience`, thì chúng ta
cần cập nhật đường dẫn tuyệt đối đến `add_to_waitlist`, nhưng đường dẫn tương
đối sẽ vẫn hợp lệ. Tuy nhiên, nếu chúng ta di chuyển riêng chức năng
`eat_at_restaurant` vào một mô-đun có tên `dining`, thì đường dẫn tuyệt đối đến
lệnh gọi `add_to_waitlist` sẽ giữ nguyên, nhưng đường dẫn tương đối sẽ cần phải
được cập nhật. Nói chung, lựa chọn của chúng ta là chỉ định các đường dẫn tuyệt
đối vì có nhiều khả năng chúng ta sẽ muốn di chuyển các định nghĩa mã và lệnh
gọi đến các đề mục độc lập với nhau.

Hãy thử biên dịch Liệt kê 7-3 và tìm hiểu tại sao nó vẫn chưa biên dịch được!
Lỗi mà chúng ta gặp phải được hiển thị trong Liệt kê 7-4.

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-03/output.txt}}
```

<span class="caption">Liệt kê 7-4: Lỗi trình biên dịch khi xây dựng mã trong
Liệt kê 7-3</span>

Các thông báo lỗi nói rằng mô-đun `hosting` là riêng tư. Nói cách khác, chúng ta
có đường dẫn chính xác cho mô-đun `hosting` và chức năng `add_to_waitlist`,
nhưng Rust không cho phép chúng ta sử dụng chúng vì nó không có quyền truy cập
vào các phần riêng tư. Trong Rust, tất cả các mục (hàm, phương thức, cấu trúc,
enum, theo mặc định, các mô-đun và hằng số) là riêng tư đối với các mô-đun
chính. Nếu bạn muốn đặt một mục như hàm hoặc cấu trúc ở chế độ riêng tư, bạn đặt
mục đó vào một mô-đun.

Các đề mục trong mô-đun chính không thể sử dụng các đề mục riêng bên trong các
mô-đun con, nhưng các đề mục trong mô-đun con có thể sử dụng các đề mục trong
mô-đun tổ tiên của chúng. Điều này là do các mô-đun con đóng kín và ẩn các
triển khai chi tiết của chúng, nhưng các mô-đun con có thể thấy ngữ cảnh ở nơi
mà chúng được khai báo. Để tiếp tục với phép ẩn dụ của chúng ta, hãy nghĩ về các
quy tắc quyền riêng tư giống như văn phòng phía sau của một nhà hàng: những gì
diễn ra trong đó là riêng tư đối với khách hàng của nhà hàng, nhưng các nhà quản
lý văn phòng có thể xem và làm mọi thứ trong nhà hàng mà họ điều hành.

Rust đã chọn để hệ thống mô-đun hoạt động theo cách này để mặc định là ẩn các
chi tiết triển khai bên trong. Bằng cách đó, bạn biết bạn có thể thay đổi phần
nào của mã bên trong mà không vi phạm mã bên ngoài. Tuy nhiên, Rust cung cấp cho
bạn tùy chọn hiển thị các phần bên trong mã của mô-đun con cho tổ tiên bên ngoài
mô-đun bằng cách sử dụng từ khóa `pub` để công khai một đề mục.

### Tiết lộ Đường dẫn với Từ khóa `pub`

Hãy quay lại lỗi trong Liệt kê 7-4, nó cho chúng ta biết mô-đun `hosting` là
riêng tư. Chúng ta muốn chức năng `eat_at_restaurant` trong mô-đun chính có
quyền truy cập vào chức năng `add_to_waitlist` trong mô-đun con, vì vậy chúng ta
đánh dấu mô-đun `hosting` bằng từ khóa `pub`, như trong Liệt kê 7-5.

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-05/src/lib.rs}}
```

<span class="caption">Liệt kê 7-5: Khai báo mô-đun `hosting` là `pub` để sử dụng nó từ `eat_at_restaurant`</span>

Thật không may, mã trong Liệt kê 7-5 vẫn dẫn đến lỗi, như trong Liệt kê 7-6.

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-05/output.txt}}
```

<span class="caption">Danh sách 7-6: Lỗi trình biên dịch khi xây dựng mã trong Liệt kê 7-5</span>

Chuyện gì đã xảy ra? Việc thêm từ khóa `pub` trước `mod hosting` sẽ làm cho
mô-đun trở nên công khai. Với thay đổi này, nếu chúng ta có thể truy cập
`front_of_house`, chúng ta có thể truy cập `hosting`. Nhưng *nội dung* của
`hosting` vẫn là riêng tư; công khai mô-đun không công khai nội dung của nó. Từ
khóa `pub` trên một mô-đun chỉ cho phép mã trong các mô-đun tổ tiên của nó tham
chiếu đến nó, không truy cập vào mã lệnh bên trong của nó. Bởi vì các mô-đun là
các hộp chứa (container), nên chúng ta không thể làm được gì nhiều bằng cách chỉ
công khai mô-đun; chúng ta cần đi xa hơn và chọn công khai một hoặc nhiều đề mục
trong mô-đun.

Các lỗi trong Liệt kê 7-6 nói rằng chức năng `add_to_waitlist` là riêng tư. Các
quy tắc về quyền riêng tư áp dụng cho các cấu trúc, enum, hàm và phương thức
cũng như các mô-đun.

Chúng ta cũng hãy công khai hàm `add_to_waitlist` bằng cách thêm từ khóa `pub`
trước định nghĩa của nó, như trong Liệt kê 7-7.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-07/src/lib.rs}}
```

<span class="caption">Liệt kê 7-7: Thêm từ khóa `pub` vào `mod hosting` và `fn add_to_waitlist` cho phép chúng ta gọi hàm từ `eat_at_restaurant`</span>

Bây giờ mã sẽ biên dịch được! Để biết lý do tại sao việc thêm từ khóa `pub` cho
phép chúng ta sử dụng các đường dẫn này trong `add_to_waitlist` đối với các quy
tắc về quyền riêng tư, hãy xem xét các đường dẫn tuyệt đối và tương đối. Trong
đường dẫn tuyệt đối, chúng ta bắt đầu với `crate`, gốc của cây mô-đun crate của
chúng ta. Mô-đun `front_of_house` được xác định trong thư mục gốc của crate. Mặc
dù `front_of_house` không công khai, vì chức năng `eat_at_restaurant` được xác
định trong cùng một mô-đun với `front_of_house` (nghĩa là `eat_at_restaurant` và
`front_of_house` là anh em ruột), chúng ta có thể tham khảo `front_of_house` từ
`eat_at_restaurant `. Tiếp theo là mô-đun `hosting` được đánh dấu bằng `pub`.
Chúng ta có thể truy cập mô-đun chính của `hosting`, vì vậy chúng ta có thể truy
cập `hosting`. Cuối cùng, chức năng `add_to_waitlist` được đánh dấu bằng `pub`
và chúng ta có thể truy cập mô-đun mẹ của nó, vì vậy lệnh gọi hàm này hoạt động!

Trong đường dẫn tương đối, logic giống như đường dẫn tuyệt đối ngoại trừ bước
đầu tiên: thay vì bắt đầu từ crate gốc, đường dẫn bắt đầu từ `front_of_house`.
Mô-đun `front_of_house` được xác định trong cùng một mô-đun với
`eat_at_restaurant`, do đó, đường dẫn tương đối bắt đầu từ mô-đun trong đó
`eat_at_restaurant` được xác định sẽ hoạt động. Sau đó, vì `hosting` và
`add_to_waitlist` được đánh dấu bằng `pub`, phần còn lại của đường dẫn sẽ hoạt
động và lệnh gọi hàm này hợp lệ!

Nếu bạn dự định chia sẻ crate thư viện của mình để các dự án khác có thể sử dụng
mã của bạn, API công khai của bạn là hợp đồng của bạn với người dùng crate của
bạn, để xác định cách họ có thể tương tác với mã của bạn. Có nhiều cân nhắc xung
quanh việc quản lý các thay đổi đối với API công khai của bạn, để giúp mọi người
dễ dàng hơn khi phụ thuộc vào crate của bạn. Những cân nhắc này nằm ngoài phạm
vi của cuốn sách này; nếu bạn quan tâm đến chủ đề này, hãy xem [Các Nguyên tắc
của Rust API ][api-guidelines].

> #### Các Thực hành tốt nhất cho các Gói với tệp Nhị phân và Thư viện
> 
> Chúng ta đã đề cập rằng một gói có thể chứa cả crate gốc nhị phân
> *src/main.rs* cũng như crate gốc thư viện *src/lib.rs* và cả hai crate sẽ có
> tên gói theo mặc định. Thông thường, các gói có mẫu chứa cả thư viện và crate
> nhị phân này sẽ có đủ mã trong crate nhị phân để bắt đầu một tệp thực thi gọi
> mã bằng crate thư viện. Điều này cho phép các dự án khác được hưởng lợi từ hầu
> hết các chức năng mà package cung cấp, bởi vì mã của crate thư viện có thể
> được chia sẻ.
> 
> Cây mô-đun phải được định nghĩa trong *src/lib.rs*. Sau đó, bất kỳ đề mục công
> khai nào cũng có thể được sử dụng trong crate nhị phân bằng cách bắt đầu các
> đường dẫn với tên của package. Crate nhị phân trở thành người dùng của crate
> thư viện giống như tất cả các crate bên ngoài sẽ sử dụng crate thư viện: nó
> chỉ có thể sử dụng API công khai. Điều này giúp bạn thiết kế một API tốt; bạn
> không chỉ là tác giả, bạn còn là khách hàng!
> 
> Trong [Chương 12][ch12]<!-- ignore -->, chúng ta sẽ chứng minh cách tổ chức
> này bằng một chương trình chạy trên dòng lệnh chứa cả crate nhị phân và crate
> thư viện.

### Bắt đầu Đường dẫn Tương đối với `super`

Chúng ta có thể bắt đầu xây dựng các đường dẫn tương đối trong mô-đun chính,
thay vì mô-đun hiện tại hoặc crate gốc, bằng cách sử dụng `super` ở đầu đường
dẫn. Điều này giống như bắt đầu một đường dẫn hệ thống tệp bằng cú pháp `..`.
Việc sử dụng `super` cho phép chúng ta tham chiếu một đề mục mà chúng ta biết là
nằm trong mô-đun chính, điều này có thể giúp việc sắp xếp lại cây mô-đun dễ dàng
hơn khi mô-đun gần nhau liên quan đến cha mẹ, nhưng cha mẹ có thể được chuyển
đến nơi khác trong cây mô-đun vào một ngày nào đó.

Hãy xem xét mã trong Liệt kê 7-8 mô hình hóa tình huống trong đó một đầu bếp sửa
một đơn đặt hàng không chính xác và đích thân mang nó ra cho khách hàng. Hàm
`fix_incorrect_order` được xác định trong mô-đun `back_of_house` gọi hàm
`deliver_order` được xác định trong mô-đun chính bằng cách chỉ định đường dẫn
đến `deliver_order` bắt đầu bằng `super`:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-08/src/lib.rs}}
```

<span class="caption">Liệt kê 7-8: Gọi một hàm sử dụng đường dẫn tương đối bắt đầu bằng `super`</span>

Hàm `sửa_lệnh_lệnh` nằm trong mô-đun `back_of_house`, vì vậy chúng ta có thể sử
dụng `super` để chuyển đến mô-đun mẹ của `back_of_house`, trong trường hợp này
là `crate`, là điểm gốc. Từ đó, chúng ta tìm kiếm `deliver_order` và tìm thấy
nó. Thành công! Chúng ta nghĩ rằng mô-đun `back_of_house` và chức năng
`deliver_order` có thể giữ nguyên mối quan hệ với nhau và được di chuyển cùng
nhau, nếu chúng ta quyết định tổ chức lại cây mô-đun crate. Do đó, chúng ta đã
sử dụng `super` để chúng ta sẽ có ít chỗ cần cập nhật mã hơn trong tương lai nếu
mã này được chuyển sang một mô-đun khác.

### Công khai Cấu trúc và Enums

Chúng ta cũng có thể sử dụng `pub` để chỉ định các cấu trúc và enum là công
khai, nhưng có một vài chi tiết bổ sung cho việc sử dụng `pub` với các cấu trúc
và enum. Nếu chúng ta sử dụng `pub` trước định nghĩa cấu trúc, thì chúng ta đặt
cấu trúc ở chế độ công khai, nhưng các trường của cấu trúc sẽ vẫn ở chế độ riêng
tư. Chúng ta có thể công khai hoặc không công khai từng trường tùy theo từng
trường hợp. Trong Liệt kê 7-9, chúng ta đã định nghĩa một cấu trúc công khai
`back_of_house::Breakfast` với một trường `toast` công khai nhưng một trường
`season_fruit` riêng tư. Điều này mô hình hóa trường hợp trong một nhà hàng, nơi
khách hàng có thể chọn loại bánh mì đi kèm với bữa ăn, nhưng đầu bếp quyết định
loại trái cây đi kèm với bữa ăn dựa trên những gì theo mùa và có trong kho. Trái
cây có sẵn thay đổi nhanh chóng nên khách hàng không thể lựa chọn trái cây hoặc
thậm chí không nhìn thấy trái cây nào họ sẽ được nhận.

<span class="filename">Tên tệp: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-09/src/lib.rs}}
```

<span class="caption">Danh sách 7-9: Một cấu trúc có một số trường công khai và một số trường riêng tư</span>

Vì trường `toast` trong cấu trúc `back_of_house::Breakfast` là công khai nên
trong `eat_at_restaurant`, chúng ta có thể viết và đọc trường `toast` bằng cách
sử dụng ký hiệu dấu chấm. Lưu ý rằng chúng ta không thể sử dụng trường
`seasonal_fruit` trong `eat_at_restaurant` vì `seasonal_fruit` là riêng tư. Hãy
thử bỏ ghi chú dòng sửa đổi giá trị trường `seasonal_fruit` để xem bạn gặp lỗi
gì!

Ngoài ra, hãy lưu ý rằng vì `back_of_house::Breakfast` có một trường riêng tư,
nên cấu trúc cần cung cấp một hàm liên kết công khai để xây dựng một nhân bản
(instance) của `Breakfast` (ở đây chúng ta đã đặt tên cho nó là `summer`). Nếu
`Breakfast` không có chức năng như vậy, thì chúng ta không thể tạo phiên bản của
`Breakfast` trong `eat_at_restaurant`, vì chúng ta không thể thay đổi giá trị
của thuộc tính riêng tư `seasonal_fruit` trong `eat_at_restaurant`.

Ngược lại, nếu chúng ta công khai một enum, thì tất cả các biến thể của nó sẽ
được công khai. Chúng ta chỉ cần `pub` trước từ khóa `enum`, như trong Liệt kê
7-10.

<span class="filename">Tên tệp: src/lib.rs</span>


```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-10/src/lib.rs}}
```

<span class="caption">Danh sách 7-10: Chỉ định một enum là công khai sẽ công khai tất cả các biến thể của nó</span>

Bởi vì chúng ta đã công khai enum `Appetizer`, nên chúng ta có thể sử dụng các
biến thể `Soup` và `Salad` trong `eat_at_restaurant`.

Enums không hữu ích lắm trừ khi các biến thể của chúng được công khai; sẽ rất
khó chịu khi phải chú thích tất cả các biến thể enum bằng `pub` trong mọi trường
hợp, vì vậy, các biến thể enum mặc định là công khai. Các cấu trúc thường hữu
ích mà không cần công khai các trường của chúng, vì vậy các trường cấu trúc tuân
theo quy tắc chung là mọi thứ đều ở chế độ riêng tư theo mặc định trừ khi được
khai báo bằng từ khóa `pub`.

Còn một tình huống nữa liên quan đến `pub` mà chúng ta chưa đề cập đến và đó là
tính năng mô-đun hệ thống cuối cùng của chúng ta: từ khóa `use`. Trước tiên,
chúng ta sẽ đề cập đến `use`, sau đó chúng ta sẽ chỉ ra cách kết hợp `pub` và
`use`.

[pub]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[api-guidelines]: https://rust-lang.github.io/api-guidelines/
[ch12]: ch12-00-an-io-project.html
