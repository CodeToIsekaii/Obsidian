---
date: 2025-04-22T15:23:00
---
Related : [[JavaScript]]
Tag: #js #asynchronous #callback
___

# 🧠 JavaScript Bất Đồng Bộ: Callback, Call Stack & Event Loop
- bản thân js là ngôn ngữ đơn luồng
- js chạy trên web, và nodejs thì đc hộ trợ đa luồng

## 1️⃣ JavaScript là ngôn ngữ đơn luồng (Single-threaded)

- JavaScript chỉ có **1 call stack** để thực thi lệnh.
- Trên **trình duyệt** hoặc **Node.js** sẽ có hệ thống hỗ trợ bất đồng bộ như: `setTimeout`, `Event`, `AJAX`, v.v.

💡 Các ngôn ngữ như PHP, Java (mặc định) là **đa luồng**, cần dùng `thread` để tạo bất đồng bộ.

---

## 2️⃣ Đồng bộ (Synchronous) vs. Bất đồng bộ (Asynchronous)

### 🔗 Synchronous
- Các tác vụ được thực hiện **tuần tự** từ trên xuống dưới.
- Nếu có 2 tác vụ `L1 (3s)` và `L2 (2s)` → tổng thời gian là `5s`.

### 🔀 Asynchronous
- Nếu `L1` và `L2` **không phụ thuộc lẫn nhau**, ta có thể cho chạy song song → `3s` là xong cả hai.
- Phù hợp với những thao tác **không chặn luồng chính** như:
  - Đọc file
  - Gọi API
  - Chờ thời gian (`setTimeout`)

---

## 📦 Call Stack: Ngăn xếp thực thi

```js
function a(x) {
  console.log(x);
}

function b(y) {
  a(y + 2);
}

function c(z) {
  b(z + 1);
}

c(5); // In ra: 8
```

🧮 **Call Stack hoạt động như sau (LIFO):**
`c(5) => b(z+1)=>z+1`
`c(5) => b(z+1)=>a(y+2)=>y+2`
`c(5) => b(z+1)=>a(y+2)=>log(x)`
- `c(5)` gọi → `b(6)` gọi → `a(8)` gọi → `console.log(8)`
- Sau đó, các hàm lần lượt được gỡ bỏ khỏi stack.

---

## 🔁 Event Loop & Callback Queue

### Mô hình tổng thể:

```
+--------------------+
|   Memory Heap      |
+--------------------+

+--------------------+
|    Call Stack      |  <----+
+--------------------+       |
                             | Event Loop
+--------------------+       |
|  Callback Queue    |  -----+
+--------------------+
```

### Web APIs (trình duyệt cung cấp):
- `setTimeout`
- `DOM events` (click, submit,…)
- `AJAX`

### Cách hoạt động:
1. Khi `setTimeout`, browser/webAPI sẽ xử lý **ngoài call stack**.
2. Sau thời gian chờ, hàm callback được đẩy vào **callback queue**.
3. **Event loop** kiểm tra call stack rỗng mới đưa callback lên stack để thực thi.
event loop: liên tục lặp đi lặp lại chờ đợi 1 sự kiện "click,load,onDone, submit,..."
      event loop                                  callback queue

---

## 🧪 Ví dụ minh hoạ: `setTimeout`

```js
function main() {
  console.log("commend1");

  setTimeout(() => {
    console.log("commend2");
  }, 3000);

  console.log("commend3");

  setTimeout(() => {
    console.log("commend4");
  }, 1000);
}

main();
```

### 🕓 Thứ tự thực thi:
```
commend1
commend3
// (sau 1s)
commend4
// (sau thêm 2s)
commend2
```

---

## 😵 Callback Hell

- Khi lồng quá nhiều hàm bất đồng bộ với `callback`, ta rất dễ bị "callback hell":
```js
login(() => {
  getUser(() => {
    getArticles(() => {
      getComments(() => {
        //...
      });
    });
  });
});
```

### ❌ Nhược điểm:
- Khó đọc, khó debug.
- Khó kiểm soát luồng.

---

## 🌈 Promise – Giải pháp cho bất đồng bộ

- Promise giúp đưa code bất đồng bộ về dạng **chuỗi xử lý tuần tự** (gần như đồng bộ).

```js
getUser()
  .then(getArticles)
  .then(getComments)
  .catch(handleError);
```

---

## 🔁 Ví dụ kinh điển `setTimeout` + `var` vs. `let`

```js
for (var i = 0; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1);
}
// Kết quả: 4 4 4 4
```

➡️ Vì `var` không có block scope, vòng lặp chạy xong `i = 4`, nên mọi callback đều dùng chung `i = 4`.

```js
for (let i = 0; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1);
}
// Kết quả: 0 1 2 3
```

➡️ Với `let`, mỗi vòng lặp có `i` riêng biệt.

---

## ✅ Kết luận

| Tính năng        | Ý nghĩa                                           |
|------------------|----------------------------------------------------|
| Call Stack       | Quản lý hàm đang gọi và thực thi theo LIFO        |
| Web APIs         | Chạy tác vụ bất đồng bộ (timeout, event, ajax)    |
| Callback Queue   | Nơi chờ callback để được đưa lên stack             |
| Event Loop       | Luôn kiểm tra call stack rỗng để đẩy callback vào |
| `setTimeout`     | Cơ chế delay nhưng không chặn luồng                |
| Promise          | Viết code async mà gọn gàng, dễ bảo trì hơn        |
