---
date: 2024-10-20T16:04:00
---
Related:[[JavaOOP]]
Tag: #java #trick
___

### **Khi nào cần khởi tạo với `#new` và khi nào dùng `#null`:**

1. **Khởi tạo với `null`:**
   - **`User user = null;`** được sử dụng khi chúng ta không biết chắc chắn rằng biến `user` sẽ có giá trị ngay lập tức. Nó thường được sử dụng trong các tình huống mà giá trị của biến này sẽ được xác định sau khi thực hiện một thao tác (ví dụ, sau khi truy vấn cơ sở dữ liệu, kiểm tra điều kiện, hoặc xử lý một nhiệm vụ nào đó).
   - Trong đoạn code của bạn, giá trị của `user` được gán trong quá trình truy vấn cơ sở dữ liệu. Nếu không có kết quả từ cơ sở dữ liệu, `user` sẽ vẫn giữ giá trị `null`, cho phép chương trình biết rằng không có người dùng hợp lệ.

   Ví dụ:
   ```java
   if (rs.next()) {
       user = new User(useName, password, lastName, isAdmin);
   }
   ```

   - Chỉ khi tìm thấy kết quả phù hợp (`rs.next()` trả về `true`), một đối tượng `User` mới được tạo ra bằng `new User(...)`. Điều này giúp tiết kiệm bộ nhớ và tránh tạo ra đối tượng `User` không cần thiết nếu truy vấn không trả về kết quả nào.

2. **Khởi tạo với `new`:**
   - Bạn có thể sử dụng **`User user = new User();`** nếu ngay từ đầu bạn muốn tạo một đối tượng `User` rỗng và sau đó sẽ cập nhật thông tin của nó. Điều này có thể hữu ích khi bạn chắc chắn rằng sẽ cần sử dụng đối tượng này ngay lập tức, hoặc khi muốn khởi tạo mặc định một đối tượng `User` mà không phụ thuộc vào điều kiện hay truy vấn bên ngoài.
   
   Ví dụ:
   ```java
   User user = new User();
   user.setUserName(useName);
   user.setPassword(password);
   ```

##### **Tại sao trong trường hợp này khởi tạo với `null` là hợp lý:**
   - Trong đoạn code của bạn, giá trị của biến `user` được xác định dựa trên kết quả của một truy vấn cơ sở dữ liệu. Do đó, ban đầu biến `user` không cần được khởi tạo với `new User()`, vì không chắc chắn rằng có thông tin người dùng hợp lệ. Nếu kết quả truy vấn không trả về thông tin người dùng nào, thì `user` vẫn giữ giá trị `null` và không có đối tượng `User` nào được tạo ra không cần thiết.

   Điều này giúp tối ưu hóa việc sử dụng bộ nhớ và tránh tạo ra các đối tượng không cần thiết.

##### **Khi nào nên dùng `new`:**
   - Nếu bạn biết chắc chắn rằng đối tượng `User` sẽ luôn luôn cần được khởi tạo và sử dụng ngay lập tức, dù cho có thông tin đầy đủ hay không, thì bạn có thể khởi tạo bằng `new User()` từ đầu.

Nhưng trong trường hợp của đoạn code này, khởi tạo `user = null` trước, và chỉ khởi tạo đối tượng `User` khi có thông tin hợp lệ là cách làm tối ưu và hợp lý.


### So sánh `#RequestDispatcher.forward()` và `#response.sendRedirect()`

Cả **`RequestDispatcher.forward()`** và **`response.sendRedirect()`** đều được sử dụng để điều hướng người dùng từ servlet này đến servlet khác hoặc đến một trang HTML, nhưng chúng có một số **khác biệt quan trọng** trong cách hoạt động:

##### 1. **RequestDispatcher.forward()**

- **Chuyển tiếp (Forward)**: Chuyển tiếp yêu cầu và phản hồi đến một tài nguyên (servlet hoặc trang HTML) khác **trên cùng server** mà **không thay đổi URL** trên trình duyệt của người dùng.
- **Giữ nguyên yêu cầu ban đầu**: Dữ liệu của yêu cầu (request) và phản hồi (response) vẫn được giữ nguyên, có nghĩa là mọi tham số và dữ liệu request ban đầu sẽ vẫn tồn tại trong quá trình chuyển tiếp.
- **Nội bộ (Internal)**: Việc chuyển tiếp được thực hiện **bên trong server**, nên người dùng sẽ không biết rằng họ đã được chuyển đến một servlet hoặc trang khác, vì URL trên trình duyệt không thay đổi.

#### Ví dụ:
Nếu bạn sử dụng:
```java
RequestDispatcher rd = request.getRequestDispatcher("LoginServlet");
rd.forward(request, response);
```
- Trình duyệt vẫn hiển thị URL ban đầu (ví dụ: `ProcessServlet`), nhưng yêu cầu được chuyển nội bộ đến `LoginServlet`. Người dùng không nhận thấy sự thay đổi về URL trên trình duyệt.

#### Khi nào nên dùng:
- Sử dụng khi bạn muốn giữ nguyên dữ liệu của yêu cầu và thực hiện xử lý thêm mà người dùng không cần biết họ đã được chuyển hướng.
- Thích hợp khi bạn muốn làm việc với dữ liệu từ `request`, như dữ liệu từ form hoặc các tham số khác.

---

##### 2. **response.sendRedirect()**

- **Chuyển hướng (Redirect)**: Chuyển hướng trình duyệt đến một URL khác, và **URL trên trình duyệt sẽ thay đổi**.
- **Tạo yêu cầu mới**: Khi sử dụng `sendRedirect()`, một **yêu cầu HTTP mới** sẽ được tạo ra. Điều này có nghĩa là bất kỳ thông tin nào trong yêu cầu gốc sẽ **không được giữ lại**, và bạn phải gửi lại dữ liệu cần thiết qua tham số URL hoặc các phương thức khác nếu cần.
- **Chuyển hướng ngoại bộ (External)**: Không chỉ chuyển hướng đến các tài nguyên nội bộ (trong cùng ứng dụng), bạn còn có thể chuyển hướng đến **bất kỳ URL nào**, bao gồm các trang web bên ngoài.

#### Ví dụ:
Nếu bạn sử dụng:
```java
response.sendRedirect("LoginServlet");
```
- Trình duyệt sẽ thực hiện một yêu cầu HTTP mới đến `LoginServlet`, và URL trong thanh địa chỉ của trình duyệt sẽ thay đổi thành `LoginServlet`.

#### Khi nào nên dùng:
- Sử dụng khi bạn muốn **chuyển hướng hoàn toàn** người dùng đến một trang khác (ví dụ, sau khi đăng nhập thành công, chuyển đến trang chủ).
- Thích hợp khi bạn muốn điều hướng người dùng đến một trang hoặc một domain khác, hoặc khi bạn cần xóa dữ liệu cũ trong yêu cầu gốc.

---

##### So sánh tóm tắt:

| **Thuộc tính**                | **RequestDispatcher.forward()**                                | **response.sendRedirect()**                             |
|-------------------------------|----------------------------------------------------------------|---------------------------------------------------------|
| **Thay đổi URL trên trình duyệt** | Không (giữ nguyên URL cũ)                                      | Có (URL mới xuất hiện trên trình duyệt)                  |
| **Yêu cầu mới**                | Không (yêu cầu cũ được giữ nguyên)                             | Có (yêu cầu mới được tạo ra)                             |
| **Giữ dữ liệu request**        | Có (dữ liệu của request ban đầu vẫn còn)                       | Không (dữ liệu request ban đầu không được giữ lại)       |
| **Chuyển hướng nội bộ/ngoại bộ**| Chỉ nội bộ (trong cùng server)                                | Có thể chuyển hướng cả nội bộ và ngoại bộ                |
| **Tốc độ**                     | Nhanh hơn do không tạo yêu cầu HTTP mới                        | Chậm hơn một chút do tạo ra yêu cầu HTTP mới             |
| **Sử dụng khi nào**            | Khi bạn cần chuyển tiếp yêu cầu nội bộ mà không thay đổi URL    | Khi bạn cần điều hướng đến trang khác hoặc ngoài ứng dụng |

---

##### Ví dụ sử dụng:

1. **`RequestDispatcher.forward()`**:
   Bạn có một form đăng nhập và muốn chuyển tiếp đến một servlet xử lý xác thực đăng nhập. Nhưng dù quá trình xử lý đăng nhập xảy ra, người dùng sẽ không nhận ra vì URL vẫn giữ nguyên.

2. **`response.sendRedirect()`**:
   Sau khi người dùng đăng nhập thành công, bạn muốn chuyển họ đến trang chủ và URL của trang chủ cần xuất hiện trên thanh địa chỉ của trình duyệt.

---

Tóm lại, **`forward()`** giữ nguyên yêu cầu gốc và không thay đổi URL, trong khi **`sendRedirect()`** tạo một yêu cầu mới và thay đổi URL trên trình duyệt.

### So sánh `#JSP (JavaServer Pages)` và `#HTML (HyperText Markup Language)`

##### 1. **JSP là gì?**
   **JSP (JavaServer Pages)** là một công nghệ Java được sử dụng để tạo ra các trang web động trên phía máy chủ. JSP cho phép nhúng mã Java vào các trang HTML để xử lý logic động và tương tác với cơ sở dữ liệu, API, hoặc các tài nguyên khác trên server.

   Khi một yêu cầu được gửi tới trang JSP, mã Java bên trong trang JSP sẽ được thực thi trên máy chủ và kết quả sẽ được trả về dưới dạng HTML tĩnh cho trình duyệt của người dùng.

   - **Tập tin JSP** có phần mở rộng là **.jsp**.
   - JSP là một phần của nền tảng **Java EE** và thường được triển khai trên các máy chủ ứng dụng như **Apache Tomcat**, **GlassFish**, hoặc **JBoss**.

   **Ví dụ về trang JSP:**
   ```jsp
   <html>
   <body>
       <h1>Hello World from JSP</h1>
       <p>The current time is: <%= new java.util.Date() %></p>
   </body>
   </html>
   ```
   Ở đây, `<%= new java.util.Date() %>` là mã Java được nhúng vào trang HTML. Khi trang JSP này được yêu cầu, server sẽ xử lý mã Java và trả về thời gian hiện tại trong trang HTML cho trình duyệt.

##### 2. **HTML là gì?**
   **HTML (HyperText Markup Language)** là ngôn ngữ đánh dấu được sử dụng để tạo ra các trang web tĩnh. Nó chỉ chứa cấu trúc và nội dung của trang web, như văn bản, hình ảnh, liên kết và các phần tử đa phương tiện khác. HTML **không có logic phía máy chủ** và **không thể xử lý mã Java hoặc bất kỳ mã lập trình nào**.

   - **Tập tin HTML** có phần mở rộng là **.html** hoặc **.htm**.
   - HTML được xử lý trực tiếp bởi trình duyệt và không có tương tác trực tiếp với máy chủ ngoài việc tải tệp.

   **Ví dụ về trang HTML:**
   ```html
   <html>
   <body>
       <h1>Hello World from HTML</h1>
       <p>The current time is: 12:00 PM</p>
   </body>
   </html>
   ```
   Trong HTML, không có mã động. Mọi thông tin được viết sẵn và hiển thị tĩnh cho người dùng.

##### 3. **Sự khác biệt chính giữa JSP và HTML**

| **Tiêu chí**          | **JSP (JavaServer Pages)**                                      | **HTML (HyperText Markup Language)**                      |
|-----------------------|----------------------------------------------------------------|----------------------------------------------------------|
| **Loại trang**        | Trang web **động** (xử lý phía server).                        | Trang web **tĩnh** (xử lý phía client).                  |
| **Mã động**           | Có thể nhúng mã Java vào trong trang để xử lý logic động.      | Không thể nhúng mã lập trình. Chỉ hiển thị nội dung tĩnh.|
| **Xử lý mã**          | Xử lý mã Java trên máy chủ và trả về HTML tĩnh cho người dùng. | Không xử lý mã. Chỉ cung cấp cấu trúc và nội dung cho trình duyệt. |
| **Khả năng tương tác với cơ sở dữ liệu** | Có thể kết nối với cơ sở dữ liệu, API, và các tài nguyên khác trên server. | Không có khả năng tương tác trực tiếp với cơ sở dữ liệu. |
| **Phía xử lý**        | Phía máy chủ (server-side).                                   | Phía trình duyệt (client-side).                          |
| **Phần mở rộng**      | `.jsp`.                                                       | `.html` hoặc `.htm`.                                      |
| **Hiệu suất**         | Chậm hơn HTML do cần xử lý logic trên server trước khi gửi về client. | Nhanh hơn vì không có xử lý server-side, chỉ là tệp tĩnh.|
| **Ứng dụng**          | Thường dùng cho các ứng dụng web động, cần xử lý logic phía máy chủ. | Thường dùng cho các trang web tĩnh hoặc nội dung tĩnh.   |

##### 4. **JSP và HTML cùng hoạt động như thế nào?**

Khi bạn sử dụng **JSP**, trang JSP thực chất được **dịch sang HTML** và được máy chủ web gửi về cho trình duyệt sau khi xử lý các mã Java. Do đó, người dùng cuối chỉ nhìn thấy mã **HTML** sau khi các mã Java trong JSP đã được thực thi.

Quy trình hoạt động của JSP:

1. **Yêu cầu từ trình duyệt**: Người dùng gửi yêu cầu đến trang JSP (ví dụ: `index.jsp`).
2. **Máy chủ xử lý**: Máy chủ (như Tomcat) xử lý yêu cầu này và thực thi mã Java trong trang JSP.
3. **Trả về kết quả HTML**: Máy chủ trả về kết quả là trang HTML đã được xử lý (bao gồm cả dữ liệu động) cho trình duyệt.
4. **Trình duyệt hiển thị**: Trình duyệt hiển thị trang HTML tĩnh với nội dung mà người dùng có thể tương tác.

##### 5. **Khi nào sử dụng JSP và khi nào sử dụng HTML?**

- **Sử dụng HTML** khi bạn chỉ cần hiển thị nội dung tĩnh, không yêu cầu xử lý phức tạp trên máy chủ. Ví dụ: các trang giới thiệu, blog đơn giản hoặc nội dung chỉ hiển thị mà không cần xử lý.
  
- **Sử dụng JSP** khi bạn cần tạo các trang web **động**, tương tác với cơ sở dữ liệu, thực hiện các hành động trên server (như xác thực người dùng, xử lý form, v.v.), và trả về kết quả động cho người dùng.

##### Kết luận:

- **JSP** giúp bạn tạo các ứng dụng web động bằng cách kết hợp **mã Java** với **HTML** để xử lý các logic phức tạp trên máy chủ.
- **HTML** là ngôn ngữ đánh dấu được sử dụng để tạo các trang web tĩnh, chỉ cung cấp nội dung cho người dùng mà không có xử lý động. 

Tùy thuộc vào mục đích của trang web, bạn có thể sử dụng **JSP** để xử lý các yêu cầu động hoặc chỉ sử dụng **HTML** cho các trang tĩnh đơn giản.