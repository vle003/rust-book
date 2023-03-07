## Lưu trữ Danh sách các Giá trị bằng Vectơ

Loại bộ sưu tập đầu tiên mà chúng ta sẽ xem xét là `Vec<T>`, còn được gọi là
*vector*. Vectơ cho phép bạn lưu trữ nhiều giá trị trong một cấu trúc dữ liệu
đặt tất cả các giá trị cạnh nhau trong bộ nhớ. Các vectơ chỉ có thể lưu trữ các
giá trị cùng loại. Chúng rất hữu ích khi bạn có một danh sách các đề mục, chẳng
hạn như các dòng văn bản trong tệp hoặc giá của các mặt hàng trong giỏ hàng.

### Tạo một vectơ mới

Để tạo một vectơ trống mới, chúng ta gọi hàm `Vec::new`, như trong Liệt kê 8-1.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-01/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-1: Tạo một vectơ trống mới để giữ các giá trị của loại `i32`</span>

Lưu ý rằng chúng ta đã thêm một chú thích kiểu ở đây. Bởi vì chúng ta không chèn
bất kỳ giá trị nào vào vectơ này, Rust không biết kiểu phần tử nào chúng ta dự
định lưu trữ. Đây là một điểm quan trọng. Các vectơ được thực hiện bằng cách sử
dụng các chủng loại (generic); chúng ta sẽ giới thiệu cách sử dụng chủng loại
với các kiểu của riêng bạn trong Chương 10. Hiện tại, hãy biết rằng kiểu
`Vec<T>` do thư viện chuẩn cung cấp có thể chứa bất kỳ kiểu dữ liệu nào. Khi
chúng ta tạo một vectơ để lưu một kiểu dữ liệu cụ thể, chúng ta có thể chỉ định
kiểu trong cặp dấu <>. Trong Liệt kê 8-1, chúng ta đã nói với Rust rằng `Vec<T>`
trong `v` sẽ chứa các phần tử thuộc loại `i32`.

Thường xuyên hơn, bạn sẽ tạo một `Vec<T>` với các giá trị ban đầu và Rust sẽ suy
ra kiểu giá trị mà bạn muốn lưu trữ, vì vậy bạn hiếm khi cần thực hiện chú thích
loại này. Rust cung cấp macro `vec!` một cách tiện lợi, macro này sẽ tạo ra một
vectơ mới chứa các giá trị mà bạn cung cấp cho nó. Liệt kê 8-2 tạo một
`Vec<i32>` mới chứa các giá trị `1`, `2` và `3`. Kiểu số nguyên là `i32` vì đó
là kiểu số nguyên mặc định, như chúng ta đã thảo luận trong phần [“Kiểu dữ
liệu”][data-types]<!-- ignore --> của Chương 3.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-02/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-2: Tạo một vectơ mới chứa giá trị</span>

Bởi vì chúng ta đã cung cấp các giá trị `i32` ban đầu, Rust có thể suy luận rằng
loại `v` là `Vec<i32>` và chú thích loại là không cần thiết. Tiếp theo, chúng ta
sẽ xem cách sửa đổi một vectơ.

### Cập nhật Vector

Để tạo một vectơ và sau đó thêm các phần tử vào nó, chúng ta có thể sử dụng
phương thức `push`, như trong Liệt kê 8-3.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-03/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-3: Sử dụng phương thức `push` để thêm các giá trị vào một vectơ</span>

Như với bất kỳ biến nào, có thể thay đổi giá trị của nó nếu chúng ta muốn, chúng
ta cần khai báo biến đó khả chuyển bằng cách sử dụng từ khóa `mut`, như đã thảo
luận trong Chương 3. Các số chúng ta đặt bên trong đều thuộc loại `i32` và Rust
suy ra điều này từ dữ liệu, vì vậy chúng ta không cần chú thích `Vec<i32>`.

### Đọc các phần tử của vectơ

Có hai cách để tham chiếu một giá trị được lưu trữ trong vectơ: thông qua lập
chỉ mục hoặc sử dụng phương thức `get`. Trong các ví dụ sau, chúng ta đã chú
thích các loại giá trị được trả về từ các hàm này để làm rõ hơn.

Liệt kê 8-4 cho thấy cả hai phương pháp truy cập một giá trị trong một vectơ,
với cú pháp lập chỉ mục và phương thức `get`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-04/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-4: Sử dụng cú pháp lập chỉ mục hoặc phương thức
`get` để truy cập một mục trong vectơ</span>

Lưu ý một vài chi tiết ở đây. Chúng ta sử dụng giá trị chỉ mục của `2` để lấy
phần tử thứ ba vì các vectơ được lập chỉ mục theo số, bắt đầu từ 0. Việc sử dụng
`&` và `[]` cho chúng ta tham chiếu đến phần tử ở giá trị chỉ mục. Khi chúng ta
sử dụng phương thức `get` với chỉ mục được truyền dưới dạng đối số, chúng ta
nhận được kết quả kiểu `Option<&T>` mà chúng ta có thể sử dụng với `match`.

Lý do Rust cung cấp hai cách này để tham chiếu một phần tử là để bạn có thể chọn
cách chương trình hoạt động khi bạn cố gắng sử dụng một giá trị chỉ mục bên
ngoài phạm vi của các phần tử hiện có. Ví dụ, hãy xem điều gì sẽ xảy ra khi
chúng ta có một vectơ gồm năm phần tử và sau đó chúng ta cố gắng truy cập một
phần tử ở chỉ số 100 với mỗi kỹ thuật, như trong Liệt kê 8-5.

```rust,should_panic,panics
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-05/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-5: Cố gắng truy cập phần tử ở chỉ số 100 trong một vectơ chứa năm phần tử</span>

Khi chúng ta chạy mã này, phương thức `[]` đầu tiên sẽ khiến chương trình `phát
rồ` (panic) vì nó tham chiếu đến một phần tử không tồn tại. Phương pháp này là
cách tốt nhất khi bạn *muốn chương trình của mình gặp sự cố* nếu cố truy cập một
phần tử vượt quá số phẩn tử giới hạn của vectơ.

Khi phương thức `get` được truyền một chỉ mục nằm ngoài vectơ, nó sẽ trả về
`None` mà không bị hoảng loạn. Bạn sẽ sử dụng phương pháp này nếu việc truy cập
một phần tử nằm ngoài phạm vi của vectơ đôi khi có thể xảy ra trong các trường
hợp bình thường. Sau đó, mã của bạn sẽ có logic để xử lý việc có giá trị
`Some(&element)` hoặc `None`, như đã thảo luận trong Chương 6. Ví dụ: chỉ mục có
thể đến từ một người dùng nhập vào. Nếu họ vô tình nhập một số quá lớn và chương
trình nhận được giá trị `None`, thì bạn có thể cho người dùng biết có bao nhiêu
mục trong vectơ hiện tại và cho họ một cơ hội khác để nhập giá trị hợp lệ. Điều
đó sẽ thân thiện với người dùng hơn làchương trình bị dừng hoạt động do lỗi đánh
máy!

Khi chương trình có một tham chiếu hợp lệ, trình kiểm tra mượn quyền (borrow
checker) áp dụng quyền sở hữu và các quy tắc mượn (được đề cập trong Chương 4)
để đảm bảo tham chiếu này và bất kỳ tham chiếu nào khác đến nội dung của vectơ
vẫn hợp lệ. Nhớ lại quy tắc nói rằng bạn không thể có các tham chiếu khả biến và
bất biến trong cùng một phạm vi. Quy tắc đó được áp dụng trong Liệt kê 8-6, nơi
chúng ta giữ một tham chiếu bất biến đến phần tử đầu tiên trong một vectơ và cố
gắng thêm một phần tử vào cuối. Chương trình này sẽ không hoạt động nếu sau đó
chúng ta cũng cố gắng tham chiếu đến phần tử đó trong hàm:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-06/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-6: Cố gắng thêm một phần tử vào một vectơ trong khi giữ tham chiếu đến một mục</span>

Biên dịch mã này sẽ dẫn đến lỗi này:

```console
{{#include ../listings/ch08-common-collections/listing-08-06/output.txt}}
```

Đoạn mã trong Liệt kê 8-6 có vẻ như sẽ hoạt động: tại sao một tham chiếu đến
phần tử đầu tiên lại quan tâm đến những thay đổi ở phần cuối của vectơ? Lỗi này
là do cách hoạt động của vectơ: vì vectơ đặt các giá trị cạnh nhau trong bộ nhớ,
nên việc thêm một phần tử mới vào cuối vectơ có thể yêu cầu cấp phát bộ nhớ mới
và sao chép các phần tử cũ sang không gian mới, nếu có Không đủ chỗ để đặt tất
cả các phần tử cạnh nhau tại nơi lưu trữ vectơ hiện tại. Trong trường hợp đó,
tham chiếu đến phần tử đầu tiên sẽ trỏ đến bộ nhớ đã giải phóng. Các quy tắc
mượn ngăn chặn các chương trình kết thúc trong tình huống đó.

> Lưu ý: Để biết thêm chi tiết triển khai của loại `Vec<T>`, hãy xem [“The >
> Rustonomicon”][nomicon].

### Duyệt Tuần tự các giá trị trong một vectơ

Để truy cập lần lượt từng phần tử trong một vectơ, chúng ta sẽ duyệt qua tất cả
các phần tử thay vì sử dụng các chỉ mục để truy cập từng phần tử một. Liệt kê
8-7 cho thấy cách sử dụng vòng lặp `for` để lấy các tham chiếu bất biến cho từng
phần tử trong một vectơ gồm các giá trị `i32` và in chúng.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-07/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-7: In mỗi phần tử trong một vectơ bằng cách lặp qua các phần tử bằng vòng lặp `for`</span>

Chúng ta cũng có thể lặp lại các tham chiếu khả biến đối với từng phần tử trong
một vectơ khả biến để thực hiện các thay đổi đối với tất cả các phần tử. Vòng
lặp `for` trong Liệt kê 8-8 sẽ thêm `50` vào mỗi phần tử.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-08/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-8: Lặp lại các tham chiếu có thể thay đổi tới các phần tử trong một vectơ</span>

Để thay đổi giá trị mà tham chiếu khả biến đề cập đến, chúng ta phải sử dụng
toán tử tham trị (dereference) `*` để lấy giá trị trong `i` trước khi chúng ta
có thể sử dụng toán tử `+=`. Chúng ta sẽ nói nhiều hơn về toán tử tham trị trong
phần [“Theo sau con trỏ tới giá trị bằng toán tử tham trị”][deref]<!-- ignore
--> của Chương 15.

Việc duyệt qua một véc-tơ, dù cố định hay có thể thay đổi, đều an toàn do các
quy tắc của trình kiểm tra mượn (borrow checker). Nếu chúng ta cố chèn hoặc xóa
các mục trong thân vòng lặp `for` như trong Liệt kê 8-7 và Liệt kê 8-8, thì
chúng ta sẽ gặp lỗi trình biên dịch tương tự như lỗi mà chúng ta gặp phải với mã
trong Liệt kê 8-6. Tham chiếu đến vectơ mà vòng lặp `for` sử dụng sẽ ngăn việc
sửa đổi đồng thời toàn bộ vectơ.

### Sử dụng Enum để lưu trữ nhiều kiểu dữ liệu

Các vectơ chỉ có thể lưu trữ các giá trị cùng loại. Điều này có thể bất tiện;
chắc chắn có những trường hợp sử dụng cần lưu trữ danh sách các mục thuộc các
kiểu khác nhau. May mắn thay, các biến thể của một enum được định nghĩa theo
cùng một kiểu enum, vì vậy khi chúng ta cần một kiểu để biểu diễn các phần tử
thuộc các kiểu khác nhau, chúng ta có thể định nghĩa và sử dụng một enum!

Ví dụ: giả sử chúng ta muốn nhận giá trị từ một hàng trong bảng tính trong đó
một số cột trong hàng chứa số nguyên, một số số dấu phẩy động và một số chuỗi.
Chúng ta có thể định nghĩa một enum có các biến thể sẽ chứa các loại giá trị
khác nhau và tất cả các biến thể của enum sẽ được coi là cùng một loại: đó là
của enum. Sau đó, chúng ta có thể tạo một vectơ để giữ enum đó và cuối cùng, lưu
giữ các loại khác nhau. Chúng ta chứng minh điều này trong Liệt kê 8-9.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-09/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-9: Định nghĩa một `enum` để lưu trữ các giá trị thuộc các loại khác nhau trong một vectơ</span>

Rust cần biết những kiểu nào sẽ có trong vectơ tại thời điểm biên dịch để nó
biết chính xác cần bao nhiêu bộ nhớ trên heap để lưu trữ từng phần tử. Chúng ta
cũng phải minh xác về những kiểu được phép trong vectơ này. Nếu Rust cho phép
một vectơ lưu giữ bất kỳ kiểu nào, sẽ có khả năng một hoặc nhiều kiểu sẽ gây ra
lỗi với các thao tác được thực hiện trên các phần tử của vectơ. Sử dụng một enum
cộng với một biểu thức `match`, như đã thảo luận trong Chương 6, có nghĩa là
Rust tại thời điểm biên dịch sẽ đảm bảo rằng mọi trường hợp có thể xảy ra đều
được xử lý.

Nếu bạn không biết tập hợp đầy đủ các kiểu mà một chương trình sẽ có trong thời
gian chạy để lưu trữ trong một vectơ, thì kỹ thuật enum sẽ không hoạt động. Thay
vào đó, bạn có thể sử dụng một đối tượng trait (đặc điểm) mà chúng ta sẽ trình
bày trong Chương 17.

Bây giờ chúng ta đã thảo luận về một số cách phổ biến nhất để sử dụng vectơ, hãy
nhớ xem lại [tài liệu API][vec-api]<!-- ignore --> để biết tất cả về nhiều
phương thức hữu ích được khai báo trên `Vec<T >` bởi thư viện tiêu chuẩn. Ví dụ:
ngoài `push`, phương thức `pop` sẽ xóa và trả về phần tử cuối cùng.

### Loại bỏ một Vector Loại bỏ các phần tử của nó

Giống như bất kỳ `struct` nào khác, một vectơ được giải phóng khi nó nằm ngoài
phạm vi, như được chú thích trong Liệt kê 8-10.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-10/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-10: Hiển thị vị trí của vectơ và các phần tử của
nó</span>

Khi vectơ bị loại bỏ, tất cả nội dung của nó cũng bị loại bỏ, nghĩa là các số
nguyên mà nó chứa sẽ bị xóa sạch. Trình kiểm tra mượn đảm bảo rằng mọi tham
chiếu đến nội dung của vectơ chỉ được sử dụng khi bản thân vectơ đó hợp lệ.

Hãy chuyển sang kiểu collection tiếp theo: `String`!

[data-types]: ch03-02-data-types.html#data-types
[nomicon]: ../nomicon/vec/vec.html
[vec-api]: ../std/vec/struct.Vec.html
[deref]: ch15-02-deref.html#following-the-pointer-to-the-value-with-the-dereference-operator