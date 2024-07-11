# Duchess

![Untitled](Duchess%208c3fc60f44f04cb7b2e2ace7737522a8/32ae2b4e-6a5a-4c09-8ed5-2ac259f0bcd3.png)

Đề bài yêu cầu tìm token PyPi có trong repo, sau khi clone về, thử search với một vài keyword như secret, token, Pypi nhưng có vẻ nó không tồn tại hoặc không đúng.

![Untitled](Duchess%208c3fc60f44f04cb7b2e2ace7737522a8/Untitled.png)

Sau khi pull code về, add project vào sourcetree, ta thấy repo này lưu rất nhiều history chứa các commit, liệu rằng trong các commit cũ có chứa PyPi token ?

Sau khi kiểm tra history, ta thấy có commit với message “**remove pypi token”**, trong commit đó có chứa token pypi:

![Untitled](Duchess%208c3fc60f44f04cb7b2e2ace7737522a8/Untitled%201.png)

Submit flag:

![Untitled](Duchess%208c3fc60f44f04cb7b2e2ace7737522a8/Untitled%202.png)

## **Điểm yếu bảo mật:**

- Không kiểm tra chặt chẽ các commit, để cho những commit chứa nội dung nhảy cảm được lưu lại trong git dẫn tới việc lộ lọt thông tin.

## Biện pháp khắc phục:

- Kiểm soát chặt chẽ các commit, không để những commit chứa nội dung nhạy cảm được push lên repo.
- Giám sát và kiểm tra mã nguồn thường xuyên, đảm bảo rằng các thay đổi trong mã nguồn được xem xét và phê duyệt bởi các thành viên có kinh nghiệm trong nhóm.
- Cần thay đổi lại những thông tin nhảy cảm khi phát hiện chúng đã bị lộ lọt.
- Cấu hình github hook (pre-commit) ngăn chặn những commit chứa thông tin nhạy cảm (có thể dùng regex)
- Các thông tin nhạy cảm nên được khai báo trong file env và được add vào file .gitignore để git bỏ qua chúng.