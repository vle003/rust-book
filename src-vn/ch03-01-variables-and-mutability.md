## Biến và Khả năng thay đổi

Như đã đề cập trong phần [“Lưu trữ giá trị với biến”][storing-values-with-variables]<!-- ignore -->, theo mặc định, các biến là bất biến. Đây là một trong nhiều cú huých mà Rust cung cấp cho bạn để viết mã theo cách đồng thời tận dụng được tính an toàn cùng với tính dễ dàng mà Rust cung cấp. Tuy nhiên, bạn vẫn có lựa chọn để các biến của mình có thể thay đổi được.
Hãy khám phá cách thức và lý do tại sao Rust khuyến khích bạn ưu tiên tính bất biến và tại sao đôi khi bạn có thể muốn từ chối.

Khi một biến là bất biến, khi một giá trị được liên kết với một tên, bạn không thể thay đổi giá trị đó. Để minh họa điều này, hãy tạo một dự án mới tên là *variables* trong thư mục *projects* của bạn bằng cách sử dụng `cargo new variables`.

Sau đó, trong thư mục *variables* mới của bạn, hãy mở *src/main.rs* và thay thế mã của nó bằng mã sau, mã này sẽ không biên dịch được:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

Lưu và chạy chương trình bằng `cargo run`. Bạn sẽ nhận được một thông báo lỗi liên quan đến lỗi không thể thay đổi, như thể hiện trong kết quả này:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

Ví dụ này cho thấy trình biên dịch giúp bạn tìm lỗi trong chương trình của mình như thế nào.
Lỗi trình biên dịch có thể gây khó chịu, nhưng thực sự chúng chỉ có nghĩa là chương trình của bạn chưa thực hiện những gì bạn muốn một cách an toàn; chúng *không* nghĩa là bạn không phải là một lập trình viên giỏi! Rustacean có kinh nghiệm vẫn gặp lỗi trình biên dịch.

Bạn nhận được thông báo lỗi `` cannot assign twice to immutable variable `x` `` bởi vì bạn đã cố gán giá trị mới cho biến `x`.

Điều quan trọng là chúng ta gặp phải lỗi trong lúc biên dịch khi cố gắng thay đổi một giá trị được khai báo là không thay đổi vì chính tình huống này có thể dẫn đến lỗi. Nếu một phần trong mã của chúng ta hoạt động dựa trên giả định rằng một giá trị sẽ không bao giờ thay đổi và một phần khác trong mã của chúng ta sẽ thay đổi giá trị đó, thì có thể phần đầu tiên của mã sẽ không thực hiện những gì nó được thiết kế để làm. Nguyên nhân của loại lỗi này có thể khó theo dõi sau khi xảy ra trong thực tế, đặc biệt là khi đoạn mã thứ hai chỉ *đôi khi* thay đổi giá trị. Trình biên dịch Rust đảm bảo rằng khi bạn khai báo rằng một giá trị sẽ không thay đổi, thì nó thực sự sẽ không thay đổi, vì vậy bạn không cần phải tự mình theo dõi nó. Do đó, mã của bạn sẽ hợp lý hơn.

Nhưng khả năng thay đổi có thể rất hữu ích và có thể giúp viết mã thuận tiện hơn.

Mặc dù các biến là bất biến theo mặc định, nhưng bạn có thể khiến cho chúng có thể thay đổi được bằng cách thêm `mut` vào trước tên biến như bạn đã làm trong [Chương 2][storing-values-with-variables]<!-- ignore -->. Việc thêm `mut` cũng truyền tải ý định tới những người đọc mã trong tương lai bằng cách chỉ ra rằng các phần khác của mã sẽ thay đổi giá trị của biến này.

Ví dụ: hãy thay đổi *src/main.rs* như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

Bây giờ khi chạy chương trình, chúng ta sẽ nhận được:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```

Chúng ta được phép thay đổi giá trị được liên kết thành `x` từ `5` thành `6` khi `mut` được sử dụng. Cuối cùng, việc quyết định có sử dụng khả năng thay đổi hay không là tùy thuộc vào bạn và phụ thuộc vào những gì bạn cho là rõ ràng nhất trong mỗi tình huống cụ thể.

### Hằng số

Giống như biến bất biến, *hằng* là các giá trị được gắn với tên và không được phép thay đổi, nhưng có một số khác biệt giữa hằng và biến.

Đầu tiên, bạn không được phép sử dụng `mut` với các hằng số. Các hằng số không chỉ là bất biến theo mặc định—chúng luôn luôn là bất biến. Bạn khai báo các hằng bằng cách sử dụng từ khóa `const` thay vì từ khóa `let` và loại giá trị *phải* được chú thích. Chúng tôi sẽ đề cập đến các loại và nhập chú thích trong phần tiếp theo, [“Data Types”][data-types]<!-- ignore -->, vì vậy, lúc này bạn đừng lo lắng về các chi tiết. Chỉ cần biết rằng bạn phải luôn khia báo các kiểu dữ liệu.

Các hằng số có thể được khai báo trong bất kỳ phạm vi nào, kể cả phạm vi toàn cục, điều này khiến chúng hữu ích cho các giá trị mà nhiều phần khác trong chương trình cần sử dụng nó.

Điểm khác biệt cuối cùng là các hằng số chỉ có thể được đặt thành một biểu thức hằng số, giá trị của nó không phải là kết quả của một tính toán khi chương trình chạy.

Đây là một ví dụ về khai báo hằng số:


```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Tên của hằng số là `THREE_HOURS_IN_SECONDS` và giá trị của nó được đặt thành kết quả của phép nhân 60 (số giây trong một phút) với 60 (số phút trong một giờ) nhân 3 (số giờ chúng tôi muốn đếm trong chương trình này). Quy ước đặt tên của Rust cho các hằng là sử dụng tất cả chữ hoa với gạch dưới giữa các từ. Trình biên dịch có thể đánh giá một tập hợp giới hạn các thao tác tại thời điểm biên dịch, điều này cho phép chúng tôi chọn ghi giá trị này theo cách dễ hiểu và dễ xác minh hơn thay vì đặt hằng số này thành giá trị 10.800. Xem phần [Rust Reference’s section on constant evaluation] [const-eval] để biết thêm thông tin về những thao tác nào có thể được sử dụng khi khai báo hằng số.

Các hằng có giá trị trong toàn bộ thời gian chương trình chạy, trong phạm vi mà chúng được khai báo. Thuộc tính này làm cho các hằng số hữu ích cho các giá trị trong miền ứng dụng của bạn mà nhiều phần của chương trình có thể cần biết, chẳng hạn như số điểm tối đa mà bất kỳ người chơi trò chơi nào được phép kiếm được hoặc tốc độ ánh sáng.

Việc đặt tên cho các giá trị được mã hóa cứng được sử dụng trong toàn bộ chương trình của bạn dưới dạng hằng số rất hữu ích trong việc truyền đạt ý nghĩa của giá trị đó cho những người bảo trì mã trong tương lai. Nó cũng hữu ích khi chỉ có một vị trí trong mã của bạn mà bạn sẽ cần thay đổi nếu giá trị mã hóa cứng cần được cập nhật trong tương lai.

### Che khuất

Như bạn đã thấy trong phần hướng dẫn trò chơi đoán số ở [Chương 2][comparing-the-guess-to-the-secret-number]<!-- ignore -->, bạn có thể khai báo một biến mới có cùng tên với biến trước đó. Rustaceans nói rằng biến đầu tiên bị *che khuất (shadowed)* bởi biến thứ hai, có nghĩa là biến thứ hai là thứ mà trình biên dịch sẽ thấy khi bạn sử dụng tên của biến.
Trên thực tế, biến thứ hai làm lu mờ biến thứ nhất, sử dụng chính nó cho bất kỳ tham chiếu nào cho đến khi chính nó bị che khuất hoặc kết thúc khối lệnh.
Chúng ta có thể ẩn một biến bằng cách sử dụng cùng một tên biến và lặp lại việc sử dụng từ khóa `let` như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```

Đầu tiên, chương trình này gán giá trị `5` cho biến `x`. Sau đó, nó tạo một biến `x` mới bằng cách lặp lại `let x =`, lấy giá trị ban đầu và thêm `1` giá trị của `x` lúc này là `6`. Sau đó, trong một khối lệnh bên trong được tạo ra bằng các dấu ngoặc nhọn, câu lệnh `let` thứ ba cũng *che khuất* `x` và tạo một biến mới, nhân giá trị trước đó với `2` để giá trị của  `x` lúc này là `12`.
Khi khối lệnh đó kết thúc, *che khuất* bên trong kết thúc và `x` trở lại là `6`.
Khi chúng ta chạy chương trình này, nó sẽ xuất ra như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```
Che khuất (shadowing) khác với đánh dấu một biến là `mut` vì chúng ta sẽ gặp lỗi biên dịch nếu vô tình cố gán lại cho biến này mà không sử dụng từ khóa `let`. Bằng cách sử dụng `let`, chúng ta có thể thực hiện một số phép biến đổi trên một giá trị nhưng biến đó không thể thay đổi sau khi các phép biến đổi đó hoàn thành.

Điều khác biệt nữa giữa `mut` và shadowing là vì chúng ta đang tạo một biến mới một cách hiệu quả khi chúng ta sử dụng lại từ khóa `let`, chúng ta có thể thay đổi kiểu dữ liệu của biến nhưng sử dụng lại cùng một tên. Ví dụ: chương trình của chúng ta yêu cầu người dùng cho biết họ muốn có bao nhiêu khoảng trắng giữa một số văn bản bằng cách nhập các ký tự khoảng trắng, sau đó chúng tôi muốn lưu trữ đầu vào đó dưới dạng số:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

Biến `spaces` đầu tiên là một kiểu chuỗi và biến `spaces` thứ hai là một kiểu số. Do đó, việc che khuất giúp chúng ta không phải nghĩ ra các tên khác nhau, chẳng hạn như `spaces_str` và `spaces_num`; thay vào đó, chúng ta có thể sử dụng lại tên `spaces` đơn giản hơn. Tuy nhiên, nếu chúng tôi cố gắng sử dụng `mut` cho việc này, như được hiển thị ở đây, chúng tôi sẽ gặp lỗi biên dịch:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

Lỗi cho biết chúng tôi không được phép thay đổi kiểu của một biến:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

Bây giờ chúng ta đã khám phá cách các biến hoạt động, hãy xem xét thêm các loại dữ liệu mà chúng có thể có.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[const-eval]: ../reference/const_eval.html