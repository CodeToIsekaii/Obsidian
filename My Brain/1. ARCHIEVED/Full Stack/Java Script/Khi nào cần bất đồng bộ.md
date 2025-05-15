---
date: 2025-05-15T22:28:00
---
Related : [[JavaScript]]
Tag: #áynchronous
___
Trong JavaScript, **bất đồng bộ (asynchronous)** thường được dùng để xử lý những **tác vụ mất thời gian** mà bạn không muốn **chặn (block)** luồng chính, ví dụ như đợi tải dữ liệu, chờ phản hồi từ server, hoặc xử lý tệp. Dưới đây là **các tình huống phổ biến nhất cần xử lý bất đồng bộ**, kèm **ví dụ cụ thể**:

---

## ✅ 1. **Gọi API / Fetch dữ liệu từ server**

### 🔸Khi nào cần:

* Khi bạn cần gọi đến server để lấy dữ liệu, ví dụ gọi `fetch()`, `axios.get()`, hoặc `fetchUserData()`.

### 🔹Ví dụ:

```js
async function getUser() {
  const response = await fetch('https://api.example.com/user');
  const data = await response.json();
  console.log(data);
}
```

> Nếu không dùng bất đồng bộ (`await`), chương trình sẽ không đợi dữ liệu trả về, dẫn đến lỗi hoặc dữ liệu chưa sẵn sàng.

---

## ✅ 2. **Tương tác với database (ở backend)**

### 🔸Khi nào cần:

* Khi backend (Node.js) cần đọc hoặc ghi dữ liệu vào database như MongoDB, MySQL, PostgreSQL, v.v.

### 🔹Ví dụ:

```js
app.get("/users", async (req, res) => {
  const users = await db.getAllUsers(); // giả sử db.getAllUsers trả về Promise
  res.json(users);
});
```

> Truy vấn database có thể mất vài mili giây đến vài giây, nên cần xử lý bất đồng bộ.

---

## ✅ 3. **Đọc / ghi file (trong Node.js)**

### 🔸Khi nào cần:

* Khi bạn cần đọc tệp (file) từ ổ đĩa, điều này mất thời gian và không nên block chương trình.

### 🔹Ví dụ:

```js
const fs = require("fs/promises");

async function readFile() {
  const content = await fs.readFile("data.txt", "utf-8");
  console.log(content);
}
```

---

## ✅ 4. **Xử lý timer hoặc delay (setTimeout, setInterval)**

### 🔸Khi nào cần:

* Khi bạn cần đợi một khoảng thời gian nào đó trước khi thực hiện hành động (ví dụ đợi hiệu ứng, debounce input).

### 🔹Ví dụ:

```js
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function showMessage() {
  console.log("Loading...");
  await delay(2000); // đợi 2 giây
  console.log("Done!");
}
```

---

## ✅ 5. **Xử lý nhiều tác vụ bất đồng bộ cùng lúc (Promise.all)**

### 🔸Khi nào cần:

* Khi bạn cần chạy nhiều API hoặc tác vụ cùng lúc để tăng hiệu suất.

### 🔹Ví dụ:

```js
async function loadData() {
  const [user, posts] = await Promise.all([
    fetch('/api/user').then(res => res.json()),
    fetch('/api/posts').then(res => res.json())
  ]);
  console.log(user, posts);
}
```

---

## ✅ 6. **Lắng nghe sự kiện bất đồng bộ (event listeners, socket, stream)**

### 🔸Khi nào cần:

* Khi làm việc với `socket.io`, `WebSocket`, hoặc các sự kiện như `on("data")`, `on("message")`, bạn cần xử lý dữ liệu bất đồng bộ khi nó đến.

### 🔹Ví dụ (WebSocket):

```js
socket.on("message", async (data) => {
  await saveToDatabase(data);
});
```

---

## ✅ 7. **Gọi lại (callback) khi xử lý xong việc nào đó**

### 🔸Khi nào cần:

* Trước đây (trước ES6), khi chưa có `async/await`, bạn dùng **callback** cho các hàm bất đồng bộ như `fs.readFile`, `setTimeout`, v.v.

### 🔹Ví dụ:

```js
setTimeout(() => {
  console.log("3 giây đã trôi qua!");
}, 3000);
```

---

## 🧠 Tóm tắt: Những lúc cần dùng bất đồng bộ là khi...

| Tình huống                       | Cần async không? | Ví dụ                       |
| -------------------------------- | ---------------- | --------------------------- |
| Gọi API / fetch dữ liệu          | ✅ Có             | `fetch()`                   |
| Giao tiếp database               | ✅ Có             | `await db.query()`          |
| Đọc / ghi file trong Node.js     | ✅ Có             | `fs.promises.readFile()`    |
| Đợi thời gian (delay, timeout)   | ✅ Có             | `await delay(1000)`         |
| Chạy nhiều tác vụ song song      | ✅ Có             | `Promise.all([...])`        |
| Sự kiện không đồng bộ (socket)   | ✅ Có             | `socket.on("event", async)` |
| Tính toán đơn giản, xử lý UI     | ❌ Không          | `add(1, 2)`                 |
| DOM manipulation (trừ fetch API) | ❌ Không          | `document.getElementById()` |
