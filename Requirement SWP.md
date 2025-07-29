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


staff thấy danh sach các đngư kí xét nghiệm tại nhà ví dụ trong cơ sở có 10 bộ kit thì quản lý riêng từng bô kit có mã riêng , gán bọ kit vào cái phiếu bấm nút gửi người ta , sẽ chuyển thành trạng thái đã chuyển bộ kit, thì khi shipper giao bộ kit đến,staff xác nhận chuyển qua cho phòng xét nghiêm,có kết quả staff  cập nhật lên hệ thống


custommer:
- bang tiến trình 
- lịch sử các dịch vụ đang ký 
- kết quả trả về

-- luồng chọn chọn dịch vụ:
cus đăng nhập vào web vào trang dịch vụ sẽ thấy được 2 loại dịch vụ hành chính và dân sự , trong 2 loại dịch vụ này sẽ có từng tên dịch vụ kèm giá cả ,trong từng dịch vụ sẽ lại có loại lấy 2 mẫu (ví dụ của cha và con) và loại lấy 3 mẫu(ví dụ cha mẹ con) , mấy cái dịch vụ này sẽ được admin quản lý crud

	- ở hành chính: chỉ được chọn 1 cách lấy mẫu duy nhất là tại cơ sở
		- lấy tại cơ sở: 
		customer khi nhấn  choose trên trang dịch vụ nó sẽ nhảy vào đơn đăng kí với đầy đủ thông tin loại  dịch vụ (hành chính hoặc dân sự,tên ,giá cả của dịch vụ đó,lấy 2 mẫu hay 3 mẫu)=>customer chọn ngày giờ và đăng kí=>thanh toán(bằng vnpay)(chỉ được thay đổi lịch trước 24h)=>thanh toán thành công đơn đăng kí mới được hệ thống tự động tạo=> staff thấy danh sách đơn đăng kí trên hệ thống =>staff xác nhận đơn đăng kí đó mở đơn ra =>staff nhập thông tin những người cung cấp mẫu tùy vào số mẫu customer chọn ( tên,năm sinh,giới tính, mối quan hệ,loại mẫu,cam kết, add file ảnh chữ kí của staff và customer nếu trong UserProfile đã có file ảnh chữ kí của staff và customer thì hệ thống tự động nhét chữ kí của staff và customer vào) đồng thời staff nhập mã kit vào đơn (mã kit như là đoạn chuỗi K01 gì đó thôi) và nhấn gửi=>chuyển qua trạng thái đang xét nghiệm=>staff nhập kết quả=>manger xác nhận kết quả=>staff trả kết quả (đã có kết quả)=> customer thấy kết quả trên hệ thống và có thể xuất kết quả ra file pdf
		
	- ở dân sự: được chọn lấy mẫu tại nhà hoặc tại cơ sở
		- lấy tại nhà: 
		customer khi nhấn  choose trên trang dịch vụ nó sẽ nhảy vào đơn đăng kí với đầy đủ thông tin loại  dịch vụ (hành chính hoặc dân sự,tên giá cả của dịch vụ đó,lấy 2 mẫu hay 3 mẫu)=>đăng kí=>thanh toán(bằng vnpay) (chỉ được thay đổi lịch trước 24h)=>thanh toán thành công mới được hệ thống tự động tạo => staff thấy danh sách đơn đăng kí trên hệ thống=>staff xác nhận đơn rồi mở đơn ra add file ảnh chữ kí staff(nếu có sẵn trong UserProfile thì hệ thống tự lấy nhét vào) và đồng thời staff nhập mã kit vào đơn đó=>staff nhấn gửi đơn đăng kí  (đổi trạng thái đã gửi kit )=>customer nhận đơn=>customer mở đơn ra =>nhập thông tin những người cung cấp mẫu tùy vào số mẫu đã chọn mà điền( tên,năm sinh,giới tính, mối quan hệ,loại mẫu,cam kết, add file ảnh chữ kí của account(nếu có sẵn trong UserProfile thì hệ thống tự lấy nhét vào)) sau đó nhấn gửi=>staff nhận mẫu  rồi nhấn xác nhận =>chuyển qua trạng thái đang xét nghiệm=>staff nhập kết quả=>manager xác nhận kết quả (trong list kết quả staff nhập)=>staff gửi kết quả(trạng thái đã có kết quả)=>customer thấy kết quả trên hệ thống  và có thể xuất kết quả ra file pdf
		- lấy tại cơ sở: 
		customer khi nhấn choose trên trang dịch vụ nó sẽ nhảy vào đơn đăng kí với đầy đủ thông tin loại  dịch vụ (hành chính hoặc dân sự,tên ,giá cả của dịch vụ đó,lấy 2 mẫu hay 3 mẫu)=>customer chọn ngày giờ và đăng kí=>thanh toán(bằng vnpay)(chỉ được thay đổi lịch trước 24h)=>thanh toán thành công đơn đăng kí mới được hệ thống tự động tạo=> staff thấy danh sách đơn đăng kí trên hệ thống =>staff xác nhận đơn đăng kí đó mở đơn ra =>staff nhập thông tin những người cung cấp mẫu tùy vào số mẫu customer chọn ( tên,năm sinh,giới tính, mối quan hệ,loại mẫu,cam kết, add file ảnh chữ kí của staff và customer nếu trong UserProfile đã có file ảnh chữ kí của staff và customer thì hệ thống tự động nhét chữ kí của staff và customer vào) đồng thời staff nhập mã kit vào đơn (mã kit như là đoạn chuỗi K01 gì đó thôi) và nhấn gửi=>chuyển qua trạng thái đang xét nghiệm=>staff nhập kết quả=>manger xác nhận kết quả=>staff trả kết quả (đã có kết quả)=> customer thấy kết quả trên hệ thống và có thể xuất kết quả ra file pdf

nghĩa đăng nhập vào web nhân vào trang dịch vụ sẽ có 2 tap phân ra hành chính và dân sự  nhấn vào bên nào thì hiện bảng list dịch vụ tên ,giá tiền, mô tả, số mẫu và có nút choose ở từng hàng trong bảng đó khi nhấn choose nó sẽ hiện tất cả thông tin cái dịch vụ đó và nút xác nhận khi nhấn xác nhận thì sẽ cho người dùng chọn ngày giờ nới lấy mẫu, nếu chọn tại nhà thì sẽ không cho chọn ngày giờ, nhấn đăng kí sẽ qua thanh toán.....



file .env của bloodline-dna-backend: PORT=5000, JWT_SECRET=123@, DB_USER=sa, DB_PASSWORD=12345, DB_SERVER=localhost, DB_DATABASE=BloodTestServiceDB, DEFAULT_ADMIN_EMAIL=admindeptraivl@gmail.com, DEFAULT_ADMIN_PASSWORD=admin123!!!, FRONTEND_URL=http://localhost:3000, VNP_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html ,VNP_TMN_CODE=less50more500, VNP_HASH_SECRET=less50more500, VNP_RETURN_URL=http://localhost:3000/payment/callback, -------------- file .env của bloodline-dna-frontend : VITE_APP_NAME=GenUnity, VITE_API_URL=http://localhost:5000 ,VITE_API_BASE_URL=http://localhost:5000/api ,VITE_APP_VERSION=1.0.0 ,VITE_NODE_ENV=development
### **Truy cập trang dịch vụ**
luồng chọn chọn dịch vụ:
cus đăng nhập vào web vào trang dịch vụ sẽ thấy được 2 loại dịch vụ hành chính và dân sự , trong 2 loại dịch vụ này sẽ có từng tên dịch vụ kèm giá cả ,trong từng dịch vụ sẽ lại có loại lấy 2 mẫu (ví dụ của cha và con) và loại lấy 3 mẫu(ví dụ cha mẹ con) , mấy cái dịch vụ này sẽ được admin quản lý crud

- Sau khi **đăng nhập**, khách hàng truy cập vào **trang Dịch vụ**.
- Giao diện trang này hiển thị **hai tab phân loại dịch vụ**:
    - **Dịch vụ Hành chính**
    - **Dịch vụ Dân sự**
- Khi nhấn vào từng tab, hệ thống hiển thị một **bảng danh sách các dịch vụ** tương ứng. Trong mỗi bảng dịch vụ sẽ bao gồm các thông tin:
    - Tên dịch vụ
    - Giá cả
    - Mô tả
    - Loại mẫu (2 mẫu hoặc 3 mẫu)
    - Nút **Choose** (Chọn)
---
### 2. **Chọn dịch vụ**
- Khi khách hàng nhấn **Choose** cho một dịch vụ bất kỳ, hệ thống sẽ hiển thị **thông tin chi tiết** của dịch vụ bao gồm:
    - Loại dịch vụ (Hành chính hoặc Dân sự)
    - Tên và mô tả dịch vụ
    - Giá tiền
    - Loại mẫu (2 hay 3 mẫu)
    - Phương thức lấy mẫu:
        - Hành chính: **chỉ lấy tại cơ sở**
        - Dân sự: chọn giữa **lấy tại nhà** hoặc **tại cơ sở**
- Giao diện hiển thị nút **Xác nhận**. Khi khách hàng nhấn xác nhận:
    - Nếu dịch vụ là **lấy mẫu tại cơ sở**, khách hàng sẽ được **chọn ngày giờ lấy mẫu**.
    - Nếu dịch vụ là **lấy tại nhà**, **không hiển thị phần chọn ngày giờ**.
---
### 3. **Đăng ký dịch vụ*
- Sau khi xác nhận dịch vụ và chọn ngày giờ (nếu có), khách hàng nhấn nút **Đăng ký**.
- Hệ thống chuyển đến bước **thanh toán qua VNPay**.
- Lưu ý: Khách hàng **chỉ được phép thay đổi lịch hẹn trước 24 giờ** so với lịch đã đặt.
---
## 📌 **Luồng xử lý theo từng loại dịch vụ**

### 🔸 **Dịch vụ Hành chính** (Chỉ lấy mẫu tại cơ sở)
1. Sau khi thanh toán thành công, **đơn đăng ký được hệ thống tự động tạo**.
2. Nhân viên (staff) thấy đơn trên hệ thống, **mở đơn và xác nhận**.
3. Nhân viên nhập thông tin người cung cấp mẫu (tùy theo số mẫu đã chọn):
    - Họ tên
    - Năm sinh
    - Giới tính
    - Mối quan hệ
    - Loại mẫu
    - Cam kết
    - Ảnh chữ ký của khách hàng và staff
        - Nếu đã có ảnh chữ ký sẵn trong **UserProfile**, hệ thống tự động chèn vào.
4. Nhân viên nhập **mã kit** (ví dụ: K01), sau đó nhấn **Gửi**.
5. Đơn chuyển sang trạng thái **Đang xét nghiệm**.
6. Staff nhập kết quả xét nghiệm.
7. Quản lý (manager) xác nhận kết quả.
8. Staff nhấn **Trả kết quả** → đơn chuyển sang trạng thái **Đã có kết quả**.
9. Khách hàng thấy kết quả trên hệ thống và có thể **xuất kết quả ra file PDF**.
----
### 🔸 **Dịch vụ Dân sự**
#### 👉 **Lấy mẫu tại nhà*
1. Sau khi khách hàng thanh toán thành công, **đơn đăng ký được tự động tạo**.
2. Nhân viên thấy đơn, mở và xác nhận → nhập **mã kit** và chèn ảnh chữ ký staff (nếu có trong UserProfile, hệ thống tự động chèn).
3. Nhấn **Gửi** → đơn chuyển sang trạng thái **Đã gửi kit**.
4. Khách hàng nhận kit, mở đơn và nhập thông tin người cung cấp mẫu (tùy vào số mẫu đã chọn):
    - Họ tên
    - Năm sinh
    - Giới tính
    - Mối quan hệ
    - Loại mẫu
    - Cam kết
    - Ảnh chữ ký của tài khoản (tự động lấy nếu có sẵn trong UserProfile)
5. Khách hàng nhấn **Gửi**.
6. Nhân viên nhận mẫu, nhấn **Xác nhận** → đơn chuyển sang trạng thái **Đang xét nghiệm**.
7. Staff nhập kết quả.
8. Manager xác nhận kết quả.
9. Staff gửi kết quả → trạng thái **Đã có kết quả**.
10. Khách hàng xem kết quả trên hệ thống và có thể **xuất ra file PDF**.
#### 👉 **Lấy mẫu tại cơ sở**
1. Sau khi thanh toán thành công, **đơn đăng ký được hệ thống tự động tạo**.
2. Nhân viên (staff) thấy đơn trên hệ thống, **mở đơn và xác nhận**.
3. Nhân viên nhập thông tin người cung cấp mẫu (tùy theo số mẫu đã chọn):
    - Họ tên
    - Năm sinh
    - Giới tính
    - Mối quan hệ
    - Loại mẫu
    - Cam kết
    - Ảnh chữ ký của khách hàng và staff
        - Nếu đã có ảnh chữ ký sẵn trong **UserProfile**, hệ thống tự động chèn vào.    
4. Nhân viên nhập **mã kit** (ví dụ: K01), sau đó nhấn **Gửi**.
5. Đơn chuyển sang trạng thái **Đang xét nghiệm**.
6. Staff nhập kết quả xét nghiệm.
7. Quản lý (manager) xác nhận kết quả.
8. Staff nhấn **Trả kết quả** → đơn chuyển sang trạng thái **Đã có kết quả**.
9. Khách hàng thấy kết quả trên hệ thống và có thể **xuất kết quả ra file PDF**.
   ---
## ✅ **1. Validate & Ràng buộc cho Dịch vụ**
### 1.1. **Tại giao diện chọn dịch vụ*
- ✅ **Loại dịch vụ** chỉ được là `"Hành chính"` hoặc `"Dân sự"`.
- ✅ **Giá tiền** phải là số lớn hơn 0.
- ✅ **Số mẫu** chỉ được chọn là **2** hoặc **3**.
- ✅ **Cách lấy mẫu**
    - Với **hành chính**: chỉ cho phép **tại cơ sở**.
    - Với **dân sự**: được chọn **tại nhà** hoặc **tại cơ sở**.
- ✅ **Tên dịch vụ** phải duy nhất trong từng loại.
- ✅ **Mỗi dịch vụ** phải có mô tả (không để trống).
---
## ✅ **2. Validate & Ràng buộc khi đăng ký dịch vụ**

### 2.1. **Thông tin dịch vụ được tự động đổ vào đơn đăng ký**, không cho người dùng chỉnh sửa:
- Loại dịch vụ
- Tên dịch vụ
- Giá
- Số mẫu
- Cách lấy mẫu
### 2.2. **Chọn ngày giờ lấy mẫu**
- **Chỉ áp dụng khi chọn lấy mẫu tại cơ sở**.
- Phải chọn **ngày giờ trong tương lai**, tối thiểu **>= 24 giờ kể từ thời điểm hiện tại**.
- Không cho phép chọn **quá giới hạn thời gian phục vụ của trung tâm** (ví dụ: chỉ nhận mẫu từ 8:00 đến 17:00).
- **Không cho phép sửa lịch nếu còn < 24h đến lịch hẹn**.
---
## ✅ **3. Validate & Ràng buộc khi thanh toán**
- ✅ Thanh toán **bắt buộc** thông qua VNPay (các trường hợp chưa thanh toán thì không tạo đơn).
- ✅ Trạng thái thanh toán phải là **thành công** để tiếp tục quy trình.
- ✅ Mỗi đơn chỉ được thanh toán **một lần duy nhất**.
---
## ✅ **4. Validate thông tin người cung cấp mẫu**
Tùy theo số mẫu (2 hoặc 3), yêu cầu **điền đủ thông tin cho từng người**
- Họ tên: không rỗng, tối đa 100 ký tự.
- Năm sinh: là số, trong khoảng hợp lý (ví dụ: từ 1900 đến năm hiện tại).
- Giới tính: chỉ chọn “Nam”, “Nữ” 
- Mối quan hệ: không được trống.
- Loại mẫu: ví dụ “Niêm mạc miệng”, “Tóc”, v.v. → phải nằm trong danh sách hợp lệ.
- Cam kết: checkbox bắt buộc tick trước khi gửi.
- Ảnh chữ ký:
    - Nếu trong UserProfile đã có, sẽ tự động điền.
    - Nếu không có sẵn, **bắt buộc upload ảnh trước khi gửi**.
---
## ✅ **5. Validate mã kit*
- ✅ Mã kit là chuỗi **bắt đầu bằng chữ K** và theo sau là số (ví dụ: `K01`, `K12`,...).
- ✅ Mã kit **không được trùng lặp** trong các đơn khác.
- ✅ Bắt buộc phải nhập mã kit trước khi gửi đơn sang bước xét nghiệm.
---
## ✅ **6. Ràng buộc trạng thái đơn đăng ký**
- Đơn đăng ký có các **trạng thái xác định rõ ràng** và chỉ được thay đổi theo luồng hợp lệ:
    `Đang chờ thanh toán → Đã thanh toán → Đã xác nhận → Đã gửi kit (nếu lấy tại nhà) → Đang xét nghiệm → Đã có kết quả`
- Không được nhảy bước hoặc quay ngược trạng thái.
---
## ✅ **7. Ràng buộc nhập kết quả*
- ✅ Chỉ cho phép **staff đã được phân công** đơn mới được nhập kết quả.
- ✅ Phải nhập đầy đủ thông tin kết quả theo mẫu chuẩn.
- ✅ Sau khi nhập, **quản lý (manager)** phải xác nhận thì mới được staff gửi kết quả cho khách hàng.
---
## ✅ **8. Xuất PDF kết quả**

- Chỉ cho phép **khách hàng có đơn trạng thái “Đã có kết quả”** được xuất PDF.
- File PDF phải có:
    - Thông tin dịch vụ
    - Người cung cấp mẫu
    - Kết quả xét nghiệm
    - Ngày trả kết quả
    - Chữ ký số hoặc ảnh chữ ký của staff & customer (nếu có)
---
## ✅ **9. Bảo mật & Quyền truy cập*
- Khách hàng **chỉ được xem** các đơn và kết quả của chính họ.
- Nhân viên **chỉ được xem và chỉnh sửa** các đơn được phân công.
- Quản lý **chỉ được xác nhận kết quả**, không sửa nội dung xét nghiệm.
- Admin có quyền **CRUD dịch vụ**, không can thiệp đơn đăng ký.
  

+Yêu cầu 1: code theo cấu trúc này:
1. Frontend (React + Vite + Tailwind + TypeScript)
File Structure:
/src
  /assets                    # Static assets such as images, fonts, etc.
    /images                  # Folder for storing image files
      logo.png               # Example logo image
  /components                # Reusable UI components that can be used across multiple pages
    /auth                    # Folder for authentication-related components
      Login.tsx              # Login component, handles user login
      Register.tsx           # Register component, handles user registration
      ResetPassword.tsx      # Component for resetting a user's password
    /service                 # Components for displaying and managing services
      ServiceList.tsx        # Displays a list of all available services
      ServiceDetail.tsx      # Displays detailed information about a specific service
      ServiceForm.tsx        # Form for creating or editing a service
    /common                  # Shared components used across the application
      Button.tsx             # A button component
      Input.tsx              # A reusable input component
      Modal.tsx              # Modal component for showing popups
  /context                   # Contains global context for state management
    AuthContext.tsx          # Manages user authentication state globally
  /hooks                     # Custom React hooks
    useAuth.ts               # Custom hook for handling authentication logic (e.g., login, logout)
  /pages                     # Page components that represent different views/screens in the app
    Home.tsx                 # Home page, often acts as the landing page of the app
    Dashboard.tsx            # Dashboard page, typically seen after login
    ServicePage.tsx          # Page for displaying details about a specific service
  /services                  # Contains business logic and API calls
    authService.ts           # Handles API calls for authentication (login, register, etc.)
    serviceService.ts        # Handles API calls related to services (e.g., create, fetch services)
  /utils                     # Utility functions and helpers
    api.ts                   # Contains Axios or fetch API setup for making requests
    validation.ts            # Contains helper functions for validating input data
  App.tsx                    # Root component of the application where routes and context are defined
  main.tsx                  # Entry point of the application (renders the App component)
  tailwind.config.js         # Configuration file for Tailwind CSS (customization of the utility classes)
  tsconfig.json              # TypeScript configuration file to define compiler options and paths
  package.json               # Defines the project's dependencies, scripts, and metadata

- **`/assets`**: This folder holds static assets like images, fonts, and other media used throughout the app.
    - **`/images`**: Contains image files like logos, icons, and other visual assets.    
- **`/components`**: Reusable components that can be used throughout your app.
    - **`/auth`**: Contains all authentication-related components such as Login, Register, and ResetPassword.    
    - **`/service`**: Contains components related to services (e.g., listing services, displaying service details).   
    - **`/common`**: Generic components like buttons, inputs, and modals that are used throughout the app.    
- **`/context`**: Used for React's **Context API** to manage global state like authentication information.
    - **`AuthContext.tsx`**: Manages authentication status (e.g., if the user is logged in or not).    
- **`/hooks`**: Custom hooks used to encapsulate logic that is reused across components.
    - **`useAuth.ts`**: A custom hook to handle authentication logic (e.g., checking if a user is logged in).     
- **`/pages`**: This folder contains components that represent entire pages/screens of the app (e.g., Home, Dashboard).
    - **`Home.tsx`**: The landing page of the app.    
    - **`Dashboard.tsx`**: The main page shown after the user logs in.    
    - **`ServicePage.tsx`**: A page that displays information about a particular service.    
- **`/services`**: Contains files responsible for handling API calls and business logic.  
    - **`authService.ts`**: Contains functions for logging in, registering, and resetting passwords by interacting with the backend.
    - **`serviceService.ts`**: Handles API calls related to the services in the application.
- **`/utils`**: Utility functions that don't belong to any specific component but are used throughout the app
    - **`api.ts`**: A file containing the configuration for making HTTP requests to the backend API (e.g., using Axios).
    - **`validation.ts`**: A utility for validating form inputs or other data types.
- **`App.tsx`**: This is the root component where you define the main structure of the application, such as the layout, context providers, and routing.
- **`main.tsx`**: The entry point of your React application. It typically renders the **`App.tsx`** component and wraps it with **React.StrictMode** or other providers like `BrowserRouter` for routing.


--------------------
Backend Folder Structure
/src
  ├── /types
  │     └── type.d.ts                  # Type definitions for the entire backend
  ├── /config
  │     ├── config.ts                  # Environment config, DB connection setup
  │     └── cors.ts                    # CORS configuration for cross-origin requests
  ├── /constants
  │     ├── roles.ts                   # Constants for user roles (Admin, Customer, Staff)
  │     └── serviceTypes.ts            # Constants for service types (Administrative, Civil)
  ├── /middlewares
  │     ├── authMiddleware.ts          # Token validation middleware
  │     ├── errorMiddleware.ts         # Global error handler
  │     └── validationMiddleware.ts    # Input validation middleware
  ├── /controllers
  │     ├── authController.ts          # Handles login, registration, password change
  │     ├── serviceController.ts       # Handles CRUD operations for services
  │     └── userController.ts          # Handles user profile management
  ├── /services
  │     ├── authService.ts             # Authentication and account management logic
  │     ├── serviceService.ts          # Service logic for CRUD and other operations
  │     └── userService.ts             # Logic for user profile and account management
  ├── /models
  │     ├── Account.ts                 # User account model (attributes and relations)
  │     ├── Role.ts                    # User role model
  │     ├── Service.ts                 # Service model (for service-related data)
  │     ├── UserProfile.ts             # User profile model
  │     └── RefreshToken.ts            # Refresh token model for handling JWT refresh tokens
  ├── /routes
  │     ├── authRoutes.ts              # Routes for login, registration, password reset
  │     ├── serviceRoutes.ts           # Routes for CRUD operations for services
  │     └── userRoutes.ts              # Routes for user profile management
  ├── /utils
  │     ├── api.ts                     # Utility functions for API calls (frontend integration)
  │     ├── email.ts                   # Utility for sending emails (e.g., account verification)
  │     └── validation.ts              # Input validation functions
  ├── app.ts                           # Express app setup
  ├── index.ts                         # Entry point for the backend
  ├── tsconfig.json                    # TypeScript configuration
  ├── package.json                     # Package dependencies and scripts
  └── .env                             # Environment variables (DB credentials, JWT secret)

#### **1. `/types`** (TypeScript Type Declarations)
- **`type.d.ts`**:
    - This file is used to define **global types and interfaces** that can be accessed throughout the entire backend codebase. These types include user-related data, service models, JWT payloads, API response formats, etc. By declaring types globally, you avoid redundancy and ensure consistency across your app.
#### **2. `/config`** (Configuration Files
- **`config.ts`**:
    - This file is for setting up environment-specific configurations, such as the port number, JWT secret key, and other global settings.
    - It will read configurations from the `.env` file to make the application flexible across different environments (development, production, etc.)
    - This file establishes a **connection to SQL Server** using Sequelize, TypeORM, or another ORM (or raw SQL).
    - It can handle database connection setup, connection pooling, and error handling related to the database.
- **`cors.ts`** 
	- The **CORS** will be responsible for handling Cross-Origin Resource Sharing. It ensures that only allowed domains (front-end apps) can interact with your backend. CORS is critical when your backend and frontend are hosted on different domains. 
#### **3. `/constants`** (Fixed Constants for Application)
- **`roles.ts`**:
    - This file defines the **constants for user roles** (e.g., `Admin`, `Customer`, `Staff`) that the application uses.
    - These constants ensure that role-based permissions are handled consistently across the backend.
- **`serviceTypes.ts`**
    - This file stores constants related to different types of services offered in the system (e.g., `Administrative`, `Civil`).
    - It helps maintain a consistent naming convention and ensures that types of services are correctly handled.
#### **4. `/middlewares`** (Middleware Functions)
- **`authMiddleware.ts`**:
    - This middleware is responsible for **token validation**. It checks if the incoming request contains a valid JWT token in the headers.    
    - It ensures that only authenticated users can access certain routes, such as `/profile` or `/settings`.     
- **`errorMiddleware.ts`**:
    - This is a **global error handler** that catches any errors thrown throughout the application and returns a consistent error response to the client.    
    - It's useful for ensuring that unhandled errors don’t cause unexpected server crashes.    
- **`validationMiddleware.ts`**:
    - This middleware performs **input validation** for request bodies, ensuring that required fields are present and formatted correctly (e.g., validating email formats, password strength).    
    - It is typically used in conjunction with libraries like `express-validator` to ensure that user input is safe and conforms to the expected structure.   
#### **5. `/controllers`** (Request Handlers)
- **`authController.ts`**:
    - This controller handles requests related to **authentication** (login, registration, password reset).  
    - It receives the request, interacts with the service layer (e.g., `authService`), and sends the appropriate response back to the client.    
- **`serviceController.ts`**:
    - This controller manages **CRUD operations for services** (creating, reading, updating, and deleting services).  
    - For example, it would handle requests to create a new service or get a list of all available services.   
- **`userController.ts`**:    
    - This controller handles requests related to **user profiles** (updating user details, fetching profile information, etc.).    
    - It typically communicates with the `userService` to fetch or modify user-related data.
#### **6. `/services`** (Business Logic Layer)
- **`authService.ts`**:
    - Contains **business logic** for user authentication, including handling login requests, validating credentials, and generating JWT tokens.    
    - It interfaces with the database (via models like `Account` and `Role`) to manage user records.    
- **`serviceService.ts`**: 
    - This service layer is responsible for the **business logic related to services**. For instance, it would define how to create a service or how to update an existing one.    
    - It communicates with the database models to perform the necessary actions.    
- **`userService.ts`**:
    - This service layer handles business logic related to **user profiles**, such as updating user details and interacting with the `UserProfile` model.    
    - It might handle tasks like checking if a username is available or fetching the user's data from the database.       

#### **8. `/routes`** (Express Routes)
- **`authRoutes.ts`**:
    - Defines the routes related to **authentication** operations, such as `/login`, `/register`, and `/reset-password`.   
    - Each route is connected to a controller function that handles the specific task.  
- **`serviceRoutes.ts`**
    - Defines routes related to **service management**. For example, it could contain routes for CRUD operations like `/create-service`, `/update-service`, or `/get-services`. 
- **`userRoutes.ts`**:
    - Defines routes for **user profile management** like `/update-profile`, `/get-profile`.
#### **9. `/utils`** (Utility Functions)
- **`api.ts`**:
    - Contains utility functions to facilitate interaction between the frontend and backend (for instance, making API calls). 
- **`email.ts`**:
    - Contains functions for sending **emails** such as account confirmation, password reset, etc. It could use services like **Nodemailer** to send emails.    
- **`validation.ts`**:
    - Contains utility functions for **input validation** that can be reused across various parts of the application. For example, validating if an email is in the correct format.   

#### **10. Other Files**
- **`app.ts`**:
    - This is the main **Express application setup** file. It initializes Express, configures middlewares (like CORS, body parsing, error handling), and connects routes to the app.
- **`main.ts`**:
    - The entry point to the backend application, where the Express app is started and listens for incoming requests on a specified port.
- **`tsconfig.json`**:
    - TypeScript configuration file where you specify compiler options (like module resolution and output directory) to ensure TypeScript compiles correctly.    
- **`package.json`**:
    - Contains metadata about the project (name, version, dependencies, scripts) and manages package dependencies for the project.    
- **`.env`**:
    - A configuration file that stores environment variables (such as `DB_HOST`, `DB_USER`, `JWT_SECRET`). This allows you to keep sensitive information outside the source code and change configuration settings easily based on the environment.
      
+yêu cầu 2: đọc và phân tích đày đủ chi tiết:
4 role customer,staff,manager,admin thì làm luồng đăng kí(bằng email,password,confirmPassword,full name,phone number,address,date of birth,signature image)(role customer là mặc định sau đó được admin phân lại khi thấy danh sách các đăng kí trong dashboard), đăng nhập(email ,password) ,access refresh token,đổi mật khẩu(chỉ cần có mật khẩu cũ), phân quyền do admin phân , với tài khoản admin là mặc định chỉ có 1 lưu trong .env 
luồng chọn chọn dịch vụ:
cus đăng nhập vào web vào trang dịch vụ sẽ thấy được 2 loại dịch vụ hành chính và dân sự , trong 2 loại dịch vụ này sẽ có từng tên dịch vụ kèm giá cả ,trong từng dịch vụ sẽ lại có loại lấy 2 mẫu (ví dụ của cha và con) và loại lấy 3 mẫu(ví dụ cha mẹ con) , mấy cái dịch vụ này sẽ được admin quản lý crud

- Sau khi **đăng nhập**, khách hàng truy cập vào **trang Dịch vụ**.
- Giao diện trang này hiển thị **hai tab phân loại dịch vụ**:
    - **Dịch vụ Hành chính**
    - **Dịch vụ Dân sự**
- Khi nhấn vào từng tab, hệ thống hiển thị một **bảng danh sách các dịch vụ** tương ứng. Trong mỗi bảng dịch vụ sẽ bao gồm các thông tin:
    - Tên dịch vụ
    - Giá cả
    - Mô tả
    - Loại mẫu (2 mẫu hoặc 3 mẫu)
    - Nút **Choose** (Chọn)
---
### 2. **Chọn dịch vụ**
- Khi khách hàng nhấn **Đăng ký** cho một dịch vụ bất kỳ, hệ thống sẽ hiển thị **thông tin chi tiết** của dịch vụ bao gồm:
    - Loại dịch vụ (Hành chính hoặc Dân sự)
    - Tên và mô tả dịch vụ
    - Giá tiền
    - Loại mẫu (2 hay 3 mẫu)
    - Phương thức lấy mẫu:
        - Hành chính: **chỉ lấy tại cơ sở**
        - Dân sự: chọn giữa **lấy tại nhà** hoặc **tại cơ sở**
- Giao diện hiển thị nút **Xác nhận**. Khi khách hàng nhấn xác nhận:
    - Nếu dịch vụ là **lấy mẫu tại cơ sở**, khách hàng sẽ được **chọn ngày giờ lấy mẫu**.
    - Nếu dịch vụ là **lấy tại nhà**, **không hiển thị phần chọn ngày giờ**.
---
### 3. **Gửi Đăng ký dịch vụ*
- Sau khi xác nhận dịch vụ và chọn ngày giờ (nếu có), khách hàng nhấn nút **Gửi đăng ký**.
- Hệ thống chuyển đến bước **thanh toán qua VNPay**.

---
## 📌 **Luồng xử lý theo từng loại dịch vụ**

### 🔸 **Dịch vụ Hành chính** (Chỉ lấy mẫu tại cơ sở)
1. Sau khi thanh toán thành công, **đơn đăng ký được hệ thống tự động tạo**.
2. customer sẽ thấy đơn đang ở trạng thái cần thông tin(input infor) , sau khi điền đầy đủ thông tin và xác nhận 
3. Nhân viên (staff) sẽ thấy đơn trên hệ thống trạng thái dang chờ(pending), **xem và xác nhận**. trang thái đổi thành đã xác nhận(confirmed)
4. Nhân viên xác nhận lại mẫu mà khách hàng đăng ký và cập nhật trạng thái  **Đang xét nghiệm(in progress)**.
5. Staff nhập kết quả xét nghiệm.
6. Quản lý (manager thấy những test result đang chờ xác nhận (pending)
7. Quản lý (manager) xác nhận kết quả.→ đơn chuyển sang trạng thái **Đã có kết quả(verified)**.nếu từ chối thì xóa testresult và testrequest vẫn là in progress
8. Khách hàng thấy kết quả trên hệ thống và có thể **xuất kết quả ra file PDF**.
----
### 🔸 **Dịch vụ Dân sự**
#### 👉 **Lấy mẫu tại nhà*
1. Sau khi khách hàng thanh toán thành công, **đơn đăng ký được tự động tạo**.
2. Nhân viên thấy đơn, xem và xác nhận →  kit sẽ được gửi đến cho customer-> customer gủi lại mẫu kit
3. Nhân viên nhận mẫu, tick đã lây mẫu→ đơn chuyển sang trạng thái **Đang xét nghiệm**.
4. Staff nhập kết quả.
5. Quản lý (manager) xác nhận kết quả.→ đơn chuyển sang trạng thái **Đã có kết quả(verified)**.nếu từ chối thì xóa testresult và testrequest vẫn là in progress
6. Khách hàng xem kết quả trên hệ thống và có thể **xuất ra file PDF**.
#### 👉 **Lấy mẫu tại cơ sở**
1. Sau khi thanh toán thành công, **đơn đăng ký được hệ thống tự động tạo**.
2. customer sẽ thấy đơn đang ở trạng thái cần thông tin(input infor) , sau khi điền đầy đủ thông tin và xác nhận 
3. Nhân viên (staff) sẽ thấy đơn trên hệ thống trạng thái dang chờ(pending), **xem và xác nhận**. trang thái đổi thành đã xác nhận(confirmed)
4. Nhân viên xác nhận lại mẫu mà khách hàng đăng ký và cập nhật trạng thái  **Đang xét nghiệm(in progress)**.
5. Staff nhập kết quả xét nghiệm.
6. Quản lý (manager thấy những test result đang chờ xác nhận (pending)
7. Quản lý (manager) xác nhận kết quả.→ đơn chuyển sang trạng thái **Đã có kết quả(verified)**.nếu từ chối thì xóa testresult và testrequest vẫn là in progress
8. Khách hàng thấy kết quả trên hệ thống và có thể **xuất kết quả ra file PDF**.
   ---
## ✅ **1. Validate & Ràng buộc cho Dịch vụ**
### 1.1. **Tại giao diện chọn dịch vụ*
- ✅ **Loại dịch vụ** chỉ được là "Hành chính" hoặc "Dân sự".
- ✅ **Giá tiền** phải là số lớn hơn 0.
- ✅ **Số mẫu** chỉ được chọn là **2** hoặc **3**.
- ✅ **Cách lấy mẫu**
    - Với **hành chính**: chỉ cho phép **tại cơ sở**.
    - Với **dân sự**: được chọn **tại nhà** hoặc **tại cơ sở**.
- ✅ **Tên dịch vụ** phải duy nhất trong từng loại.
- ✅ **Mỗi dịch vụ** phải có mô tả (không để trống).
---
## ✅ **2. Validate & Ràng buộc khi đăng ký dịch vụ**

### 2.1. **Thông tin dịch vụ được tự động đổ vào đơn đăng ký**, không cho người dùng chỉnh sửa:
- Loại dịch vụ
- Tên dịch vụ
- Giá
- Số mẫu
- Cách lấy mẫu
### 2.2. **Chọn ngày giờ lấy mẫu**
- **Chỉ áp dụng khi chọn lấy mẫu tại cơ sở**.
- Phải chọn **ngày giờ trong tương lai**, tối thiểu **>= 24 giờ kể từ thời điểm hiện tại**.
- Không cho phép chọn **quá giới hạn thời gian phục vụ của trung tâm** (ví dụ: chỉ nhận mẫu từ 8:00 đến 17:00).
- **Không cho phép sửa lịch nếu còn < 24h đến lịch hẹn**.
---
## ✅ **3. Validate & Ràng buộc khi thanh toán**
- ✅ Thanh toán **bắt buộc** thông qua VNPay (các trường hợp chưa thanh toán thì không tạo đơn).
- ✅ Trạng thái thanh toán phải là **thành công** để tiếp tục quy trình.
- ✅ Mỗi đơn chỉ được thanh toán **một lần duy nhất**.
---
## ✅ **4. Validate thông tin người cung cấp mẫu**
Tùy theo số mẫu (2 hoặc 3), yêu cầu **điền đủ thông tin cho từng người**
- Họ tên: không rỗng, tối đa 100 ký tự.
- Năm sinh: là số, trong khoảng hợp lý (ví dụ: từ 1900 đến năm hiện tại).
- Giới tính: chỉ chọn “Nam”, “Nữ” 
- Mối quan hệ: không được trống.
- Loại mẫu: ví dụ “Niêm mạc miệng”, “Tóc”, v.v. → phải nằm trong danh sách hợp lệ.
- Cam kết: checkbox bắt buộc tick trước khi gửi.
- Ảnh chữ ký:
    - Nếu trong UserProfile đã có, sẽ tự động điền.
    - Nếu không có sẵn, **bắt buộc upload ảnh trước khi gửi**.
---
## ✅ **5. Validate mã kit*
- ✅ Mã kit là chuỗi **bắt đầu bằng chữ K** và theo sau là số (ví dụ: K01, K12,...).
- ✅ Mã kit **không được trùng lặp** trong các đơn khác.
- ✅ Bắt buộc phải nhập mã kit trước khi gửi đơn sang bước xét nghiệm.
---
## ✅ **6. Ràng buộc trạng thái đơn đăng ký**
- Đơn đăng ký có các **trạng thái xác định rõ ràng** và chỉ được thay đổi theo luồng hợp lệ:
    Đang chờ thanh toán → Đã thanh toán → Đã xác nhận → Đã gửi kit (nếu lấy tại nhà) → Đang xét nghiệm → Đã có kết quả
- Không được nhảy bước hoặc quay ngược trạng thái.
---
## ✅ **7. Ràng buộc nhập kết quả*
- ✅ Chỉ cho phép **staff đã được phân công** đơn mới được nhập kết quả.
- ✅ Phải nhập đầy đủ thông tin kết quả theo mẫu chuẩn.
- ✅ Sau khi nhập, **quản lý (manager)** phải xác nhận thì mới được staff gửi kết quả cho khách hàng.
---
## ✅ **8. Xuất PDF kết quả**

- Chỉ cho phép **khách hàng có đơn trạng thái “Đã có kết quả”** được xuất PDF.
- File PDF phải có:
    - Thông tin dịch vụ
    - Người cung cấp mẫu
    - Kết quả xét nghiệm
    - Ngày trả kết quả
    - Chữ ký số hoặc ảnh chữ ký của staff & customer (nếu có)
---
## ✅ **9. Bảo mật & Quyền truy cập*
- Khách hàng **chỉ được xem** các đơn và kết quả của chính họ.
- Nhân viên **chỉ được xem và chỉnh sửa** các đơn được phân công.
- Quản lý **chỉ được xác nhận kết quả**, không sửa nội dung xét nghiệm.
- Admin có quyền **CRUD dịch vụ**, không can thiệp đơn đăng ký.
  
+Yêu cầu 3: kết hợp Yêu cầu 1 và yêu cầu 2 (bắt buộc phải code theo các file đã tạo sẵn) lại rồi code từ database tới backend rồi tới frontend
- nếu database thiếu bảng thì tạo bảng cho đủ với cái luồng yêu cầu  2, có ràng buộc đầy đủ hợp lý,id dùng int number chứ không được dùng uuid,dùng axios, async await,..., react router
- code đúng ràng buộc type prettier eslint 
+Yêu cầu 4:
1. **Xác Minh Tính Tích Hợp Giữa Frontend và Backend**:

   * **Đảm bảo Kết Nối API**: Xác nhận rằng API của backend (Express.js với TypeScript) được kết nối chính xác với frontend (React, Vite, TypeScript và Tailwind CSS).
   * **Kiểm Tra Các Điểm Cuối API**: Đảm bảo tất cả các điểm cuối API từ backend (dành cho dịch vụ, hồ sơ người dùng, v.v.) có thể truy cập từ frontend và trả về dữ liệu chính xác.
   * **Xử Lý Xung Đột**: Kiểm tra xem có bất kỳ xung đột nào giữa các tệp của frontend và backend (ví dụ: tên biến, cách nhập tệp) không.
   * **Đảm Bảo Dữ Liệu Đầy Đủ**: Xác nhận rằng tất cả dữ liệu cần thiết (như chi tiết dịch vụ, hồ sơ người dùng) được hiển thị đầy đủ trên giao diện frontend mà không thiếu thông tin.

2. Kiểm Tra và Hoàn Thiện Cấu Hình Database
Kiểm tra sơ đồ cơ sở dữ liệu:
Đảm bảo rằng tất cả các bảng cần thiết (ví dụ: bảng services, roles, users, refresh tokens, ...) đã được tạo và có quan hệ chính xác.
Nếu thiếu bảng, tạo các bảng với các ràng buộc và quan hệ đầy đủ (ví dụ: khóa ngoại, kiểu dữ liệu, ràng buộc duy nhất, ...).
Ví dụ: Bảng RefreshToken phải lưu các refresh token hợp lệ liên kết với người dùng và có thời gian hết hạn.

Chạy lại migrations database nếu cần, để đảm bảo rằng cơ sở dữ liệu đã được thiết lập chính xác.
3. **Xem Lại Các Tệp Hiện Tại**:

   * **Sử Dụng Lại Các Tệp Có Sẵn**: Trước khi tạo tệp mới, hãy kiểm tra xem chức năng yêu cầu đã có trong mã nguồn hiện tại chưa. Nếu có, **sử dụng lại tệp** đó để tránh trùng lặp mã.
   * **Tạo Tệp Mới**: Nếu cần tạo tệp mới, hãy thông báo cho tôi biết **vị trí** và **mục đích** của tệp đó để tránh lỗi và cấu hình sai.

4. **Yêu Cầu Giao Diện Frontend**:

   * **Đảm Bảo UI Theo Vai Trò Người Dùng**: Kiểm tra rằng giao diện người dùng hoạt động chính xác cho từng vai trò người dùng (Admin, Customer, Staff). Các vai trò phải có quyền truy cập phù hợp vào các trang như hồ sơ người dùng và danh sách dịch vụ.
   * **Xác Minh Các Trang UI**: Đảm bảo các trang **hồ sơ người dùng**, **trang dịch vụ**, **chi tiết dịch vụ** đã được xây dựng đầy đủ và hiển thị chính xác.
   * **Kiểm Tra Thiết Kế Phản Hồi**: Đảm bảo giao diện sử dụng **Tailwind CSS** có thiết kế phản hồi tốt và hiển thị đúng trên nhiều kích thước màn hình khác nhau.

5. **Kiểm Tra Backend**:

   * **Kiểm Tra API**: Xác nhận rằng tất cả **điểm cuối API** (POST, PUT, GET, DELETE) hoạt động đúng. Kiểm tra các API để đảm bảo chúng trả về dữ liệu chính xác cho frontend.
   * **Xử Lý Dữ Liệu Đúng Cách**: Đảm bảo **validation dữ liệu** đang được thực hiện đúng cả ở frontend (trước khi gửi) và backend (qua middleware hoặc trong logic dịch vụ).
   * **Xử Lý Lỗi**: Đảm bảo backend xử lý đúng các lỗi khi dữ liệu không hợp lệ được gửi lên, và frontend hiển thị lỗi đó một cách dễ hiểu cho người dùng.
   * **Kết Nối Cơ Sở Dữ Liệu**: Đảm bảo backend kết nối đúng với cơ sở dữ liệu và các điểm cuối API tương tác với cơ sở dữ liệu mà không gặp lỗi (ví dụ: các hoạt động CRUD cho dịch vụ, hồ sơ người dùng).

6. **Các Nhiệm Vụ Chung**:

   * **Sử Dụng Enum (Nếu Cần)**: Đảm bảo sử dụng **enum** cho các giá trị cố định như loại dịch vụ, trạng thái đơn hàng, v.v. nếu chưa có.
   * **Cấu Hình và Biến Môi Trường**: Kiểm tra rằng các **tệp cấu hình** (như `.env`) đã được thiết lập chính xác cho cả frontend và backend, và các biến môi trường (như thông tin kết nối DB, JWT secret) được sử dụng đúng.
   * **Dữ Liệu Đồng Nhất**: Đảm bảo backend luôn trả về **dữ liệu đồng nhất** để frontend có thể hiển thị đúng.

7. **Kiểm Tra**:

   * **Kiểm Tra Phản Hồi API**: Kiểm tra tất cả các điểm cuối API (qua Postman hoặc gọi trực tiếp từ frontend) để xác nhận rằng chúng trả về phản hồi chính xác.
   * **Kiểm Tra Giao Diện**: Kiểm tra các luồng giao diện người dùng và các tương tác của người dùng để đảm bảo rằng dữ liệu hiển thị đúng và lỗi được xử lý một cách thân thiện.




---

**+Requirement 2: Read and analyze fully and in detail:**  
4 roles customer, staff, manager, admin will implement the registration flow (with email, password, confirmPassword, full name, phone number, address, date of birth, signature image) (role customer is default, then assigned by admin when viewing the list of registrations in dashboard), login (email, password), access refresh token, change password (only need old password), role assignment by admin, with the admin account being the default (only one, stored in .env).

Service selection flow:  
Customer logs in to the web, enters the service page and will see 2 types of services: administrative and civil. Within these, there are service names with prices. Each service has either 2-sample (e.g. father and child) or 3-sample (e.g. father, mother, child) types. These services are CRUD managed by admin.

- After **logging in**, customer goes to **Service page**.
    
- This page shows **two service category tabs**:
    
    - **Administrative services**
        
    - **Civil services**
        
- Clicking each tab shows a **list table of services**. Each table includes:
    
    - Service name
        
    - Price
        
    - Description
        
    - Sample type (2 or 3 samples)
        
    - **Choose** button
        

---

### 2. **Choose service**

- When customer clicks **Choose** for any service, the system displays **detailed information** of the service, including:
    
    - Service type (Administrative or Civil)
        
    - Name and description
        
    - Price
        
    - Sample type (2 or 3)
        
    - Sample collection method:
        
        - Administrative: **only at facility**
            
        - Civil: choose between **at home** or **at facility**
            
- Interface displays **Confirm** button. When customer clicks confirm:
    
    - If service is **at facility**, customer can **choose date and time**
        
    - If **at home**, **do not display date/time selection**
        

---

### 3. **Service registration**

- After confirming service and selecting date/time (if any), customer clicks **Register**.
    
- System moves to **VNPay payment** step.
    
- Note: Customers **may only change appointment time if more than 24 hours before** the booked time.
    

---

## 📌 **Processing flows per service type**

### 🔸 **Administrative Service** (Only collect sample at facility)

1. After successful payment, **application is auto-created**.
    
2. Staff sees order in the system, **opens and confirms**.
    
3. Staff confirms the sample registered and updates status to **Sample received**.
    
4. Order moves to **Testing** status.
    
5. Staff enters test result.
    
6. Manager confirms result → order moves to **Result available** status.
    
7. Customer sees result and can **export to PDF**.
    

---

### 🔸 **Civil Service**

#### 👉 **Sample collected at home**

1. After successful payment, **application auto-created**.
    
2. Staff sees order, opens and confirms → **kit code** in TestAtHome and staff signature image (if available in UserProfile, system auto-inserts).
    
3. Click **Send** → order moves to **Kit sent**.
    
4. Customer sends kit back, clicks **Send**.
    
5. Staff receives sample, clicks **Confirm** if incorrect then updates → order moves to **Testing**.
    
6. Staff enters result.
    
7. Manager confirms result → status **Result available**.
    
8. Customer views result and can **export to PDF**.
    

#### 👉 **Sample collected at facility**

1. After successful payment, **application auto-created**.
    
2. Staff sees order in the system, **opens and confirms**.
    
3. Staff confirms the sample registered and updates status to **Sample received**.
    
4. Order moves to **Testing** status.
    
5. Staff enters test result.
    
6. Manager confirms result → order moves to **Result available** status.
    
7. Customer sees result and can **export to PDF**.
    

---

## ✅ **1. Validate & Constraints for Service**

### 1.1. **At service selection UI**

- ✅ **Service type** must be only "Administrative" or "Civil".
    
- ✅ **Price** must be a number greater than 0.
    
- ✅ **Sample count** only 2 or 3.
    
- ✅ **Collection method**
    
    - For **administrative**: only **at facility**.
        
    - For **civil**: choose **at home** or **at facility**.
        
- ✅ **Service name** must be unique within type.
    
- ✅ **Each service** must have a description (not empty).
    

---

## ✅ **2. Validate & Constraints for service registration**

### 2.1. **Service information is auto-filled into registration, cannot be edited by user:**

- Service type
    
- Service name
    
- Price
    
- Sample count
    
- Collection method
    

### 2.2. **Choose date/time for sampling**

- **Only applies for “at facility”**
    
- Must be **future date/time**, at least **>= 24 hours from now**.
    
- Cannot choose **beyond center’s working hours** (e.g. only accept samples 8:00–17:00).
    
- **Cannot reschedule if less than 24h left**.
    

---

## ✅ **3. Payment validation**

- ✅ Payment **must use VNPay** (no order is created if not paid).
    
- ✅ Payment status must be **success** to continue.
    
- ✅ Each order can only be paid **once**.
    

---

## ✅ **4. Validate sample provider information**

Depending on sample count (2 or 3), require **full info for each person**

- Full name: not empty, max 100 characters
    
- Year of birth: number, reasonable range (e.g. 1900–current year)
    
- Gender: only “Male”, “Female”
    
- Relationship: cannot be empty
    
- Sample type: e.g. “Buccal swab”, “Hair”, etc. → must be from valid list
    
- Commitment: required checkbox tick before submitting
    
- Signature image:
    
    - If already in UserProfile, auto-fill.
        
    - If not, **must upload before submitting**.
        

---

## ✅ **5. Validate kit code**

- ✅ Kit code must **start with K** followed by number (e.g. K01, K12,...).
    
- ✅ Kit code **must not duplicate** in other orders.
    
- ✅ Must enter kit code before moving order to testing.
    

---

## ✅ **6. Registration status constraints**

- Registration has **clear statuses** and can only be changed in valid flow:  
    Awaiting payment → Paid → Confirmed → Kit sent (if at home) → Testing → Result available
    
- Cannot skip steps or go back.
    

---

## ✅ **7. Result entry constraints**

- ✅ Only **assigned staff** can enter result for an order.
    
- ✅ Must enter full result info per standard template.
    
- ✅ After entry, **manager** must confirm before staff sends result to customer.
    

---

## ✅ **8. Export PDF result**

- Only **customers with “Result available” orders** can export PDF.
    
- PDF must include:
    
    - Service info
        
    - Sample provider info
        
    - Test result
        
    - Result date
        
    - Digital or image signature of staff & customer (if any)
        

---

## ✅ **9. Security & Access rights**

- Customer **can only see** their own orders and results.
    
- Staff **can only view and edit** orders assigned to them.
    
- Manager **can only confirm results**, not edit result content.
    
- Admin can **CRUD services**, not interfere with registrations.
    

---

**+Requirement 3:** Combine Requirement 1 and 2 (must code according to files already created) then code from database to backend to frontend.

- If the database lacks tables, create enough tables with constraints to fit flow of requirement 2, use reasonable constraints, id must be int number, not uuid, use axios, async/await,..., react router
    
- Code must match type, prettier, eslint
    

**+Requirement 4:**

1. **Verify Integration Between Frontend and Backend**:
    
    - **Ensure API Connection**: Confirm that backend API (Express.js with TypeScript) is correctly connected to frontend (React, Vite, TypeScript, and Tailwind CSS).
        
    - **Check API Endpoints**: Ensure all backend API endpoints (for services, user profiles, etc.) are accessible from frontend and return correct data.
        
    - **Handle Conflicts**: Check for any conflicts between frontend and backend files (e.g. variable names, file imports).
        
    - **Ensure Complete Data**: Confirm all necessary data (service details, user profile, etc.) is fully shown in frontend with nothing missing.
        
2. Check and Finalize Database Configuration  
    Check database schema:  
    Ensure all required tables (e.g. services, roles, users, refresh tokens, ...) are created and properly related.  
    If tables are missing, create them with all necessary constraints and relationships (e.g. foreign keys, unique constraints, ...).  
    Example: RefreshToken table must store valid refresh tokens linked to users with expiry time.
    

Run database migrations if needed, to ensure DB is set up correctly.

3. **Review Current Files**:
    
    - **Reuse Existing Files**: Before creating a new file, check if the required functionality already exists. If yes, **reuse that file** to avoid duplicate code.
        
    - **Create New Files**: If a new file is needed, let me know the **location** and **purpose** so as to avoid errors and misconfigurations.
        
4. **Frontend UI Requirements**:
    
    - **Ensure UI By User Role**: Check that UI works correctly for each role (Admin, Customer, Staff). Roles must have appropriate access to pages such as user profile, service list, etc.
        
    - **Verify UI Pages**: Ensure **user profile**, **service page**, **service detail** pages are fully built and display correctly.
        
    - **Check Responsive Design**: Ensure Tailwind CSS UI is responsive and displays well on all screen sizes.
        
5. **Check Backend**:
    
    - **Test APIs**: Confirm all **API endpoints** (POST, PUT, GET, DELETE) work correctly. Check that APIs return correct data for frontend.
        
    - **Correct Data Handling**: Ensure **data validation** is performed both on frontend (before sending) and backend (via middleware or service logic).
        
    - **Error Handling**: Ensure backend properly handles errors when invalid data is submitted, and frontend shows errors in a user-friendly way.
        
    - **Database Connection**: Ensure backend correctly connects to DB and API endpoints interact with database without errors (e.g. CRUD for services, user profiles).
        
6. **General Tasks**:
    
    - **Use Enum (If Needed)**: Ensure enums are used for fixed values like service type, order status, etc., if not present.
        
    - **Config and Env Variables**: Check that **config files** (e.g. `.env`) are correctly set up for both frontend and backend, and env vars (e.g. DB connection, JWT secret) are used properly.
        
    - **Consistent Data**: Ensure backend always returns **consistent data** for correct frontend display.
        
7. **Testing**:
    
    - **Test API Responses**: Test all API endpoints (via Postman or direct frontend calls) to confirm correct responses.
        
    - **Test UI**: Test all UI flows and user interactions to make sure data displays correctly and errors are user-friendly.
        

---

Let me know if you want it formatted in markdown or docx, or want any section further explained!


[main.tsx]
  └──▶ [App.tsx]
          ├──▶ [ProtectedRoute.tsx] ────▶ [hooks/useAuth.ts] ────▶ [context/AuthContext.tsx]
          │                                               └──▶ [services/authService.ts] ────▶ [utils/api.ts]
          │                                                                                 └──▶ [.env]
          └──▶ [pages/*] (admin, staff, manager, customer)
[App.tsx] ────▶ [utils/messages.ts]         ← chứa các thông báo (SUCCESS, ERROR...)
         └──▶ [components/Common/*]            ← Header, Footer, Button, Input,...
         └──▶ [components/Auth/*]              ← Login, Register, ResetPassword
[Login.tsx] ────▶ [hooks/useAuth.ts] ────▶ [authService.ts]
               └──▶ [utils/messages.ts]
[Register.tsx] ────▶ [authService.ts]
                  └──▶ [utils/messages.ts]
[ResetPassword.tsx] ────▶ [authService.ts]
[pages/admin/*.tsx] ────▶ [services/userService.ts], [services/managerService.ts], [services/serviceService.ts]
                      └──▶ [components/Common/Input.tsx], [Button.tsx]
[pages/staff/*.tsx] ────▶ [services/serviceService.ts], [components/Common]
[pages/customer/*.tsx] ────▶ [components/Common], [services/serviceService.ts]
[services/*.ts] ────▶ [utils/api.ts]
                  └──▶ [utils/types.ts]
[hooks/useAuth.ts] ────▶ [context/AuthContext.tsx]
                     └──▶ [services/authService.ts]
                     └──▶ [utils/types.ts]
[context/AuthContext.tsx] ────▶ [services/authService.ts] ────▶ [utils/types.ts]
[ProtectedRoute.tsx] ────▶ [hooks/useAuth.ts]


_____________________________
// All admin routes require authentication and admin role

// Get all users for role assignment

router.get('/users', authenticate, authorize(['Admin']), userController.getAllUsers)

// Update user role

router.put('/users', authenticate, authorize(['Admin']), userController.updateUserRole)

// Public routes

router.post('/register', registerHandler)

router.post('/login', loginHandler)

router.post('/refresh-token', refreshAccessTokenHandler)

router.post('/logout', logoutHandler)

// Protected routes

router.get('/profile', authenticate, userController.getProfile)

router.put('/change-password', authenticate, PasswordChangeHandler)

// Get all services (public)

router.get('/', serviceController.getAllServices)

router.get('/type/:serviceType', serviceController.getServiceByType)

// Get service by ID (public)

router.get('/:serviceId', serviceController.getServiceById)

// Admin only routes

router.post(

'/',

authenticate,

authorize(['Admin']),

serviceController.createService

)

router.put('/:serviceId', authenticate, authorize(['Admin']), serviceController.updateService)

router.delete('/:serviceId', authenticate, authorize(['Admin']), serviceController.deleteService)

const router = Router()

router.get(

'/testRequestCustomer',

authenticate,

authorize(['Customer']),

testRequestController.getTestRequestByCustomer

)

router.get('/testRequestStaff', authenticate, authorize(['Staff']), testRequestController.getTestRequestByStaff)

router.get('/', authenticate, authorize(['Staff']), testRequestController.getAllTestRequests)

router.post('/', authenticate, authorize(['Customer']), testRequestController.createTestRequest)

router.put(

'/:testRequestId/submit-sample',

authenticate,

authorize(['Customer']),

testRequestController.submitSampleInfo

)

router.get('/:testRequestId/pdf', authenticate, testRequestController.exportTestResultsPDF)

// router.get('/:testRequestId', authenticate, testRequestController.getTestRequestById)

router.put('/:testRequestId/confirm', authenticate, authorize(['Staff']), testRequestController.createTestConfirm)

router.put('/:testRequestId/in-progress', authenticate, authorize(['Staff']), testRequestController.markInProgress)

router.post(

'/:testRequestId/createTestResult',

authenticate,

authorize(['Staff']),

testRequestController.createTestResultbyStaff

)

router.get('/viewCreateTestResult', authenticate, authorize(['Manager']), testRequestController.viewCreateTestResult)

router.put(

'/:testResultId/verifyTestResult',

authenticate,

authorize(['Manager']),

testRequestController.verifyTestResult

)

router.get(

'/:testRequestId/results',

authenticate,

authorize(['Staff', 'Manager']),

testRequestController.getTestResults

)

// Configure multer for file uploads

const storage = multer.diskStorage({

destination: (req, file, cb) => {

cb(null, 'uploads/signatures/')

},

filename: (req, file, cb) => {

const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1e9)

cb(null, 'signature-' + uniqueSuffix + path.extname(file.originalname))

}

})

const upload = multer({

storage,

limits: {

fileSize: 5 * 1024 * 1024 // 5MB

},

fileFilter: (req, file, cb) => {

const allowedTypes = ['image/jpeg', 'image/jpg', 'image/png', 'image/gif']

if (allowedTypes.includes(file.mimetype)) {

cb(null, true)

} else {

cb(new Error('Invalid file type. Only JPEG, PNG, and GIF are allowed.'))

}

}

})

// Protected user routes

router.get('/profile', authenticate, userController.getProfile)

router.put('/profile', authenticate, userController.updateProfile)

const app = express()

// Create upload directories if they don't exist

const uploadDirs = ['uploads', 'uploads/signatures', 'uploads/documents']

uploadDirs.forEach((dir) => {

if (!fs.existsSync(dir)) {

fs.mkdirSync(dir, { recursive: true })

}

})

// Security middleware

app.use(helmet())

// Rate limiting

const limiter = rateLimit({

windowMs: 15 * 60 * 1000, // 15 minutes

max: 100, // limit each IP to 100 requests per windowMs

message: {

success: false,

message: 'Too many requests from this IP, please try again later.'

},

standardHeaders: true,

legacyHeaders: false

})

app.use(limiter)

// CORS configuration

app.use(

cors({

origin: config.cors.origin,

credentials: config.cors.credentials,

methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],

allowedHeaders: ['Content-Type', 'Authorization']

})

)

// Body parsing middleware

app.use(express.json({ limit: '10mb' }))

app.use(express.urlencoded({ extended: true, limit: '10mb' }))

// Serve static files

app.use('/uploads', express.static(path.join(__dirname, '../uploads')))

// Health check endpoint

app.get('/health', (req, res) => {

res.status(200).json({

success: true,

message: 'Server is running',

timestamp: new Date().toISOString(),

environment: process.env.NODE_ENV || 'development'

})

})

if (config.nodeEnv === 'development') {

app.use(morgan('dev'))

}

// API routes

app.use('/api/auth', authRoutes)

app.use('/api/users', userRoutes)

// app.use("/api/services", serviceRoutes)

app.use('/api/admin', adminRoute)

app.use('/api/services', serviceRoutes)

// app.use('/api/payment', paymentRoutes)

app.use('/api/test-requests', testRequestRoutes)

// 404 handler

app.use('*', (req, res) => {

res.status(404).json({

success: false,

message: `Route ${req.originalUrl} not found`,

method: req.method

})

})

// Global error handler (must be last)

app.use(errorMiddleware)

----------------------------------------

return result.recordset[0] ---->kiểu dữ liệu trả ra

--------------------------------

-- Create Roles table

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='Roles' AND xtype='U')

BEGIN

CREATE TABLE Roles (

RoleID INT IDENTITY(1,1) PRIMARY KEY,

RoleName NVARCHAR(50) NOT NULL UNIQUE,

);

END

GO

-- Create Accounts table

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='Accounts' AND xtype='U')

BEGIN

CREATE TABLE Accounts (

AccountID INT IDENTITY(1,1) PRIMARY KEY,

Email NVARCHAR(255) NOT NULL UNIQUE,

PasswordHash NVARCHAR(255) NOT NULL,

RoleID INT NOT NULL,

CreatedAt DATETIME DEFAULT GETDATE(),

FOREIGN KEY (RoleID) REFERENCES Roles(RoleID)

);

END

GO

-- Create UserProfiles table

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='UserProfiles' AND xtype='U')

BEGIN

CREATE TABLE UserProfiles (

ProfileID INT IDENTITY(1,1) PRIMARY KEY,

AccountID INT NOT NULL,

FullName NVARCHAR(100),

PhoneNumber NVARCHAR(20),

Address NVARCHAR(500),

DateOfBirth DATE,

SignatureImage NVARCHAR(500),

CreatedAt DATETIME DEFAULT GETDATE(),

UpdatedAt DATETIME DEFAULT GETDATE(),

FOREIGN KEY (AccountID) REFERENCES Accounts(AccountID) ON DELETE CASCADE

);

END

-- Create RefreshTokens table

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='RefreshTokens' AND xtype='U')

BEGIN

CREATE TABLE RefreshTokens (

TokenID INT IDENTITY(1,1) PRIMARY KEY,

AccountID INT NOT NULL,

Token NVARCHAR(500) NOT NULL UNIQUE,

ExpiresAt DATETIME NOT NULL,

Revoked BIT DEFAULT 0,

CreatedAt DATETIME DEFAULT GETDATE(),

FOREIGN KEY (AccountID) REFERENCES Accounts(AccountID) ON DELETE CASCADE

);

END

GO

-- Create Services table

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='Services' AND xtype='U')

BEGIN

CREATE TABLE Services (

ServiceID INT IDENTITY(1,1) PRIMARY KEY,

ServiceName NVARCHAR(200) NOT NULL,

ServiceType NVARCHAR(50) NOT NULL CHECK (ServiceType IN ('Administrative', 'Civil')),

Description NVARCHAR(1000) NOT NULL,

Price DECIMAL(10,2) NOT NULL CHECK (Price > 0),

SampleCount INT NOT NULL CHECK (SampleCount IN (2, 3)),

CreatedAt DATETIME DEFAULT GETDATE(),

UpdatedAt DATETIME DEFAULT GETDATE(),

);

END

GO

-- Create TestRequests table

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='TestRequests' AND xtype='U')

BEGIN

CREATE TABLE TestRequests (

TestRequestID INT IDENTITY(1,1) PRIMARY KEY,

AccountID INT NOT NULL,

ServiceID INT NOT NULL,

AssignedTo NVARCHAR(20),

Status NVARCHAR(50) DEFAULT 'Input Infor' CHECK (Status IN ('Input Infor' ,'Pending', 'Confirmed' ,'In Progress', 'Completed')),

CollectionMethod NVARCHAR(50) NOT NULL CHECK (CollectionMethod IN ('Home', 'Facility')),

Appointment DATE,

CreatedAt DATETIME DEFAULT GETDATE(),

UpdatedAt DATETIME DEFAULT GETDATE(),

FOREIGN KEY (ServiceID) REFERENCES Services(ServiceID),

FOREIGN KEY (AccountID) REFERENCES Accounts(AccountID),

);

END

GO

CREATE TABLE TestAtHome(

TestHomeID INT IDENTITY(1,1) PRIMARY KEY,

TestRequestID INT,

KitID VARCHAR(4),

Status NVARCHAR(50) DEFAULT 'Sent' CHECK (Status IN ('Sent','Received'))

FOREIGN KEY(TestRequestID ) REFERENCES dbo.TestRequests(TestRequestID)

);

CREATE TABLE TestAtFacility(

TestFacilityID INT IDENTITY(1,1) PRIMARY KEY,

TestRequestID INT,

FOREIGN KEY(TestRequestID ) REFERENCES dbo.TestRequests(TestRequestID)

);

CREATE TABLE SampleCategories(

SampleID INT IDENTITY(1,1) PRIMARY KEY,

SampleType NVARCHAR(100),

Status NVARCHAR(50) DEFAULT 'Pending' CHECK (Status IN ('Pending' ,'Confirmed')),

TestRequestID INT,

TesterName NVARCHAR(30),

CMND NVARCHAR(12),

YOB INT,

Gender NVARCHAR(10) NOT NULL CHECK (Gender IN ('Male', 'Female')),

Relationship NVARCHAR(40),

SignatureImage NVARCHAR(500),

FOREIGN KEY(TestRequestID ) REFERENCES dbo.TestRequests(TestRequestID)

);

CREATE TABLE TestResults(

TestResultID INT IDENTITY(1,1) PRIMARY KEY,

TestRequestID INT UNIQUE,

Result NVARCHAR(20),

EnterBy INT,

EnterDate DATETIME DEFAULT GETDATE(),

ConfirmBy NVARCHAR(20),

ConfirmDate DATETIME DEFAULT GETDATE(),

Status NVARCHAR(50) DEFAULT 'Pending' CHECK (Status IN ('Pending', 'Verified')),

FOREIGN KEY(TestRequestID ) REFERENCES dbo.TestRequests(TestRequestID)

);

GO

CREATE TABLE Blogs(

BlogID INT IDENTITY(1,1) PRIMARY KEY,

AccountID INT,

Tilte NVARCHAR(100),

Description NVARCHAR(1000) NOT NULL,

Image NVARCHAR(500),

CreatedAt DATETIME DEFAULT GETDATE(),

FOREIGN KEY (AccountID) REFERENCES Accounts(AccountID) ON DELETE CASCADE

);

CREATE TABLE Feedbacks(

FeedbackID INT IDENTITY(1,1) PRIMARY KEY,

TestResultID INT,

AccountID INT,

Comment NVARCHAR(50),

Rating INT,

Status NVARCHAR(50) DEFAULT 'Pending' CHECK (Status IN ('Pending', 'Rated')),

CreatedAt DATETIME DEFAULT GETDATE(),

FOREIGN KEY (AccountID) REFERENCES Accounts(AccountID) ,

FOREIGN KEY (TestResultID) REFERENCES TestResults(TestResultID)

);

---------------------------------------


## ️ **BACKEND STRUCTURE (Node.js + Express + TypeScript)**

### **Cấu trúc thư mục:**

```plaintext
bloodline-dna-backend/
├── src/
│   ├── app.ts                 # Main application setup
│   ├── index.ts              # Server entry point
│   ├── config/
│   │   ├── config.ts         # Environment configuration
│   │   └── database.ts       # Database connection & setup
│   ├── controllers/          # Request handlers
│   │   ├── authController.ts
│   │   ├── staffController.ts
│   │   ├── adminController.ts
│   │   └── ...
│   ├── services/             # Business logic layer
│   │   ├── authService.ts
│   │   ├── staffService.ts
│   │   └── ...
│   ├── routes/               # API route definitions
│   │   ├── authRoutes.ts
│   │   ├── staffRoutes.ts
│   │   └── ...
│   ├── middlewares/          # Custom middleware
│   │   ├── authMiddleware.ts
│   │   └── errorMiddleware.ts
│   ├── constants/            # Application constants
│   ├── utils/                # Utility functions
│   └── types/                # TypeScript type definitions
├── nodemon.json              # Development configuration
└── package.json
```

### **Cách hoạt động Backend:**

1. **Entry Point (`index.ts`):**


```typescript
// Khởi tạo server và database
const startServer = async () => {
  await initializeApp()  // Connect DB, create admin
  app.listen(config.port)
}
```

2. **Application Setup (`app.ts`):**


```typescript
// Setup middleware, routes, error handling
app.use(cors(), helmet(), express.json())
app.use('/api/auth', authRoutes)
app.use('/api/staff', staffRoutes)
// Global error handler
app.use(errorMiddleware)
```

3. **MVC Pattern:**

4. **Routes** → **Controllers** → **Services** → **Database**
5. Routes định nghĩa endpoints
6. Controllers xử lý HTTP requests/responses
7. Services chứa business logic
8. Database layer tương tác với SQL Server





---

## **FRONTEND STRUCTURE (React + TypeScript + Vite)**

### **Cấu trúc thư mục:**

```plaintext
bloodline-dna-frontend/
├── src/
│   ├── App.tsx               # Main app component & routing
│   ├── main.tsx             # React entry point
│   ├── components/          # Reusable UI components
│   │   ├── Common/
│   │   │   ├── Header.tsx
│   │   │   ├── Footer.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   └── ...
│   │   └── Auth/
│   │       ├── Login.tsx
│   │       └── Register.tsx
│   ├── pages/               # Page components
│   │   ├── Home.tsx
│   │   ├── staff/
│   │   │   ├── StaffDashboard.tsx
│   │   │   ├── ManageRequests.tsx
│   │   │   └── ...
│   │   ├── admin/
│   │   └── customer/
│   ├── services/            # API service layer
│   │   ├── authService.ts
│   │   ├── staffService.ts
│   │   └── ...
│   ├── context/             # React Context for state
│   │   └── AuthContext.tsx
│   ├── hooks/               # Custom React hooks
│   │   └── useAuth.ts
│   ├── utils/               # Utility functions
│   │   ├── api.ts           # Axios configuration
│   │   └── types.ts         # TypeScript types
│   └── index.css            # Global styles (Tailwind)
├── index.html
├── vite.config.ts           # Vite configuration
└── package.json
```

### **Cách hoạt động Frontend:**

1. **Entry Point (`main.tsx`):**


```typescript
// Render React app
ReactDOM.createRoot(document.getElementById('root')!).render(
  <App />
)
```

2. **App Component (`App.tsx`):**


```typescript
// Setup routing, authentication, global providers
<Router>
  <AuthProvider>
    <Routes>
      <Route path="/staff/dashboard" element={<StaffDashboard />} />
      // Protected routes with role-based access
    </Routes>
  </AuthProvider>
</Router>
```

3. **Architecture Pattern:**

4. **Components** → **Services** → **API** → **Backend**
5. Components render UI và handle user interactions
6. Services gọi API endpoints
7. API client (axios) gửi HTTP requests
8. Context API quản lý global state





---

## **CÁCH FRONTEND VÀ BACKEND TƯƠNG TÁC**

### **1. Authentication Flow:**

```typescript
// Frontend: AuthContext.tsx
const login = async (data: LoginRequest) => {
  const response = await authService.login(data)  // Call API
  setUser(response.user)
  navigate('/dashboard')
}

// Backend: authController.ts
export const login = async (req, res) => {
  const user = await authService.validateUser(email, password)
  const tokens = generateTokens(user)
  res.json({ user, ...tokens })
}
```

### **2. Data Flow Example (Staff Dashboard):**

**Frontend:**

```typescript
// StaffDashboard.tsx
useEffect(() => {
  const fetchData = async () => {
    const data = await staffService.getDashboardStats()  // API call
    setStats(data.stats)
  }
}, [])
```

**API Service:**

```typescript
// staffService.ts
async getDashboardStats() {
  const response = await apiClient.get('/staff/dashboard/stats')
  return response.data.data
}
```

**Backend:**

```typescript
// staffController.ts
getDashboardStats = async (req, res) => {
  const stats = await staffService.getDashboardStats(staffId)
  res.json({ data: { stats, recentRequests } })
}
```

### **3. API Client Configuration:**

```typescript
// utils/api.ts
export const apiClient = axios.create({
  baseURL: 'http://localhost:5000/api',
  headers: { 'Content-Type': 'application/json' }
})

// Auto-attach JWT token
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('accessToken')
  if (token) config.headers.Authorization = `Bearer ${token}`
  return config
})
```

---

## ️ **SECURITY & AUTHENTICATION**

1. **JWT Token-based Authentication**
2. **Role-based Access Control (RBAC)**
3. **Protected Routes** với middleware
4. **Token Refresh** mechanism
5. **CORS** configuration


## **DATABASE INTEGRATION**

- **SQL Server** với **mssql** package
- **Connection pooling** cho performance
- **Prepared statements** để tránh SQL injection
- **Transaction support** cho data consistency


Cấu trúc này tuân theo **best practices** của modern web development với **separation of concerns**, **scalability**, và **maintainability**.