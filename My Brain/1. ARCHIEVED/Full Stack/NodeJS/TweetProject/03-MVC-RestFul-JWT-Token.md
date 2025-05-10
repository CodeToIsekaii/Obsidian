---
date: 2025-05-08T11:57:00
---
Related : [[NodeJS]]
Tag: #nodejs #prj #mvc #restful #jwt #token
___

# MVC Pattern trong Express

đây là mô hình mvc cơ bản
![[image-23.png]]
ta có thể thấy điểm tương đồng trên với hình dưới này
![[image-22.png]]
ta có để xem lại code của mình phần register để thấy sự tương đồng trong mô hình mvc mà mình đang làm

# REST

## REST là gì?(viết chuản này đỡ đọc document tự hiểu, lỡ document mày viết dở quá)

**REST** là viết tắt của **RE**presentational **S**tate **T**ransfer (chuyển trạng thái), là quy ước một số quy tắc ràng buộc khi thiết kế hệ thống mạng.

REST giúp client tương tác với server mà không cần biết cách hoạt động server như thế nào.

REST có một số ràng buộc:

- **Uniform Interface** (Giao diện thông nhất)

- **Stateless** (Không trạng thái) cái gì ko đáng kể ko nên lưu, lưu nhiều tốn dung lượng

- **Cacheable** (Dữ liệu có thể lưu vào bộ nhớ cache)

- **Client-Server** (Máy khách - Máy chủ)client truyền lên server xử lý xong trả về

- **Layered System** (Hệ thống phân lớp) chia các tầng dữ liệu ra middleware,controller,route,...

- **Code on Demand** (Code theo yêu cầu) ứng dụng cần gì thì code đó chứ ko nghĩ ra gì thì code

## API là gì?

80% ứng dụng trên thế giới này đều dựa trên 1 bộ api của ứng dụng khác
**API** là cơ chế cho phép 2 thành phần phần mềm giao tiếp với nhau bằng một tập hợp các định nghĩa và giao thức.

**Ví dụ**: hệ thống phần mềm của cơ quan thời tiết chứa dữ liệu về thời tiết hàng ngày. Ứng dụng thời tiết trên điện thoại của bạn sẽ “trò chuyện” với hệ thống này qua API và hiển thị thông tin cập nhật về thời tiết hàng ngày trên điện thoại của bạn.

## RESTful API là gì?

**RESTful API** là một bộ API viết theo tiêu chuẩn REST. Chuẩn REST đọc khá là khó hiểu, học thuật vậy nên API của bạn chỉ cần áp dụng những kỹ thuật dưới đây thì có thể coi là chuẩn REST.

### Sử dụng các phương thức HTTP để request có ý nghĩa

- **GET**: Đọc tài nguyên
- **PUT**: Cập nhật tài nguyên
- **DELETE**: Xóa tài nguyên
- **POST**: Tạo mới tài nguyên
  còn nữa nhưng thường xài 4 này

### Cung cấp tên tài nguyên hợp lý

Tạo ra một API tuyệt vời là 80% nghệ thuật và 20% khoa học.
Ví dụ:

- Sử dụng id định danh cho URL thay vì dùng query-string. Sử dụng URL query-string cho việc filter chứ không phải cho việc lấy một tài nguyên

  - **Good**: `/users/123`
  - **Poor**: `/api?type=user&id=23` //query-string

- Thiết kế cho người sử dụng chứ không phải thiết kế cho data của bạn(tức là người dùng muốn thì mới làm)

- Giữ cho URL ngắn và dễ đọc nhất cho client //(/users/123)

- Sử dụng số nhiều trong URL để có tính nhất quán
  - **Nên dùng**: `/customers/33245/orders/8769/lineitems/1`
  - **Không nên**: `/customer/33245/order/8769/lineitem/1`

### Sử dụng các HTTP response code để xác định trạng thái API trả về

- **200 OK**: Thành công
- **201 CREATED**: Tạo thành công (có thể từ method POST hoặc PUT)
- **204 NO CONTENT**: Thành công nhưng không có gì trả về trong body cả, thường được dùng cho DELETE hoặc PUT (thành công nhưng biết trả gì về cho người dùng)
- **400 BAD REQUEST**: Lỗi, có thể nguyên nhân từ validate lỗi, thiếu data,...
- **401 UNAUTHORIZED**: Lỗi liên quan đến thiếu hoặc sai authentication token (lỗi này là khi em đưa cho anh cái token xong anh verify token của em ra, cái anh phát hiện cái token ko phải của anh tạo cho em nghĩa là em dưa cho anh token fake là đéo đc hay bị lẫn với 403) hay token hết hạn cũng 401
- **403 FORBIDDEN**: Lỗi liên quan đến không có quyền truy cập (lỗi này là verify token của em ra là thấy ok luôn ngon luôn ,nhưng em là nhân viên quèn mà đồi truy cập vào admin, nghĩa là em chỉ đc truy cập vùng này mà đòi truy cập vùng khác là bị đấm)
- **404 NOT FOUND**: Lỗi liên quan tài nguyên không tìm thấy
- **405 METHOD NOT ALLOWED**: Lỗi liên quan method không được chấp nhận. Ví dụ API chỉ cho phép sử dụng GET, PUT, DELETE nhưng bạn dùng POST thì sẽ trả về lỗi này.
- **500 INTERNAL SERVER ERROR**: Lỗi liên quan đến việc server bị lỗi khi xử lý một tác vụ nào đó (Server không cố ý trả về lỗi này cho bạn) server chưa xử lý đc nên quăng lỗi đó cho em lun, mấy hacker vui lắm

### Sử dụng định dạng JSON hoặc XML để giao tiếp client-server

**JSON** là kiểu dữ liệu tiện dụng cho server và client giao tiếp với nhau.

Có thể xử dụng **XML** nhưng phổ biến hơn cả vẫn là **JSON**

# Authentication với JWT

- JSON Web Token (JWT) (đọc là giót), là một chuẩn mở (RFC 7519) giúp truyền tải thông tin dưới dạng JSON.

- Token là một chuỗi ký tự được tạo ra để đại diện cho một đối tượng hoặc một quyền truy cập nào đó, ví dụ như access token, refresh token, jwt...(tao cầm token của mày là tao đại diện cho mày)

- Tất cả các JWT đều là token, nhưng không phải tất cả các token đều là JWT.(vì có thể dùng thằng khác ngoài jwt để tạo token)

- Token thường được sử dụng trong các hệ thống xác thực và ủy quyền để kiểm soát quyền truy cập của người dùng đối với tài nguyên hoặc dịch vụ.(lưu id hay các thông tin liên quan đến bạn )

-1 chuỗi jwt(token) gồm 3 phần header.payload.signature

```json
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiNjQ0MTE4NDdhZmJkYjUxMmE1MmMwNTQ4IiwidHlwZSI6MCwiaWF0IjoxNjgyMDgyNTA0LCJleHAiOjE2OTA3MjI1MDR9.QjSI3gJZgDSEHz6eYkGKIQ6gYiiizg5C0NDbGbGxtWU

```

- 1 - Header(lưu thông tin mã hóa,thông tin của cái token đó): Phần này chứa thông tin về loại token (thường là "JWT") và thuật toán mã hóa được sử dụng để tạo chữ ký (ví dụ: HMAC SHA256 hoặc RSA). Header sau đó được mã hóa dưới dạng chuỗi Base64Url. (Thử decode Base64 cái chuỗi eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9 này ra thì nó sẽ có dạng '{"alg":"HS256","typ":"JWT"}')
   ![[image-24.png]]
   ![[image-25.png]]

- Payload(lưu dữ liệu, những thông tin giúp định danh đối tượng để gửi lên): Phần này chứa các thông tin mà người dùng định nghĩa. Payload cũng được mã hóa dưới dạng chuỗi Base64Url.
  ![[image-26.png]]

- Signature(server ký tên vô 1 chử ký bí mật chỉ server biết ,lưu cái chữ ký ẩn, cái này ko chiều mã hóa ngược chỉ có chiều xuôi, mã hóa lên thì bạn đưa đúng mật khẩu thì là signature còn ko thì thôi): Phần này được tạo bằng cách dùng thuật toán HMACSHA256 (cái này có thể thay đổi) với nội dung là Base64 encoded Header + Base64 encoded Payload kết hợp một "secret key" (khóa bí mật - do server giữ). Signature (Chữ ký) giúp đảm bảo tính toàn vẹn và bảo mật của thông tin trong JWT, dưới đây là công thức của signature

  ```js
  HMACSHA256(
    base64UrlEncode(header) + "." + base64UrlEncode(payload),
    secret_key
  );
  ```

- tất cả mọi người đều biết được thông tin Header và Payload của cái JWT
- chỉ có server mới biết được secret_key để tạo ra Signature. Vì vậy chỉ có server mới có thể verify được cái JWT này là do chính server tạo ra

# Access Token là gì ?(ở đây mô tả nếu chỉ có access thì như nào)

## 1. session authenication(đc ủy quyền sau mỗi 1 đợt,1 mùa)

nghĩa là mày login 1 lần là mày xài tới già nhưng ko cái session này là xài trong 1 khoản thời gian nào đó thì phải login và đăng nhập lại (1 hệ thống mà ko cài session authenication mà bị lỗi dự liệu ra ngoài là đi tù)

- mỗi một request ta gữi lên server đều có đính kèm **session id**(1 đoạn mã 1 token, cái gì cũng đc mà thường là token)
- **server** sẽ dùng **session id** để định danh xem chúng ta là ai mà lại gữi yêu cầu, từ đó quyết định có đáp ứng hay không
- **session id** được lưu trữ ở database, nên mỗi lần request là mỗi lần **server** sẽ phải vào mò vào tìm kiếm (tốn thời gian - dẫn đến quá tải)nghĩa là mỗi lần gửi 1 cái request(là đưa cái mã) anh sẽ cầm vào database để anh tìm nếu đúng là có sẽ biết em là ai và cung cấp dịch vụ cho em, mỗi lần 1 request là 1 lần vào database từ đó có base authentication

## 2. token base authentication

- với cái này ta chỉ cần dùng JWT tạo 1 token, lưu thông tin `user_id` , `role`(phân quyền admin hay nhân viên j đấy) rồi gữi cho người dùng
- server không cần lưu jwt này
- mỗi lần người dùng request thì họ sẽ gữi kèm cái jwt đó lên
- server nhận đc và xác nhận(verify) người dùng đó là ai, có quyền truy cập nội dung hay không ? ko cần vào database tìm nữa, nên giảm đc rất nhìu thời gian tìm kiếm nhờ jwt
  nờ vậy anh giới hạn access token vd cho access token 5p thì trong 5p ko cần vô database để anh tìm hết 5p cấp cho mã mới tính tiếp thì đay cũng là stateless lun vì em ko cần lưu dữ liệu lại nó càng stateless thì càng ít ảnh hưởng database của em,càng ít lưu càng đỡ truy cập nên người ta thiết kế ưu tiên stateless
- **ai đó tạo token giả để truy cập thì sao ?**(verify token đếu phải signature của tao thì là giả)

  - khi server tạo ra jwt cho người dùng, ta đã kèm theo `secret_key`, nên nếu họ gữi lại jwt cho mình, mà k phải `secret_key` của mình thì mình biết đó là 1 jwt giả
  - nhưng nếu server bị tấn công thì điều này hoàn toàn có thể xảy ra

- ưu điểm: nhanh, tiết kiệm lưu trữ `session id`, thay vì kiểm tra tính chính xác của `session id` thì nó chỉ verify(xem thử jwt có phải của server không)

- **vậy jwt trên giúp ta xác thực xem người dùng có được quyền truy cập tài nguyên hay không** nên nó được gọi là **access token** -**access token** thì chuỗi định dạng nào cũng đc, nhưng phổ biến là JWT
  cấu trúc data trong access token sẽ theo [chuẩn này!](https://datatracker.ietf.org/doc/html/rfc9068) Tuy nhiên bạn có thể thay đổi theo ý

- ví dụ ta có đoạn access token sau

  ```json
  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiNjQ0MTE4NDdhZmJkYjUxMmE1MmMwNTQ4IiwiaWF0IjoxNjgyMDgyNTA0LCJleHAiOjE2OTA3MjI1MDR9.tWlX7E7NPNftg37fXrdsXvkgEWB_8zaHIQmryAXzElY
  ```

  thì payload của nó nếu ta giải ra sẽ chỉ là

  ```json
  {
    "user_id": "64411847afbdb512a52c0548", //mã định danh ,lâu lâu có thêm cái role
    "iat": 1682082504, //thời gian bắt đầu hiệu lực
    "exp": 1690722504 //thời gian kết thúc hiệu lực
  }
  ```

  2 cái token có thể giống nhau hoàn toàn ko có nếu giông nhau tất cả mọi thứ thì giống nhau hoành toàn nhưng ko thể vì khi tạo 2 token ra thì có sự chênh lệch về số giây nên bị mã hóa khác chỉ có chủ động đổi cho giống

## flow xác thực người dùng của access token(flow dành cho ko gì khác ngoài access token)

- `tập thể piedTeam cảm ơn anh Thanh Được vì đóng góp model này`
  ![[image-27.png]]

## vấn đề của Access token

- access token không lưu ở server, mà để cho client lưu (gọi là stateless), vì vậy ta sẽ không thu hồi lại đc accesstoken dẫn đến k ép người dùng đăng xuất khỏi hệ thống được (nếu thằng hacker lấy đc token của mình, client báo bị mất acc kệ mmm luôn chứ làm gì đc,access token nào bị mất có lưu đâu mà biết,vậy làm cho access token thời gian ngắn lại thì người ăn cắp ko xài đc quá lâu và em hết hiệu lực em cũng phải đăng nhập lại)
- nếu client bị hack chiến dụng máy, thì hacker sẽ lấy đc accesstoken và gữi request lên cho server xử lý, dù mình biết client đó đã bị hack nhưng không làm gì được  
  (ngày xưa có cái đòn khi mà bị mất acc, thì người bị mất sẽ báo cáo là mình bị mất acc, thì mình truy ngược ra mình tìm có ai gửi cái access token có thông tin của thằng bị mất acc ko từ đó mình biết token của thằng hacker là token nào, xong họ lưu vào blacklist, nên mỗi lần ai gửi blacklist xem phải nó ko để khỏi cung cấp dịch vụ, thì cái này dính vào vấn đề gọi là statefull)
  - nếu mà mình tạo 1 collection lưu blackList AccessToken để nếu thấy accessToken bị hack gữi lên ta sẽ chặn k làm request thì cũng oke, nhưng như vậy thì phải vào collection để kiểm tra, gọi chung là (statefull- vi phạm quy tắc của REST)
- giải pháp: thiết lập thời gian hiệu lực của accessToken ngắn lại (ví dụ như 5p), để hacker có ít thời gian sử dụng quyền truy cập hơn
  - nhưng nếu làm vậy thì người dùng sẽ phải 5p login 1 lần =]]
  - giải pháp hiệu quả nhất là dùng **reset token**(refesh token)

# Reset Token(refesh token)

- **Reset Token** là 1 token khác, nhưng được tạo cùng lúc với access token
- **Reset Token** có hiệu lực lâu hơn access token (1 tuần, 1 tháng, 1 năm)

## flow của reset token

![[image-28.png]]

## vấn đề lý thuyết và thực hành

- **tại sao refresh lúc này là stateful, chẳng phải như vậy là vi phạm rest hay sao ?**
  - lý thuyết và thực hành khác nhau, ta không nên cứng ngắt theo lý thuyết
  - nếu access token là stateful thì mỗi request là mỗi lần ta truy cập vào database để kiểm tra
  - nhưng nếu ta có refresh token là stateful và access token là stateless thì
    mỗi 5 phút ta mới vào database kiểm tra 1 lần
  - nếu ta chủ động lưu trữ refresh token thì ta cũng sẽ chủ động xóa đc refresh token (trong trường hợp refresh token bị lộ) (tước quyền truy cập sau khi hết hạn access token, ta cũng có thể ép người dùng logout ngay lập tức bằng websoket io)
  - và ta luôn hiểu rằng , tốc độ là kết quả của đánh đổi bảo mật

# các câu hỏi về token thường gặp của người mới học authentication

- `Tại sao lại tạo một refresh token mới khi chúng ta thực hiện refresh token?`
  (tại vì nếu như mỗi lần em yêu cầu refesh token anh đưa em access mới và refesh cũ thì khi em bị mất acc thằng hacker sẽ lấy đc refesh token cũ của em mà em cũng xài refesh token cũ của em thì 2 thằng cùng xài chung 1 acc mà em ko bít, nó cứ âm thầm xài thoi)

  - nếu refresh token bị lộ, hacker có thể sử dụng nó để lấy access token mới
  - refresh token có thời gian tồn tại rất lâu, nhưng cứ sau vài phút khi access token hết hạn và thực hiện refresh token thì mình lại tạo một refresh token mới và xóa refresh token cũ
  - Refresh Token mới vẫn giữ nguyên ngày giờ hết hạn của Refresh Token cũ cũ hết hạn vào 5/10/2023 thì cái mới cũng hết hạn vào 5/10/2023.
    Cái này gọi là **refresh token rotation**.

- `Làm thế nào để revoke (thu hồi) một access token?`
  - access token chúng ta thiết kế nó là stateless, nên không có cách nào revoke ngay lập tức phải thông qua thông qua websocket và revoke refresh token
  - cách 2 là lưu access token vào trong database, khi muốn revoke thì xóa nó trong database là được, nhưng điều này sẽ làm access token không còn stateless nữa.
- `Có khi nào có 2 JWT trùng nhau hay không?`
  - Nếu payload và secret key giống nhau thì 2 JWT sẽ giống nhau.
  - trong payload JWT sẽ có trường iat (issued at) là thời gian tạo ra JWT (đây là trường mặc định, trừ khi bạn disable nó). Và trường iat nó được tính bằng giây.
  - 2 JWT trong cùng 1 giây thì lúc thì trường iat của 2 JWT này sẽ giống nhau, cộng với việc payload các bạn truyền vào giống nhau nữa thì sẽ cho ra 2 JWT giống nhau
- `Ở client thì nên lưu access token và refresh token ở đâu?`
  - trình duyệt thì các bạn lưu ở cookie hay local storage đều được, mỗi cái đều có ưu nhược điểm riêng. Nhưng cookie sẽ có phần chiếm ưu thế hơn "1 tí xíu" về độ bảo mật.
  - nếu là mobile app thì các bạn lưu ở bộ nhớ của thiết bị.
- `Gửi access token lên server như thế nào?`
  có 2 trường hợp
  - Lưu cookie: Nó sẽ tự động gửi mỗi khi request đến server, không cần quan tâm nó.
  - Lưu local storage: Các bạn thêm vào header với key là `Authorization` và giá trị là `Bearer <access_token>`.
    (frontend sẽ gửi access token thông qua header và header có thuộc tính Authorization và trong thuộc tính đó có cấu trúc Bear kèm mã khi mà cấu hình fetch nó sẽ cấu hình thuộc tính Authorization = với Bear kèm mã để nó gửi khi mà server nhận đc server phải xử lý để lấy đc access_token)
- `Tại sao phải thêm Bearer vào trước access token?`

  - phụ thuộc vào cách server backend họ code như thế nào.
  - Để mà code api authentication chuẩn, thì server nên yêu cầu client phải thêm Bearer vào trước access token. Mục đích để nhắc rằng đây là `xác thực là "Bearer Authentication" `(xác thực dựa trên token).
  - **Bearer Authentication được đặt tên dựa trên từ "bearer" có nghĩa là "người mang" - tức là bất kỳ ai có token này sẽ được coi là người có quyền truy cập vào tài nguyên được yêu cầu. Điều này khác với các phương pháp xác thực khác như "Basic Authentication" (xác thực cơ bản) hay "Digest Authentication" (xác thực băm), cần sử dụng thông tin đăng nhập người dùng.**
  - Khi sử dụng Bearer Authentication, tiêu đề Authorization trong yêu cầu HTTP sẽ trông như sau:

    ```json
    Authorization: Bearer your_access_token
    ```

- `Khi tôi logout, tôi chỉ cần xóa access token và refresh token ở bộ nhớ của client là được chứ?`
  khi mà em đăng xuất thì máy tính tự xóa access và refesh là đúng ròi nhưng server cũng phải xóa refesh token .Server mà ko xóa refesh bị rò rỉ mà em đăng xuất mẹ ròi thì thằng hacker sẽ cầm nó xài nó xài đc bạn em gia đình em, nên khi đăng xuất thì em phải tự xóa cho người ta luôn đọa đức nghề nghiệp
  - sẽ không tốt cho hệ thống về mặt bảo mật. Vì refresh token vẫn còn tồn tại ở database, nếu hacker có thể lấy được refresh token của bạn thì họ vẫn có thể lấy được access token mới.
- `OAuth 2.0, vậy nó là gì?`
  - OAuth 2.0 là một giao thức xác thực và ủy quyền (dành cho các ứng dụng với nhau tức là ứng dụng A nó những user của nó xong ứng dụng B nói là tôi mún xài những user của anh để xài chức năng bên tôi thì A sẽ ủy quyền cho B để B xài những account bên ứng dụng A ) tiêu chuẩn dành cho ứng dụng web, di động và máy tính để bàn.
  - Nó cho phép ứng dụng của bên thứ ba (còn gọi là ứng dụng khách) truy cập dữ liệu và tài nguyên của người dùng từ một dịch vụ nhà cung cấp (như Google, Facebook, Twitter, ...) mà không cần biết thông tin đăng nhập của người dùng.
  - nó chỉ là một giao thức thôi, ứng dụng là làm mấy chức năng như đăng nhập bằng google, facebook trên chính website.

