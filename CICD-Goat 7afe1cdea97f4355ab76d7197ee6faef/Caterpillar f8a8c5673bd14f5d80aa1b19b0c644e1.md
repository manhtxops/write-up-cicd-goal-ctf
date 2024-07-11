# Caterpillar

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/925ddb1b-521e-4e7f-9594-df37da08d4b8.png)

Clone code về, thử push code lên nhưng đã bị chặn do không có quyền write, như vậy ta sẽ không trigger sang Jenkins bằng cách thay đổi code trên repo được.

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled.png)

Liệu khi fork về, ta push code thì nó có trigger sang Jenkins không ?

Tạo một branch mới trên repo vừa fork về, sau đó tạo PR vào branch main của repo gốc, thấy Jenkins đã được trigger.

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%201.png)

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%202.png)

Sửa pipeline để lấy flag2:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%203.png)

Lỗi khi build:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%204.png)

Sửa lại pipeline:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%205.png)

Sau khi chạy pipeline, ta thấy thông báo lỗi khi flag2 không tồn tại

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%206.png)

Đọc lại file Jenkins, ta thấy rằng ở stage deploy có chỉ định giá trị của env.BRANCH_NAME bằng 'main'. Điều này đảm bảo rằng các bước trong stage này chỉ được thực hiện trên branch main của repo

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%207.png)

Khi push code, ta thấy jenkins trigger sang project **wonderland-caterpillar-test**, theo thông báo lỗi, thì credentials ID flag2 không được set trong project này, khả năng cao nó được set trong project **wonderland-caterpillar-prod.** Bây giờ tìm cách để build bằng **wonderland-caterpillar-prod.**

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%208.png)

Thử log env xem có dùng được gì không.

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%209.png)

Sau khi in, ta thấy một biến có thể là access token của GITEA:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%2010.png)

Thử dùng token này để pull code về và đẩy code lên branch main được hay không (trường hợp token được gán quyền read, write repo mới push lên được)

Push thành công vào branch main:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%2011.png)

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%2012.png)

Pipeline đã được kích hoạt trên **wonderland-caterpillar-prod**:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%2013.png)

Kiểm tra log:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%2014.png)

Decode base64 get flag:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%2015.png)

Submit flag:

![Untitled](Caterpillar%20f8a8c5673bd14f5d80aa1b19b0c644e1/Untitled%2016.png)

## **Điểm yếu bảo mật:**

- Cấu hình sai event trigger dẫn đến hậu quả build các branch không mong muốn.
- Access token gán nhiều quyền không cần thiết, cụ thể ở đây, access token có quyền ghi.
- Không cấu hình bảo vệ các branch chính dẫn tới có thể push code trực tiếp vào các branch chỉnh đó.

## Biện pháp khắc phục:

- Branch chính cần được bảo vệ, không ai có quyền push trực tiếp code vào branch chính ngoại trừ project owner.
- Khi gán quyền cho token thì nên gán các quyền tối thiếu, không nên gán các quyền dư thừa, nhất là các quyền có khả năng thay đổi source code.
- Cấu hình event trigger chỉ nên build những branch được chỉ định trước.