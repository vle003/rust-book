# Hiểu về Quyền Sở hữu (Ownership)

Quyền sở hữu là tính năng độc đáo nhất của Rust và có ý nghĩa sâu sắc đối với
phần còn lại của ngôn ngữ. Nó cho phép Rust đảm bảo an toàn bộ nhớ mà không cần
bộ thu gom rác bộ nhớ (garbage collector), vì vậy điều quan trọng là phải hiểu cách hoạt động của quyền sở hữu. Trong chương này, chúng ta sẽ nói về quyền sở hữu cũng như một số tính năng liên quan: nguyên lý mượn (borrowing), lát cắt (slice) và cách Rust đưa dữ liệu vào bộ nhớ.