## Lưu trữ khóa với các giá trị được liên kết trong *hash map*

Collection phổ biến cuối cùng của chúng ta là *hash map ( bản đồ băm )*. Loại
`HashMap<K, V>` lưu trữ ánh xạ giữa các khóa kiểu `K` với các giá trị kiểu `V`,
bằng cách sử dụng *hash map* để xác định cách đặt các khóa và giá trị này vào bộ
nhớ. Nhiều ngôn ngữ lập trình hỗ trợ loại cấu trúc dữ liệu này, nhưng chúng
thường sử dụng một tên khác, chẳng hạn như một số tên có thể kể ra như sau:
hash, map, object, hash table, dictionary hoặc mảng kết hợp.

*Hash map* rất hữu ích khi bạn muốn tra cứu dữ liệu không phải bằng cách sử dụng
chỉ mục, như bạn có thể làm với vectơ, mà bằng cách sử dụng khóa có thể thuộc
bất kỳ kiểu nào. Ví dụ: trong một trò chơi, bạn có thể theo dõi điểm số của từng
đội trong `hash map`, trong đó mỗi khóa là tên của một đội và các giá trị là
điểm của từng đội. Bạn có thể truy xuất điểm số của đội bằng cách đưa ra tên của
nó.

Chúng ta sẽ xem qua API cơ bản của *hash map* trong phần này, nhưng nhiều tính
năng bổ sung khác đang ẩn trong các hàm được thư viện chuẩn được khai báo trên
`HashMap<K, V>`. Như mọi khi, hãy kiểm tra tài liệu của thư viện tiêu chuẩn để
biết thêm thông tin.

### Tạo một Hash Map mới

Một cách để tạo *hash map* trống là sử dụng `new` và thêm các phần tử bằng
`insert`. Trong Liệt kê 8-20, chúng ta đang theo dõi điểm số của hai đội có tên
là *Blue* và *Yellow*. Đội Blue bắt đầu với 10 điểm và đội Yellow bắt đầu với 50
điểm.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-20/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-20: Tạo một hashmap mới và chèn một số khóa và giá trị</span>

Lưu ý rằng trước tiên chúng ta cần `use` `HashMap` từ phần các collection của
thư viện chuẩn. Trong số ba collection phổ biến của chúng ta, bộ sưu tập này ít
được sử dụng nhất, vì vậy nó không được tự động đưa vào các tính năng được đưa
vào phạm vi trong phần dạo đầu. *Hash map* cũng có ít hỗ trợ hơn từ thư viện
tiêu chuẩn; chẳng hạn, không có macro tích hợp để xây dựng chúng.

Cũng giống như vectơ, *hash map* lưu trữ dữ liệu của chúng trên heap. `HashMap`
này có các khóa thuộc loại `String` và các giá trị thuộc loại `i32`. Giống như
vectơ, hash map là đồng nhất: tất cả các khóa phải có cùng kiểu với nhau và tất
cả các giá trị phải có cùng kiểu.

### Truy cập các giá trị trong __*Hash Map*__

Chúng ta có thể lấy một giá trị từ *hash map* bằng cách cung cấp khóa của nó cho
phương thức `get`, như trong Liệt kê 8-21.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-21/src/main.rs:here}}
```

<span class="caption">Danh sách 8-21: Truy cập điểm số của đội Blue được lưu trữ trong *hash map*</span>

Tại đây, `score` sẽ có giá trị được liên kết với đội Blue và kết quả sẽ là `10`.
Phương thức `get` trả về một `Option<&V>`; nếu khóa đó trong *hash map* không có
giá trị, `get` sẽ trả về `None`. Chương trình này xử lý `Option` bằng cách gọi
`copied` để nhận `Option<i32>` thay vì `Option<&i32>`, sau đó `unwrap_or` để đặt
`scores` thành 0 nếu `scores` không có một mục cho khóa.

Chúng ta có thể duyệt qua từng cặp khóa/giá trị trong *hash map* theo cách tương
tự như chúng ta làm với vectơ, sử dụng vòng lặp `for`:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-03-iterate-over-hashmap/src/main.rs:here}}
```

Mã này sẽ in từng cặp theo thứ tự tùy ý:

```text
Yellow: 50
Blue: 10
```

### Hash Maps và Quyền sở hữu

Đối với các kiểu dữ liệu triển khai trait (đặc trưng) `Copy`, như `i32`, các giá
trị được sao chép vào *hash map*. Đối với các giá trị được sở hữu như `String`,
các giá trị sẽ được di chuyển và *hash map* sẽ là chủ sở hữu của các giá trị đó,
như được minh họa trong Liệt kê 8-22.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-22/src/main.rs:here}}
```

<span class="caption">Danh sách 8-22: Hiển thị rằng các khóa và giá trị được sở hữu bởi *hash map* sau khi chúng được chèn vào</span>

Chúng ta không thể sử dụng các biến `field_name` và `field_value` sau khi chúng
được chuyển vào *hash map* với lệnh gọi `insert`.

Nếu chúng ta chèn tham chiếu đến các giá trị vào *hash map*, thì các giá trị đó
sẽ không được chuyển vào *hash map*. Các giá trị mà tham chiếu trỏ tới phải hợp
lệ ít nhất trong thời gian *hash map* hợp lệ. Chúng ta sẽ nói nhiều hơn về những
vấn đề này trong phần [“Xác thực tham chiếu với thời gian
sống”][validating-references-with-lifetimes]<!-- ignore --> của Chương 10.

### Cập nhật *hash map*

Mặc dù số lượng cặp khóa và giá trị có thể tăng lên, nhưng mỗi khóa duy nhất chỉ
có thể có một giá trị được liên kết với nó tại một thời điểm (chứ không phải
ngược lại: ví dụ: cả đội Blue và nhóm Yellow có thể có giá trị 10 được lưu trữ
trong  *hash map* `scores`).

Khi bạn muốn thay đổi dữ liệu trong *hash map*, bạn phải quyết định cách xử lý
trong trường hợp khi một khóa đã được gán giá trị. Bạn có thể thay thế giá trị
cũ bằng giá trị mới, hoàn toàn bỏ qua giá trị cũ. Bạn có thể giữ giá trị cũ và
bỏ qua giá trị mới, chỉ thêm giá trị mới nếu khóa *không* có giá trị. Hoặc bạn
có thể kết hợp giá trị cũ và giá trị mới. Hãy xem cách để thực hiện những điều
này!

#### Ghi đè một giá trị

Nếu chúng ta chèn một khóa và một giá trị vào *hash map*, sau đó chèn cùng một
khóa đó với một giá trị khác, thì giá trị được liên kết với khóa đó sẽ bị thay
thế. Mặc dù mã trong Liệt kê 8-23 gọi `insert` hai lần, *hash map* sẽ chỉ chứa
một cặp khóa/giá trị vì chúng ta đang chèn giá trị cho khóa của đội Blue hai
lần.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-23/src/main.rs:here}}
```

<span class="caption">Danh sách 8-23: Thay thế một giá trị được lưu trữ bằng một khóa cụ thể</span>

Mã này sẽ in `{"Blue": 25}`. Giá trị ban đầu của `10` đã bị ghi đè.

<!-- Old headings. Do not remove or links may break. -->
<a id="only-inserting-a-value-if-the-key-has-no-value"></a>

#### Chỉ thêm Khóa và Giá trị nếu không có khóa

Thông thường, việc kiểm tra xem một khóa cụ thể đã tồn tại hay chưa trong *hash
map* với một giá trị, sau đó thực hiện các hành động sau: nếu khóa đó tồn tại
trong *hash map*, thì giá trị hiện có sẽ giữ nguyên như vậy. Nếu khóa không tồn
tại, hãy chèn nó và một giá trị cho nó.

*hash map* có một API đặc biệt cho điều này được gọi là `entry` lấy khóa mà bạn
muốn kiểm tra làm tham số. Giá trị trả về của phương thức `entry` là một enum
gọi là `Entry` đại diện cho một giá trị có thể tồn tại hoặc không tồn tại. Giả
sử chúng ta muốn kiểm tra xem khóa dành cho đội Yellow có giá trị được liên kết
với nó hay không. Nếu không, chúng ta muốn chèn giá trị 50 và tương tự cho đội
Xanh lam. Sử dụng API `entry`, mã sẽ giống như Liệt kê 8-24.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-24/src/main.rs:here}}
```

<span class="caption">Liệt kê 8-24: Sử dụng phương thức `entry` để chỉ chèn nếu
khóa chưa có giá trị</span>

Phương thức `or_insert` trên `Entry` được định nghĩa để trả về một tham chiếu có
thể thay đổi thành giá trị cho khóa `Entry` tương ứng nếu khóa đó tồn tại và nếu
không, hãy chèn tham số làm giá trị mới cho khóa này và trả về một tham chiếu có
thể thay đổi sang giá trị mới. Kỹ thuật này rõ ràng hơn nhiều so với việc tự
viết logic và ngoài ra, hoạt động tốt hơn với *borrow checker*.

Chạy mã trong Liệt kê 8-24 sẽ in ra `{"Yellow": 50, "Blue": 10}`. Lệnh gọi
`entry` đầu tiên sẽ chèn khóa cho đội Yellow với giá trị 50 vì đội Yellow chưa
có giá trị điểm. Lệnh gọi `entry` thứ hai sẽ không thay đổi *hash map* vì đội
Blue đã có giá trị điểm là 10.

#### Cập nhật Giá trị dựa trên Giá trị cũ

Một trường hợp sử dụng phổ biến khác cho *hash map* là tra cứu giá trị của khóa
và sau đó cập nhật giá trị đó dựa trên giá trị cũ. Chẳng hạn, Liệt kê 8-25 hiển
thị mã đếm số lần mỗi từ xuất hiện trong một số văn bản. Chúng ta sử dụng *hash
map* với các từ làm khóa và tăng giá trị để theo dõi số lần chúng ta đã thấy từ
đó. Nếu đây là lần đầu tiên chúng ta nhìn thấy một từ, trước tiên chúng ta sẽ
chèn giá trị 0.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-25/src/main.rs:here}}
```

<span class="caption">Danh sách 8-25: Đếm số lần xuất hiện của các từ bằng cách sử dụng *hash map* lưu trữ các từ và số đếm</span>

Mã này sẽ in ra `{"world": 2, "hello": 1, "wonderful": 1}`. Bạn có thể thấy các
cặp khóa/giá trị giống nhau được in theo một thứ tự khác: xem lại phần [“Truy
cập giá trị trong *hash map*”][access]<!-- ignore --> việc duyệt qua *hash map*
diễn ra theo thứ tự tùy ý.

Phương thức `split_whitespace` trả về một tiến trình duyệt trên các lát cắt con,
được phân tách bằng khoảng trắng, của giá trị trong `text`. Phương thức
`or_insert` trả về một tham chiếu khả biến (`&mut V`) thành giá trị cho khóa đã
chỉ định. Ở đây, chúng ta lưu trữ tham chiếu khả biến đó trong biến `count`, vì
vậy để gán cho giá trị đó, trước tiên chúng ta phải lấy giá trị của `count` bằng
cách sử dụng dấu hoa thị (`*`). Tham chiếu có thể thay đổi nằm ngoài phạm vi ở
cuối vòng lặp `for`, vì vậy tất cả các thay đổi này đều an toàn và được cho phép
theo quy tắc mượn.

### Các Hàm Băm

Theo mặc định, `HashMap` sử dụng hàm băm được gọi là *SipHash* có thể cung cấp
khả năng chống lại các cuộc tấn công Từ chối dịch vụ (DoS) liên quan đến bảng
băm[^siphash]<!-- ignore -->. Đây không phải là thuật toán băm nhanh nhất hiện
có, nhưng sự đánh đổi để có bảo mật tốt hơn đi kèm với việc giảm hiệu suất là
xứng đáng. Nếu bạn cấu hình mã của mình và thấy rằng hàm băm mặc định quá chậm
so với mục đích của mình, bạn có thể chuyển sang một hàm khác bằng cách chỉ định
một hàm băm khác. Một *hasher* là một loại triển khai đặc điểm
(trait)`BuildHasher`. Chúng ta sẽ nói về các trait và cách triển khai chúng
trong Chương 10. Bạn không nhất thiết phải triển khai hasher của riêng mình từ
đầu; [crates.io](https://crates.io/)<!-- ignore --> có các thư viện được chia sẻ
bởi những người dùng Rust khác cung cấp các hasher triển khai nhiều thuật toán
băm phổ biến.

[^siphash]: [https://en.wikipedia.org/wiki/SipHash](https://en.wikipedia.org/wiki/SipHash)

## Tổng kêt chương

Các vectơ, chuỗi và *hash map* sẽ cung cấp một lượng lớn chức năng cần thiết
trong các chương trình khi bạn cần lưu trữ, truy cập và sửa đổi dữ liệu. Dưới
đây là một số bài tập bây giờ bạn nên luyện tập :

* Đưa ra một danh sách các số nguyên, sử dụng một vectơ và trả về giá trị trung
  bình (khi được sắp xếp, giá trị ở vị trí giữa) và chế độ (giá trị xuất hiện
  thường xuyên nhất; *hash map* sẽ hữu ích ở đây) của danh sách.
* Chuyển đổi chuỗi sang tiếng *Latin lợn (Pig Latin)*. Phụ âm đầu tiên của mỗi
  từ được chuyển đến cuối từ và “ay” được thêm vào, vì vậy “first” trở thành
  “irst-fay”. Thay vào đó, những từ bắt đầu bằng một nguyên âm sẽ được thêm
  “hay” vào cuối (“apple” trở thành “apple-hay”). Hãy ghi nhớ các chi tiết về mã
  hóa UTF-8!
* Sử dụng *hash map* và vectơ, tạo giao diện văn bản để cho phép người dùng thêm
  tên nhân viên vào một bộ phận trong công ty. Ví dụ: “Thêm Sally vào Kỹ thuật”
  hoặc “Thêm Amir vào Bán hàng”. Sau đó, cho phép người dùng truy xuất danh sách
  tất cả những người trong một bộ phận hoặc tất cả những người trong công ty
  theo bộ phận, được sắp xếp theo thứ tự bảng chữ cái.

Tài liệu API thư viện tiêu chuẩn mô tả các phương thức mà vectơ, chuỗi và *hash
map* có sẽ hữu ích cho các bài tập này!

Chúng ta đang tham gia vào các chương trình phức tạp hơn, trong đó các hoạt động
có thể bị lỗi, vì vậy, đây là thời điểm hoàn hảo để thảo luận về xử lý lỗi.
Chúng ta sẽ làm điều đó trong phầntiếp theo!

[validating-references-with-lifetimes]:
ch10-03-lifetime-syntax.html#validating-references-with-lifetimes
[access]: #accessing-values-in-a-hash-map
