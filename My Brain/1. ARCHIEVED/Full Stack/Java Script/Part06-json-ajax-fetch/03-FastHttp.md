---
date: 2025-04-26T00:36:00
---
Related : [[JavaScript]]
Tag: #js #project #fasthttp
___
[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part06-json-ajax-fetch\03-FastHttp\index.html)

# 📦 `FastHttp` class - Promise + Fetch API

```javascript
const baseURL = "https://65141bfb8e505cebc2eabb96.mockapi.io/users";

// Định nghĩa class FastHttp
class FastHttp {
  // Phương thức send dùng để gửi yêu cầu HTTP
  send(method, url, body) {
    // Phải return fetch để chuỗi promise hoạt động (dùng .then)
    return fetch(url, {
      method: method,
      headers: {
        "Content-Type": "application/json",
      },
      body: body ? JSON.stringify(body) : null,
    }).then((response) => {
      if (response.ok) {
        return response.json();
      } else {
        // response.statusTexxt => lỗi gõ sai, phải là statusText
        throw new Error(response.statusText);
      }
    });
  }

  // Gửi yêu cầu GET
  get(url) {
    return this.send("GET", url, null);
  }

  // Gửi yêu cầu POST (thêm mới dữ liệu)
  post(url, body) {
    return this.send("POST", url, body);
  }

  // Gửi yêu cầu PUT (cập nhật dữ liệu)
  put(url, body) {
    return this.send("PUT", url, body);
  }

  // Gửi yêu cầu DELETE (xóa dữ liệu)
  delete(url) {
    return this.send("DELETE", url, null);
  }
}

// Khởi tạo đối tượng http từ class FastHttp
let http = new FastHttp();

// Ví dụ sử dụng:
// 1. GET tất cả users
// http.get(baseURL)

// 2. POST - thêm user mới
// http.post(baseURL, { name: "Linh xe ôm", yob: 1968 })

// 3. DELETE user có id = 3
// http.delete(`${baseURL}/3`)

// 4. PUT - cập nhật user có id = 2
http.put(`${baseURL}/2`, { name: "Tài chó điên" })
  .then((data) => {
    console.log(data); // In kết quả ra console
  });
```

---

# 📌 Lưu ý:
- **Quan trọng**: Phải `return fetch(...)` trong hàm `send()` thì các hàm `get`, `post`, `put`, `delete` mới là Promise => mới `.then()` được.
- **Sửa lỗi**: Trong `throw new Error(response.statusTexxt)`, `statusTexxt` bị gõ sai, phải là `statusText`.
- Các phương thức GET, POST, PUT, DELETE đều dựa trên `send()`.

Ok, mình sẽ trình bày gọn đẹp phần code bạn mới gửi, rồi **so sánh** hai cách (`then/catch` vs `async/await`) cho bạn dễ note vô Obsidian nhé!

---

# 📦 `FastHttp` class - Phiên bản `async/await`

```javascript
const baseURL = "https://65141bfb8e505cebc2eabb96.mockapi.io/users";

// Định nghĩa class FastHttp dùng async/await
class FastHttp {
  async send(method, url, body) {
    const response = await fetch(url, {
      method: method,
      headers: {
        "Content-Type": "application/json", 
      },
      body: body ? JSON.stringify(body) : null,
    });

    if (response.ok) {
      return response.json();
    } else {
      throw new Error(response.statusText);
    }
  }

  get(url) {
    return this.send("GET", url, null);
  }

  post(url, body) {
    return this.send("POST", url, body);
  }

  put(url, body) {
    return this.send("PUT", url, body);
  }

  delete(url) {
    return this.send("DELETE", url, null);
  }
}

// Khởi tạo đối tượng http
let http = new FastHttp();

// Sử dụng async/await trong một hàm IIFE
(async () => {
  try {
    const data = await http.put(`${baseURL}/2`, { name: "Tài chó điên" });
    console.log(data);
  } catch (error) {
    console.log(error);
  }
})();
```
# ⚔️ So sánh **then/catch** và **async/await**:

| Tiêu chí | then/catch | async/await |
|:---------|:-----------|:------------|
| Viết code | Dùng `.then()` và `.catch()` để xử lý promise nối tiếp. | Dùng `await` chờ kết quả và `try/catch` để bắt lỗi. |
| Độ dễ đọc | Khó đọc hơn nếu nhiều `.then().then().then()` (callback hell nhẹ). | Code rõ ràng, giống code tuần tự. |
| Quản lý lỗi | `.catch(error => {...})` | `try { await... } catch(error) {}` |
| Tính chất kỹ thuật | Dùng Promise chaining. | Dùng syntactic sugar của Promise (async function luôn trả về Promise). |
| Khi nào dùng | Các flow đơn giản, ít lồng ghép. | Flow phức tạp, cần nhiều bước tuần tự hoặc try nhiều lần. |

---

# 🎯 Kết luận nhanh:
- Nếu project cần xử lý nhiều bước async liên tục 👉 **dùng `async/await` cho dễ đọc.**
- Nếu đơn giản hoặc gọn nhẹ 👉 **dùng `then/catch` cũng ok.**
