## Đưa Đường dẫn vào Phạm vi với từ khóa `use`

Việc phải viết ra và lặp đi lặp lại các đường dẫn để gọi hàm khiến ta cảm thấy
bất tiện. Trong Liệt kê 7-7, cho dù chúng ta chọn đường dẫn tuyệt đối hay tương
đối đến hàm `add_to_waitlist`, mỗi lần chúng ta muốn gọi `add_to_waitlist`,
chúng ta cũng phải chỉ định `front_of_house` và `hosting`. May mắn thay, có một
cách để đơn giản hóa quá trình này: chúng ta có thể tạo lối tắt đến đường dẫn
bằng từ khóa `use`, sau đó ta có thể sử dụng lời gọi hàm ngắn gọn ở mọi nơi khác
trong phạm vi sử dụng.

Trong Liệt kê 7-11, chúng ta đưa mô-đun `crate::front_of_house::hosting` vào
phạm vi của hàm `eat_at_restaurant` vì vậy chúng ta chỉ phải chỉ định
`hosting::add_to_waitlist` để gọi hàm `add_to_waitlist` trong
`eat_at_restaurant` .

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-11/src/lib.rs}}
```

<span class="caption">Liệt kê 7-11: Đưa một mô-đun vào phạm vi với `use`</span>

Việc thêm `use` và một đường dẫn trong một phạm vi tương tự như việc tạo một
liên kết tượng trưng trong hệ thống tệp. Bằng cách thêm `use
crate::front_of_house::hosting` vào crate gốc, `hosting` sẽ là một tên hợp lệ
trong phạm vi đó, giống như thể mô-đun `hosting` đã được khai báo trong crate
gốc. Các đường dẫn được đưa vào phạm vi `use` cũng kiểm tra quyền riêng tư,
giống như bất kỳ đường dẫn nào khác.

Lưu ý rằng `use` chỉ tạo lối tắt cho phạm vi cụ thể mà `use` hiện hữu. Liệt kê
7-12 di chuyển hàm `eat_at_restaurant` vào một mô-đun con mới có tên `customer`,
khi đó mô-đun này có phạm vi khác với câu lệnh `use`, vì vậy phần thân hàm sẽ
không biên dịch được:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness,does_not_compile,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-12/src/lib.rs}}
```

<span class="caption">Liệt kê 7-12: Câu lệnh `use` chỉ áp dụng trong phạm vi của nó</span>

Lỗi trình biên dịch cho thấy lối tắt không được áp dụng trong mô-đun `customer`:

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-12/output.txt}}
```

Lưu ý rằng cũng có một cảnh báo là `use` không còn được sử dụng trong phạm vi
của nó nữa! Để khắc phục sự cố này, hãy chuyển `use` vào trong cả mô-đun
`customer` hoặc tham chiếu lối tắt trong mô-đun chính với `super::hosting` trong
mô-đun con `customer`.

### Tạo các Thành ngữ Đường dẫn `use`

Trong Liệt kê 7-11, bạn có thể thắc mắc tại sao chúng tôi chỉ định `use
crate::front_of_house::hosting` và sau đó gọi `hosting::add_to_waitlist` trong
`eat_at_restaurant` thay vì chỉ định đường dẫn `use` dẫn đến `add_to_waitlist`
để đạt được kết quả tương tự, như trong Liệt kê 7-13.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-13/src/lib.rs}}
```

<span class="caption">Liệt kê 7-13: Đưa hàm `add_to_waitlist` vào phạm vi với `use`, một hàm đơn điệu</span>

Mặc dù cả Liệt kê 7-11 và 7-13 đều hoàn thành cùng một nhiệm vụ, nhưng Liệt kê
7-11 là cách thành ngữ để đưa một hàm vào phạm vi với `use`. Đưa mô-đun mẹ của
hàm vào phạm vi với `use` có nghĩa là chúng ta phải chỉ định mô-đun mẹ khi gọi
hàm. Chỉ định mô-đun chính khi gọi hàm làm rõ rằng hàm không được khai báo cục
bộ trong khi vẫn giảm thiểu sự lặp lại của đường dẫn đầy đủ. Đoạn mã trong Liệt
kê 7-13 không rõ ràng về nơi  khai báo `add_to_waitlist`.

Mặt khác, việc chỉ định đường dẫn đầy đủ là điều bình thường khi đưa vào các cấu
trúc, enum và các mục khác với `use`. Liệt kê 7-14 cho thấy cách thành ngữ để
đưa cấu trúc `HashMap` của thư viện chuẩn vào phạm vi của một crate nhị phân.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-14/src/main.rs}}
```

<span class="caption">Danh sách 7-14: Đưa `HashMap` vào phạm vi theo cách thành ngữ</span>

Không có lý do chính đáng nào đằng sau thành ngữ này: đó chỉ là quy ước đã xuất
hiện và mọi người đã quen với việc đọc và viết mã Rust theo cách này.

Ngoại lệ đối với thành ngữ này là Rust không cho phép nếu chúng ta đưa hai mục
có cùng tên vào phạm vi với câu lệnh `use`. Liệt kê 7-15 cho thấy cách đưa hai
loại `Result` vào phạm vi có cùng tên nhưng khác mô-đun cha và cách tham chiếu
đến chúng.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-15/src/lib.rs:here}}
```

<span class="caption">Liệt kê 7-15: Đưa hai loại có cùng tên vào cùng một phạm vi yêu cầu sử dụng các mô-đun cha của chúng.</span>

Như bạn có thể thấy, việc sử dụng các mô-đun chính sẽ phân biệt hai kiểu
`Result`. Thay vào đó, nếu chúng ta chỉ định `use std::fmt::Result` và `use
std::io::Result`, chúng ta sẽ có hai kiểu `Result` trong cùng một phạm vi và
Rust sẽ không biết chúng ta muốn nói đến loại nào.

### Tạo các Tên mới với từ khóa `as`

Có một giải pháp khác cho vấn đề đưa hai kiểu cùng tên vào cùng một phạm vi với
`use`: sau đường dẫn, chúng ta có thể chỉ định `as` và một tên cục bộ mới hoặc
*bí danh* cho loại đó. Liệt kê 7-16 minh họa một cách khác để viết lại mã trong
Liệt kê 7-15 và đổi tên một trong hai kiểu `Result` bằng cách sử dụng `as`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-16/src/lib.rs:here}}
```

<span class="caption">Liệt kê 7-16: Đổi tên một loại khi nó được đưa vào phạm vi với từ khóa `as`</span>

Trong câu lệnh `use` thứ hai, chúng ta đã chọn tên mới `IoResult` cho kiểu
`std::io::Result`, tên này sẽ không xung đột với `Result` từ `std::fmt` mà chúng
ta đã đưa vào phạm vi. Liệt kê 7-15 và Liệt kê 7-16 được coi là thành ngữ, vì
vậy lựa chọn cách nào là tùy thuộc vào bạn!

### Xuất lại tên với `pub use`

Khi chúng ta đưa một tên vào phạm vi với từ khóa `use`, tên có sẵn trong phạm vi
mới là riêng tư. Để cho phép chương trình gọi mã của chúng ta tham chiếu đến tên
đó như thể nó đã được xác định trong phạm vi của mã đó, chúng ta có thể kết hợp
`pub` và `use`. Kỹ thuật này được gọi là *tái xuất* vì chúng ta đang đưa một đề
mục vào phạm vi nhưng cũng cung cấp đề mục đó cho những người khác sử dụng trong
phạm vi mã của họ.

Liệt kê 7-17 hiển thị mã trong Liệt kê 7-11 với `use` trong mô-đun gốc được đổi
thành `pub use`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-17/src/lib.rs}}
```

<span class="caption">Liệt kê 7-17: Cung cấp tên cho bất kỳ mã nào để sử dụng từ một phạm vi mới với `pub use`</span>

Trước khi có thay đổi này, khi gọi hàm `add_to_waitlist`, mã bên ngoài sẽ phải
sử dụng đường dẫn `restaurant::front_of_house::hosting::add_to_waitlist()`. Bây
giờ `pub use` đã tái xuất mô-đun `hosting` từ mô-đun gốc, lúc này mã bên ngoài
có thể sử dụng đường dẫn `restaurant::hosting::add_to_waitlist()` để thay thế.

Tái xuất rất hữu ích khi cấu trúc bên trong mã của bạn khác với điều các lập
trình viên nghĩ về vùng hoạt đông của mã khi gọi mã của bạn. Ví dụ, trong phép
ẩn dụ về nhà hàng này, những người điều hành nhà hàng nghĩ về “phía trước nhà”
và “phía sau nhà”. Nhưng những khách hàng ghé thăm một nhà hàng có lẽ sẽ không
nghĩ về các bộ phận của nhà hàng theo những thuật ngữ đó. Với `pub use`, chúng
ta có thể viết mã của mình bằng một cấu trúc nhưng hiển thị như một cấu trúc
khác. Làm như vậy giúp thư viện của chúng ta được tổ chức tốt cho các lập trình
viên làm việc trên thư viện và các lập trình viên gọi đến thư viện. Chúng ta sẽ
xem xét một ví dụ khác về `pub use` và cách nó ảnh hưởng đến tài liệu của crate
của bạn trong phần [“Xuất API công cộng thuận tiện với `pub
use`”][ch14-pub-use]<!-- ignore --> của Chương 14.

### Sử dụng các Gói từ bên ngoài

Trong Chương 2, chúng ta đã viết một trò chơi đoán số sử dụng một gói bên ngoài
có tên `rand` để tạo và nhận các số ngẫu nhiên. Để sử dụng `rand` trong dự án
của mình, chúng ta đã thêm dòng này vào mnội dung tệp *Cargo.toml*:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:9:}}
```

Việc bổ sung `rand` làm phần phụ thuộc trong *Cargo.toml* sẽ khiến Cargo tải
xuống gói `rand`, và bất kỳ phần phụ thuộc nào từ
[crates.io](https://crates.io/), điều này làm cho `rand` khả dụng trong dự án
của chúng ta.

Sau đó, để đưa các định nghĩa của `rand` vào phạm vi gói của chúng ta, chúng ta
đã thêm một dòng `use` bắt đầu bằng tên của crate là `rand` và liệt kê các đề
mục mà chúng ta muốn dùng trong phạm vi mã của mình. Nhớ lại rằng trong phần
[“Tạo số ngẫu nhiên”][rand]<!-- ignore --> của Chương 2, chúng ta đã đưa đặc
điểmn (trait) `Rng` vào phạm vi và gọi hàm `rand::thread_rng`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:ch07-04}}
```

Các thành viên của cộng đồng Rust đã cung cấp rất nhiều package (gói) tại
[crates.io](https://crates.io/) và việc đưa bất kỳ gói nào vào gói của bạn cũng
bao gồm các bước sau: liệt kê chúng trong tệp *Cargo.toml* của gói của bạn và sử
dụng `use` để đưa các đề mục (item) từ crate của chúng vào phạm vi sử dụng trong
mã của mình.

Lưu ý rằng thư viện `std` tiêu chuẩn cũng là một thùng nằm ngoài gói của chúng
ta. Vì thư viện tiêu chuẩn được gộp sẵn với Rust khi cài đặt nên chúng ta không
cần thay đổi *Cargo.toml* để bao gồm `std`. Nhưng chúng ta cần phải tham chiếu
nó với `use` để đưa các đề mục của nó vào phạm vi sử dụng trong gói của chúng
ta. Ví dụ: với `HashMap`, chúng ta sẽ sử dụng dòng này:

```rust
use std:
```

Đây là đường dẫn tuyệt đối bắt đầu bằng `std`, tên của thùng thư viện tiêu
chuẩn.

### Sử dụng các Đường dẫn Lồng nhau để Dọn dẹp các Danh sách `use` Lớn

Nếu chúng ta đang sử dụng nhiều đề mục được khai báo trong cùng một crate hoặc
cùng một mô-đun, việc liệt kê từng đề mục trên từng dòng riêng có thể chiếm
nhiều dòng mã trong tệp mã nguồn của chúng ta. Ví dụ: hai câu lệnh `use` mà
chúng ta có trong Trò chơi Đoán số trong Liệt kê 2-4 đưa các mục từ `std` vào
phạm vi mã:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-01-use-std-unnested/src/main.rs:here}}
```

Thay vào đó, chúng ta có thể sử dụng các đường dẫn lồng nhau để đưa các mục
giống nhau vào phạm vi trong một dòng. Chúng ta làm điều này bằng cách xác định
phần chung của đường dẫn, theo sau là hai dấu hai chấm, sau đó đặt dấu ngoặc
nhọn xung quanh danh sách các phần khác nhau của đường dẫn, như được hiển thị
trong Liệt kê 7-18.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-18/src/main.rs:here}}
```

<span class="caption">Danh sách 7-18: Chỉ định một đường dẫn lồng nhau để đưa nhiều mục có cùng tiền tố vào phạm vi</span>

Trong các chương trình lớn hơn, việc đưa nhiều mục vào phạm vi từ cùng một thùng
hoặc mô-đun bằng cách sử dụng các đường dẫn lồng nhau có thể giảm rất nhiều số
lượng câu lệnh `use` riêng biệt cần thiết!

Chúng ta có thể sử dụng một đường dẫn lồng nhau ở bất kỳ cấp độ nào trong một
đường dẫn, điều này rất hữu ích khi kết hợp hai câu lệnh `use` có chung một
đường dẫn con. Ví dụ, Liệt kê 7-19 hiển thị hai câu lệnh `use`: một câu lệnh đưa
`std::io` vào phạm vi và một câu lệnh đưa `std::io::Write` vào phạm vi.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-19/src/lib.rs}}
```

<span class="caption">Liệt kê 7-19: Hai câu lệnh `use` trong đó một câu lệnh là đường dẫn con của câu lệnh kia</span>

Phần chung của hai đường dẫn này là `std::io` và đó là đường dẫn đầu tiên hoàn
chỉnh. Để hợp nhất hai đường dẫn này thành một câu lệnh `use`, chúng ta có thể
sử dụng `self` trong đường dẫn lồng nhau, như trong Liệt kê 7-20.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-20/src/lib.rs}}
```

<span class="caption">Danh sách 7-20: Kết hợp các đường dẫn trong Liệt kê 7-19 thành một câu lệnh `use`</span>

Dòng này đưa `std::io` và `std::io::Write` vào phạm vi.

### Toán tử Toàn cục (Glob Operator)

Nếu chúng ta muốn đưa *tất cả* các mục công khai được xác định trong một đường
dẫn vào phạm vi, chúng ta có thể chỉ định đường dẫn đó theo sau bởi toán tử toàn
cục `*`:

```rust
use std::collections::*;
```

Câu lệnh `use` này mang tất cả các mục được khai báo là công khai trong
`std::collections` vào phạm vi mã hiện tại. Hãy cẩn thận khi sử dụng toán tử
toàn cục! Nó có thể khiến việc xác định tên nào nằm trong phạm vi nào và nơi
khai báo tên được sử dụng trong chương trình của bạn trở nên khó khăn hơn.

Toán tử toàn cục thường được sử dụng khi thử nghiệm để đưa mọi thứ đang thử
nghiệm vào mô-đun `tests`; chúng ta sẽ nói về điều đó trong phần [“Cách viết các
Test”][writing-tests]<!-- ignore --> trong Chương 11. Toán tử toàn cục đôi khi
cũng được sử dụng như một phần của mẫu prelude: xem [tài liệu thư viện
chuẩn](../std/prelude/index.html#other-preludes)<!-- ignore --> để biết thêm
thông tin về các mẫu đó.

[ch14-pub-use]: ch14-02-publishing-to-crates-io.html#exporting-a-convenient-public-api-with-pub-use
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number
[writing-tests]: ch11-01-writing-tests.html#how-to-write-tests
