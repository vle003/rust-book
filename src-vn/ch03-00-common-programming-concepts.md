# Các Khái niệm lập trình chung

Chương này bao gồm các khái niệm xuất hiện trong hầu hết mọi ngôn ngữ lập trình và cách chúng hoạt động trong Rust. Nhiều ngôn ngữ lập trình có nhiều điểm cốt lõi chung. Không có khái niệm nào được trình bày trong chương này là duy nhất đối với Rust, nhưng chúng ta sẽ thảo luận về chúng trong bối cảnh của Rust và giải thích các quy ước xung quanh việc sử dụng các khái niệm này.

Cụ thể, bạn sẽ tìm hiểu về các biến, các kiểu dữ liệu cơ bản, chức năng, chú giải và luồng điều khiển. Những kiến thức cơ sở này sẽ có trong mọi chương trình Rust và việc học chúng sớm sẽ giúp bạn có nền tảng vững chắc để bắt đầu.

#### Từ khóa
>
> Giống như trong các ngôn ngữ khác, ngôn ngữ Rust có một bộ *từ khóa* chỉ dành 
> cho ngôn ngữ sử dụng. Hãy nhớ rằng bạn không thể sử dụng những từ này làm tên 
> biến hoặc hàm. Hầu hết các từ khóa đều có ý nghĩa đặc biệt và bạn sẽ sử dụng 
> chúng để thực hiện các tác vụ khác nhau trong các chương trình Rust của mình; 
> một số từ hiện tại không có chức năng được liên kết với chúng, nó được dự trữ 
> cho các chức năng có thể được thêm vào Rust trong tương lai. Bạn có thể tìm 
> thấy danh sách các từ khóa trong [Phụ lục A][appendix_a]<!-- ignore -->.

[appendix_a]: appendix-01-keywords.md