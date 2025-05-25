---
date: 2025-05-16T21:33:00
---
ZRelated : [[]]
Tag: #
___

- Role Guest(chưa đăng kí)
	- xem thông tin cơ sở ý tế, dịch vụ xét nghiệm hướng dẫn,blog,chia sẽ kiến thức
	- xem hướng dẫn đặt lịch và quy trình xét nghiệm
	- xem đánh gia phản hồi của người khác 
	- truy cập form đang kí, form đăng nhập
	- Tư vấn 
- Role Customer
	- đặt lịch xét nghiệm (tự thu mẫu hoặc đến cơ sở y tế)
	- theo dõi trạng thái mẫu (đã nhận, đang xử lý,hoàn tất)
	- Nhận thông báo kết quả qua mail
	- xem kết quả 
	- xem lại lịch sử xét nghiệm
	- quản lý hồ sơ cá nhân
	- gửi đánh giá ,feedback dịch vụ
	- hủy hoặc thay đổi lịch trước 24h
- Staff
	- Quản lý lịch hẹn đã đc đặt(duyệt lịch , điều phối thời gian)
	- Ghi nhận quá trình thực hiện xét nghiệm và nhập kết quả lên hệ thống.
	- tiếp nhẫn mẫu 
	- Gửi thông báo kết quả đến khách hàng.
	- Quản lý trạng thái mẫu (đang xử lý, hoàn tất...).
	- Hỗ trợ tư vấn khách hàng về kỹ thuật mẫu (qua form nội bộ hoặc kênh riêng).
- Manager(tùi hỏi thầy )
	- toàn bộ chức năng staff
	- xem bao cáo thống kê số lưỡng mẫu,thời gian xử lý, tỷ lệ thành công,..
	- quản lý feedback,đánh giá về dịch vụ
	- 
- Admin
	- toàn bộ chức năng manager
	- quản trị hệ thống(backup,update,fixbug)
	- quản lý người dùng n(tạo sửa ,xóa,phân quyền)

Manager và admin khác nhau ở đâu ạ ? vì ở đây gần như chỉ có 2 luồng chính là quản lí lịch trình từ vấn và quản lí xét nghiệm . Luồng theo dõi chu kì sinh sản thì sẽ ko cần ai quản lí vì sẽ có thuật toán và chỉ cần lấy thông tin bỏ vào hồ sơ sức khỏe consultant coi.

Consultants là Bác sĩ hay là chuyên viên tư vấn ạ?

Còn staff hầu như sẽ ko làm gì trong hệ thống này vì phần tư vấn đã có ( consultant ) và phần quản lí đã có ( manager , admin ) nên có cần role staff không ạ?

App web nặng phần quản lí đặt lịch trình hơn và xét nghiệm là phụ view cho người dùng  hay là App web nặng về phần quản lí xét nghiệm giúp Consutants theo dõi và phần đặt lịch là phụ ạ ? vì app em có 1 là phần app public cho khách hàng , 1 là phần app để quản lí cho các role còn lại và chưa biết cần chứ tâm phần nào hơn.



quản lí xung đột có nhiều kĩ thuật: lock table, transaction (kiểm tra lịch trống, insert lịch book mới trong 1 transaction), hoặc kĩ thuật hàng đợi (queue-based): RabbitMQ, Kafka, Redis Queue


Thầy ơi, cho em hỏi là tính năng nạp vào ví tiền phục vụ cho mục đích mua gói, nếu mà nạp thì hệ thống nó sẽ tự động approve cho user (em chưa có ý tưởng về cách hoạt động) hay là admin làm thủ công ạ, em cảm ơn ạ.
khi bạn nạp tiền vào ví (ví của app) thì sẽ là 2 việc: khách chuyển tiền vào tk của cty qua ví thật, tiền thật ví dụ MoMo, VNPay,thì nền tảng này sẽ trả về 1 trạng thái đã chuyển bao nhiêu và tình trạng thành công, thì bạn sẽ cập nhật vào hệ thống bạn số tiền ví (ảo) tương ứng

dạ trong phần mềm quản lí dịch vụ xét nghiệm huyết thống adn và staff sẽ đăng ký dịch vụ xét nghiệm adn cho khách hàng. thì em tính thiết kế bảng đăng ký dịch vụ thông tin bao gồm: mã đăng ký, loại xét nghiệm, người thực hiện đăng ký thì có hợp lí không thầy


![[Pasted image 20250517231222.png]]


![[Pasted image 20250518215232.png]]

![[Pasted image 20250518215554.png]]

![[Pasted image 20250518220050.png]]

![[Pasted image 20250518220542.png]]

![[Pasted image 20250518220841.png]]


Hãy nói tôi nghe về quy trình OOAD một cách chi tiết từng bước, có giải thích các "mẹo" giải quyết vấn đề.  
  
Sau đó...  
Trong môi trường làm phần mềm, ứng dụng web, bạn đóng vai Phân tích viên, BA, Developer.   
Bạn cần làm một web app với mô tả sơ bộ như dưới đây, bao gồm các Actor và Chức năng chính của app.  
Bạn có thể đưa thêm những giả định hợp ngữ cảnh, hợp scope, nhằm giúp người dùng có được một app hỗ trợ hiệu quả công việc trong tương lai

????

Bạn hãy tiếp cận phân tích, thiết kế bài toán theo những best practice của phương pháp OOAD, và hiển thị kết quả ở từng giai đoạn của OOAD. Mỗi giai đoạn của OOAD bạn đều giải thích rõ nguyên lý, cách làm, kết quả, và những thứ liên quan.  
Bạn cũng có thể vừa OOAD vừa phân tích luôn cả ERD.

Đoạn cuối, hãy thiết kế ERD cho bài toán này, theo ngôn ngữ DSL, để có thể nhúng vào dbdiagram.io render mô hình, và ra cả database script trên SQL Server

Bao nhiêu table là đủ: câu trả lời là DB đã đủ lưu trữ các nghiệp vụ bài toán của bạn chưa. Câu hỏi tiếp: nghiệp vụ đã đủ cho nhóm chưa (scope). 
Thầy cô trên lớp sẽ cùng bạn căn scop
theo cách bạn tách bảng thì hầu như tự nó rơi về chuẩn 3NF
nếu bàn về RDBMS nếu tách quá nhiều bảng, tính trùng lặp dữ liệu giảm thiểu, nhưng giá phải trả cho phần join ảnh hưởng performance