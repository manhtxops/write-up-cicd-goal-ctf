# Hearts

![Untitled](Hearts%205bbbeaf0803843158dba8e6cda1a8e09/Untitled.png)

Đề bài có gợi ý: “A permission to admin agents is something you might find useful…”

Kiểm tra các user được tạo trong Jenkins, tìm xem user nào là Agents admin, ta tìm được user knave.

![Untitled](Hearts%205bbbeaf0803843158dba8e6cda1a8e09/Untitled%201.png)

Sử dụng dictionary attack để dò password của user knave, tìm được pass của knave là rockme

![Untitled](Hearts%205bbbeaf0803843158dba8e6cda1a8e09/Untitled%202.png)

Đăng nhập thành công vào tài khoản user knave

![Untitled](Hearts%205bbbeaf0803843158dba8e6cda1a8e09/Untitled%203.png)

Đến đây hơi bí cách get credential do không thao tác được với pipeline nào, nên ta xem hint ^^

![Untitled](Hearts%205bbbeaf0803843158dba8e6cda1a8e09/Untitled%204.png)

Họ gợi ý là tạo 1 agent, sau đó dùng flag8 là thông tin đăng nhập SSH và capture lại nó

Tạo nhanh 1 con server EC2 để agent ssh vào, sau đó capture lại thông tin đăng nhập

![Untitled](Hearts%205bbbeaf0803843158dba8e6cda1a8e09/Untitled%205.png)

Cấu hình agent mới để ssh được vào EC2

![Untitled](Hearts%205bbbeaf0803843158dba8e6cda1a8e09/Untitled%206.png)

Sử dụng lệnh này trên EC2 để log, có kết nối tới nhưng không hiển thị password: sudo tail -f /var/log/auth.log

## **Điểm yếu bảo mật:**

- User trong hệ thống sử dụng phương thức xác thực yếu, dễ bị chiếm quyền kiểm soát tài khoản.
- Chưa cấu hình rate limit cho Jenkins, dẫn tới không giới hạn số lần request.

## Biện pháp khắc phục:

- Sử dụng mật khẩu mạnh
- Sử dụng 2FA
- Hạn chế số lần nhập sai password
- Cấu hình rate limit cho Jenkins, có thể sử dụng CloudFlare để tránh tấn công bruteforce, dictionary attack, …