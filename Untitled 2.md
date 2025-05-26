
1. WHAT – xUnit là gì?
•	xUnit là một framework mã nguồn mở dùng để viết unit test cho các ứng dụng .NET.
•	Là một phần của xUnit family (như JUnit cho Java, NUnit cho .NET...).
________________________________________
2. WHY – Tại sao lại dùng xUnit?
giúp code sạch
- **Không dùng `[TestClass]`, `[TestMethod]` dư thừa**  
    → Chỉ cần `[Fact]`, `[Theory]`, giúp test ngắn gọn.
- **Không có `SetUp()` / `TearDown()` như NUnit**  
    → Thay bằng constructor và `IDisposable`, tránh lạm dụng cấu trúc phức tạp.
- **Không khuyến khích test phụ thuộc lẫn nhau**  
    → Tạo test **độc lập**, tăng tính tường minh và khả năng tái sử dụng.
- **Tách biệt rõ giữa test logic và dữ liệu test**  
    → Nhờ `Theory`, `InlineData`, `MemberData` → code test ngắn hơn và rõ ràng hơn.
•	Giúp đảm bảo chất lượng mã nguồn bằng cách kiểm tra tự động các hàm/method.
•	Tăng độ tin cậy khi refactor hoặc thêm tính năng mới.
•	Tích hợp tốt với CI/CD, giúp phát hiện lỗi sớm.
•	Có hỗ trợ tốt cho Test-Driven Development (TDD).
________________________________________
3. WHO – Ai sử dụng xUnit?
•	Các developer .NET (C#, VB.NET…).
•	Các QA/Tester viết automated tests cho backend.
•	Được sử dụng bởi các công ty phát triển phần mềm chuyên nghiệp và dự án mã nguồn mở.
________________________________________
4. HISTORY – Lịch sử phát triển
•	Ra đời năm 2007.
•	Do Brad Wilson và James Newkirk phát triển (James là người tạo ra NUnit).
•	Được xây dựng lại từ đầu để phù hợp với .NET hiện đại.
•	Được Microsoft lựa chọn là unit test framework mặc định cho ASP.NET Core.
________________________________________
5. CUSTOMER REFERENCE – Ai đang sử dụng?
Được sử dụng rộng rãi trong cộng đồng .NET.
•	Microsoft (ASP.NET Core).
Microsoft **tự dùng xUnit** trong các thư viện quan trọng như:
- ASP.NET Core
- Entity Framework Core
- .NET Runtime tests
Vì Microsoft là nhà phát triển chính của .NET, việc họ dùng xUnit khiến nó trở thành “chuẩn ngầm” cho các công ty khác.
Điều này khiến nhiều công ty khác cũng **đồng bộ theo hệ sinh thái .NET mới**.
•	StackOverflow sử dụng trong hệ thống kiểm thử.
•	JetBrains, GitHub Projects, và hàng ngàn công ty lớn nhỏ.
