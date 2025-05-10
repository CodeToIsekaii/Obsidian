---
date: 2025-05-02T11:57:00
---
Related : [[NodeJS]]
Tag: #nodejs #server
___
# tạo `ch02-serverNodejs`

# tạo server đầu tiên với nodejs

tạo folder **01-demoServer**

```bash
npm init --khởi tạo package.json
```

- mỗi dự án sẽ có setup riêng về file `main`, ta có thể tìm hiểu ở trong `package.json`

- file `package.json` có thuộc tính `main` đang là `index.js`

```json
  "main": "index.js",
```

nên mình có thể tạo `index.js` và code trong đó, ở đây mình sẽ tạo 1 file `main.js`

```bash
touch main.js
```

- mình sẽ dùng file `main.js` này làm `server` cho cho mình

  - trong file `main.js`

    ```js
    //http là 1 module có sẵn dùng để tạo server bằng nodejs (chứa những hàm giúp tạo server)(kỹ thuật cổ xưa)
    //http là module core của expressjs sau này(cũng dùng để tạo server)
    const http = require("http");

    console.log(http);
    ```

  - chạy file xem thử

    ```bash
    node main.js
    ```

- bây giờ, trong file `main.js` ta sẽ dùng http để tạo server

  ```js
  const http = require("http");
  //và server của mình nó phải mở trên 1 băng thông nào đó,1 máy tính có rất nhiều băng thông gọi là cổng mạng và mình phải mở trên 1 cái PORT(viết hoa ko ai chỉnh hết á)

  //Để giải thích đơn giản thì Port mạng là cổng vào máy tính, tất cả các hoạt động như gửi và nhận gói dữ liệu trên máy tính đều đi qua Port
  //Mỗi port mạng được định danh bởi một số nguyên từ 0 đến 65535. Trong đó:
  //Các port từ 0 đến 1023 được xem như là các port cố định, được sử dụng cho các dịch vụ mạng chuẩn như HTTP (port 80), FTP (port 21), Telnet (port 23), và SSH (port 22).
  //Các port từ 1024 đến 49151 là các port được sử dụng cho các ứng dụng và dịch vụ tùy chỉnh.
  //Các port từ 49152 đến 65535 được sử dụng cho các kết nối tạm thời hoặc các kết nối ngẫu nhiên giữa các thiết bị.

  //server hay để PORT 4000, client(giao diện) 3000 thói quen thoi
  const PORT = 4000;

  //bây giờ tạo 1 cái route mặc định

  //Trong mạng, đường đi (route) đề cập đến đường dẫn mà một gói tin di chuyển trên một mạng. Đường đi (route) bao gồm mọi thiết bị xử lý gói tin từ nguồn đến đích của nó, bao gồm router, switch và tường lửa (firewall).
  //Route là một thành phần cực kỳ quan trọng của một website, nó giúp website biết được người dùng truy cập đến nơi nào của trang web, từ đó phản hồi lại một cách thích hợp.

  //route này là localhost:4000/ và đây là api của mình
  //này giúp mình tạo server trước
  const server = http.createServer((req, res) => {
    //phương thức createServer của module http,phương thức này tạo một máy chủ HTTP mới.
    res.end("hello"); //Hàm end được sử dụng để kết thúc phản hồi và gửi nội dung đến client.
  }); //đến đây server chạy ròi

  //server sẽ chạy trên port 4000
  //viết thêm 1 cái nữa kt server chạy chưa
  server.listen(PORT, () => {
    console.log("Server đang chạy trên port" + PORT);
  });

//viết theo es module
import http from "http"; // dùng import thay vì require

const PORT = 4000;

// Tạo server và định nghĩa route mặc định
const server = http.createServer((req, res) => {
  res.end("hello");
});

// Lắng nghe server trên port 4000
server.listen(PORT, () => {
  console.log("Server đang chạy trên port " + PORT);
});

  ```

  //khi mà chạy server sẽ tạo ra đường dẫn này localhost:4000/ nếu truy cập vào đường dẫn này server sẽ trả 1 cái respone 4000

- chạy `node main.js` và truy cập `localhost:4000` ta sẽ nhận được 'hello'
  muốn ngừng server lại thì crtl+C
  nếu muốn trả về json thì ta thay chỗ `res.end` thành

```js
const server = http.createServer((req, res) => {
  res.setHeader("Content-type", "application/json"); //khi mà truyền dữ liệu mà json thì phải set header
  res.end(`{"msg": "ahihi json nè"}`); //mình gửi request lên thì phải set header thì server gửi
  // response xuống cũng phải set header
});

//es module
const server = http.createServer((req, res) => {
  // Set header để báo cho client biết dữ liệu trả về là JSON
  res.setHeader("Content-Type", "application/json");

  // Gửi response dạng JSON
  res.end(`{ msg: "ahihi json nè" }`);
});

```

xong chạy lại `server` thì ta sẽ dùng postman test thử và thấy nó đã thành json rồi

# fix xung đột port

tại sao lại có xung đột
nhiều khi `mysql`, `firewall` nó đã dùng `port` đó rồi, nhiều khi bạn đang xài server của mình mà tắt k đc, cuối cùng là nó bị xung đột
có nhiều cách

1. reset lại máy
2. lên google gõ "kill port window" và làm theo các tiền bối trên mạng

# tạo server bằng expressjs

## expressjs là frw tạo từ nodejs

1. `expressjs` chỉ là những hàm có sẵn thay vì phải code rất nhiều trên `nodejs`
2. `expressjs` quy chuẩn cách tạo server cho tất cả mọi người thống nhất
   ngoài `expressjs` còn nestjs| fastify
   [so sánh lượng tải của 3 thằng](https://npmtrends.com/@nestjs/core-vs-express-vs-fastify)

## cài đặt express

- cài đặt `express` trước
  ```bash
  npm i express
  ```

- tạo file `index.js` để demo `server` do `express` tạo ra
  ```bash
  touch index.js
  ```

- file `index.js` nội dung này lấy từ trang chủ [link](https://expressjs.com/en/starter/hello-world.html)
  ```js
  const express = require("express");
  const app = express(); //câu lệnh này  giống http.createServer ở trên , chỉ cần gọi express() là nó tự động tạo server và thằng này giúp quản lý code trực quan
  //app(đặt tên j chả đc) được sử dụng để định nghĩa các route handlers, xử lý yêu cầu HTTP, và cấu hình ứng dụng web của bạn. Điều này bao gồm việc xác định các tuyến đường (routes), xử lý yêu cầu từ client và trả lại các phản hồi.
  const PORT = 4000; //tạo biến port

  //tạo route mặc định
  //localhost:4000/
  app.get("/", (req, res) => {
    res.send("Hello World!"); //send được sử dụng để gửi nội dung này đến trình duyệt hoặc ứng dụng client.
  });

  app.get("/hi", (req, res) => {
    res.send("hi World!");
  });

  app.listen(PORT, () => {
    //Đây là một dòng mã để bắt đầu máy chủ Express.js và lắng nghe các yêu cầu trên một cổng (port) cụ thể. Đối số port cho biết cổng mà máy chủ sẽ lắng nghe.
    console.log(`server express đang mở trên port ${PORT}`);
  });

//es module
import express from "express";

const app = express(); // Tạo ứng dụng Express
const PORT = 4000;

// Route mặc định
app.get("/", (req, res) => {
  res.send("Hello World!");
});
//localhost:4000/hi/bao => hi bao
app.get("/hi/:name", (req, res) => {
  res.send("hi"+ req.params.name);
});

// Lắng nghe server trên PORT
app.listen(PORT, () => {
  console.log(`server express đang mở trên port ${PORT}`);
});

  ```

  -route handler trong Express.js [https://expressjs.com/en/guide/routing.html](https://expressjs.com/en/guide/routing.html)

- chạy thử `node index.js`
![[hack.png]]
  <script></script>:này để viết js
  vì expressjs xịn nên bị mã hóa tầm này mà code = http là lụm liền
  sercer đang chặn em khi thấy dấu này <> thì đổi thành %3C nhung mode lại đc,<>:thay cặp dấu này = html special character , tìm hiểu tiếp đi
### 🧠 Giải thích:

#### 1. **URL encoding (mã hóa URL)**:

* Khi bạn gửi dữ liệu qua URL (ví dụ như query string), các ký tự đặc biệt như `<`, `>`, `&`, `"`... **phải được mã hóa** để tránh bị hiểu sai là cú pháp HTML hay script.
* Ví dụ:

  * `<` → `%3C`
  * `>` → `%3E`
  * `&` → `%26`

→ Đây là lý do vì sao khi gửi URL có dấu `<`, trình duyệt hoặc Express sẽ tự mã hóa nó thành `%3C`.

#### 2. **HTML Special Characters (HTML Entities)**:

* Khi hiển thị dữ liệu trong HTML, bạn **cũng cần mã hóa các ký tự đặc biệt** để tránh bị **XSS (Cross-Site Scripting)**.
* Ví dụ:

  * `<` → `&lt;`
  * `>` → `&gt;`
  * `"` → `&quot;`
  * `'` → `&#39;`

→ Nếu không mã hóa, hacker có thể chèn script độc hại như:

```html
<script>alert("XSS")</script>
```

---
#### 🔐 Vì sao Express "xịn" hơn `http` thuần?

* Express tự động xử lý nhiều thứ giúp bạn:

  * Mã hóa/giải mã URL đúng chuẩn.
  * Không cần viết `if/else` dài dòng cho từng route.
  * Bảo vệ tốt hơn trước những input độc hại nhờ các middleware.
---
#### ✅ Tổng kết:

* `http` module là nền tảng thấp, bạn phải tự xử lý thủ công gần như mọi thứ.
* `express` giúp bạn làm đúng và an toàn hơn theo mặc định.


### ⚔️ Các kiểu tấn công web phổ biến (và ví dụ minh họa)

---

#### 1. **XSS (Cross-Site Scripting)** – bạn đã thấy ở trên

**Mục tiêu:** chèn mã JavaScript vào trang để đánh cắp cookie, token, redirect...

🔹 **Ví dụ** (dễ dính khi hiển thị input người dùng mà không mã hóa):

```url
http://localhost:4000/?msg=<script>alert('xss')</script>
```

**Cách phòng tránh:**

* Escape HTML output (như dùng `&lt;`, `&gt;`).
* Dùng template engine an toàn (Pug, EJS, Handlebars... auto escape).
* CSP headers (Content Security Policy).

---

#### 2. **SQL Injection** (nếu dùng DB như MySQL, PostgreSQL)

**Mục tiêu:** tiêm câu lệnh SQL độc vào truy vấn DB → lấy hết data, xóa bảng, đăng nhập bypass...

🔹 **Ví dụ query sai cách:**

```js
const result = db.query(`SELECT * FROM users WHERE username = '${input}'`);
```

Nếu `input = ' OR 1=1 --`, câu SQL thành:

```sql
SELECT * FROM users WHERE username = '' OR 1=1 --'
```

→ Lấy toàn bộ user!

**Cách phòng tránh:**

* Dùng **prepared statement** hoặc ORM như Prisma, Sequelize, TypeORM.
* Không bao giờ nối chuỗi trực tiếp với input người dùng.

---

#### 3. **Command Injection**

**Mục tiêu:** chèn lệnh hệ điều hành vào nơi bạn gọi shell command.

🔹 **Ví dụ sai cách:**

```js
import { exec } from "child_process";

app.get("/ping", (req, res) => {
  exec(`ping ${req.query.host}`, (err, stdout) => {
    res.send(stdout);
  });
});
```

Nguy hiểm nếu người dùng nhập:

```plaintext
?host=google.com; rm -rf /
```

**Cách phòng tránh:**

* Không bao giờ truyền input người dùng vào shell command.
* Dùng thư viện an toàn (như `ping` module chứ không `exec`).

---

#### 4. **Path Traversal**

**Mục tiêu:** truy cập file nằm ngoài thư mục được phép.

🔹 **Ví dụ:**

```js
app.get("/file", (req, res) => {
  const file = req.query.name;
  res.sendFile(`/app/files/${file}`);
});
```

Nguy hiểm nếu người dùng nhập:

```plaintext
?name=../../../../etc/passwd
```

**Cách phòng tránh:**

* Dùng `path.resolve()` để giới hạn truy cập trong thư mục hợp lệ.
* Kiểm tra kỹ tên file, không cho nhập dấu `..`.

---

#### 5. **CSRF (Cross-Site Request Forgery)**

**Mục tiêu:** dụ trình duyệt gửi request nguy hiểm đến server khi người dùng đang đăng nhập.

🔹 Ví dụ: khi user đang login, hacker gắn `<img src="http://site.com/delete-account">` vào một trang khác → account bị xóa nếu không có token bảo vệ.

**Cách phòng tránh:**

* Dùng CSRF Token.
* Dùng SameSite Cookie.
* Kiểm tra `Referer`, `Origin`.

---

#### 6. **DoS/DDoS (Tấn công từ chối dịch vụ)**

**Mục tiêu:** gửi quá nhiều request làm server treo.

🔹 Ví dụ: spam request vào `/api/search?q=...` hoặc gửi body JSON siêu to.

**Cách phòng tránh:**

* Dùng rate limiting (`express-rate-limit`).
* Dùng firewall/nginx để lọc trước.
* Dùng `body-parser` có limit, ví dụ:

  ```js
  app.use(express.json({ limit: "100kb" }));
  ```

---

#### 7. **Insecure Deserialization (nếu bạn nhận JSON tùy ý từ client)**

🔹 Dạng nhẹ: người dùng gửi lên object chứa field bất ngờ → có thể làm lỗi chương trình hoặc chiếm quyền logic.

**Cách phòng tránh:**

* Luôn kiểm tra, validate dữ liệu đầu vào (dùng thư viện như `zod`, `joi`).
* Đừng tin input client.

---

#### 🧰 Một số gợi ý bảo vệ server:

| Biện pháp       | Dùng gì                                 |
| --------------- | --------------------------------------- |
| Escape HTML     | `escape-html`, template engine có sẵn   |
| Sanitize input  | `express-validator`, `joi`, `zod`       |
| Rate limit      | `express-rate-limit`                    |
| Helmet          | `helmet` để set các HTTP header bảo mật |
| CSRF protection | `csurf`                                 |
| Logging         | `morgan`, `winston`                     |
| Body size limit | `express.json({ limit: "100kb" })`      |



### 1. **XSS (Cross-Site Scripting)**

#### 🐞 Ví dụ bị tấn công XSS:

Trong ví dụ này, nếu người dùng nhập một chuỗi có chứa thẻ `<script>`, server sẽ không xử lý đúng và trả về cho client. Điều này cho phép kẻ tấn công chèn mã JavaScript để thực thi trong trình duyệt của người dùng.

```js
app.get("/xss", (req, res) => {
  const name = req.query.name || "khách";
  res.send(`<h1>Xin chào, ${name}</h1>`);
});
```

**Cách khai thác:**

* Nếu truy cập `http://localhost:4000/xss?name=<script>alert('XSS')</script>`, script sẽ được thực thi và một popup xuất hiện.

#### 🛡️ Phòng chống XSS (escape HTML):

Để phòng tránh XSS, bạn cần **escape** các ký tự đặc biệt trong HTML như `<`, `>`, `&`, v.v.

```js
function escapeHtml(str = "") {
  return str.replace(/&/g, "&amp;")
            .replace(/</g, "&lt;")
            .replace(/>/g, "&gt;")
            .replace(/"/g, "&quot;")
            .replace(/'/g, "&#39;");
}

app.get("/xss/safe", (req, res) => {
  const name = escapeHtml(req.query.name || "khách");
  res.send(`<h1>Xin chào, ${name}</h1>`);
});
```

Khi bạn nhập một chuỗi có `<script>`, nó sẽ không bị thực thi mà chỉ được hiển thị như một văn bản thông thường.

---

### 2. **SQL Injection**

#### 🐞 Ví dụ bị tấn công SQL Injection:

Nếu không xử lý các dữ liệu đầu vào của người dùng, một truy vấn SQL có thể bị tấn công bằng cách chèn SQL độc hại.

```js
app.get("/sql-injection", (req, res) => {
  const { username, password } = req.query;
  const query = `SELECT * FROM users WHERE username = '${username}' AND password = '${password}'`;
  db.query(query, (err, result) => {
    if (err) return res.send("Lỗi cơ sở dữ liệu");
    res.send(result);
  });
});
```

**Cách khai thác:**

* Nếu bạn truyền `username=admin'--&password=123`, SQL query sẽ thành `SELECT * FROM users WHERE username = 'admin'--' AND password = '123'`. Điều này sẽ bypass được kiểm tra mật khẩu vì phần sau `--` sẽ bị comment lại.

#### 🛡️ Phòng chống SQL Injection:

Sử dụng **Prepared Statements** hoặc **parameterized queries** để tránh SQL injection.

```js
app.get("/sql-injection/safe", (req, res) => {
  const { username, password } = req.query;
  const query = "SELECT * FROM users WHERE username = ? AND password = ?";
  db.query(query, [username, password], (err, result) => {
    if (err) return res.send("Lỗi cơ sở dữ liệu");
    res.send(result);
  });
});
```

---

### 3. **Command Injection**

#### 🐞 Ví dụ bị tấn công Command Injection:

Khi bạn sử dụng các hàm như `exec()` mà không kiểm tra kỹ các tham số đầu vào, kẻ tấn công có thể **chạy các lệnh hệ thống** độc hại.

```js
app.get("/command-injection", (req, res) => {
  const host = req.query.host;
  exec(`ping ${host}`, (err, stdout, stderr) => {
    if (err) return res.send("Lỗi");
    res.send(stdout);
  });
});
```

**Cách khai thác:**

* Nếu người dùng truyền tham số `host=google.com; ls`, hệ thống sẽ thực thi lệnh `ping google.com` và tiếp theo là lệnh `ls`, liệt kê tất cả các tệp trong thư mục hiện tại.

#### 🛡️ Phòng chống Command Injection:

Sử dụng các API không cho phép tấn công như `dns.lookup` thay vì `exec()`.

```js
import dns from "dns";

app.get("/command-injection/safe", (req, res) => {
  const host = req.query.host;
  if (!host) return res.send("Thiếu host");
  
  dns.lookup(host, (err, address) => {
    if (err) return res.send("Không resolve được tên miền");
    res.send(`Resolved IP: ${address}`);
  });
});
```

---

### 4. **Path Traversal**

#### 🐞 Ví dụ bị tấn công Path Traversal:

Kẻ tấn công có thể truy cập các tệp không mong muốn trong hệ thống bằng cách sử dụng dấu `..` để đi lên thư mục.

```js
app.get("/path-traversal", (req, res) => {
  const fileName = req.query.file;
  fs.readFile(`/home/user/${fileName}`, (err, data) => {
    if (err) return res.send("File không tồn tại");
    res.send(data);
  });
});
```

**Cách khai thác:**

* Nếu truy cập `/path-traversal?file=../../etc/passwd`, kẻ tấn công có thể đọc các tệp quan trọng của hệ thống như `/etc/passwd`.

#### 🛡️ Phòng chống Path Traversal:

Sử dụng `path.resolve()` để đảm bảo không thoát ra ngoài thư mục an toàn.

```js
import path from "path";
import fs from "fs";

app.get("/path-traversal/safe", (req, res) => {
  const fileName = req.query.file;
  const safeDir = path.join(__dirname, "files");
  const filePath = path.resolve(safeDir, fileName);

  if (!filePath.startsWith(safeDir)) {
    return res.status(403).send("Không được phép truy cập ngoài thư mục.");
  }

  fs.readFile(filePath, (err, data) => {
    if (err) return res.send("Không thể đọc file");
    res.send(data);
  });
});
```

---

### 5. **DoS (Denial of Service)**

#### 🐞 Ví dụ bị tấn công DoS:

Kẻ tấn công có thể gửi một lượng lớn yêu cầu tới server, làm server quá tải và không thể phục vụ các yêu cầu hợp lệ.

```js
app.post("/json", express.json(), (req, res) => {
  res.send("Đã nhận JSON");
});
```

**Cách khai thác:**

* Gửi một lượng lớn payload JSON sẽ làm server gặp lỗi **413 Payload Too Large** nếu không có giới hạn kích thước.

#### 🛡️ Phòng chống DoS:

Giới hạn kích thước body của các request gửi lên server.

```js
app.post("/json/safe", express.json({ limit: "1kb" }), (req, res) => {
  res.send("Đã nhận JSON");
});
```

---

### 6. **CSRF (Cross-Site Request Forgery)**

#### 🐞 Ví dụ bị tấn công CSRF:

Kẻ tấn công có thể lừa người dùng thực hiện hành động trái phép (như chuyển tiền, thay đổi thông tin) bằng cách gửi yêu cầu đến server mà không cần xác thực.

```js
app.get("/csrf", (req, res) => {
  res.send("<form action='/update' method='POST'><input type='text' name='data'><button type='submit'>Submit</button></form>");
});

app.post("/update", (req, res) => {
  res.send("Dữ liệu đã được cập nhật!");
});
```

**Cách khai thác:**

* Tạo một form và gửi yêu cầu từ một trang khác.

#### 🛡️ Phòng chống CSRF:

Sử dụng CSRF tokens để đảm bảo yêu cầu đến từ trang web hợp lệ.

```js
import csrf from "csurf";
const csrfProtection = csrf({ cookie: true });

app.get("/csrf/safe", csrfProtection, (req, res) => {
  res.send(`<form action='/update' method='POST'>
            <input type='hidden' name='_csrf' value='${req.csrfToken()}'>
            <button type='submit'>Submit</button></form>`);
});

app.post("/update", csrfProtection, (req, res) => {
  res.send("Dữ liệu đã được cập nhật!");
});
```

---

Với các minh họa này, bạn có thể dễ dàng nhận biết cách thức tấn công và cách phòng tránh từng loại lỗ hổng. Những ví dụ này sẽ giúp bạn hiểu rõ hơn về lý do tại sao bảo mật lại quan trọng và cách thức bảo vệ ứng dụng của mình.

### cài thêm nodemon nữa để nó mỗi lần mình chỉnh server thì nó tự chạy lại

- chứ cứ phải bật tắt hoài mệt

  ```bash
  npm i nodemon -D
  ```

- cài script cho package

  ```json
  "start": "nodemon index.js"
  ```

- chạy bằng "npm run start"

# setup dự án nodejs với TS và ESLint:chuẩn hóa chung Prettier:format cho đẹp

- cấu trúc mvc(model view controller) dự án cơ bản
  📦nodejs-typescript
  ┣ 📂dist
  ┣ 📂src
  ┃ ┣ 📂constants
  ┃ ┃ ┣ 📜enum.ts
  ┃ ┃ ┣ 📜httpStatus.ts
  ┃ ┃ ┗ 📜message.ts
  ┃ ┣ 📂controllers
  ┃ ┃ ┗ 📜users.controllers.ts
  ┃ ┣ 📂middlewares
  ┃ ┃ ┣ 📜error.middlewares.ts
  ┃ ┃ ┣ 📜file.middlewares.ts
  ┃ ┃ ┣ 📜users.middlewares.ts
  ┃ ┃ ┗ 📜validation.middlewares.ts
  ┃ ┣ 📂models
  ┃ ┃ ┣ 📂database
  ┃ ┃ ┃ ┣ 📜Blacklist.ts
  ┃ ┃ ┃ ┣ 📜Bookmark.ts
  ┃ ┃ ┃ ┣ 📜Follower.ts
  ┃ ┃ ┃ ┣ 📜Hashtag.ts
  ┃ ┃ ┃ ┣ 📜Like.ts
  ┃ ┃ ┃ ┣ 📜Media.ts
  ┃ ┃ ┃ ┣ 📜Tweet.ts
  ┃ ┃ ┃ ┗ 📜User.ts
  ┃ ┃ ┣ 📜Error.ts
  ┃ ┃ ┗ 📜Success.ts
  ┃ ┣ 📂routes
  ┃ ┃ ┗ 📜users.routes.ts
  ┃ ┣ 📂services
  ┃ ┃ ┣ 📜bookmarks.services.ts
  ┃ ┃ ┣ 📜database.services.ts
  ┃ ┃ ┣ 📜followers.services.ts
  ┃ ┃ ┣ 📜hashtags.services.ts
  ┃ ┃ ┣ 📜likes.services.ts
  ┃ ┃ ┣ 📜medias.services.ts
  ┃ ┃ ┣ 📜tweets.services.ts
  ┃ ┃ ┗ 📜users.services.ts
  ┃ ┣ 📂utils
  ┃ ┃ ┣ 📜crypto.ts
  ┃ ┃ ┣ 📜email.ts
  ┃ ┃ ┣ 📜file.ts
  ┃ ┃ ┣ 📜helpers.ts
  ┃ ┃ ┗ 📜jwt.ts
  ┃ ┣ 📜index.ts
  ┃ ┗ 📜type.d.ts
  ┣ 📜.editorconfig
  ┣ 📜.env
  ┣ 📜.eslintignore
  ┣ 📜.eslintrc
  ┣ 📜.gitignore
  ┣ 📜.prettierignore
  ┣ 📜.prettierrc
  ┣ 📜nodemon.json
  ┣ 📜package.json
  ┣ 📜tsconfig.json
  ┗ 📜yarn.lock
  Giải thích các thư mục:

**dist**: Thư mục chứa các file build
**src**: Thư mục chứa mã nguồn
**src/constants**: Chứa các file chứa các hằng số
**src/middlewares**: Chứa các file chứa các hàm xử lý middleware, như validate, check token, ...
**src/controllers**: Chứa các file nhận request, gọi đến service để xử lý logic nghiệp vụ, trả về response
**src/services**: Chứa các file chứa method gọi đến database để xử lý logic nghiệp vụ
**src/models**: Chứa các file chứa các model, các cái mẫu database schema,table(vd user có prop gì,product chứa gì,)
**src/routes**: Chứa các file chứa các route, đường dẫn
**src/utils**: Chứa các file chứa các hàm tiện ích, như mã hóa, gửi email, ...
Còn lại là những file config cho project như .eslintrc, .prettierrc, ... mình sẽ giới thiệu ở bên dưới

## tiến hành cài đặt nodejs + ts + eslint + prettier

-trong `ch02-serverNodejs` tạo thư mục lưu dự án **02-demoNodejsTs**

```bash
npm init -y --tạo package.json
npm i typescript -D --vì nó chỉ dùng để làm , chứ sản phẩm vẫn là js
npm i @types/node -D --thêm kiểu ts cho thằng nodejs hiểu mình đang code dưới dạng file ts
npm install eslint prettier eslint-config-prettier eslint-plugin-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser ts-node tsc-alias tsconfig-paths rimraf nodemon -D

```

**eslint**: Linter (bộ kiểm tra lỗi) chính
**prettier**: Code formatter chính
**eslint-config-prettier**: Cấu hình ESLint để không bị xung đột với Prettier
**eslint-plugin-prettier**: Dùng thêm một số rule prettier cho eslint
**@typescript-eslint/eslint-plugin**: ESLint plugin cung cấp các rule cho Typescript
**@typescript-eslint/parser**: Parser cho phép ESLint kiểm tra lỗi Typescript
**ts-node**: Dùng để chạy TypeScript code trực tiếp mà không cần build
**tsc-alias**: Xử lý alias(đặt tên lại thì xài thằng này) khi build
**tsconfig-paths**: Khi setting alias import trong dự án dùng ts-node thì chúng ta cần dùng tsconfig-paths để nó hiểu được paths và baseUrl trong file tsconfig.json (giúp build sản phẩm ko bị sai đường dẫn)
**rimraf**: Dùng để xóa folder dist khi trước khi build  
**nodemon**: Dùng để tự động restart server khi có sự thay đổi trong code

### cấu hình ts bằng file #tsconfig.json(file này giúp mọi người quy định từ ts chuyển về js cấu hình như nào)

- tạo cùng cấp với `package.json`

```bash
touch tsconfig.json
```

thêm vào `tsconfig.json` nội dung sau

```json
{
  "compilerOptions": {
    "module": "CommonJS", // Quy định output module được sử dụng
    "moduleResolution": "node", //
    "target": "ES2020", // Target ouput cho code
    "outDir": "dist", // Đường dẫn output cho thư mục build
    "esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
    "strict": true /* Enable all strict type-checking options. */,
    "skipLibCheck": true /* Skip type checking all .d.ts files. */,
    "baseUrl": ".", // Đường dẫn base cho các import
    "paths": {
      "~/*": ["src/*"] // Đường dẫn tương đối cho các import (alias) mặc định là cái src khi truy cập đg dẫn mặc  định là src luôn đỡ phải chấm xa từ ngoài vào thì nó sẽ buil hết có trong đây
    }
  },
  "ts-node": {
    "require": ["tsconfig-paths/register"]
  },
  "files": ["src/type.d.ts"], // Các file dùng để defined global type cho dự án
  "include": ["src/**/*"] // Đường dẫn include cho các file cần build
}
```

### cấu hình eslint bằng file #eslintrc

cài `extensions eslint`

- tạo `file .eslintrc`

  ```bash
  touch .eslintrc
  ```

- nội dung

  ```js
  {
    "root": true,
    "parser": "@typescript-eslint/parser", 
    "plugins": ["@typescript-eslint", "prettier"],
    "extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended", "eslint-config-prettier", "prettier"],
    "rules": {
      "@typescript-eslint/no-explicit-any": "off",
      "@typescript-eslint/no-unused-vars": "off",
      "prettier/prettier": [
        "warn",
        {
          "arrowParens": "always",
          "semi": false,
          "trailingComma": "none",
          "tabWidth": 2,
          "endOfLine": "auto",
          "useTabs": false,
          "singleQuote": true,
          "printWidth": 120,
          "jsxSingleQuote": true
        }
      ]
    }
  }

  ```
### Giải thích
#### 🧱 1. Gốc và trình phân tích

```json
"root": true,
```

* Xác định rằng đây là **file cấu hình gốc**, không kế thừa file `.eslintrc` nào từ thư mục cha.

```json
"parser": "@typescript-eslint/parser",
```

* Dùng **parser dành cho TypeScript** do `@typescript-eslint` cung cấp, giúp ESLint hiểu cú pháp `.ts` và `.tsx`.
---
#### 🔌 2. Plugin

```json
"plugins": ["@typescript-eslint", "prettier"],
```

* Dùng 2 plugin:

  * `@typescript-eslint`: Các rule dành riêng cho TypeScript.
  * `prettier`: Tích hợp Prettier vào ESLint để cảnh báo khi định dạng chưa đúng theo Prettier.
---
#### 🧬 3. Mở rộng cấu hình từ các preset

```json
"extends": [
  "eslint:recommended",
  "plugin:@typescript-eslint/recommended",
  "eslint-config-prettier",
  "prettier"
],
```

* **"eslint\:recommended"**: Bật các rule mặc định do ESLint đề xuất cho JS.
* **"plugin:@typescript-eslint/recommended"**: Thêm các rule khuyên dùng cho TypeScript.
* **"eslint-config-prettier"** & **"prettier"**: Tắt các rule của ESLint có thể xung đột với Prettier (Prettier sẽ kiểm soát định dạng, không để ESLint can thiệp vào style như indent, tab, v.v.).
---
#### 📏 4. Các quy tắc (rules)

```json
"rules": {
```

#### a. Tắt rule cấm dùng `any`

```json
"@typescript-eslint/no-explicit-any": "off",
```

* Cho phép dùng kiểu `any` trong TypeScript mà không bị cảnh báo.

#### b. Tắt cảnh báo biến không dùng tới

```json
"@typescript-eslint/no-unused-vars": "off",
```

* Không cảnh báo khi khai báo biến nhưng không sử dụng — đôi khi hữu ích trong phát triển ban đầu.

#### c. Cấu hình Prettier thông qua ESLint

```json
"prettier/prettier": [
  "warn",
  {
    "arrowParens": "always",
    "semi": false,
    "trailingComma": "none",
    "tabWidth": 2,
    "endOfLine": "auto",
    "useTabs": false,
    "singleQuote": true,
    "printWidth": 120,
    "jsxSingleQuote": true
  }
]
```

* `"warn"`: Nếu code không đúng format, chỉ **cảnh báo** thay vì lỗi.
* `"arrowParens": "always"`: Luôn thêm dấu ngoặc đơn cho tham số arrow function, ví dụ `() => x` và `(x) => y`.
* `"semi": false`: **Không dùng dấu chấm phẩy** (`;`) ở cuối dòng.
* `"trailingComma": "none"`: Không thêm dấu phẩy cuối cùng trong mảng/object.
* `"tabWidth": 2`: Mỗi tab tương đương 2 khoảng trắng.
* `"endOfLine": "auto"`: Dùng theo hệ điều hành (Unix hoặc Windows).
* `"useTabs": false`: Dùng khoảng trắng thay vì ký tự tab.
* `"singleQuote": true`: Dùng **nháy đơn** `'` thay vì nháy đôi `"`.
* `"printWidth": 120`: Cảnh báo nếu dòng dài hơn 120 ký tự.
* `"jsxSingleQuote": true`: Dùng nháy đơn trong JSX: `<div className='test' />`.


- cài thêm #eslintignore để loại bỏ những file mà mình không muốn nó format code của mình
- tạo `file .eslintignore`

  ```bash
  touch .eslintignore
  ```

- nội dung

  ```js
  node_modules/
  dist/
  ```

nghĩa là nếu có kiểm tra và fix format code thì k đụng vào các thư mục trên

### cấu hình cho #prettier tự canh chỉnh lề cho đẹp

cài `extensions prettier`

- tạo `file .prettierrc` để cấu hình

  ```bash
  touch .prettierrc
  ```

- nội dung `.prettierrc` là

  ```js
  {
    "arrowParens": "always",
    "semi": false,
    "trailingComma": "none",
    "tabWidth": 2,
    "endOfLine": "auto",
    "useTabs": false,
    "singleQuote": true,
    "printWidth": 120,
    "jsxSingleQuote": true
  }

  ```

- cài thêm file `.prettierignore` để nó k canh lề cho những cái mình k thích

  ```bash
  touch .prettierignore
  ```

- nội dung `.prettierignore` là

  ```js
  node_modules/
  dist/
  ```

### editor(là vscode) để chuẩn hóa khi code

cài extensions EditorConfig for VS Code
tạo file .editorconfig

```bash
touch .editorconfig
```

nội dung .editorconfig

```js
indent_size = 2;
indent_style = space;
```

### thêm .gitignore

- để tránh push những thứ k cần thiết lên git
  tạo file .gitignore

  ```bash
  touch .gitignore
  ```

- mọi người vào trang này [link](https://www.toptal.com/developers/gitignore)
  tìm nodejs, và chép nội dung đó vào file

### cấu hình nodemon.json

tạo file nodemon.json

```bash
touch nodemon.json
```

nội dung

```json
{
  "watch": ["src"],
  "ext": ".ts,.js", //tracking các file có ts và js
  "ignore": [], //liệt kê file nào mà bạn k thích theo dõi vào
  "exec": "npx ts-node ./src/index.ts" //chủ động chạy file index trước ko cần gõ nodemon index.js nữa gõ, nodemon thoi
}
```

### cấu hình package.json

vào file package.json
thay script thành

```json
  "scripts": {
    "dev": "npx nodemon", //dùng để code dùng npx cho nhanh
    "build": "rimraf ./dist && tsc && tsc-alias",//code xong build ra sản phẩm
    "start": "node dist/index.js", //run code vừa build, phải build trước
    "lint": "eslint .", //kiểm tra rỗi
    "lint:fix": "eslint . --fix",//fix lỗi
    "prettier": "prettier --check .",//kiểm trá lỗi format trừ những file ignore
    "prettier:fix": "prettier --write ."//fix lỗi format
  }

```

### tạo type.d.ts

(là 1 server của ts giúp ts chạy ngầm trong máy mình)
==tạo thư mục src== và tạo file type.d.ts (nhớ là tạo file src trước nha)

```bash
touch type.d.ts
```

type.d.ts là file giúp mình định nghĩa các kiểu dữ liệu của biến trong khi code ts
ta sẽ học nó sau nay
nếu mà file tsconfig bị lỗi, có thể là do nó bị lag
ta phải vào file tsconfig ctrl + shift + p gõ **restart ts server**

### tạo file index.ts để kiểm tra các cấu hình đã làm

trong src ta thêm file index.ts
thử nội dung sau

```ts
const name: string = "Anh điệp đẹp trai";
console.log(name);
```

bạn sẽ thấy nó nói rằng "ai cũng hiểu đây là string k cần phải có keyword string này" và bạn thấy đây là eslint báo cho bạn

nên ta xóa đi

```ts
const name = "Anh điệp đẹp trai";
console.log(name);
```

vscode sẽ báo là k nên đặt tên biến là name

```ts
const fullname = "Anh điệp đẹp trai";
console.log(fullname);
```

# cài đặt xong rồi, giờ ta chạy thử dự án của mình

```bash
npm run dev
```

ta sẽ nhận được kết quả là "Anh điệp đẹp trai" vì nó sẽ chạy index.ts

ta sẽ vào index.ts code thêm tý xíu ts xem nodemon có hoạt động bình thường không, cũng như các format có chạy không ?

index.ts thêm từng dòng sau

```ts
type Handle = () => Promise<string>; //định nghĩa rằng Handle là 1 promise trả ra string,nghĩa là mình tạo ra 1 kiểu dữ liệu xong đặt vào đây
const handleF: Handle = () => Promise.resolve(fullname + " ahihi"); //handleF có kiểu dữ liệu là Handle
//xài thử thử hàm handleF
handleF().then((res) => {
  console.log(res);
});

//có thể thay khúc xài hàm bằng thế này
//handleF().then(console.log);
```

kiểm tra terminal để xem kết quả nhe

### test thử xem eslint có hoạt động không

index.ts thêm nội dung

```ts
const person: any = {};
```

any có nghĩa là bất cứ thứ gì, nhưng viết như zậy thì khó để kiểm soát, đâu biết trong person có gì đâu khuyến khích ko viết như vậy

nó k báo gì cả, ta vào .eslintrc
đổi
no-explicit-any: không cho xài any

```json
"@typescript-eslint/no-explicit-any": "off", đổi thành
"@typescript-eslint/no-explicit-any": "warn", để nó cảnh báo, hehe
```

ta k cần phải fix nó bằng tay, ta sẽ dùng cái script lint fix

```bash
npm run lint --xem lỗi
npm run lint:fix
```

nhưng mà cái này là một lỗi logic nên k fix đc, nên mình sẽ fix bằng tay
bằng cách nói rỏ là tính lưu object trông như nào

```ts
const person: { name: string; age: number } = { name: "Điệp", age: 15 };
//person là object có name string và age là number và khi chuyền giá trị vô phải chuyền như trên object đã định nghĩa
```

### test prettier

ta phải cái format mặt định là prettier trong setting của vscode
sau đó ta thử vào index.ts thêm vài dấu space thừa, sau đó save lại thì nó sẽ tự fix

nếu có nhiều file quá thì mình dùng script nha

```bash
npm run prettier --xem lỗi
npm run prettier:fix
```

**sau này mình có dự án nào cần làm nodejs ts thì mình có thể setting như này, hoặc copy để tiện đỡ phải làm lại mọi người nhe**
