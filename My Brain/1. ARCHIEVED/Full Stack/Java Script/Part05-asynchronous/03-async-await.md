---
date: 2025-04-22T15:31:00
---
Related : [[JavaScript]]
Tag: #js #async #await
___

## 📘 JavaScript: Xử lý Bất Đồng Bộ với Async/Await

---

### 🧠 Kiến thức cơ bản

#### 🕰️ Quá khứ:
- Trước đây, JavaScript xử lý bất đồng bộ bằng **callback** → dễ gây **callback hell**.

#### ✅ Sau này:
- **ES6**: Giới thiệu `Promise` để xử lý bất đồng bộ.
- **ES7**: Giới thiệu `async/await`, giúp viết mã bất đồng bộ như đồng bộ, dễ đọc và dễ bảo trì hơn. dùng để kết hợp promise, <mark style="background: #BBFABBA6;">nó là cách viết tắt của 1 function return 1 promise</mark>

> [!note] tip nè cu
> js thì đừng hiểu mọi thứ theo kiểu code từ trên xuống dưới,
nó là ngôn ngữ kịch bản nghĩa là người dùng bấm cái j
thì tao sẽ chạy khối chức năng đó

#### 📌 Ghi nhớ:
- `async` luôn trả về một **Promise**.
- `await` chỉ được dùng bên trong **hàm async**.

---

### 🧪 So sánh Promise và Async

```js
// Dùng Promise
function handle() {
  return Promise.resolve("ahihi");
}

// Dùng Async (cách viết gọn hơn)
async function handle() {
  return "ahihi"; // tương đương return Promise.resolve("ahihi")
}
```

---

### 🧾 Ví dụ: Mô phỏng lấy dữ liệu từ database

#### Giả lập lấy `profile` và `article`

```js
let getProfile = () =>
  new Promise((resolve) => {
    setTimeout(() => {
      resolve({ name: "Điệp đẹp trai", age: 22 });
    }, 3000);
  });

let getArticle = () =>
  new Promise((resolve) => {
    setTimeout(() => {
      resolve(["Hoàng Tử Bé", "Nhà Giả Kim", "Mèo dạy hải âu bay"]);
    }, 2000);
  });
```

#### ⏱️ Cách 1: Lấy tuần tự (mất 5s)

```js
let getData = async () => {
  let profile = await getProfile(); // 3s  //trả về resolve ròi đưa profile lưu lại
  let article = await getArticle(); // 2s //await chỉ nằm trong hàm async
  console.log(profile, article);    // Tổng 5s
};
```

#### ⚡ Cách 2: Lấy song song với `Promise.all` (mất 3s)

```js
let getData = async () => {
  let [profile, article] = await Promise.all([getProfile(), getArticle()]); //Promise.all: lời hứa chạy  2 thằng cùng lúc
  console.log(profile, article); // Tổng 3s
};
```

> 💡 Dùng `Promise.all` khi các tác vụ không phụ thuộc nhau.

---

### ❌ Xử lý lỗi

#### Ví dụ: Hàm trả về lỗi

```js
let getStudents = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("Lỗi kinh hoàng");
    }, 3000);
  });
```

#### 💬 Cách 1: Dùng `.then()` và `.catch()`

```js
getStudents()
  .then((value) => {
    console.log("Danh sách sv là " + value);
  })
  .catch((error) => {
    console.log("server bị lỗi là " + error);
  });
```

#### 🛡️ Cách 2: Dùng `try...catch` với `async/await`

```js
let handle3 = async () => {
  try {
    let students = await getStudents();
    console.log(students);
  } catch (error) {
    console.log(error);
  }
};
```

#### ⚡ Cách 3: Dùng IIFE (Immediately Invoked Function Expression)

```js
(async () => {
  try {
    let students = await getStudents();
    console.log(students);
  } catch (error) {
    console.log(error);
  }
})();
```

---

### ⚠️ Lưu ý quan trọng

> ❗ **Không dùng async/await trực tiếp với các toán tử đồng bộ như `+=`, `-=`, `*=`, `/=`.**

Ví dụ:

```js
let x = 0;

let handle4 = async () => {
  x += 1;
  console.log(x);
  return 5;
};

let handle5 = async () => {
  let tmp = await handle4(); // tránh dùng x += await handle4(); đừng múa nha cu
  x += tmp;
  console.log(x);
};

handle5();
```
