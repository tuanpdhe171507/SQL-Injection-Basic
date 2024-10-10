# Triển khai Lap
 ###### Đầu tiên điều hướng đến trang 'Gifts' và quan sát thông số được gửi bởi yêu cầu http như chúng ta có thể thấy em có tham số danh mục
![alt text](image.png)
###### Thêm dấu nháy đơn `Gifts'` để xem máy chủ có gửi lỗi nội bộ hay không.
![alt text](image-1.png)
###### Có thể quan sát yêu cầu trên proxy, gửi một số yêu cầu, sẽ gửi yêu cầu đó đến repeater.
![alt text](image-2.png)
###### Thêm lệnh SQL vào tham số danh mục của mình để quan sát lỗ hổng và bảng có 2 cột.
![alt text](image-3.png)
###### Thử đến cột thứ 3 thì phát hiện ra lỗi.
![alt text](image-4.png)
######  Lấy phiên bản cơ sở dữ liệu. Thêm `+UNION+SELECT+banner,+NULL+from+v$version--` để được thực hiện trên cơ sở dữ liệu với tư cách là người trợ giúp trên bảng cheat của trang web được đề cập
![alt text](image-5.png)
