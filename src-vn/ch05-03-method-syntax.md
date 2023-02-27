## Cú pháp của Phương thức

*Phương thức* tương tự như hàm: chúng ta khai báo chúng bằng từ khóa `fn` và
tên, chúng có thể có tham số và giá trị trả về, đồng thời chứa một số mã sẽ chạy
khi phương thức được gọi từ một vị trí khác. Không giống như các hàm, các phương
thức được định nghĩa trong ngữ cảnh của một cấu trúc (hoặc một đối tượng enum
hoặc một đặc điểm mà chúng ta sẽ trình bày trong [Chương 6][enums]<!-- ignore
--> và [Chương 17][trait-objects]<!-- ignore -->, tương ứng) và tham số đầu tiên
của chúng luôn là `self`, đại diện cho nhân bản (instance) của cấu trúc mà
phương thức đang được gọi.

### Khai báo Phương thức

Hãy thay đổi hàm `area` có nhân bản `Rectangle` làm tham số và thay vào đó, như
minh họa trong Liệt kê 5-13, tạo một phương thức `area` được xác định trong cấu
trúc `Rectangle`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-13/src/main.rs}}
```
<span class="caption">Liệt kê 5-13: Định nghĩa một phương thức `area` trên cấu trúc `Rectangle`</span>

Để khai báo hàm trong ngữ cảnh của `Rectangle`, chúng ta bắt đầu một khối `impl`
cho `Rectangle`. Mọi thứ trong khối `impl` này sẽ được gắn với kiểu `Rectangle`.
Sau đó, chúng ta di chuyển hàm `area` vào trong dấu ngoặc nhọn của khối lệnh
`impl` và thay đổi tham số đầu tiên (và chỉ trong trường hợp này) thành `self`
trong khai báo và mọi nơi trong khối lệnh. Trong `main`, nơi chúng ta đã gọi hàm
`area` và truyền đối số `rect1`, thay vào đó, chúng ta sử dụng *cú pháp phương
thức* để gọi phương thức `area` trên nhân bản `Rectangle`. Cú pháp gọi phương
thức đi sau một nhân bản là: chúng ta thêm một dấu chấm sau đó là tên phương
thức, dấu ngoặc đơn và đối số nếu có.

Trong khai báo của `area`, chúng ta sử dụng `&self` thay vì `rectangle:
&Rectangle`. Cú pháp `&self` thực ra là viết tắt của `self: &Self`. Trong khối
`impl`, kiểu `Self` là bí danh của kiểu thuộc về `impl`. Các phương thức buộc
phải có tham số có tên `self` của kiểu `Self` như là tham số đầu tiên của chúng,
vì vậy Rust cho phép bạn viết tắt tham số này chỉ với tên `self` ở vị trí tham
số đầu tiên. Lưu ý rằng chúng ta vẫn cần sử dụng `&` trước từ viết tắt `self` để
chỉ ra rằng phương thức này mượn nhân bản `Self`, giống như chúng ta đã làm
trong `rectangle: &Rectangle`. Các phương thức có thể sở hữu `self`, mượn `self`
một cách bất biến, như chúng ta đã làm ở đây, hoặc mượn tính khả biến của
`self`, ta cũng có thể làm vậy với bất kỳ tham số nào khác.

Chúng ta chọn `&self` ở đây vì lý do tương tự mà chúng ta đã sử dụng
`&Rectangle` trong phiên bản *Hàm*: chúng ta không muốn sở hữu và chúng ta chỉ
muốn đọc dữ liệu trong cấu trúc chứ không phải ghi vào nó. Nếu chúng ta muốn
thay đổi nhân bản mà chúng ta đã gọi phương thức như một phần của những gì
phương thức thực hiện, chúng ta sẽ sử dụng `&mut self` làm tham số đầu tiên. Rất
hiếm khi có một phương thức chiếm quyền sở hữu đối tượng bằng cách chỉ sử dụng
`self` làm tham số đầu tiên; kỹ thuật này thường được sử dụng khi phương thức
chuyển đổi `self` thành một thứ khác và bạn muốn ngăn người gọi sử dụng bản gốc
sau khi chuyển đổi.

Lý do chính để sử dụng phương thức thay vì hàm là để tổ chức mã, ngoài việc cung
cấp cú pháp phương thức và không phải lặp lại kiểu `self` trong khai báo của mọi
phương thức. Chúng ta đã đặt tất cả những thứ chúng ta có thể làm với một nhân
bản của một kiểu trong khối `impl` thay vì khiến cho người dùng mã của chúng ta
trong tương lai tìm kiếm các khả năng của `Rectangle` ở nhiều nơi khác nhau
trong thư viện mà chúng ta cung cấp.

Lưu ý rằng chúng ta có thể chọn đặt cho một phương thức cùng tên với một trường
của cấu trúc. Ví dụ: chúng ta có thể định nghĩa một phương thức cũng có tên
`width` trong cấu trúc `Rectangle`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-06-method-field-interaction/src/main.rs:here}}
```

Ở đây, chúng ta đang chọn làm cho phương thức `width` trả về `true` nếu giá trị
trong trường `width` của đối tượng lớn hơn `0` và `false` nếu giá trị là `0`:
chúng ta có thể sử dụng một trường cùng tên trong một phương thức cùng tên cho
bất kỳ mục đích nào. Trong `main`, khi chúng ta viết `rect1.width()`, Rust biết
chúng ta muốn nói đến phương thức `width`. Khi chúng ta không sử dụng dấu ngoặc
đơn, Rust biết chúng tôi muốn nói đến trường `width`.

Thông thường, nhưng không phải lúc nào cũng vậy, khi chúng ta đặt tên một phương
thức trùng tên với một trường, chúng ta muốn nó chỉ trả về giá trị trong trường
và không làm gì khác. Các phương thức như thế này được gọi là *getters* và Rust
không tự động triển khai chúng cho các trường cấu trúc như một số ngôn ngữ khác
thực hiện. *Getters* rất hữu ích vì bạn có thể đặt trường ở chế độ Riêng tư
(Private) nhưng phương thức lại ở chế độ Công khai (Public) và do đó cho phép
quyền truy cập chỉ đọc vào trường đó như một phần của API công khai của loại.
Chúng ta sẽ thảo luận về Công khai và Riêng tư là gì và cách chỉ định một trường
hoặc phương thức là Công khai hoặc Riêng tư trong [Chương 7][public]<!-- ignore
-->.

> ### Vị trí của toán tử `->` ở nơi nào?
>
> Trong C và C++, có hai toán tử khác nhau được sử dụng để gọi các phương thức: 
> bạn sử dụng `.` nếu bạn đang gọi trực tiếp một phương thức trên đối tượng 
> và `->` nếu bạn đang gọi phương thức trên một con trỏ tới đối tượng và đối 
> tượng cần hủy tham chiếu con trỏ trước. Nói cách khác, nếu `object` là một 
> con trỏ, thì `object->something()` tương tự như `(*object).something()`.
>
> Rust không có toán tử tương đương `->`; thay vào đó, Rust có một tính năng
> được gọi là *tham chiếu và hủy tham chiếu tự động*. Gọi Phương thức là một 
> trong số ít chỗ mà Rust có hành vi này.
>
> Đây là cách nó hoạt động: khi bạn gọi một phương thức `object.something()`, 
> Rust tự động thêm vào `&`, `&mut` hoặc `*` để `object` khớp với khai báo của 
> phương thức. Nói cách khác, những điều sau đây giống nhau:
> 
> <!-- CAN'T EXTRACT SEE BUG https://github.com/rust-lang/mdBook/issues/1127 -->
> ```rust
> # #[derive(Debug,Copy,Clone)]
> # struct Point {
> #     x: f64,
> #     y: f64,
> # }
> #
> # impl Point {
> #    fn distance(&self, other: &Point) -> f64 {
> #        let x_squared = f64::powi(other.x - self.x, 2);
> #        let y_squared = f64::powi(other.y - self.y, 2);
> #
> #        f64::sqrt(x_squared + y_squared)
> #    }
> # }
> # let p1 = Point { x: 0.0, y: 0.0 };
> # let p2 = Point { x: 5.0, y: 6.5 };
> p1.distance(&p2);
> (&p1).distance(&p2);
> ```
>
> Cái đầu tiên trông gọn gàng hơn. Hành vi tham chiếu tự động này hoạt động vì
> các phương thức có bộ tiếp nhận rõ ràng — kiểu `self`. Chỉ với người nhận và
> tên của một phương thức, Rust có thể xác định chắc chắn liệu phương thức đó
> đang đọc (`&self`), biến đổi (`&mut self`) hay đang sử dụng (`self`). Thực tế
> là Rust ẩn đi việc cho mượn đối với người nhận phương thức là một phần quan
> trọng trong việc làm cho quyền sở hữu trở nên thuận tiện trong thực tế.

### Các phương thức có nhiều tham số hơn

Hãy thực hành sử dụng các phương thức bằng cách triển khai phương thức thứ hai
trên cấu trúc `Rectangle`. Lần này, chúng ta muốn một nhân bản của `Rectangle`
lấy một nhân bản khác của `Rectangle` và trả về `true` nếu `Rectangle` thứ hai
có thể hoàn toàn khớp với `self` (`Rectangle` đầu tiên); nếu không, nó sẽ
trả về `false`. Nghĩa là, một khi chúng ta đã định nghĩa phương thức `can_hold`,
chúng ta muốn có thể viết chương trình như trong Liệt kê 5-14.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-14/src/main.rs}}
```

<span class="caption">Liệt kê 5-14: Sử dụng phương thúc `can_hold` vẫn chưa được viết ra</span>

Đầu ra dự kiến sẽ giống như sau bởi vì cả hai kích thước của `rect2` đều nhỏ hơn
kích thước của `rect1`, nhưng `rect3` rộng hơn `rect1`:

```text
Can rect1 hold rect2? true
Can rect1 hold rect3? false
```

Chúng tôi biết rằng chúng tôi muốn xác định một phương thức, vì vậy nó sẽ nằm
trong khối `impl Rectangle`. Tên phương thức sẽ là `can_hold`, và nó sẽ mượn bất
biến của một `Rectangle` khác làm tham số. Chúng ta có thể biết loại tham số sẽ
là gì bằng cách xem mã gọi phương thức: `rect1.can_hold(&rect2)` chuyển vào
`&rect2`, đây là mượn một bất biến của `rect2`, một phiên bản của `Rectangle`.
Điều này có ý nghĩa bởi vì chúng ta chỉ cần đọc `rect2` (chứ không phải ghi giá
trị vào nó, nó có nghĩa là chúng ta cần mượn sự khả biến) và chúng ta muốn
`main` giữ quyền sở hữu `rect2` để chúng tôi có thể sử dụng lại nó sau khi gọi
phương thức `can_hold`. Giá trị trả về của `can_hold` sẽ là Boolean và quá trình
triển khai sẽ kiểm tra xem các giá trị chiều rộng và chiều cao của `self` có lớn
hơn chiều rộng và chiều cao của `Rectangle` khác hay không. Hãy thêm phương thức
`can_hold` mới vào khối `impl` từ Liệt kê 5-13, nội dung giống như trong Liệt kê
5-15.

<span class="filename">Tên tệp: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-relative-data/listing-05-15/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-15: Triển khai phương thức `can_hold` trên `Rectangle` lấy một phiên bản `Rectangle` khác làm tham số</span>

Khi chạy mã này với hàm `main` trong Liệt kê 5-14, chúng ta sẽ nhận được đầu ra
mong muốn. Các phương thức có thể nhận nhiều tham số mà chúng ta thêm vào khai
báo sau tham số `self` và các tham số đó hoạt động giống như các tham số trong
hàm.

### Các Hàm gắn kết

Tất cả các hàm được định nghĩa trong một khối `impl` được gọi là *các hàm liên
kết* vì chúng được liên kết với kiểu được đặt tên sau từ khóa `impl`. Chúng ta
có thể định nghĩa các hàm được liên kết không có `self` làm tham số đầu tiên (và
vì thế nó không phải là phương thức) vì chúng không cần một nhân bản của kiểu để
hoạt động. Chúng ta đã sử dụng một hàm như sau: hàm `String::from` được xác định
trên loại `String`.

Các hàm liên kết không phải là cách thường được sử dụng cho các hàm tạo sẽ trả
về một phiên bản mới của cấu trúc. Chúng thường được gọi là `new`, nhưng `new`
không phải là tên đặc biệt và không được tích hợp sẵn trong ngôn ngữ. Ví dụ:
chúng ta có thể chọn cung cấp một hàm được liên kết có tên `square` sẽ có một
tham số thứ nguyên và sử dụng tham số đó làm cả chiều rộng và chiều cao, do đó
giúp tạo hình vuông `Rectangle` dễ dàng hơn thay vì phải chỉ định cùng một giá
trị hai lần :

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-03-associated-functions/src/main.rs:here}}
```

Từ khóa `Self` trong kiểu được trả về và trong phần thân của hàm là bí danh cho
kiểu xuất hiện sau từ khóa `impl`, trong trường hợp này là `Rectangle`.

Để gọi hàm liên kết này, chúng ta sử dụng cú pháp `::` với tên cấu trúc; `let sq
= Rectangle::square(3);` là một ví dụ. Hàm này được đặt tên theo cấu trúc: cú
pháp `::` được sử dụng cho cả các hàm được liên kết và các vùng tên (namespaces)
được tạo bởi các mô-đun. Chúng ta sẽ thảo luận về các mô-đun trong [Chương
7][modules]<!-- ignore -->.

### Đa khối `impl`

Mỗi cấu trúc được phép có nhiều khối `impl`. Ví dụ: Liệt kê 5-15 tương đương với
mã được hiển thị trong Liệt kê 5-16, có phương thức riêng trong khối `impl` của
riêng nó.

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-16/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-16: Viết lại Liệt kê 5-15 bằng cách sử dụng nhiều khối `impl`</span>

Không có lý do gì để tách các phương thức này thành nhiều khối `impl` ở đây,
nhưng đây là cú pháp hợp lệ. Chúng ta sẽ thấy một trường hợp trong đó nhiều khối
`impl` hữu ích trong Chương 10, nơi chúng ta thảo luận về các kiểu và các đặc
trưng (trait) chung.

## Tổng kết chương

các cấu trúc cho phép bạn tạo ra các kiểu tùy biến có ý nghĩa đối với vùng dữ
liệu của mình. Bằng cách sử dụng các cấu trúc, bạn có thể giữ cho các phần dữ
liệu liên kết được kết nối với nhau và đặt tên cho từng phần để làm cho mã của
bạn rõ ràng. Trong các khối `impl`, bạn có thể xác định các hàm được liên kết
với kiểu dữ liệu của mình và các phương thức là một loại hàm được liên kết cho
phép bạn chỉ định hành vi mà các nhân bản cấu trúc của bạn có.

Nhưng cấu trúc không phải là cách duy nhất để bạn có thể tạo các kiểu tùy biến:
hãy chuyển sang tính năng enum của Rust để thêm một công cụ khác vào bộ công cụ
của bạn.

[enums]: ch06-00-enums.html
[trait-objects]: ch17-02-trait-objects.md
[public]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html