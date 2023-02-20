## Quyền sở hữu là gì?

*Quyền sở hữu (Ownership)* là một bộ quy tắc chi phối cách chương trình Rust quản lý bộ nhớ.

Tất cả các chương trình phải quản lý cách chúng sử dụng bộ nhớ của máy tính trong khi chạy.

Một số ngôn ngữ có bộ sưu tập rác thường xuyên tìm kiếm và giải phóng các vùng bộ nhớ không còn sử dụng khi chương trình chạy; trong các ngôn ngữ khác, lập trình viên phải phân bổ và giải phóng bộ nhớ một cách rõ ràng. Rust sử dụng cách tiếp cận thứ ba: bộ nhớ được trình biên dịch kiểm tra rồi quản lý thông qua một hệ thống sở hữu cùng với một bộ các quy tắc. Nếu bất kỳ quy tắc nào bị vi phạm, chương trình sẽ không biên dịch. Không có tính năng nào của quyền sở hữu sẽ làm chậm chương trình của bạn khi nó đang chạy.

Bởi vì quyền sở hữu là một khái niệm mới đối với nhiều lập trình viên nên sẽ mất một thời gian để làm quen. Tin tốt là bạn càng có nhiều kinh nghiệm với Rust và các quy tắc của hệ thống sở hữu, bạn sẽ càng dễ dàng phát triển mã một cách tự nhiên, an toàn và hiệu quả. Hãy vững vàng lên!

Khi bạn hiểu về quyền sở hữu, bạn sẽ có một nền tảng vững chắc để hiểu các tính năng khiến Rust trở nên độc đáo. Trong chương này, bạn sẽ tìm hiểu quyền sở hữu bằng cách làm việc thông qua một số ví dụ tập trung vào cấu trúc dữ liệu rất phổ biến: chuỗi.

> ### Ngăn xếp và Đống

> Nhiều ngôn ngữ lập trình không yêu cầu bạn phải quan tâm thường xuyên về ngăn
> xếp và đống (*heap*). Nhưng trong một ngôn ngữ lập trình hệ thống như Rust, 
> việc một giá trị nằm trên ngăn xếp hay đống sẽ ảnh hưởng đến cách ngôn ngữ 
> hoạt động và đó là lý do bạn phải đưa ra quyết định chắc chắn. Các phần của 
> quyền sở hữu sẽ được mô tả liên quan đến ngăn xếp và đống ở phần sau của 
> chương này, vì vậy đây là một lời giải thích ngắn gọn để bạn chuẩn bị.
>
> Cả ngăn xếp và đống đều là những phần bộ nhớ khả dụng cho phép mã của bạn sử
> dụng trong thời gian chạy, nhưng chúng được cấu trúc theo những cách khác 
> nhau.
> Ngăn xếp lưu trữ các giá trị theo thứ tự nhận được và loại bỏ các giá trị theo
> thứ tự ngược lại. Điều này được gọi là *vào sau, ra trước*. Hãy so sánh như 
> khi ta xếp một chồng đĩa: khi bạn cho thêm đĩa, bạn đặt chúng lên ví trí trên 
> cùng của chồng đĩa và khi bạn cần một cái đĩa, bạn lấy một đĩa ra khỏi vị trí 
> trên cùng. Việc thêm hoặc bớt những cái đĩa từ giữa hoặc dưới cùng sẽ không 
> hiệu quả!
> Việc thêm dữ liệu được gọi là *đẩy lên ngăn xếp* và xóa dữ liệu được gọi là 
> *đẩy ra khỏi ngăn xếp*. Tất cả dữ liệu được lưu trữ trên ngăn xếp phải có 
> kích thước xác định và không thay đổi. Thay vào đó, dữ liệu có kích thước 
> không xác định tại thời điểm biên dịch hoặc kích thước có thể thay đổi phải 
> được lưu trữ trên đống (*heap*).
>
> Heap ít tính tổ chức hơn: khi bạn đặt dữ liệu vào heap, bạn yêu cầu một lượng
> không gian nhất định. Bộ cấp phát bộ nhớ tìm một vị trí trống đủ lớn trong 
> heap, đánh dấu nó là đang được sử dụng và trả về một *con trỏ*, là địa chỉ 
> của vị trí đó. Quá trình này được gọi là *phân bổ trên heap* và đôi khi được 
> viết tắt là *phân bổ* (việc đẩy các giá trị vào ngăn xếp không được coi là 
> phân bổ). Vì con trỏ tới heap có kích thước cố định, đã biết, nên bạn có thể 
> lưu trữ con trỏ trên ngăn xếp, nhưng khi bạn muốn dữ liệu thực sự, bạn phải 
> đi theo con trỏ. Hãy nghĩ về việc ngồi trong một nhà hàng. Khi bạn bước vào, 
> bạn cho biết số lượng người trong nhóm của mình và người dẫn chương trình tìm 
> một bàn trống phù hợp với mọi người và dẫn bạn đến đó. Nếu ai đó trong nhóm 
> của bạn đến muộn, họ có thể hỏi bạn đã ngồi ở đâu để tìm bạn.
>
> Đẩy vào ngăn xếp nhanh hơn cấp phát trên heap vì người cấp phát không bao giờ
> phải tìm kiếm nơi chứa dữ liệu mới; vị trí đó luôn ở trên cùng của ngăn xếp. 
> Một cách tương đối, việc phân bổ không gian trên heap đòi hỏi nhiều công việc 
> hơn vì trước tiên người cấp phát phải tìm một không gian đủ lớn để chứa dữ 
> liệu và sau đó thực hiện ghi sổ sách để chuẩn bị cho lần phân bổ tiếp theo.
>
> Truy cập dữ liệu trong heap chậm hơn truy cập dữ liệu trên ngăn xếp vì bạn 
> phải đi theo một con trỏ để đến đó. Các bộ xử lý hiện đại sẽ nhanh hơn nếu 
> chúng ít phải nhảy xung quanh bộ nhớ hơn. Tiếp tục phép loại suy, hãy xem xét 
> một người phục vụ  nhận đơn đặt hàng từ nhiều bàn tại một nhà hàng. Hiệu quả 
> nhất là lấy tất cả các đơn đặt hàng tại một bàn trước khi chuyển sang bàn 
> tiếp theo. Lấy một đơn đặt hàng từ bàn A, sau đó một đơn đặt hàng từ bàn B, 
> sau đó một lần nữa từ A, rồi lại một lần nữa từ B sẽ là một quá trình chậm 
> hơn nhiều. Tương tự như vậy, bộ xử lý có thể thực hiện công việc của mình tốt 
> hơn nếu nó hoạt động trên dữ liệu gần với dữ liệu khác (như ở trên ngăn xếp) 
> thay vì ở xa hơn (như ở trên đống).
>
> Khi mã của bạn gọi một hàm, các giá trị được truyền vào hàm (có khả năng bao 
> gồm các con trỏ tới dữ liệu trên heap) và các biến cục bộ của hàm được đẩy 
> lên ngăn xếp. Khi hàm này kết thúc, những giá trị đó sẽ được bật ra khỏi ngăn 
> xếp.
>
> Theo dõi những phần mã nào đang sử dụng dữ liệu trên heap, giảm thiểu lượng dữ
> liệu trùng lặp trên heap và dọn sạch dữ liệu không sử dụng trên heap để bạn
> không bị hết dung lượng là tất cả các vấn đề mà quyền sở hữu giải quyết. Khi 
> bạn hiểu về quyền sở hữu, bạn sẽ không cần phải suy nghĩ thường xuyên về ngăn 
> xếp và đống, nhưng biết rằng mục đích chính của quyền sở hữu là quản lý dữ 
> liệu đống có thể giúp giải thích lý do tại sao nó hoạt động theo cách như thế.

### Các Quy tắc Quyền Sở hữu

Trước tiên, hãy xem các quy tắc sở hữu. Hãy ghi nhớ những quy tắc này khi chúng
ta làm việc thông qua các ví dụ minh họa chúng:

* Mỗi giá trị trong Rust có một *chủ sở hữu*.
* Chỉ có thể có một chủ sở hữu tại một thời điểm.
* Khi chủ sở hữu vượt ra ngoài phạm vi, giá trị sẽ bị loại bỏ.

### Phạm vi của biến

Bây giờ chúng ta đã đi qua phần cú pháp cơ bản của Rust, chúng ta sẽ không bao
gồm tất cả mã của hàm `fn main() {` trong các ví dụ, vì vậy nếu bạn đang theo
dõi, hãy đảm bảo bạn tự đặt các ví dụ sau vào hàm `main`. Như thế, các ví dụ của
chúng ta sẽ ngắn gọn hơn một chút, điều đó giúp chúng ta tập trung vào các chi
tiết thực tế hơn là mã soạn sẵn.

Như một ví dụ đầu tiên về quyền sở hữu, chúng ta sẽ xem xét *phạm vi* của một số
biến. Một phạm vi là phạm vi trong một chương trình mà một mục là hợp lệ. Lấy
biến sau:

```rust
let s = "hello";
```

Biến `s` đề cập đến một chuỗi ký tự, trong đó giá trị của chuỗi được mã hóa cứng
vào văn bản chương trình của chúng ta. Biến có giá trị từ thời điểm nó được khai
báo cho đến khi kết thúc *phạm vi* hiện tại. Liệt kê 4-1 cho thấy một chương
trình với các nhận xét chú thích nơi biến `s` sẽ hợp lệ.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-01/src/main.rs:here}}
```

<span class="caption">Liệt kê 4-1: Một biến và phạm vi nó có hiệu lực</span>

Nói cách khác, có hai thời điểm quan trọng ở đây:

* Khi `s` đi *vào* phạm vi, nó hợp lệ.
* Nó vẫn có hiệu lực cho đến khi nó *ra khỏi* phạm vi.

Tại thời điểm này, mối quan hệ giữa các phạm vi và thời điểm các biến hợp lệ
tương tự như mối quan hệ trong các ngôn ngữ lập trình khác. Bây giờ chúng ta sẽ
xây dựng dựa trên sự hiểu biết này bằng cách giới thiệu loại `String`.

### Kiểu `String`

Để minh họa các quy tắc về quyền sở hữu, chúng ta cần một loại dữ liệu phức tạp
hơn những loại chúng ta đã đề cập trong phần [“Kiểu dữ liệu”][data-types]<!--
ignore --> của Chương 3. Các kiểu đã đề cập trước đó có kích thước đã biết, có
thể được lưu trữ trên ngăn xếp và bật ra khỏi ngăn xếp khi phạm vi của chúng kết
thúc và có thể được sao chép nhanh chóng và dễ dàng để tạo một phiên bản mới,
độc lập nếu một phần khác của mã cần sử dụng cùng một giá trị trong một phạm vi
khác nhau. Nhưng chúng tôi muốn xem xét dữ liệu được lưu trữ trên heap và khám
phá cách Rust biết khi nào cần xóa dữ liệu đó và loại `String` là một ví dụ
tuyệt vời.

Chúng tôi sẽ tập trung vào các phần liên quan đến quyền sở hữu của `String`. Các
khía cạnh này cũng áp dụng cho các kiểu dữ liệu phức tạp khác, cho dù chúng được
cung cấp bởi thư viện chuẩn hay do bạn tự tạo. Chúng ta sẽ thảo luận sâu hơn về
`String` trong [Chương 8][ch8]<!-- ignore -->.

Chúng ta đã thấy các chuỗi đúng nghĩa đen, trong đó một giá trị chuỗi được mã
hóa cứng vào chương trình của chúng ta. Một cách tự nhiên, dùng chuỗi là tiện
lợi, nhưng chúng không phù hợp với mọi tình huống mà chúng ta có thể muốn sử
dụng văn bản. Một lý do là chúng không thay đổi được. Một điều nữa là không phải
mọi giá trị chuỗi đều được biết rõ khi chúng ta viết mã của mình: ví dụ: nếu
chúng ta muốn lấy đầu vào của người dùng và lưu trữ thì sao? Đối với những
trường hợp này, Rust có kiểu chuỗi thứ hai, `String`. Kiểu này được quản lý dữ
liệu bằng cách phân bổ trên heap và do đó có thể lưu trữ một lượng văn bản mà
chúng ta không biết tại thời điểm biên dịch. Bạn có thể tạo `String` từ một
chuỗi ký tự bằng cách sử dụng hàm `from`, như sau:

```rust
let s = String::from("hello");
```

Toán tử dấu hai chấm `::` cho phép chúng ta định danh cho hàm `from` cụ thể này
dưới kiểu `String` thay vì sử dụng một số tên kiểu như `string_from`. Chúng ta
sẽ thảo luận thêm về cú pháp này trong phần [“Cú pháp phương
thức”][method-syntax]<!-- ignore --> của Chương 5, và khi chúng ta nói về cách
đặt tên với các mô-đun trong phần [“Đường dẫn tham chiếu đến một mục trong Cây
mô-đun”][paths-module-tree]<!-- ignore --> trong Chương 7.

Loại chuỗi này *có thể* bị biến đổi:

```rust
{{#rustdoc_include ../listings/ch04-undering-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```

Vì vậy, sự khác biệt ở đây là gì? Tại sao `String` có thể thay đổi nhưng các con
chữ thì không thể? Sự khác biệt giữa hai kiểu này là cách xử lý bộ nhớ.

### Bộ nhớ và cấp phát bộ nhớ

Trong trường hợp của một chuỗi ký tự, chúng ta biết nội dung tại thời điểm biên
dịch, vì vậy văn bản được mã hóa cứng trực tiếp vào tệp thực thi cuối cùng. Đây
là lý do tại sao chuỗi ký tự nhanh và hiệu quả. Nhưng những thuộc tính này chỉ
đến từ tính bất biến của chuỗi kí tự. Thật không may, chúng ta không thể đặt một
khối bộ nhớ vào hệ nhị phân cho từng đoạn văn bản có kích thước không xác định
tại thời điểm biên dịch và kích thước của chúng có thể thay đổi khi chạy chương
trình.

Với kiểu `String`, để hỗ trợ một đoạn văn bản có thể thay đổi, có thể phát
triển, chúng ta cần phân bổ một lượng bộ nhớ trên heap, không xác định tại thời
điểm biên dịch, để lưu trữ nội dung. Điều này có nghĩa là:

* Bộ nhớ phải được yêu cầu từ bộ cấp phát bộ nhớ khi chạy.
* Chúng ta cần một cách thức để trả lại bộ nhớ này cho bộ cấp phát khi chúng ta
  kết thúc thao tác với `String` của mình.

Phần đầu tiên sẽ do chúng ta thực hiện: khi chúng ta gọi `String::from`, nó  sẽ
yêu cầu bộ nhớ mà nó cần trong quá trình triển khai. Điều này khá phổ biến trong
các ngôn ngữ lập trình.

Tuy nhiên, phần thứ hai thì khác. Trong các ngôn ngữ có *bộ thu gom rác (garbage
collector - GC)*, GC theo dõi và dọn sạch các phần bộ nhớ không được sử dụng nữa
và chúng ta không cần phải quan tâm về điều đó. Trong hầu hết các ngôn ngữ không
có GC, chúng ta có trách nhiệm xác định thời điểm bộ nhớ không còn được sử dụng
nữa và gọi mã để giải phóng bộ nhớ một cách rõ ràng, giống như chúng ta đã làm
khi yêu cầu bộ nhớ. Trong lịch sử, làm điều này một cách chính xác đã luôn là
một vấn đề khó khăn khi lập trình. Nếu chúng ta quên, chúng ta sẽ lãng phí bộ
nhớ. Nếu chúng ta làm điều đó quá sớm, chúng ta sẽ có một biến không hợp lệ. Nếu
chúng ta làm điều đó hai lần, đó cũng là một lỗi. Chúng ta cần ghép chính xác
một `allocate` với  một `free`.

Rust đi theo một con đường khác: bộ nhớ sẽ tự động được trả về sau khi biến sở
hữu nó vượt quá phạm vi. Đây là một phiên bản ví dụ cho phạm vi của chúng ta từ
Liệt kê 4-1 bằng cách sử dụng `String` thay vì một chuỗi ký tự trục tiếp:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```

Có một điểm tự nhiên, nơi mà chúng ta có thể trả lại bộ nhớ cho bộ cấp phát
lượng bộ nhớ mà `String` cần dùng: khi `s` vượt quá phạm vi. Khi một biến nằm
ngoài phạm vi, Rust sẽ gọi một hàm đặc biệt cho chúng tôi. Hàm này được gọi là
[`drop`][drop]<!-- ignore -->, và đó là nơi tác giả của `String` có thể đặt mã
để trả về bộ nhớ. Rust tự động gọi `drop` tại dấu ngoặc nhọn đóng.

> Lưu ý: Trong C++, kiểu thu hồi tài nguyên này ở cuối vòng đời của một mục đôi
> khi được gọi là *Khởi tạo thu nhận tài nguyên (Resource Acquisition Is
> Initialization - RAII)*. Bạn sẽ quen thuộc với hàm `drop` trong Rust nếu bạn
> đã sử dụng các mẫu RAII.

Mẫu này có tác động sâu sắc đến cách viết mã Rust. Nó có vẻ đơn giản ngay lúc
này, nhưng hành vi của mã có thể trở thành không mong đợi trong các tình huống
phức tạp hơn khi chúng ta muốn có nhiều biến sử dụng dữ liệu mà chúng ta đã phân
bổ trên heap. Bây giờ chúng ta hãy khám phá một số tình huống đó.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-move"></a>

#### Biến và Dữ liệu Tương tác với Move

Nhiều biến có thể tương tác với cùng một dữ liệu theo những cách khác nhau trong
Rust. Hãy xem một ví dụ sử dụng một số nguyên trong Liệt kê 4-2.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

<span class="caption">Liệt kê 4-2: Gán giá trị nguyên của biến `x` đến `y`</span>

Chúng ta có thể đoán nó đang làm gì: “gán giá trị `5` cho `x`; sau đó tạo một
bản sao của giá trị trong `x` và gán nó với `y`.” Bây giờ chúng ta có hai biến,
`x` và `y`, và cả hai đều bằng `5`. Đây thực sự là những gì đang xảy ra, bởi vì
số nguyên là các giá trị đơn giản với kích thước cố định, đã biết và hai giá trị
`5` này được đẩy lên ngăn xếp.

Bây giờ hãy xem phiên bản `String`:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```

Điều này trông rất giống nhau, vì vậy chúng ta có thể cho rằng cách thức hoạt
động của nó sẽ như nhau: nghĩa là, dòng thứ hai sẽ tạo một bản sao của giá trị
trong `s1` và gán nó với `s2`. Nhưng đây không phải là những gì xảy ra.

Hãy xem Hình 4-1 để xem điều gì đang xảy ra với `String` bên dưới những vẻ bề
ngoài. `String` được tạo thành từ ba phần, được hiển thị ở bên trái: một con trỏ
tới bộ nhớ chứa nội dung của chuỗi, độ dài và dung lượng. Nhóm dữ liệu này được
lưu trữ trên ngăn xếp. Bên phải là bộ nhớ trên heap chứa nội dung.

<img alt="Hai bảng: bảng đầu tiên chứa biểu diễn của s1 trên ngăn xếp, bao gồm chiều dài (5), dung lượng (5) và một con trỏ tới giá trị đầu tiên trong bảng thứ hai. Bảng thứ hai chứa biểu diễn dữ liệu chuỗi trên heap, từng byte một." src="img/trpl04-01.svg" class="center"
style="width: 50%;" />

<span class="caption">Hình 4-1: Biểu diễn trong bộ nhớ của một `String` giữ giá trị `"hello"` được liên kết với `s1`</span>

Độ dài là dung lượng bộ nhớ, tính bằng byte, mà nội dung của `Chuỗi` hiện đang
sử dụng. Dung lượng là tổng dung lượng bộ nhớ, tính bằng byte, mà `Chuỗi` đã
nhận được từ bộ cấp phát. Sự khác biệt giữa chiều dài và dung lượng có quan
trọng, nhưng không phải trong bối cảnh này, vì vậy hiện tại, bạn có thể bỏ qua
dung lượng.

Khi chúng ta gán `s1` cho `s2`, dữ liệu `Chuỗi` sẽ được sao chép, nghĩa là chúng
ta sao chép con trỏ, độ dài và dung lượng trên ngăn xếp. Chúng tôi không sao
chép dữ liệu trên heap mà con trỏ trỏ tới. Nói cách khác, biểu diễn dữ liệu
trong bộ nhớ giống như Hình 4-2.

<img alt="Ba bảng: bảng s1 và s2 tương ứng đại diện cho các chuỗi đó trên ngăn xếp và cả hai đều trỏ đến cùng một dữ liệu chuỗi trên heap." src="img/trpl04-02.svg" class="center" style="width: 50%;" />

<span class="caption">Hình 4-2: Biểu diễn trong bộ nhớ của biến `s2` có bản sao của con trỏ, độ dài và dung lượng của `s1`</span>

Biểu diễn trông *không* giống như Hình 4-3, bộ nhớ sẽ trông như thế nào nếu thay
vào đó Rust cũng sao chép dữ liệu heap. Nếu Rust đã làm điều này, thao tác `s2 =
s1` có thể rất kém về mặt hiệu suất thời gian chạy nếu dữ liệu trên heap lớn.

<img alt="Bốn bảng: hai bảng đại diện cho dữ liệu ngăn xếp cho s1 và s2 và mỗi bảng trỏ đến bản sao dữ liệu chuỗi của chính nó trên heap." src="img/trpl04-03.svg" class="center" style="width: 50%;" />

<span class="caption">Hình 4-3: Một khả năng khác cho những gì `s2 = s1` có thể làm nếu Rust cũng sao chép cả dữ liệu heap</span>

Trước đó, chúng ta đã nói rằng khi một biến vượt quá phạm vi, Rust sẽ tự động
gọi hàm `drop` và dọn sạch bộ nhớ heap cho biến đó. Nhưng Hình 4-2 cho thấy cả
hai con trỏ dữ liệu trỏ đến cùng một vị trí. Đây là một vấn đề: khi `s2` và `s1`
vượt quá phạm vi, cả hai sẽ cố gắng giải phóng cùng một vùng nhớ. Đây được gọi
là *lỗi gấp đôi miễn phí* và là một trong những lỗi an toàn bộ nhớ mà chúng tôi
đã đề cập trước đây. Giải phóng bộ nhớ hai lần có thể dẫn đến hỏng bộ nhớ, điều
này có khả năng dẫn đến các lỗ hổng bảo mật.

Để đảm bảo an toàn cho bộ nhớ, sau dòng `let s2 = s1;`, Rust coi `s1` là không
còn giá trị. Do đó, Rust không cần giải phóng bất kỳ thứ gì khi `s1` nằm ngoài
phạm vi. Xem điều gì sẽ xảy ra khi bạn cố gắng sử dụng `s1` sau khi `s2` được
tạo; nó sẽ không hoạt động:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/src/main.rs:here}}
```

Bạn sẽ gặp lỗi như thế này vì Rust ngăn bạn sử dụng tham chiếu không hợp lệ:

```console
{{#include ../listings/ch04-undering-ownership/ no-listing-04-cant-use-after-move/output.txt}}
```

Nếu bạn đã nghe các thuật ngữ *sao chép nông* và *sao chép sâu* khi làm việc với
các ngôn ngữ khác, thì khái niệm sao chép con trỏ, độ dài và dung lượng mà không
sao chép dữ liệu có thể giống như tạo một bản sao nông. Nhưng vì Rust cũng làm
mất hiệu lực biến đầu tiên nên thay vì được gọi là bản sao nông, nó được gọi là
*di chuyển*. Trong ví dụ này, chúng tôi sẽ nói rằng `s1` đã được *di chuyển* vào
`s2`. Vì vậy, những gì thực sự xảy ra được thể hiện trong Hình 4-4.

<img alt="Ba bảng: bảng s1 và s2 tương ứng đại diện cho các chuỗi đó trên ngăn xếp và cả hai đều trỏ đến cùng một dữ liệu chuỗi trên heap. Bảng s1 chuyển sang màu xám vì s1 không còn giá trị; chỉ có thể sử dụng s2 để truy cập dữ liệu heap." src="img/trpl04-04.svg" class="center" style="width: 50%;" />

<span class="caption">Hình 4-4: Biểu diễn trong bộ nhớ sau khi `s1` bị vô hiệu</span>

Điều đó giải quyết được vấn đề của chúng ta! Chỉ với `s2` hợp lệ, khi nó vượt ra
ngoài phạm vi, nó sẽ giải phóng bộ nhớ và chúng ta đã hoàn tất.

Ngoài ra, có một lựa chọn thiết kế được ngụ ý bởi điều này: Rust sẽ không bao
giờ tự động tạo các bản sao dữ liệu “sâu” của bạn. Do đó, mọi sao chép *tự động*
có thể được coi là không tốn kém về mặt hiệu suất thời gian chạy.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-clone"></a>

#### Các biến và dữ liệu tương tác với bản sao

Nếu chúng ta *muốn* sao chép sâu dữ liệu heap của `String`, không chỉ dữ liệu ngăn xếp, thì chúng ta có thể sử dụng một phương thức phổ biến có tên là `clone`. Chúng ta sẽ thảo luận về cú pháp của phương thức trong Chương 5, nhưng vì các phương thức là một tính năng phổ biến trong nhiều ngôn ngữ lập trình nên bạn có thể đã thấy chúng trước đây.

Đây là một ví dụ về phương thức `clone` đang hoạt động:

```rỉ sét
{{#rustdoc_include ../listings/ch04-undering-ownership/no-listing-05-clone/src/main.rs:here}}
```

Điều này hoạt động tốt và rõ ràng tạo ra hành vi được hiển thị trong Hình 4-3, trong đó dữ liệu heap *không* được sao chép.

Khi bạn thấy lệnh gọi `clone`, bạn biết rằng một số mã tùy ý đang được thực thi và mã đó có thể tốn kém. Đó là một dấu hiệu trực quan cho thấy có điều gì đó khác biệt đang diễn ra.

#### Dữ liệu thuần Ngăn xếp: Sao chép

Có một nét nhỏ khác mà chúng ta chưa nói đến. Mã này sử dụng các số nguyên—một phần trong số đó được hiển thị trong Liệt kê 4-2—nó hoạt động và hợp lệ:

```rust
{{#rustdoc_include ../listings/ch04-under Hiểu- sở hữu/no-listing-06-copy/src/main.rs:here}}
```

Nhưng mã này dường như mâu thuẫn với những gì chúng ta vừa tìm hiểu: chúng ta không có lệnh gọi `clone`, nhưng `x` vẫn hợp lệ và không được chuyển vào `y`.

Lý do là các loại như số nguyên có kích thước đã biết tại thời điểm biên dịch được lưu trữ hoàn toàn trên ngăn xếp, vì vậy các bản sao của các giá trị thực được tạo nhanh chóng. Điều đó có nghĩa là: không có lý do gì chúng ta muốn ngăn cản `x` hợp lệ sau khi tạo biến `y`. Nói cách khác, không có sự khác biệt giữa sao chép nông và sâu ở đây, vì vậy lệnh gọi `clone` sẽ không làm gì khác với sao chép nông thông thường và chúng ta có thể bỏ qua.

Rust có một chú giải (annotation) đặc biệt được gọi là `Copy` đặc trưng (trait)
mà chúng ta có thể đặt trên các kiểu được lưu trữ trên ngăn xếp, giống như số
nguyên (chúng ta sẽ nói thêm về các đặc trưng  trong [Chương 10][traits]<!--ignore -- >). 
Nếu một kiểu triển khai `Copy` đặc trưng, thì các biến sử dụng nó sẽ không di
chuyển mà được sao chép như bình thường, làm cho chúng vẫn hợp lệ sau khi gán
cho một biến khác.

Rust sẽ không cho phép chúng ta chú giải một kiểu bằng `Copy` nếu kiểu đó hoặc
bất kỳ bộ phận nào của nó đã hiện thực hóa đặc trưng `Drop`. Nếu kiểu cần điều
gì đó đặc biệt xảy ra khi giá trị vượt quá phạm vi, và chúng ta thêm chú giải
`Copy` vào kiểu đó, thì chúng ta sẽ gặp lỗi khi biên dịch. Để tìm hiểu về cách
thêm chú giải `Copy` vào kiểu của bạn để triển khai đặc trưng, hãy xem [“Các Đặc
trưng có thể phái sinh”][derivable-traits]<!-- ignore --> trong Phụ lục C.

Vì vậy, kiểu nào thực hiện đặc trưng `Copy`? Bạn có thể kiểm tra tài liệu về
kiểu nhất định để chắc chắn, nhưng theo quy tắc chung, bất kỳ nhóm giá trị vô
hướng đơn giản nào cũng có thể triển khai `Copy` và không có thứ gì yêu cầu phân
bổ hoặc một số dạng tài nguyên có thể triển khai `Copy`. Dưới đây là một số loại
triển khai `Copy`:

* Tất cả các loại số nguyên, chẳng hạn như `u32`.
* Kiểu Boolean, `bool`, với các giá trị `true` và `false`.
* Tất cả các loại dấu phẩy động, chẳng hạn như `f64`.
* Loại ký tự, `char`.
* Các bộ dữ liệu, nếu chúng chỉ chứa các loại cũng triển khai `Copy`. Ví dụ:
  `(i32, i32)` thực hiện `Copy`, nhưng `(i32, String)` thì không.

### Quyền Sở hữu và Hàm

Cơ chế truyền một giá trị cho một hàm tương tự như khi gán một giá trị cho một
biến. Truyền một biến cho một hàm,giống như việc gán, sẽ di chuyển hoặc sao
chép. Liệt kê 4-3 có một ví dụ với một số chú thích cho biết nơi các biến đi vào
và ra khỏi phạm vi.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```

<span class="caption">Liệt kê 4-3: Các hàm có quyền sở hữu và phạm vi chú giải</span>

Nếu chúng ta cố gắng sử dụng `s` sau lệnh gọi `takes_ownership`, Rust sẽ đưa ra
lỗi thời gian biên dịch. Những kiểm tra tĩnh này bảo vệ chúng ta khỏi sai lầm.
Hãy thử thêm mã vào `main` sử dụng `s` và `x` để xem bạn có thể sử dụng chúng ở
đâu và ở đâu các quy tắc quyền sở hữu ngăn bạn làm như vậy.

### Giá trị trả về và Phạm vi

Các giá trị trả về cũng có thể chuyển quyền sở hữu. Liệt kê 4-4 cho thấy một ví
dụ về một hàm trả về một giá trị nào đó, với các chú thích tương tự như trong
Liệt kê 4-3.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```

<span class="caption">Liệt kê 4-4: Chuyển quyền sở hữu hàng trả lại giá trị</span>

Quyền sở hữu của một biến luôn tuân theo cùng một khuôn mẫu: gán một giá trị cho
một biến khác sẽ di chuyển nó. Khi một biến bao gồm dữ liệu trên heap nằm ngoài
phạm vi, giá trị sẽ bị `drop` làm sạch trừ khi quyền sở hữu dữ liệu đã được
chuyển sang một biến khác.

Trong khi điều này hoạt động, việc sở hữu và sau đó trả lại quyền sở hữu với mọi
chức năng là hơi tẻ nhạt. Điều gì sẽ xảy ra nếu chúng ta muốn để một hàm sử dụng
một giá trị nhưng không sở hữu? Điều khá khó chịu là bất kỳ thứ gì chúng ta
truyền vào cũng cần phải được truyền lại nếu chúng ta muốn sử dụng lại, ngoài
bất kỳ dữ liệu nào từ phần thân của hàm mà chúng ta có thể muốn trả về.

Rust cho phép chúng ta trả về nhiều giá trị bằng cách sử dụng một bộ, như trong
Liệt kê 4-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-05/src/main.rs}}
```

<span class="caption">Liệt kê 4-5: Trả lại quyền sở hữu các tham số</span>

Nhưng đây là quá nhiều nghi thức và rất nhiều công việc cho một khái niệm lẽ ra
là phổ biến. Thật may mắn cho chúng tôi, Rust có một tính năng để sử dụng một
giá trị mà không cần chuyển quyền sở hữu, được gọi là *tham chiếu*.

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
