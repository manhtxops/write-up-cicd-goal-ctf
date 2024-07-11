# White Rabbit

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/553ae568-7145-4d64-b0e1-51a86848ec79.png)

Đề bài cho ta biết có 1 secret có tên flag1 được lưu trữ trong Jenkins credential store. 

Kiểm tra trong config của project wonderland-white-rabbit trên Jenkin.

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled.png)

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%201.png)

Tuy nhiên, user alice không có quyền để xem

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%202.png)

Tiếp tục kiểm tra repo Wonderland/white-rabbit theo đề bài xem có dùng được gì không, ta thấy repo có file Jenkinsfile dùng để cấu hình pipeline

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%203.png)

Thử thay đổi code, xem repo đó có trigger sang jenkins không, do repo cấu hình không cho phép push code trực tiếp lên branh main, nên ta tạo branh khác để push code lên

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%204.png)

Khi tạo xong PR, ta thấy user thealice không có quyền merge code vào branch main.

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%205.png)

Tuy nhiên chỉ mới tạo PR mà chưa merge vào branch main, pipeline vẫn được trigger:

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%206.png)

Lợi dụng điều này, ta sẽ in ra secret có tên flag1:

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%207.png)

Sau khi push code, ta thấy biến đó đã được in ra, tuy nhiên nó đã bị ẩn bởi ký tự *

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%208.png)

Thử tìm xem có cách nào để hiển thị rõ biến đó không, ta tìm được hướng dẫn sau, bằng cách in secret dưới dạng base64:

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%209.png)

Sửa lại pipeline và push lại:

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%2010.png)

Ta nhận được secret dưới dạng base64:

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%2011.png)

Decode base64, ta nhận được giá trị của secret flag1:

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%2012.png)

Submit flag thành công:

![Untitled](White%20Rabbit%20635839d8527f4ed8b99788bf73b8c5da/Untitled%2013.png)

## **Điểm yếu bảo mật:**

- Cấu hình sai event trigger dẫn đến hậu quả build các branch không mong muốn.

## Biện pháp khắc phục:

- Cấu hình chính xác event cần trigger, ở đây khi muốn build, cần trigger sự kiện merge code thay vì sự kiện tạo PR.
- Có thể config pipeline (Jenkinsfile) trực tiếp trên project Jenkins mà không cần sử dụng file trong source code tránh việc user không có quyền chỉnh sửa.