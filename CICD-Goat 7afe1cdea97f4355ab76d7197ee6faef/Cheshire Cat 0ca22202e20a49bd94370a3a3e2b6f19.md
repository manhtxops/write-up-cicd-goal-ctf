# Cheshire Cat

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/ad532b71-c7fd-4f1b-96d5-39d5be1a44e0.png)

Challenge yêu cầu thực thi code trên Jenkins Controller chứ không chạy trên các dedicated nodes

Trong jenkins, ta thấy có sẵn 2 node với 2 agent:

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled.png)

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%201.png)

Và repo hiện tại đang chạy trên agent1:

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%202.png)

Tìm hiểu chút về built-in node

<aside>
💡 Trong Jenkins, "built-in node" (còn được gọi là "master node") là máy chủ mà Jenkins được cài đặt và chạy. Nó xử lý tất cả các tác vụ quản lý, bao gồm lập lịch các jobs, điều phối các builds đến các agents (nếu có), và quản lý giao diện người dùng web của Jenkins.
Mặc định, mọi job trong Jenkins sẽ chạy trên built-in node này trừ khi bạn cấu hình các nodes khác (còn được gọi là "slaves" hoặc "agents") để phân tán tải và thực hiện các builds một cách song song. Việc sử dụng các agents giúp giảm tải cho built-in node, cho phép nó tập trung vào việc quản lý và điều phối.

</aside>

Trong Jenkinsfile đang chỉ định agent any, điều này các job sẽ chọn ngẫu nhiên 1 agent đang available.

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%203.png)

Để chỉ định rõ agent nào thực thi job, cần phải khai báo trên pipeline bằng cách chỉ định label

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%204.png)

Tuy nhiên built-in node không hiển thị UI

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/19c8b15c-783a-41df-a7cf-b99bb9435090.png)

Do đây là agent mặc định, nên có thể cũng có label mặc định, thử search google thì tìm được label mặc định là built-in:

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%205.png)

Sửa lại pipeline:

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%206.png)

Tạo PR để run job.

Ta thấy job đã được run trực tiếp trên Jenkins agent:

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%207.png)

Get flag trong file flag5.txt:

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%208.png)

Submit flag:

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%209.png)

## **Điểm yếu bảo mật:**

- Cấu hình sai event trigger dẫn đến hậu quả build các branch không mong muốn.
- Không quản lý chặt chẽ các agent có đặc quyền cao.

## Biện pháp khắc phục:

- Cấu hình event trigger chỉ nên build những branch được chỉ định trước.
- Set executors bằng 0 để disable các task đang chạy trên built-in node.

![Untitled](Cheshire%20Cat%200ca22202e20a49bd94370a3a3e2b6f19/Untitled%2010.png)