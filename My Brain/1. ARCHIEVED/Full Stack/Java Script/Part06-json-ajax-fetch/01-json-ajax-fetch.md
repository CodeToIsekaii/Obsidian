---
date: 2025-04-23T23:29:00
---
Related : [[JavaScript]]
Tag: #js #json #ajax #fetch
___

### 🟡 JSON – JavaScript Object Notation

- JSON(js object notation) là một **chuỗi dữ liệu** được viết dưới dạng JavaScript Object.
- Dùng để **lưu trữ** và **trao đổi dữ liệu** giữa client & server.
- Hỗ trợ các kiểu dữ liệu:
  - `number`, `string`, `boolean`, `array`, `object`, `null`
- JSON **không hỗ trợ**: `function`, `method`

#### 🔁 Chuyển đổi giữa JSON & JS Object

| Hành động             | Cú pháp                    | Mô tả                   |
|-----------------------|----------------------------|--------------------------|
| Chuỗi → Object        | `JSON.parse(jsonString)`   | Phân tích cú pháp JSON  |
| Object → Chuỗi        | `JSON.stringify(obj)`      | Biến object thành JSON  |

#### 📝 Ví dụ

```js
const obj1 = {
  name: "Điệp đẹp trai",
  age: 22,
  status: "Hay giận dỗi",
  sayhi() {//   ko có sayhi vì có chục obj khác nhau sẽ có chục sayhi khác nhau sẽ rất tốn dung lượng
    console.log("hello");//những thuộc tính này sẽ bỏ vào class sẽ có cái khuôn đúc ra method sayhi lun
  },
};

let myJson = JSON.stringify(obj1);
console.log(obj1);     // in ra object
console.log(myJson);   // in ra JSON string
```

➡️ Lưu ý: `sayhi()` sẽ không được stringify vì JSON không hỗ trợ hàm.

---

### 🟢 Cú pháp JSON

- **Object**: Dùng dấu `{}`, chứa các cặp `"key": value`, ngăn cách bằng dấu `,`
- **Array**: Dùng dấu `[]`
- **String** và **Key**: Bắt buộc dùng `""` (double quotes)
- json dùng dấu "" để phân biệt với dấu '' ở ngoài cùng của string
- value chỉ thuộc dạng : number, string, boolean, array, object, null
- ko lưu đc function và method
- JSON dễ đọc hơn XML nên được dùng phổ biến trong web.
```js
//đoán đáp án
let arr = ["cam", 22, "chuối", "ổi"]; //["cam",22,"chuối","ổi"]
let a = 1; // '1' biến thành màu trắng -> chuỗi
let str = "ahihi"; // '"ahihi"'
let bool = true; // 'true' màu trắng -> chuỗi
console.log(JSON.stringify(bool));
```
---

### 🟠 AJAX – Asynchronous JavaScript and XML

- AJAX **không phải là ngôn ngữ lập trình** mà là **tập hợp nhiều công nghệ**:
  - HTML (hiển thị), CSS (giao diện), JS (xử lý logic), XML: định dạng dữ liệu (khó đọc hơn),JSON: định dạng dữ liệu (dễ đọc hơn)
- AJAX giúp:
  - giúp chúng ta đọc dữ liệu từ server trả về
  - Gửi dữ liệu lên server ở **chế độ ngầm**
  - **Cập nhật nội dung** trang mà không cần reload
  - Là nền tảng cho: **React**, **Vue**, **Angular**,...

- XMLHttpRequest(XHR): object có sẵn của trình duyệt
 - dùng để gửi và nhận data của web server
 - cái tên ajax bị lầm là ứng dụng ajax sẽ sử dụng XML

---

### 🔵 Fetch API
  -  cung cấp cho mình khả năng gửi request / nhận response thông qua trình duyệt
- API hiện đại thay thế `XMLHttpRequest`
- Dựa trên cơ chế **Promise** → dễ viết code bất đồng bộ

#### 🧪 Ví dụ `GET`

```js
//ta sẽ tiến hành thao tác lấy dữ liệu về từ server về = fetch
const baseURL = "https://65141bfb8e505cebc2eabb96.mockapi.io"; // địa chỉ để gửi request lên
//tạo 1 request và gửi lên server
//server ơi hứa với tôi rằng bạn sẽ gửi dữ liệu về cho tôi
//lấy dự liệu và viết như này là gửi 1 request lên server fetch là 1 promise nên chạy ròi ->tắt live server ko bị ban
fetch(`${baseURL}/users`)   này đang mặc định là method get
  server đợi xong đưa gói hàng
  .then((response) => {
    //kiểm tra gói hàng
    if (response.ok) { ok: true thì thành công false thì thất bại hay đọc status:200 lấy thành công,
      //khui hàng                                                          201 post thành công,404 ko tìm thấy,422: audian
      return //response.json(); khui                                   statusText: "OK" báo dạng chữ viết luôn
    } else {
      throw new Error(response.statusText);
    }
  })
  .then((data) => {
    console.log(data);
  });```

#### 🧪 Ví dụ `POST`

```js
fetch(`${baseURL}/users`, {
  method: "POST",
  headers: {
    "Content-Type": "application/json", //nhắc cho hệ thống kiểu json, application là các bộ dữ liệu ở đây là json
  },
  body: JSON.stringify({ name: "Điệp đệ quy", yob: 2004 }),
}) //chuyển qua method post
  .then((response) => {
    //kiểm tra gói hàng
    if (response.ok) {
      //khui hàng
      return response.json();
    } else {
      throw new Error(response.statusText);
    }
  })
  .then((data) => console.log(data))
  .catch((error) => console.log(error));
```

---

### 🧰 Tham số `fetch`

| Thuộc tính   | Ý nghĩa |
|--------------|--------|
| `method`     | `"GET"`, `"POST"`, `"PUT"`, `"DELETE"`, `"PATCH"` |
| `headers`    | Định dạng dữ liệu (`Content-Type`), chứa thông tin nhạy cảm (token) |
| `body`       | Dữ liệu gửi lên (thường là JSON.stringify) |

> 🔐 **Token** → Dữ liệu nhạy cảm nên được truyền qua `headers`, không nên truyền trên URL

---

### 📌 POST có 2 cách truyền dữ liệu

1. **Query string parameter (KHÔNG AN TOÀN)**  
   ```js
   fetch(`${baseURL}/users/?username=abc&password=123`)
   ```
   - Dễ bị nhìn thấy → **không nên dùng để truyền dữ liệu nhạy cảm**

2. **Body JSON (AN TOÀN HƠN)**  
   ```js
   fetch(`${baseURL}/users`, {
     method: "POST",
     headers: {
       "Content-Type": "application/json",
     },
     body: JSON.stringify({ username: "abc", password: "123" }),
   })
   ```
<mark style="background: #FFF3A3A6;">nhưng chưa đủ khi lên server thì nó thắc mắc ủa m đang gửi cái gì cho tao cái kiêu này là kiểu gì tao ko biết => cần headers</mark>
