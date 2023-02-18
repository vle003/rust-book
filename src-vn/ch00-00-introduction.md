# Giới thiệu

> Ghi chú: Ấn bản này giống với bản in và ebook của cuốn
> [The Rust Programming Language][nsprust] của nhà xuất bản [No Starch Press][nsp].

[nsprust]: https://nostarch.com/rust-programming-language-2nd-edition
[nsp]: https://nostarch.com/


Chào mừng bạn đến với cuốn sách *The Rust Programming Language*, một cuốn sách có tính chất giới thiệu về ngôn ngữ lập trình Rust.

Ngôn ngữ lập trình Rust là một ngôn ngữ sẽ giúp bạn viết ra các phần mềm máy tính nhanh hơn, tin cậy hơn.

Tính hữu dụng cao và khả năng điều khiển hệ thống ở mức thấp thường không đi cùng nhau trong việc thiết kế ngôn ngữ lập trình; Rust đã thách thức điều mâu thuẫn này. Bằng cách cân đối giữa các cảm xúc của lập trình viên và khả năng kỹ thuật, Rust đã mang lại cho bạn khả năng kiểm soát hệ thống ở mức thấp một cách chi tiết (như là sử dụng bộ nhớ) mà không có những khó khăn truyền thống luôn đi kèm với các kiểm soát hệ thống mức thấp như vậy.

## Rust dành cho ai 

Rust lý tưởng cho quần chúng vì nhiều lý do. Hãy thử xem các nhóm người sử dụng chính sau đây.

### Các nhóm Lập trình viên

Rust đã và đang được minh chứng là công cụ hiệu quả cho việc cộng tác giữa các nhóm lập trình viên lớn với nhiều cấp độ kiến thức của lập trình hệ thống. Lỗi trong mã mức thấp cho hệ thống có xu hướng là các lỗi tinh vi và khó phát hiện, trong nhiều ngôn ngữ khác phát hiện các lỗi này chỉ thành công khi sử dụng các bài kiểm tra kĩ lưỡng và người tái kiểm mã nguồn có nhiều kinh nghiệm. Trong Rust, trình biên dịch mã nguồn hoạt động như một "người gác cổng" nó từ chối biên dịch các mã lệnh gây lỗi, bao gồm các lỗi "mù mờ" cũng như các lỗi đã biết. Bằng cách làm việc cùng trình biên dịch, đội lập trình có thể dành nhiều thời gian cho logic của chương trình hơn là đi tìm lỗi.

Rust cũng mang đến cho lập trình viên hệ thống các công cụ "mốt" nhất trong thế giới lập trình:

* Cargo, công cụ quản lý các thư viện phụ thuộc và sinh mã, bổ sung khai báo biên dịch, quản lý các thư viện một cách liền lạc trong toàn hệ sinh thái Rust.
* Công cụ định dạng mã nguồn Rustfmt bảo đảm một phong cách định dạng thống nhất giữa các lập trình viên.
* Rust Language Server làm Integrated Developement Environment (IDE) có thể tích hợp việc tự hoàn thiện câu lệnh và các thông báo lỗi tức thì khi viết mã nguồn.

Việc sử dụng các công cụ này cùng các công cụ khác trong hệ sinh thái Rust giúp mang lại hiệu quả công việc cao khi lập trình hệ thống.

### Các Sinh viên

Rust thích hợp cho sinh viên và những ai mong muốn học về các khái niệm hệ thống. Bằng cách sử dụng Rust, rất nhiều người đã học thêm nhiều kiến thức như về phát triển hệ điều hành. Với những đóng góp thông qua cuốn sách này nhóm tác giả của Rust mong muốn mang các khái niệm hệ thống đến gần hơn với quảng đại quần chúng, đặc biệt là những ai mới tiếp cận với lập trình.

### Các công ty

Hàng trăm công ty, từ to tới nhỏ, sử dụng Rust trong công việc thực tế từ nhiều loại nhiệm vụ nho nhỏ, các công cụ chạy trên dòng lệnh, các web service, các chuỗi công cụ DevOps, các thiết bị nhung, phân tích và chuyển mã âm thanh hình ảnh, tiền tệ mã hóa, tin sinh học, máy tìm kiếm, máy học, và thậm chí cả các thành phần chính của trình duyệt Firefox.


### Các lập trình viên mã nguồn mở

Rust dành cho những ai muốn xây dựng ngôn ngữ lập trình Rust, cộng đồng, các công cụ cho lập trình viên và các thư viện. Chúng tôi rất vui khi có thêm các bạn đóng góp cho ngôn ngữ Rust.

### Những người cần Tốc độ và Tính ổn định

Rust dành cho những người khát khao một ngôn ngữ có tốc độ nhanh và tính ổn định. Với ý **tốc độ**, chúng tôi muốn nói cả về tốc độ chạy của chương trình và cả thời gian bạn dành cho việc lập trình. Trình biên dịch của Rust kiểm tra để bảo đảm tính ổn định thông qua các tính năng bổ sung và tái tổ chức mã nguồn của chương trinh. Điều này trái với mã yếu ớt theo kiểu cũ trong các ngôn ngữ không cung cấp các cơ chế kiểm tra, mà các lập trình viên ngại thay đổi. Rust nỗ lực để tạo ra các mã lệnh chạy nhanh và an toàn bằng cách cố gắng loại trừ những thứ trừu tượng, cung cấp các tính năng cao cấp được biên dịch cùng với các mã lệnh cấp thấp để chúng chạy nhanh như các mã lệnh viết thủ công.

Rust cũng mong muốn sẽ hỗ trợ tốt cho nhiều loại người dùng khác nhau; những nhóm người dùng được đề cập ở đây chỉ là những nhóm có nhiều liên quan nhất. Nói một cách tổng quan, tham vọng lớn nhất của Rust là loại bỏ sự đánh đổi mà các lập trình viên đã chấp nhận hàng chục năm nay bằng cách cung cấp sự an toàn *và* hiệu năng, tốc độ *và* hữu dụng. Hãy cho Rust cơ hội và xem nó có phù hợp cho công việc của bạn.

## Cuốn sách này dành cho ai

Cuốn sách này giả định rằng bạn đã lập trình với một hoặc nhiều ngôn ngữ khác thế nhưng đưa ra giả định về bất cứ ai là không nên. Chúng tôi đã cố gắng tạo ra các nguồn tài liệu có thể dễ dàng truy cập từ nhiều nền tảng lập trình khác nhau. Chúng tôi không dành nhiều thời gian để nói về việc Lập trình *là cái gì* hoặc suy nghĩ về nó thế nào. Nếu lập trình là việc hoàn toàn mới với bạn, thì tốt hơn bạn nên đọc một cuốn sách giới thiệu kĩ về nhập môn lập trình,.

## Sử dụng cuốn sách này như thế nào

Một cách tổng quan, cuốn sách này được tổ chức với giả định rằng bạn sẽ đọc nó theo thứ tự từ đầu đến cuối. Các chương về sau sẽ được xây dựng dựa trên khái niệm đã nói trong các chương phía trước, và các chương ban đầu sẽ không đi sâu vào chi tiết của một chủ đề cụ thể nhưng sẽ bàn kỹ hơn về nó trong các chương phía sau.

Bạn sẽ nhận thấy có hai loại chương trong cuốn sách này: các chương nói về khái niệm và các chương nói về dự án. Trong các chương khái niệm, bạn sẽ được học về diện mạo của Rust. Trong các chương dự án, bằng những kiến thức đã học, chúng ta sẽ xây dựng các chương trình nhỏ. Chương 2, 12 và 20 là các chương dự án; phần còn lại là các chương khái niệm

Chương 1 giải thích cách cài đặt Rust, cách viết chương trình đơn giản nhất "Hello, world!", cách sử dụng bộ quản lý gói và xây dựng mã **Cargo**. Chương 2 là giới thiệu về thực hành việc viết một chương trình bằng ngôn ngữ Rust, một trò chơi đoán số. Trong chương này chúng tôi sẽ nói về các khái niệm ở mức cao và các chương sau này sẽ nói chi tiết hơn về các khái niệm này. Nếu ngay lập tức bạn muốn *vọc* một ngôn ngữ mới thì chương 2 chính là nơi bắt đầu. Chương 3 nói về các tính năng của Rust mà tương tự như trong các ngôn ngữ lập trình khác, trong chương 4 bạn sẽ được học về quyền sở hữu hệ thống của Rust. Nếu bạn là một người học hành theo kiểu tỉ mỉ, người thích nắm rõ mọi chi tiết trước khi đi xa hơn, có thể bạn sẽ muốn bỏ qua chương 2 và đọc luôn chương 3, và bạn trở lại chương 2 khi bạn muốn làm một dự án áp dụng những thứ bạn đã học được.

Chương 5 thảo luận về các kiểu dữ liệu dạng *cấu trúc* và các *phương thức*, chương 6 nói về các kiểu dữ liệu dạng liệt kê (*enums*), các biểu thức `match`, và xây dựng điều khiển phân luồng `if let`. Bạn sẽ sử dụng các dữ liệu kiểu cấu trúc và các dữ liệu kiểu liệt kê để tạo ra các kiểu dữ liệu tùy biến trong Rust.

Trong chương 7, bạn sẽ học về hệ thống mô-đun và các quy tắc khu biệt (privacy rules) để tổ chức mã và các giao diện lập trình ứng dụng (Application Programming Interface - API) của Rust. Chương 8 thảo luận về một số các cấu trúc dữ liệu dạng tập hợp được cung cấp bởi các thư viện chuẩn, như là các véc-tơ, các chuỗi kí tự, và các bảng băm (hash maps). Chương 9 tìm hiểu về triết lý và các kỹ thuật quản lý lỗi của Rust.

Chương 10 sẽ đào sâu vào những thứ sẽ mang cho bạn năng lực để định nghĩa các mã lệnh có thể dùng cho nhiều kiểu khác nhau. như các đặc điểm chung, đặc trưng (traits) và chu kì sống (lifetimes). Toàn bộ chương 11 là về kiểm định mã, điều cần thiết ngay cả khi đã có sự đảm bảo an toàn của Rust để chắc chắn cho tính logic của chương trình. trong chương 12, chúng ta sẽ thực hiện cài đặt một số tính năng của chương trình tiện ích dòng lệnh `grep` dùng để tìm kiếm các văn bản trong một file. Để làm việc này, chúng ta sẽ sử dụng rất nhiều các khái niệm đã thảo luận trong các chương trước đó.

Chương 13 sẽ tìm hiểu về các bao đóng (closures) và các vòng lặp: các tính năng của Rust đến từ các ngôn ngữ lập trình chức năng (functional programming languages). Trong chương 14, chúng ta sẽ xem xét Cargo sâu hơn với các cách thức tốt nhất để chia sẻ các thư viện với người khác. Chương 15 thảo luận về các con trỏ thông minh được cung cấp từ các thư viện chuẩn và các đặc trưng mang đến các chức năng của chúng.

Trong chương 16, chúng ta sẽ xem xét các mô hình lập trình khác nhau một cách đồng thời và nói về cách mà Rust hỗ trợ bạn lập trình đa luồng mà không lo ngại điều gì.
Chương 17 xem xét đến các biểu diễn của Rust bằng việc so sánh với các nguyên tắc lập trình hướng đối tượng mà có thể bạn đã quen thuộc.

Chương 18 là một chương rất hữu ích để tham khảo các mẫu và khớp mẫu cho việc thể hiện các ý tưởng thông qua các chương trình bằng Rust. Chương 19 "mâm thịnh soạn" với các chủ đề cao cấp và đầy thú vị, bao gồm mã Rust phá cách, các macro, và sâu hơn về chu kì sống (lifetimes), các đặc trưng (traits), các kiểu dữ liệu, các hàm và các bao đóng.

Trong chương 20, chúng ta sẽ hoàn thành một dự án cài đặt một máy chủ web đa luồng ở mức thấp!

Cuối cùng, một số phụ lục chức các thông tin hữu ích về ngôn ngữ theo định dạng tham khảo. Phụ lục A về các từ khóa của Rust, Phụ lục B về các kí hiệu và các toán tử, Phụ lục C về các đặc trưng có thể dẫn xuất cung cấp bởi các thư viện chuẩn, Phụ lục D về một số công cụ phát triển hay được dùng, và Phụ lục E giải thích về các phiên bản Rust. Trong Phụ lục F, bạn có thể tìm danh sách các bản dịch của cuốn sách này, và trong Phụ lục G chúng tôi sẽ nói Rust được sinh ra như thế nào và Rust *nightly* nghĩa là gì.

Không có cách đọc nào là chuẩn mực cho cuốn sách này: nếu bạn muốn nhảy vọt tới, hãy làm điều đó !
Bạn có thể quay ngược lại các chương phía trước nếu bạn có nghi ngờ nào đó. Thế nhưng hãy cứ đọc theo cách mà bạn cho là tốt nhất.

<span id="ferris"></span>

Một phần quan trọng của quá trình học Rust là việc đọc các thông báo lỗi mà trình biên dịch in ra: những thông báo này sẽ dẫn dắt bạn thẳng tới khi mã lệnh hoạt động tốt.
Do đó, chúng tối sẽ cung cấp nhiều ví dụ sẽ không thể biên dịch được và chúng sẽ sinh ra lỗi để trình biên dịch hiển thị cho mỗi tình huống. Hãy hiểu rằng nếu bạn chạy một ví dụ, nó có thể không hoạt động! Hãy chắc chắn rằng bạn đã đọc các nội dung bao quanh vấn đề đó để chắc chắn rằng ví dụ đó là để tạo ra lỗi. Ferris cũng sẽ giúp bạn phân biệt mã hoạt động với mã không hoạt động:

| Ferris                                                                                                           | Meaning                                          |
|------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| <img src="img/ferris/does_not_compile.svg" class="ferris-explain" alt="Ferris with a question mark"/>            | This code does not compile!                      |
| <img src="img/ferris/panics.svg" class="ferris-explain" alt="Ferris throwing up their hands"/>                   | This code panics!                                |
| <img src="img/ferris/not_desired_behavior.svg" class="ferris-explain" alt="Ferris with one claw up, shrugging"/> | This code does not produce the desired behavior. |

Trong hầu hết các trường hợp, chúng tôi sẽ dẫn dắt bạn đến với phiên bản đúng của phiên bản không biên dịch được.

## Mã nguồn

Mã nguồn của cuốn sách này có thể tìm được tại [Github][book]

[book]: https://github.com/rust-lang/book/tree/main/src
