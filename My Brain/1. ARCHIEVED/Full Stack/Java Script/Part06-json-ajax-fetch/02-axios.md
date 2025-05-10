---
date: 2025-04-26T00:18:00
---
Related : [[JavaScript]]
Tag: #js #axios
___


## 📁 02-axios.js – Làm việc với Axios

### 📌 Giới thiệu Axios
- **axios** là công nghệ tiếp nối của thằng fetch nhưng nó can thiệp sâu hơn về response và request
- **Axios** là một HTTP Client giúp gửi request và nhận response.
- Được dùng để tương tác với API thông qua các method như: `GET`, `POST`, `PUT`, `DELETE`.
- **So với `fetch`**:
  - `fetch` có sẵn trong trình duyệt, **Axios thì không**, cần **cài đặt thêm** (`npm install axios`).
  - Axios cung cấp nhiều tính năng nâng cao như:
    - Interceptors (chặn request/response để xử lý logic).
    - Tự động chuyển đổi JSON.
    - Tùy chỉnh headers dễ dàng.
    - Timeout, cấu hình instance, v.v.

---

## 🔗 Cấu hình cơ bản

```js
const baseURL = "https://65141bfb8e505cebc2eabb96.mockapi.io";
```

---

## 📥 GET Data từ Server (Cách dài)

```js
axios({
  method: "get",
  url: `${baseURL}/users`,
})
  .then((response) => {
    if ([200, 201].includes(response.status)) { tìm xem value có tồn tại trong mảng ko (true | false)
      // ko có prop ok chỉ có status
      return response.data;//data này là 1 prop trong object đc response
    } else {
      throw new Error(response.statusText);
    }
  })
  .then((data) => {
    console.log(data);
  })
  .catch((error) => {
    console.log(error);
  });//đỡ hơn việc khui hàng ko khác j fetch
```

> ✅ Axios không có `response.ok` như fetch — thay vào đó kiểm tra `response.status`.

---

## 📤 POST Data (Cách dài)

```js
axios({
  method: "post",
  url: `${baseURL}/users`,
  data: {
    name: "Em Điệp nguyên tử",
    yob: 2001,
  },
})
  .then((response) => {
    if ([200, 201].includes(response.status)) {
      return response.data;
    } else {
      throw new Error(response.statusText);
    }
  })
  .then((data) => {
    console.log(data);
  })
  .catch((error) => {
    console.log(error);
  });
```

---

## ✨ Viết ngắn gọn hơn với Method Aliases

```js
axios
  .post(`${baseURL}/users`, {
    name: "Em Điệp nguyên tử",
    yob: 2001,
  })

  .then((response) => {
    if ([200, 201].includes(response.status)) {
      return response.data;
    } else {
      throw new Error(response.statusText);
    }
  });
```

---

## ⚙️ Tạo Axios Instance (Cấu hình sẵn)

```js
const instance = axios.create({  //  sử dụng cấu trúc này tạo 1 thằng tự động có sẵn cấu hình trước 1 đoạn nào đó
  baseURL: baseURL,
  timeout: 10000, //sau 10s thì tự hủy request
  headers: { "Content-Type": "application/json" },
});

instance
  .post("users", {
    name: "Điệp khó tính",
    yob: 2002,
  })
  .then((response) => console.log(response));
```

---

## 🛡️ Axios với Interceptors + Class (Chặn Response trước khi trả ra)

```js
class Http {  //đặt tên j chã đc
  constructor() {
    this.instance = axios.create({
      baseURL: baseURL,
      timeout: 10000,
      headers: { "Content-Type": "application/json" },
    });

    // Interceptors để xử lý response trước khi trả về
    this.instance.interceptors.response.use(
    //đón chặn
      function (response) {
        // Thành công → chỉ lấy object có data và status
        return {
          data: response.data,
          status: response.status,
        };
      },
      function (response) {
        // Thất bại → trả về object status và statusText
        return Promise.reject({
          status: response.status,
          statusText: response.statusText,
        });
      }
    );
  }
}

const http = new Http().instance;

// Sử dụng http instance đã cấu hình sẵn
http
  .post("users", {
    name: "Hùng xe đạp",
    yob: 1987,
  })
  .then((response) => {
    console.log(response); // { data: ..., status: ... }
  });
    
//post tên tào lao và do headers
```


## ✅ Đoạn code:

```js
constructor() {
  this.instance = axios.create({
    baseURL: baseURL,
    timeout: 10000,
    headers: { "Content-Type": "application/json" },
  });
}
```

---

### 💡 Đây là phần khởi tạo **Axios Instance** bên trong **constructor** của class `Http`.

---

### 🔍 Giải thích từng dòng:

#### `constructor() { ... }`
- Đây là **hàm khởi tạo** (constructor) của class.
- Khi bạn tạo một đối tượng từ class `Http`, đoạn code bên trong `constructor` sẽ được chạy đầu tiên.

#### `this.instance = axios.create({ ... })`
- Tạo một **axios instance** và gán nó vào thuộc tính `instance` của class `Http`.
- **Axios instance** là một phiên bản riêng biệt của Axios có cấu hình sẵn, rất tiện để tái sử dụng.

---

### 🧱 Giải thích các option bên trong `axios.create()`:

```js
{
  baseURL: baseURL,
  timeout: 10000,
  headers: { "Content-Type": "application/json" },
}
```

| Thuộc tính      | Ý nghĩa                                                                 |
|----------------|-------------------------------------------------------------------------|
| `baseURL`       | Đặt sẵn URL gốc cho mọi request. VD: gọi `users` thì sẽ thành `baseURL/users`. |
| `timeout`       | Nếu request mất hơn 10 giây (10000ms) thì tự động **hủy** request.     |
| `headers`       | Cài sẵn headers mặc định. Ở đây gửi request với header `Content-Type: application/json` để báo cho server biết là đang gửi dữ liệu JSON. |

---

### ✅ Tóm lại:
Khi bạn dùng:

```js
const http = new Http().instance;
```

Thì:
- `http` chính là một **Axios instance** đã được cấu hình sẵn:
  - Có `baseURL`
  - Có `timeout`
  - Có `headers`
- Bạn không cần lặp đi lặp lại các cấu hình này ở mỗi request → code ngắn gọn hơn, dễ bảo trì hơn.

---


Ahh ok, bạn hỏi khúc `interceptors.response.use(...)` đúng không? Đây là một phần **rất hay của Axios**, mình sẽ giải thích kỹ từng dòng theo kiểu dễ hiểu như nói chuyện nha 😄
## 📌 **Mục đích của interceptors:**

> Interceptor = "kẻ chặn"  
> Nó giống như bảo vệ **đứng giữa** request hoặc response để **kiểm tra** hoặc **xử lý** trước khi dữ liệu tới tay mình.

Cụ thể trong đoạn này là `response interceptor` – tức là **khi server trả dữ liệu về**, thì **nó sẽ đi qua chỗ này trước**, bạn có thể:
- Sửa đổi dữ liệu
- Bắt lỗi theo cách riêng
- Trả về cấu trúc mình muốn

---

### 🧠 Cấu trúc cụ thể:

```js
this.instance.interceptors.response.use(
  function (response) {
    // ✅ Trường hợp response thành công
    return {
      data: response.data,
      status: response.status,
    };
  },
  function (response) {
    // ❌ Trường hợp response lỗi (thất bại)
    return Promise.reject({
      status: response.status,
      statusText: response.statusText,
    });
  }
);
```

---

### 🔍 Giải thích từng khúc:

### ✅ Thành công:
```js
function (response) {
  return {
    data: response.data,
    status: response.status,
  };
}
```

- Khi server trả về thành công (HTTP status 200, 201...),
- Axios sẽ chạy qua **hàm này**.
- Thay vì trả nguyên `response` với đủ thứ như `headers`, `config`, `request`, bạn chỉ lấy ra:
  - `data`: phần dữ liệu chính
  - `status`: mã trạng thái HTTP

👉 Tức là bạn **"tối giản hóa"** dữ liệu trả về → sau này `.then((res) => console.log(res))` là thấy rõ ràng: `{ data, status }`

---

### ❌ Thất bại:
```js
function (response) {
  return Promise.reject({
    status: response.status,
    statusText: response.statusText,
  });
}
```

- Nếu server trả về lỗi (VD: 404, 500, 401...)
- Hàm thứ hai sẽ chạy.
- Thay vì để Axios trả về một object lỗi loằng ngoằng,
- Bạn tự chọn chỉ lấy ra:
  - `status`: mã lỗi
  - `statusText`: mô tả lỗi (VD: "Not Found", "Unauthorized"...)
  
👉 Và dùng `Promise.reject(...)` để đảm bảo `.catch()` bên ngoài nhận được lỗi.

---

### 🤔 Vì sao làm như vậy?

Giống như bạn **tùy biến hệ thống trả về**, để khi dùng code bên ngoài:
```js
http.post("users", { ... })
  .then((res) => console.log(res.data))
  .catch((err) => console.log(err.statusText));
```

→ Làm code dễ đọc hơn, đẹp hơn và **đồng nhất kiểu dữ liệu** trong toàn app.

---

### ✅ Tổng kết dễ hiểu:

| Không dùng Interceptor | Dùng Interceptor |
|------------------------|------------------|
| `response.data`        | `response.data` → gọn luôn |
| `response.status`      | `response.status` → rõ ràng |
| `response.config...`   | bị giấu đi nếu không cần |
| lỗi có thể lộn xộn     | lỗi rõ ràng: `{ status, statusText }` |

Tuyệt, mình cho bạn ví dụ **có và không có interceptor**, để bạn thấy sự khác biệt rõ ràng.
## 🔍 **1. Không dùng Interceptor**

```js
axios
  .get("https://65141bfb8e505cebc2eabb96.mockapi.io/users")
  .then((response) => {
    console.log("✅ Response:", response);
    // Bạn phải tự lấy ra response.data
    console.log("📦 Data:", response.data);
  })
  .catch((error) => {
    console.log("❌ Error:", error);
    // Error object có thể rất phức tạp
    console.log("❌ Status:", error.response?.status);
    console.log("❌ Message:", error.message);
  });
```

### 🔎 Kết quả:
- `response` chứa rất nhiều thứ: `config`, `headers`, `request`, `status`, `data`, v.v.
- Khi lỗi: phải đi vào `error.response.status`, `error.message`, hoặc `error.toJSON()` → khá rối.

---

### ✅ **2. Có dùng Interceptor**

```js
// Tạo instance có interceptor
const instance = axios.create({
  baseURL: "https://65141bfb8e505cebc2eabb96.mockapi.io",
  timeout: 10000,
  headers: { "Content-Type": "application/json" },
});

instance.interceptors.response.use(
  function (response) {
    // chỉ lấy phần mình cần
    return {
      data: response.data,
      status: response.status,
    };
  },
  function (error) {
    // đơn giản hóa error
    return Promise.reject({
      status: error.response?.status,
      statusText: error.response?.statusText,
    });
  }
);

// Gọi API
instance
  .get("/users")
  .then((res) => {
    console.log("✅ Data:", res.data);
    console.log("📡 Status:", res.status);
  })
  .catch((err) => {
    console.log("❌ Error status:", err.status);
    console.log("❌ Error text:", err.statusText);
  });
```

---

### 🧠 So sánh nhanh:

|                   | ❌ Không có Interceptor                         | ✅ Có Interceptor                            |
|-------------------|--------------------------------------------------|----------------------------------------------|
| Response `.then`  | Nhận **toàn bộ response**                      | Chỉ nhận `{ data, status }`                 |
| Lấy data          | `response.data`                                 | `response.data` (đơn giản hơn)              |
| Xử lý lỗi `.catch`| `error.response.status`, `error.message`       | `error.status`, `error.statusText`          |
| Gọn và dễ dùng    | ❌ Không                                         | ✅ Rất tiện lợi                              |

---

👉 **Tóm lại**: interceptor giống như bộ lọc đầu ra, giúp bạn **"chuẩn hóa"** cách app nhận dữ liệu, dễ debug, dễ code, và gọn hơn rất nhiều.



## 📌 Đoạn code bạn hỏi:

```js
http
  .post("users", {
    name: "Hùng xe đạp",
    yob: 1987,
  })
  .then((response) => {
    console.log(response); // { data: ..., status: ... }
  });
```

---

### 🧠 Giải thích từng phần:

### `http.post(...)`
- `http` là **instance của Axios** mà bạn đã cấu hình sẵn trong class `Http`:
  ```js
  let http = new Http().instance;
  ```
- Nó đã có sẵn:
  - `baseURL: "https://65141bfb8e505cebc2eabb96.mockapi.io"`
  - `timeout: 10000`
  - `headers: { "Content-Type": "application/json" }`
  - Và đã gắn interceptor để xử lý response

Vì vậy bạn chỉ cần gọi `post("users", {...})` là nó sẽ tự hiểu thành:
```js
POST https://65141bfb8e505cebc2eabb96.mockapi.io/users
```

---

### `{ name: "Hùng xe đạp", yob: 1987 }`
- Đây là **dữ liệu (body)** bạn muốn gửi lên server – một user mới có tên và năm sinh.
- Vì bạn đã cấu hình `Content-Type: application/json`, nên Axios sẽ tự động:
  - Chuyển object thành JSON
  - Gửi kèm header phù hợp

---

### `.then((response) => { ... })`
- Sau khi gửi xong, nếu server trả kết quả về **thành công** (status 200 hoặc 201), thì:
- `response` là **object đã được xử lý bởi interceptor**, nên chỉ còn:
  ```js
  {
    data: ...,     // Dữ liệu từ server trả về (VD: user vừa tạo)
    status: ...    // Mã trạng thái HTTP
  }
  ```

👉 Vì vậy: bạn chỉ cần `console.log(response)` là thấy rõ `data` và `status`.

---

### 📊 Ví dụ kết quả:

```js
{
  data: {
    id: "123",
    name: "Hùng xe đạp",
    yob: 1987,
    createdAt: "2025-04-26T12:34:56.789Z"
  },
  status: 201
}
```

---

### ✅ Lợi ích của cách làm này:

| Điều gì?                         | Tại sao hay? |
|----------------------------------|--------------|
| Không cần viết lại `baseURL`     | Gọn hơn nhiều |
| Không cần cấu hình lại headers   | Tránh lặp lại |
| Tự động xử lý lỗi + response đẹp | Nhờ interceptor |
| Code rõ ràng, dễ bảo trì         | Chuẩn best practice |

## ✅ Tổng kết

- **Ưu điểm Axios**: dễ dùng, mạnh mẽ hơn `fetch`, cấu hình linh hoạt.
- **Nên tạo Axios Instance** nếu app có nhiều API call (đỡ phải lặp lại cấu hình).
- **Interceptors** rất hữu ích để:
  - Tự động xử lý lỗi 401, 403.
  - Hiển thị toast message khi lỗi.
  - Bọc thêm logic response.
