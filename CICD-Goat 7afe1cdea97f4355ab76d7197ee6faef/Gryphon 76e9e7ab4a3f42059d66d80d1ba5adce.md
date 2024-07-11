# Gryphon

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled.png)

Ta là maintainer của project pygryphon.

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%201.png)

Kiểm tra các repo bên explore, ta thấy repo [**Wonderland / nest-of-gold](http://localhost:4000/wonderland/nest-of-gold)** đang chứa thông tin của flag11

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%202.png)

Kiểm tra ta thấy file .gitlab-ci.yml đang setup để deploy bằng docker, thực hiện build container bằng source code hiện tại.

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%203.png)

Kiểm tra Dockerfile, ta thấy nó sử dụng base image là python:3.8 

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%204.png)

Base image được pull trực tiếp từ container registry trong chính project

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%205.png)

Repo này set lịch cho user gryphon 1 thời gian lại build một lần

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%206.png)

Mục tiêu của ta là sẽ thay đổi nội dung của image python:3.8 để đọc flag

Kiểm tra tiếp, ta thấy repo awesome-app,

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%207.png)

Pipeline cũng được lên lịch để user gryphon thực thi sau 1 khoảng thời gian.

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%208.png)

Tiếp tục kiểm tra, ta thấy file dependency của repo đang dùng lib `pygryphon` mà ta có quyền kiểm soát

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%209.png)

Vậy luồng attack của ta sẽ như sau:

1. Chỉnh sửa repo `pygryphon` để thực hiện việc build 1 image python có chứa mã độc để đọc flag và push lên container registry của nest-of-gold
2. Repo awesome-app sẽ thực thi lib ****`pygryphon` có chứa mã độc
3. Sau khi pipeline của nest-of-gold chạy, nó sẽ pull base image có chứa mã độc về và thực thi.
4. Check flag được log trong pipeline của nest-of-gold

## **Điểm yếu bảo mật:**

- Sử dụng thư viện bên thứ 3 không được xác thực.
- Không validate các image sử dụng.

## Biện pháp khắc phục:

- Sử dụng thư viện các bên thứ 3 uy tín.
- Sign image và validate image trước khi sử dụng.
- Với các repo chứa nhiều thông tin nhạy cảm thì nên đặt ở chế độ private.
- Sử dụng các công cụ tự động hóa như Snyk để quét bảo mật cho tất cả các package được cài đặt trong dự án.
- Bật security alert trên các repo server để nhận các thông báo mới nhất liên quan đến các lỗ hổng trong các package được cài đặt trong dự án.

![Untitled](Gryphon%2076e9e7ab4a3f42059d66d80d1ba5adce/Untitled%2010.png)