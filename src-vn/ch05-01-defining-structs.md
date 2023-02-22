## Định nghĩa và Khởi tạo Cấu trúc

Cấu trúc tương tự như bộ dữ liệu, được thảo luận trong phần [Kiểu Bộ dữ
liệu”][tuples]<!-- ignore -->, trong đó cả hai đều chứa nhiều giá trị liên quan.
Giống như các bộ dữ liệu, các phần của cấu trúc có thể là các loại khác nhau.
Không giống như với các bộ dữ liệu, trong một cấu trúc, bạn sẽ đặt tên cho từng
phần dữ liệu để hiểu rõ ý nghĩa của các giá trị. Việc thêm các tên này có nghĩa
là các cấu trúc linh hoạt hơn các bộ dữ liệu: bạn không cần phải dựa vào thứ tự
của dữ liệu để chỉ định hoặc truy cập các giá trị của một thể hiện.

Để định nghĩa một struct, chúng ta nhập từ khóa `struct` và đặt tên cho toàn bộ
struct. Tên của cấu trúc sẽ mô tả tầm quan trọng của các phần dữ liệu được nhóm
lại với nhau. Sau đó, bên trong dấu ngoặc nhọn, chúng tôi xác định tên và loại
của các phần dữ liệu mà chúng tôi gọi là *trường*. Ví dụ, Liệt kê 5-1 hiển thị
một cấu trúc lưu trữ thông tin về tài khoản người dùng.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-01/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-1: Định nghĩa cấu trúc `Người dùng`</span>

Để sử dụng một cấu trúc sau khi chúng tôi đã xác định nó, chúng tôi tạo một
*thực thể (instance)* của cấu trúc đó bằng cách chỉ định các giá trị cụ thể cho từng
trường. Chúng tôi tạo một thực thể bằng cách nêu tên của cấu trúc và sau đó thêm
dấu ngoặc nhọn chứa các cặp *key: value*, trong đó các khóa là tên của các
trường và các giá trị là dữ liệu chúng tôi muốn lưu trữ trong các trường đó.
Chúng tôi không phải chỉ định các trường theo cùng thứ tự mà chúng tôi đã khai
báo chúng trong cấu trúc. Nói cách khác, định nghĩa cấu trúc giống như một mẫu
chung cho loại và các phiên bản điền vào mẫu đó với dữ liệu cụ thể để tạo các
giá trị của loại. Ví dụ, chúng ta có thể khai báo một người dùng cụ thể như
trong Liệt kê 5-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-02/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-2: Tạo một thực thể của cấu trúc `User`
</span>

Để lấy một giá trị cụ thể từ một cấu trúc, chúng ta sử dụng ký hiệu dấu chấm. Ví
dụ: để truy cập địa chỉ email của người dùng này, chúng ta sử dụng
`user1.email`. Nếu là một thực thể khả biến, chúng ta có thể thay đổi giá trị
bằng cách sử dụng ký hiệu dấu chấm và gán vào một trường cụ thể. Liệt kê 5-3 cho
thấy cách thay đổi giá trị trong trường `email` của một thực thể `User` khả
biến.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-relative-data/listing-05-03/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-3: Thay đổi giá trị trong trường `email` của một Ví dụ `Người dùng`</span>

Lưu ý rằng toàn bộ thực thể phải là khả biến; Rust không cho phép chúng ta chỉ
đánh dấu một số trường nhất định là khả biến. Cũng như bất kỳ biểu thức nào,
chúng ta có thể xây dựng một thực thể mới của cấu trúc làm biểu thức cuối cùng
trong thân hàm để trả về thực thể mới đó một cách hàm súc.

Liệt kê 5-4 cho thấy một hàm `build_user` trả về một thể hiện `User` với email
và tên người dùng đã cho. Trường `active` nhận giá trị `true` và `sign_in_count`
nhận giá trị `1`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-04/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-4: Hàm `build_user` nhận email và tên người dùng và trả về thực thể `User`</span>

Thật hợp lý khi đặt tên cho các tham số của hàm cùng tên với các trường của cấu
trúc, nhưng việc phải lặp lại các tên và biến của trường `email` và `username`
sẽ hơi tẻ nhạt. Nếu cấu trúc có nhiều trường hơn, việc lặp lại từng tên sẽ càng
khó chịu hơn. May mắn thay, có một cách rút gọn thuận tiện!

<!-- Old heading. Do not remove or links may break. -->
<a id="using-the-field-init-shorthand-when-variables-and-fields-have-the-same-name"></a>

### Sử dụng Khởi tạo trường rút gọn

Bởi vì tên tham số và tên trường cấu trúc hoàn toàn giống nhau trong Liệt kê
5-4, nên chúng ta có thể sử dụng cú pháp *khởi tạo trường rút gọn (field init
shorthand)* để viết lại `build_user` để nó hoạt động giống hệt nhau nhưng không
lặp lại `tên người dùng ` và `email`, như trong Liệt kê 5-5.


<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-05/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-5: Một hàm `build_user` sử dụng khởi tạo rút gọn của trường vì các tham số `username` và `email` có cùng tên với các trường cấu trúc</span>

Ở đây, chúng ta đang tạo một thực thể mới của cấu trúc `User`, có trường có tên
là `email`. Chúng ta muốn đặt giá trị của trường `email` thành giá trị trong
tham số `email` của hàm `build_user`. Vì trường `email` và tham số `email` có
cùng tên nên chúng ta chỉ cần viết `email` chứ không phải `email: email`.

### Tạo Thực thể từ các Thực thể khác với cú pháp Cập nhật Cấu trúc

Việc tạo một thực thể mới của cấu trúc bao gồm hầu hết các giá trị từ một thực
thể khác thường rất hữu ích, nhưng có một số thay đổi . Bạn có thể làm điều này
bằng cách sử dụng *cú pháp cập nhật cấu trúc*.

Đầu tiên, trong Liệt kê 5-6, chúng tôi trình bày cách thông thường để tạo một
phiên bản `User` mới trong `user2` mà không cần cú pháp cập nhật. Chúng ta đặt
một giá trị mới cho `email` nhưng nếu không thì ta sử dụng cùng các giá trị từ
`user1` mà chúng ta đã tạo trong Liệt kê 5-2.

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-06/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-6: Tạo một thể hiện `User` mới bằng cách sử dụng một trong các giá trị từ `user1`</span>

Sử dụng cú pháp cập nhật cấu trúc, chúng ta có thể đạt được hiệu quả tương tự
với ít mã hơn, như trong Liệt kê 5-7. Cú pháp `..` chỉ định rằng các trường còn
lại không được đặt rõ ràng phải có cùng giá trị với các trường trong thực thể
nguồn.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-07/src/main.rs:here}}
```

<span class="caption">Liệt kê 5-7: Sử dụng cú pháp cập nhật cấu trúc để đặt một giá trị `email` mới cho một thể hiện `User` nhưng để sử dụng các giá trị còn lại từ `người dùng1`</span>

Mã trong Liệt kê 5-7 cũng tạo một thực thể trong `user2` có giá trị
trường`email` khác nhưng có cùng giá trị cho các trường `username`, `active` và
`sign_in_count` từ `user1`. Thực thể `..user1` phải xuất hiện sau cùng để xác
định rằng mọi trường còn lại sẽ nhận giá trị của chúng từ các trường tương ứng
trong `user1`, nhưng chúng ta có thể chọn chỉ định giá trị cho bao nhiêu trường
tùy thích theo bất kỳ thứ tự nào, bất kể thứ tự của các trường trong định nghĩa
của cấu trúc.

Lưu ý rằng cú pháp cập nhật cấu trúc sử dụng `=` như một phép gán; điều này là
do nó di chuyển dữ liệu, giống như chúng ta đã thấy trong phần [“Biến và dữ liệu
tương tác với việc di chuyển”][move]<!-- ignore -->. Trong ví dụ này, chúng tôi
không còn có thể sử dụng toàn bộ `user1` sau khi tạo `user2` vì `String` trong
trường `username` của `user1` đã được chuyển vào `user2`. Nếu chúng tôi đã cung
cấp các giá trị `String` mới cho `user2` cho cả `email` và `username`, và do đó
chỉ sử dụng các giá trị `active` và `sign_in_count` từ `user1`, thì `user1` sẽ
vẫn hợp lệ sau khi tạo `user2`. Cả `active` và `sign_in_count` đều là các kiểu
khai triển đặc trưng `Copy`, vì vậy hành vi mà chúng ta đã thảo luận trong phần
[“Dữ liệu chỉ có trong ngăn xếp: Sao chép”][Copy]<!-- ignore --> sẽ được áp
dụng.

### Sử dụng Cấu trúc Tuple không tên Trường để tạo các Kiểu dữ liệu khác

Rust cũng hỗ trợ các cấu trúc trông tương tự như các bộ dữ liệu, được gọi là
*cấu trúc bộ (tuple structs)*. Các Cấu trúc bộ có ý nghĩa bổ sung mà cấu trúc
Tên cung cấp nhưng không có tên được liên kết với các trường của chúng; thay vào
đó, chúng chỉ có các trường kiểu dữ liệu. Các cấu trúc bộ dữ liệu hữu ích khi
bạn muốn đặt tên cho toàn bộ bộ dữ liệu và biến bộ dữ liệu thành một kiểu khác
với các bộ dữ liệu khác và khi đặt tên cho từng trường như trong một cấu trúc
thông thường sẽ dài dòng hoặc dư thừa.

Để xác định cấu trúc bộ dữ liệu, hãy bắt đầu với từ khóa `struct` và tên cấu
trúc theo sau là các kiểu trong bộ dữ liệu. Ví dụ: ở đây chúng tôi xác định và
sử dụng hai cấu trúc bộ có tên `Color` và `Point`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-01-tuple-structs/src/main.rs}}
```

Lưu ý rằng các giá trị `black` và `origin` là các kiểu khác nhau vì chúng là các
thực thể của các cấu trúc bộ khác nhau. Mỗi cấu trúc bạn định nghĩa là kiểu
riêng của nó, mặc dù các trường trong cấu trúc có thể có cùng kiểu. Ví dụ: một
hàm lấy tham số thuộc loại `Color` không thể lấy `Point` làm đối số, mặc dù cả
hai loại đều được tạo thành từ ba giá trị `i32`. Mặt khác, các phiên bản cấu
trúc bộ dữ liệu tương tự như bộ dữ liệu ở chỗ bạn có thể phân rã cấu trúc của
chúng thành các phần riêng lẻ và bạn có thể sử dụng `.` theo sau chỉ mục để truy
cập một giá trị riêng lẻ.

### Cấu trúc giống như đơn vị không có bất kỳ trường nào

Bạn cũng có thể xác định các cấu trúc không có bất kỳ trường nào! Chúng được gọi
là *cấu trúc giống đơn vị* vì chúng hoạt động tương tự như `()`, loại đơn vị mà
chúng tôi đã đề cập trong [“Loại Tuple”][bộ dữ liệu]<!-- bỏ qua --> phần. Các
cấu trúc giống như đơn vị có thể hữu ích khi bạn cần triển khai một đặc điểm
trên một số loại nhưng không có bất kỳ dữ liệu nào mà bạn muốn lưu trữ trong
chính loại đó. Chúng ta sẽ thảo luận về các đặc điểm trong Chương 10. Đây là một
ví dụ về khai báo và khởi tạo một cấu trúc đơn vị có tên `AlwaysEqual`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-04-unit-like-structs/src/main.rs}}
```

Để định nghĩa `AlwaysEqual`, chúng ta sử dụng từ khóa `struct`, tên chúng ta
muốn và sau đó là dấu chấm phẩy. Không cần dấu ngoặc nhọn hoặc dấu ngoặc đơn!
Sau đó, chúng ta có thể nhận được một thực thể của `AlwaysEqual` trong biến
`subject` theo cách tương tự: sử dụng tên mà chúng ta đã xác định, không có bất
kỳ dấu ngoặc nhọn hoặc dấu ngoặc đơn nào. Hãy tưởng tượng rằng sau này chúng ta
sẽ triển khai hành vi cho kiểu dữ liệu này sao cho mọi thực thể của
`AlwaysEqual` luôn tương đương với mọi thực thể của bất kỳ kiểu dữ liệu nào
khác, có lẽ để có kết quả đã biết cho mục đích thử nghiệm. Chúng ta sẽ không cần
bất kỳ dữ liệu nào để thực hiện hành vi đó! Bạn sẽ thấy trong Chương 10 cách xác
định các đặc điểm và triển khai chúng trên bất kỳ kiểu nào, kể cả các cấu trúc
giống như đơn vị.

> ### Quyền Sở hữu của Dữ liệu Cấu trúc
>
> Trong định nghĩa cấu trúc `User` trong Liệt kê 5-1, chúng ta đã sử dụng kiểu 
> chuỗi đã sở hữu `String` thay vì kiểu chuỗi cắt lát `&str`. Đây là một lựa 
> chọn có chủ ý bởi vì chúng ta muốn mỗi phiên bản của cấu trúc này sở hữu tất 
> cả dữ liệu của nó và dữ liệu đó có hiệu lực miễn sao toàn bộ cấu trúc hợp lệ.
>
> Cấu trúc cũng có thể lưu trữ tham chiếu đến dữ liệu thuộc sở hữu của thứ gì 
> đó, nhưng để làm như vậy yêu cầu sử dụng *lifetimes*, một tính năng của Rust  
> mà chúng ta sẽ thảo luận trong Chương 10. Vòng đời đảm bảo rằng khi cấu trúc 
> còn tồn tại thì dữ liệu được tham chiếu bởi cấu trúc đó là hợp lệ. Giả sử bạn 
> cố gắng lưu trữ một tham chiếu trong một cấu trúc mà không chỉ định thời gian 
> tồn tại điều này sẽ không hoạt động, ví dụ như sau: 
>
> <span class="filename">Filename: src/main.rs</span>
>
> <!-- CAN'T EXTRACT SEE https://github.com/rust-lang/mdBook/issues/1127 -->
>
> ```rust,ignore,does_not_compile
> struct User {
>     active: bool,
>     username: &str,
>     email: &str,
>     sign_in_count: u64,
> }
>
> fn main() {
>     let user1 = User {
>         active: true,
>         username: "someusername123",
>         email: "someone@example.com",
>         sign_in_count: 1,
>     };
> }
> ```

> Trình biên dịch sẽ phàn nàn rằng nó cần các bộ xác định thời gian tồn tại:

>
> ```console
> $ cargo run
>    Compiling structs v0.1.0 (file:///projects/structs)
> error[E0106]: missing lifetime specifier
>  --> src/main.rs:3:15
>   |
> 3 |     username: &str,
>   |               ^ expected named lifetime parameter
>   |
> help: consider introducing a named lifetime parameter
>   |
> 1 ~ struct User<'a> {
> 2 |     active: bool,
> 3 ~     username: &'a str,
>   |
>
> error[E0106]: missing lifetime specifier
>  --> src/main.rs:4:12
>   |
> 4 |     email: &str,
>   |            ^ expected named lifetime parameter
>   |
> help: consider introducing a named lifetime parameter
>   |
> 1 ~ struct User<'a> {
> 2 |     active: bool,
> 3 |     username: &str,
> 4 ~     email: &'a str,
>   |
> For more information about this error, try `rustc --explain E0106`.
> error: could not compile `structs` due to 2 previous errors
> ```
>
> Trong Chương 10, chúng ta sẽ thảo luận cách khắc phục những lỗi này để bạn có 
> thể lưu trữ tham chiếu trong cấu trúc, nhưng hiện tại, chúng tôi sẽ sửa các 
> lỗi như thế này bằng cách sử dụng các kiểu sở hữu như `String` thay vì tham 
> chiếu như `&str`.

[tuples]: ch03-02-data-types.html#the-tuple-type
[move]: ch04-01-what-is-ownership.html#variables-and-data-interacting-with-move
[copy]: ch04-01-what-is-ownership.html#stack-only-data-copy