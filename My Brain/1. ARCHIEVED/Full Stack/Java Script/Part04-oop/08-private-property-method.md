---
date: 2025-04-21T11:33:00
---
Related : [[JavaScript]]
Tag: #js #private #propertymethod
___

## 🔐 JavaScript - Private Property và Method (Access Modifier)

### 🎯 Mục tiêu:  
Hiểu cách **đóng gói (Encapsulation)** trong JavaScript thông qua các **modifier** như `public`, `private`, và các kỹ thuật như `get`, `set`.

---

## 📦 Access Modifier trong JS

### ✅ JavaScript chia thành 2 loại interface:

| Loại interface       | Truy cập được từ đâu                    |
|----------------------|-----------------------------------------|
| **Internal interface** | Chỉ bên trong class                     |
| **External interface** | Bên trong và cả bên ngoài class        |

> 🔸 Trong JS, chỉ có **`public`** và **`private`** chính thức.  
> 🔹 Các ngôn ngữ khác (Java, C#,...) còn có `protected`, nhưng JS không hỗ trợ cấp ngôn ngữ.

---

## 🔓 Public Property (Thuộc tính công khai)

```js
class User {
  name = "Lan"; // public
}

let u = new User();
console.log(u.name); // ✅ "Lan"
```

- Có thể truy cập mọi nơi (internal & external).

---

## 🔒 Private Property (Thuộc tính riêng tư)

### 📌 Cách 1: Quy ước đặt tên `_` (không được đảm bảo ngôn ngữ)

```js
class CoffeeMachine {
  constructor(power) {
    this._power = power; // convention: biến "private"
  }

  get power() {
    return this._power; // read-only
  }
}

let cfm = new CoffeeMachine(100);
cfm.power = 50;     // ❌ Không thay đổi được (vì chỉ có getter)
cfm._power = 50;    // ⚠️ Vẫn thay đổi được (do JS không cấm)
console.log(cfm.power);   // 100
console.log(cfm._power);  // 50 (đã bị thay đổi)
```

### 🔒 Read-Only với `getter`

- Nếu chỉ có `get`, không có `set` ⇒ thuộc tính chỉ đọc (readonly).

---

### 📌 Cách 2: Dùng cú pháp **`#`** (được đảm bảo ngôn ngữ)

```js
class CoffeeMachine1 {
  #power = 200;

  constructor(power) {
    this.#power = power;
  }

  #fixWaterAmount(value) {
    if (value < 0) return 0;
    if (value > this.#power) return this.#power;
  }

  get power() {
    return this.#power;
  }

  setWaterAmount(value) {
    this.#power = this.#fixWaterAmount(value);
  }
}

let cmf1 = new CoffeeMachine1(100);
cmf1.setWaterAmount(4); // ✅ dùng được setter gián tiếp
// console.log(cmf1.#power); ❌ lỗi - private
// cmf1.#fixWaterAmount(10); ❌ lỗi - private
```

> ✅ **Ưu điểm:** `#` là private thật sự (cấp độ ngôn ngữ, trình duyệt JS hiện đại hỗ trợ).

---

## 🧩 `instanceof` vs `typeof`

| Toán tử      | Mục đích                                                                                          | Kết quả                                             |
| ------------ | ------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| `instanceof` | Kiểm tra 1 object có **kế thừa từ class hoặc constructor** không dựa vào `[[Prototype]]` để check | `true/false`                                        |
| `typeof`     | Trả về kiểu dữ liệu (string)                                                                      | `"string"`, `"object"`, `"function"`, `"rabbit"`... |

### ✳️ Ví dụ:

```js
let arr = [];

console.log(arr instanceof Array); // ✅ true
console.log(typeof arr);           // 🔸 "object"
```

---

## 📍 Ghi nhớ:

- `#privateField`: dùng để tạo **biến/method private thật sự**, giới hạn truy cập bên trong class.
- Dùng `get` để tạo **readonly property**.
- Dùng `instanceof` khi cần xác định 1 object thuộc loại nào trong hệ kế thừa.
- Dùng `typeof` để kiểm tra kiểu dữ liệu cơ bản.
