# Sử dụng các Cấu trúc để cấu trúc Dữ liệu liên quan tới nhau

*struct* hoặc *structure*, là một loại dữ liệu tùy chỉnh cho phép bạn đóng gói
và đặt tên cho nhiều giá trị có liên quan cùng nhau để tạo nên một nhóm có ý
nghĩa. Nếu bạn đã quen thuộc với ngôn ngữ hướng đối tượng, *struct* giống như
thuộc tính dữ liệu của đối tượng. Trong chương này, chúng ta sẽ so sánh và đối
chiếu các bộ dữ liệu với các cấu trúc để xây dựng dựa trên những gì bạn đã biết
và chứng minh khi nào cấu trúc là cách tốt hơn để tổ chức dữ liệu theo nhóm.

Chúng tôi sẽ trình bày cách xác định và khởi tạo cấu trúc. Chúng ta sẽ thảo luận
cách xác định các hàm liên kết, đặc biệt là loại hàm liên kết được gọi là
*phương thức*, để chỉ định hành vi liên quan đến một kiểu cấu trúc. Cấu trúc và
enums (được thảo luận trong Chương 6) là các khối xây dựng để tạo các loại mới
trong miền chương trình của bạn để tận dụng tối đa khả năng kiểm tra kiểu khi
biên dịch của Rust.