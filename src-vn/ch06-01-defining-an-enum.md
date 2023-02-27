## Định nghĩa một Enum

Khi các cấu trúc cung cấp cho bạn cách nhóm các trường và dữ liệu có liên quan
lại với nhau, chẳng hạn như `Rectangle` với `width` và `height` của nó, thì các
enum cung cấp cho bạn cách nói một giá trị là một trong các tập hợp giá trị có
thể có. Ví dụ: chúng ta có thể muốn nói rằng `Rectangle` là một trong tập hợp
các hình dạng có thể có cũng bao gồm `Circle` và `Triangle`. Để làm điều này,
Rust cho phép chúng ta mã hóa các khả năng này dưới dạng enum.

Hãy xem xét một tình huống mà chúng ta có thể muốn diễn đạt bằng mã và xem tại
sao enum lại hữu ích và phù hợp hơn so với cấu trúc trong trường hợp này. Giả sử
chúng ta cần làm việc với địa chỉ IP. Hiện tại, hai tiêu chuẩn chính được sử
dụng cho địa chỉ IP: phiên bản IPv4 và phiên bản IPv6. Bởi vì đây là những khả
năng duy nhất cho một địa chỉ IP mà chương trình của chúng ta sẽ gặp, nên chúng
ta có thể *liệt kê (enumerate)* tất cả các biến thể có thể có, đó là nơi mà phép
liệt kê (enumeration) lấy được tên của nó.

Bất kỳ địa chỉ IP nào cũng có thể là địa chỉ phiên bản IPv4 hoặc phiên bản IPv6,
nhưng không phải cả hai cùng một lúc. Thuộc tính đó của địa chỉ IP làm cho cấu
trúc dữ liệu enum phù hợp vì giá trị enum chỉ có thể là một trong các biến thể
của nó. Cả hai địa chỉ phiên bản IPv4 và phiên bản IPv6 về cơ bản vẫn là địa chỉ
IP, vì vậy chúng phải được coi là cùng một loại khi mã đang xử lý các tình huống
áp dụng cho bất kỳ loại địa chỉ IP nào.

Chúng ta có thể diễn đạt khái niệm này trong mã bằng cách xác định một phép liệt
kê `IpAddrKind` và liệt kê các loại địa chỉ IP có thể có, `V4` và `V6`. Đây là
các biến thể của enum:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:def}}
```

`IpAddrKind` giờ đây một loại dữ liệu tùy biến mà chúng ta có thể sử dụng ở bất
kỳ đâu trong mã của mình.

## Các giá trị Enum
Chúng ta có thể tạo các nhân bản của từng biến thể trong số hai biến thể của
`IpAddrKind` như thế này:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:instance}}
```

Lưu ý rằng các biến thể của enum được đặt tên theo mã định danh của nó và chúng
ta sử dụng dấu hai chấm để phân tách hai biến thể. Điều này rất hữu ích vì bây
giờ cả hai giá trị `IpAddrKind::V4` và `IpAddrKind::V6` đều thuộc cùng một loại:
`IpAddrKind`. Ví dụ, sau đó chúng ta có thể định nghĩa một hàm nhận bất kỳ
`IpAddrKind` nào:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:fn}}
```

Và chúng ta có thể gọi hàm này bằng một trong hai biến thể:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:fn_call}}
```

Sử dụng enums thậm chí còn có nhiều lợi thế hơn. Suy nghĩ thêm về loại địa chỉ
IP của chúng ta, hiện tại chúng ta không có cách nào để lưu trữ *dữ liệu* địa
chỉ IP thực tế; chúng tôi chỉ biết *loại* đó là gì. Vì bạn vừa học về các cấu
trúc trong Chương 5, bạn có thể muốn giải quyết vấn đề này bằng các cấu trúc như
trong Liệt kê 6-1.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-01/src/main.rs:here}}
```

<span class="caption">Liệt kê 6-1: Lưu trữ dữ liệu và biến thể `IpAddrKind` của địa chỉ IP bằng `struct`</span>

Ở đây, chúng ta đã định nghĩa cấu trúc `IpAddr` có hai trường: trường `kind`
thuộc loại `IpAddrKind` (enum mà chúng ta đã định nghĩa trước đó) và trường
`address` có kiểu `String`. Chúng ta có hai trường hợp cho cấu trúc này. 

Đầu tiên là `home` và nó có `kind` là `IpAddrKind::V4` với giá trị là dữ liệu
địa chỉ được gán là `127.0.0.1`. 

Trường hợp thứ hai là `loopback`. Nó là biến thể khác của `IpAddrKind` với giá
trị `kind` của nó là `V6` và có địa chỉ `::1` được gán cho nó. 

Chúng ta đã sử dụng một cấu trúc để kết hợp các giá trị `kind` và `address` với
nhau, vì vậy bây giờ biến thể được gắn với giá trị.

Tuy nhiên, biểu diễn cùng một khái niệm chỉ sử dụng một enum sẽ ngắn gọn hơn:
thay vì một enum bên trong một cấu trúc, chúng ta có thể đưa dữ liệu trực tiếp
vào từng biến thể enum. Định nghĩa mới này về enum `IpAddr` nói rằng cả hai biến
thể `V4` và `V6` sẽ có các giá trị `String` được liên kết:


```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-02-enum-with-data/src/main.rs:here}}
```

Chúng ta gắn dữ liệu trực tiếp vào từng biến thể của enum, vì vậy không cần thêm
cấu trúc. Tại đây, cũng dễ dàng hơn để xem một chi tiết khác về cách thức hoạt
động của enums: tên của mỗi biến thể enum mà chúng ta định nghĩa cũng trở thành
một hàm khởi tạo một nhân bản của enum. Nghĩa là, `IpAddr::V4()` là lệnh gọi hàm
nhận đối số `String` và trả về một nhân bản của loại `IpAddr`. Chúng ta tự động
nhận được hàm khởi tạo này như là kết quả của việc khai báo enum.

Có một lợi thế khác khi sử dụng enum thay vì cấu trúc: mỗi biến thể có thể có
các loại và lượng dữ liệu liên quan khác nhau. Địa chỉ IPv4 sẽ luôn có bốn thành
phần số sẽ có giá trị từ 0 đến 255. Nếu chúng ta muốn lưu trữ địa chỉ `V4` dưới
dạng bốn giá trị `u8` nhưng vẫn biểu thị địa chỉ `V6` dưới dạng một giá trị
`Chuỗi`, chúng ta sẽ không thể thực hiện điều đó với một cấu trúc. Thế nhưng
Enums lại xử lý trường hợp này một cách dễ dàng:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-03-variants-with-different-data/src/main.rs:here}}
```

Chúng ta đã chỉ ra một số cách khác nhau để khai báo cấu trúc dữ liệu để lưu trữ
địa chỉ IPv4 và IPv6. Tuy nhiên, hóa ra, nhu cầu lưu trữ địa chỉ IP và mã hóa
kiểu của chúng phổ biến đến mức [thư viện chuẩn có định nghĩa sẵn mà chúng ta có
thể sử dụng!][IpAddr]<!-- ignore --> Hãy xem cách thư viện chuẩn định nghĩa
`IpAddr`: nó có định nghĩa enum chính xác và các biến thể mà chúng tôi đã xác
định và sử dụng, nhưng nó nhúng dữ liệu địa chỉ bên trong các biến thể ở dạng
hai cấu trúc khác nhau, được xác định khác nhau cho từng biến thể:


```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

Mã này minh họa rằng bạn có thể đặt bất kỳ loại dữ liệu nào bên trong một biến
thể enum: ví dụ: chuỗi, kiểu số hoặc cấu trúc. Thậm chí bạn có thể bao gồm một
enum khác! Ngoài ra, các loại thư viện tiêu chuẩn thường không phức tạp hơn
nhiều so với những gì bạn có thể nghĩ ra.

Lưu ý rằng mặc dù thư viện chuẩn chứa định nghĩa cho `IpAddr`, nhưng chúng tôi
vẫn có thể tạo và sử dụng định nghĩa của riêng mình mà không có xung đột vì
chúng tôi chưa đưa định nghĩa của thư viện chuẩn vào phạm vi của mình. Chúng ta
sẽ nói nhiều hơn về việc đưa các kiểu vào phạm vi sử dụng của chương trình trong
Chương 7.

Hãy xem một ví dụ khác về enum trong Liệt kê 6-2: ví dụ này có rất nhiều loại
được nhúng trong các biến thể của nó.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-02/src/main.rs:here}}
```

<span class="caption">Liệt kê 6-2: Một enum `Message` có các biến thể của mỗi cửa hàng số lượng và loại giá trị khác nhau</span>

Enum này có bốn biến thể với các loại khác nhau:

* `Quit` hoàn toàn không có dữ liệu nào liên quan đến nó.
* `Move` có các trường được đặt tên, giống như cấu trúc.
* `Write` bao gồm một `String` duy nhất.
* `ChangeColor` bao gồm ba giá trị `i32`.

Việc khai báo một enum với các biến thể chẳng hạn như các biến thể trong Liệt kê
6-2 tương tự như khai báo định nghĩa của các loại cấu trúc khác nhau, ngoại trừ
enum không sử dụng từ khóa `struct` và tất cả các biến thể được nhóm lại với
nhau theo loại `Message`. Các cấu trúc sau đây có thể chứa cùng dữ liệu mà các
biến thể enum trước đó lưu giữ:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-04-structs-similar-to-message-enum/src/main.rs:here}}
```

Nhưng nếu chúng ta sử dụng các cấu trúc khác nhau, mỗi cấu trúc có kiểu riêng,
thì chúng ta không thể dễ dàng xác định một hàm để nhận bất kỳ loại thông báo
nào như chúng ta có thể làm với enum `Message` được định nghĩa trong Liệt kê
6-2, nó là một kiểu duy nhất.

Có một điểm tương đồng nữa giữa enums và struct: giống như chúng ta có thể định
nghĩa các phương thức trên struct bằng cách sử dụng `impl`, chúng ta cũng có thể
định nghĩa các phương thức trên enums. Đây là một phương thức có tên `call` mà
chúng ta có thể định nghĩa trên enum `Message` của mình:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-05-methods-on-enums/src/main.rs:here}}
```

Phần thân của phương thức sẽ sử dụng `self` để lấy giá trị mà chúng ta dã gọi
trên phương thức. Trong ví dụ này, chúng ta đã tạo một biến `m` có giá trị
`Message::Write(String::from("hello"))`, và đó là nội dung `self` sẽ có trong
phần thân của phương thức `call` khi `m.call()` chạy.

Hãy xem xét một enum khác trong thư viện chuẩn rất phổ biến và hữu ích:
`Option`.

### Enum `Option` và ưu điểm của nó so với giá trị Null

Phần này khám phá một nghiên cứu điển hình về `Option`, là một enum khác được
khai báo bởi thư viện tiêu chuẩn. Kiểu `Option` mã hóa tình huống rất phổ biến
trong đó một giá trị có thể là một thứ gì đó hoặc có thể không có gì.

Ví dụ: nếu bạn yêu cầu mục đầu tiên trong danh sách không được bỏ trống, bạn sẽ
nhận được một giá trị. Nếu bạn yêu cầu mục đầu tiên trong danh sách trống, bạn
sẽ không nhận được gì. Diễn đạt khái niệm này dưới dạng hệ thống kiểu có nghĩa
là trình biên dịch có thể kiểm tra xem bạn đã xử lý tất cả các trường hợp mà bạn
nên xử lý hay chưa; chức năng này có thể ngăn chặn các lỗi cực kỳ phổ biến trong
các ngôn ngữ lập trình khác.

Thiết kế ngôn ngữ lập trình thường được nghĩ đến theo các tính năng bạn đưa vào,
nhưng các tính năng bạn loại trừ cũng rất quan trọng. Rust không có tính năng
null mà nhiều ngôn ngữ khác có. *Null* là một giá trị có nghĩa là không có giá
trị nào ở đó. Trong các ngôn ngữ có null, các biến luôn có thể ở một trong hai
trạng thái: null hoặc không null.

Tony Hoare, người phát minh ra null, trong bài thuyết trình “Tham chiếu Nll: Sai
lầm hàng tỷ đô la” năm 2009 của mình đã nói thế này:

> Tôi gọi đó là sai lầm tỷ đô của mình. Vào thời điểm đó, tôi đang thiết kế hệ
> thống kiểu dữ liệu toàn diện đầu tiên để tham khảo bằng ngôn ngữ hướng đối
> tượng. Mục tiêu của tôi là đảm bảo rằng tất cả việc sử dụng các tham chiếu
> phải tuyệt đối an toàn, việc kiểm tra phải được thực hiện tự động bởi trình
> biên dịch. Nhưng tôi không thể cưỡng lại sự cám dỗ để đưa vào một tham chiếu
> null, đơn giản vì nó rất dễ thực hiện. Điều này đã dẫn đến vô số lỗi, lỗ hổng
> bảo mật và sự cố hệ thống, có lẽ đã gây ra tổn thất và thiệt hại hàng tỷ đô la
> trong bốn mươi năm qua.

Vấn đề với các giá trị null là nếu bạn cố gắng sử dụng một giá trị null làm giá
trị không null, bạn sẽ gặp phải một lỗi nào đó. Bởi vì thuộc tính null hoặc
not-null này rất phổ biến nên rất dễ mắc loại lỗi này.

Tuy nhiên, khái niệm mà null đang cố gắng thể hiện vẫn là một khái niệm hữu ích:
null là một giá trị hiện không hợp lệ hoặc không xuát hiện vì lý do bất kì.

Vấn đề không thực sự nằm ở khái niệm mà ở cách triển khai cụ thể. Như vậy, Rust
không có null, nhưng nó có một enum có thể biểu diễn khái niệm giá trị hiện diện
hoặc vắng mặt. Enum này là `Option<T>`, và nó [được định nghĩa bởi thư viện
chuẩn][option]<!-- ignore --> như sau:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

Enum `Option<T>` hữu ích đến mức nó thậm chí còn được đưa vào `prelude`; bạn
không cần phải đưa nó vào phạm vi một cách rõ ràng. Các biến thể của nó cũng
được bao gồm trong phần dạo đầu: bạn có thể sử dụng trực tiếp `Some` và `None`
mà không cần tiền tố `Option::`. Enum `Option<T>` vẫn chỉ là một enum thông
thường và `Some(T)` và `None` vẫn là các biến thể của kiểu `Option<T>`.

Cú pháp `<T>` là một tính năng của Rust mà chúng tôi chưa nói đến. Đó là một
tham số kiểu chung chung, và chúng ta sẽ đề cập chi tiết hơn về kiểu chung trong
Chương 10. Hiện tại, tất cả những gì bạn cần biết là `<T>` có nghĩa là biến thể
`Some` của enum `Option` có thể chứa một phần dữ liệu thuộc bất kỳ loại nào và
mỗi loại cụ thể được sử dụng thay cho ` T` tổng thể làm cho kiểu `Option<T>`
thành một loại khác. Dưới đây là một số ví dụ về việc sử dụng các giá trị
`Option` để giữ các loại số và loại chuỗi:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-06-option-examples/src/main.rs:here}}
```

Kiểu của `some_number` là `Option<i32>`. Kiểu của `some_char` là `Option<char>`,
đây là một kiểu khác. Rust có thể suy ra các kiểu này vì chúng ta đã chỉ định
một giá trị bên trong biến thể `Some`. Đối với `absent_number`, Rust yêu cầu
chúng tôi chú thích kiểu tổng thể `Option`: trình biên dịch không thể suy ra
loại mà biến thể `Some` tương ứng sẽ giữ bằng cách chỉ xem giá trị `None`. Ở
đây, chúng ta nói với Rust rằng chúng ta muốn `absent_number` thuộc loại
`Option<i32>`.

Khi chúng ta có một giá trị `Some`, chúng ta biết rằng một giá trị hiện có và
giá trị đó được giữ trong `Some`. Khi chúng ta có giá trị `None`, theo một nghĩa
nào đó, điều đó có nghĩa giống như null: chúng ta không có giá trị hợp lệ. Vậy
tại sao có `Option<T>` lại tốt hơn là có null?

Hiểu một cách ngắn gọn, vì `Option<T>` và `T` (trong đó `T` có thể là bất kỳ
kiểu dữ liệu nào) là các kiểu khác nhau, nên trình biên dịch sẽ không cho phép
chúng ta sử dụng giá trị `Option<T>` như thể chắc chắn rằng nó là một giá trị
hợp lệ. Ví dụ: mã này sẽ không được biên dịch vì nó đang cố gắng thêm `i8` vào
`Option<i8>`:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-07-cant-use-option-directly/src/main.rs:here}}
```

Nếu chúng tôi chạy mã này, chúng tôi sẽ nhận được thông báo lỗi như sau:

```console
{{#include ../listings/ch06-enums-and-pattern-matching/no-listing-07-cant-use-option-directly/output.txt}}
```

Quá khủng khiếp! Trên thực tế, thông báo lỗi này có nghĩa là Rust không hiểu
cách thêm `i8` và `Option<i8>`, vì chúng là các kiểu khác nhau. Khi chúng ta có
một giá trị thuộc kiểu như `i8` trong Rust, trình biên dịch sẽ đảm bảo rằng
chúng ta luôn có một giá trị hợp lệ. Chúng ta có thể tiến hành một cách tự tin
mà không cần phải kiểm tra null trước khi sử dụng giá trị đó. Chỉ khi chúng ta
có `Option<i8>` (hoặc bất kỳ kiểu giá trị nào mà chúng ta đang làm việc), chúng
ta mới phải lo lắng về việc có thể không có giá trị và trình biên dịch sẽ đảm
bảo chúng ta xử lý trường hợp đó trước khi sử dụng giá trị.

Nói cách khác, bạn phải chuyển đổi `Option<T>` thành `T` trước khi bạn có thể
thực hiện các thao tác `T` với nó. Nói chung, điều này giúp nắm bắt được một
trong những vấn đề phổ biến nhất với null: giả định rằng một cái gì đó không
phải là null trong thực tế.

Loại bỏ rủi ro giả định sai giá trị khác null giúp bạn tự tin hơn vào mã của
mình. Để có một giá trị có thể là null, bạn phải chọn tham gia một cách rõ ràng
bằng cách đặt kiểu giá trị đó là `Option<T>`. Sau đó, khi bạn sử dụng giá trị
đó, bạn được yêu cầu phải xử lý rõ ràng trường hợp khi giá trị đó là null. Ở mọi
nơi mà một giá trị có kiểu không phải là `Option<T>`, bạn *có thể* giả định một
cách an toàn rằng giá trị đó không phải là rỗng. Đây là một quyết định thiết kế
có chủ ý của Rust nhằm hạn chế tính phổ biến của null và tăng tính an toàn cho
mã lệnh Rust.

Vậy làm thế nào để bạn lấy giá trị `T` từ biến thể `Some` khi bạn có giá trị
thuộc loại `Option<T>` để bạn có thể sử dụng giá trị đó? Enum `Option<T>` có một
số lượng lớn các phương thức hữu ích trong nhiều tình huống khác nhau; bạn có
thể kiểm tra chúng trong [tài liệu của nó][docs]<!-- ignore -->. trở nên quen
thuộc với các phương pháp trên `Option<T>` sẽ cực kỳ hữu ích trong hành trình
của bạn với Rust.

Nói chung, để sử dụng một giá trị `Option<T>`, bạn muốn có mã sẽ xử lý từng biến
thể. Bạn muốn một số mã sẽ chỉ chạy khi bạn có giá trị `Some(T)` và mã này được
phép sử dụng `T` bên trong. Bạn muốn một số mã khác chỉ chạy nếu bạn có giá trị
`None` và mã đó không có sẵn giá trị `T`. Biểu thức `match` là một cấu trúc
luồng điều khiển chỉ thực hiện điều này khi được sử dụng với enum: nó sẽ chạy mã
khác nhau tùy thuộc vào biến thể của enum mà nó có và mã đó có thể sử dụng dữ
liệu khớp với giá trị bên trong.

[IpAddr]: ../std/net/enum.IpAddr.html
[option]: ../std/option/enum.Option.html
[docs]: ../std/option/enum.Option.html