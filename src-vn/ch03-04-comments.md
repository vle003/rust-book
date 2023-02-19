## Bình luận

Tất cả các lập trình viên đều cố gắng làm cho mã của họ dễ hiểu, nhưng đôi khi
cần phải giải thích thêm. Trong những trường hợp này, các lập trình viên để lại
*chú thích* trong mã nguồn của họ mà trình biên dịch sẽ bỏ qua nhưng những người
đọc mã nguồn có thể thấy hữu ích.

Đây là một chú thích đơn giản:

```rust
// hello, world
```

Trong Rust, để định dạng một chú thích được bắt đầu bằng hai dấu gạch chéo và
chú thích tiếp tục cho đến cuối dòng. Đối với các chú thích vượt ra ngoài một
một dòng, bạn sẽ cần bao gồm `//` trên mỗi dòng, như thế này:

```rust
// So we’re doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what’s going on.
```

chú thích cũng có thể được đặt ở cuối dòng chứa mã:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-24-comments-end-of-line/src/main.rs}}
```

Nhưng bạn sẽ thường thấy chúng được sử dụng ở định dạng này hơn, với chú thích
trên một dòng riêng phía trên mã mà nó đang chú thích:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-25-comments-above-line/src/main.rs}}
```

Rust cũng có một loại chú thích khác, đó là chú thích về tài liệu, mà chúng ta
sẽ thảo luận trong phần [“Xuất bản crate lên Crate.io”][xuất bản]<!-- bỏ qua -->
của Chương 14.

[publishing]: ch14-02-publishing-to-crates-io.html