---
date: 2025-04-17T11:30:00
---
Related : [[JavaScript]]
Tag: #js #nativeprototype
___

## 🧬 Native Prototype trong JavaScript

### 🧠 Tóm tắt khái niệm quan trọng
- thuộc tính prototype của constructor function đc sử dụng rỗng rãi trong js

| Thuật ngữ            | Giải thích                                                                  |
| -------------------- | --------------------------------------------------------------------------- |
| `[[Prototype]]`      | Thuộc tính ẩn của object, đại diện cho "tiền thân" (cha) prototype thực thể |
| `__proto__`          | Là *getter/setter* để truy cập `[[Prototype]]`                              |
| `Function.prototype` | Là prototype mặc định của mọi constructor function                          |
| `Object.prototype`   | Là prototype gốc, mọi prototype đều kế thừa từ đây (trừ `null`)             |

---

### 🧱 Ví dụ: Prototype của Object cơ bản

```js
//tạo 1 đối tượng ko có thuộc tính gì cả thì vẫn có [[Prototype]] và ở trong có class Object
let obj = {}; // Hoặc: let obj = new Object();
```

Dù không có thuộc tính gì, object này vẫn có prototype:

```js
console.log(obj.toString()); // 👉 [object Object] //hạn chế dùng .toString kiểu này vì ko ra j hết ko ngon
console.log(obj.__proto__ === Object.prototype); // 👉 true //class Object (Object này là constructor, constructor có prototype mà protoypte của constructor là class Object)
console.log(Object.prototype.__proto__); // 👉 null //bị hoisting thì undefined
console.log(obj.__proto__.__proto__); // 👉 null
```

> ✔️ `obj.__proto__` chính là `Object.prototype`  
> ❌ `Object.prototype` không có prototype nữa → `null`

---

### 📚 Prototype của Array

```js
//bản chất mảng cũng là object nên sẽ có tiền thân là [[Prototype]]
let mang1 = [1, 2, 3];
```

Mảng cũng là object nên có chuỗi prototype tương tự:

```js
console.log(mang1.__proto__ === Array.prototype);              // 👉 true
console.log(Array.prototype.__proto__ === Object.prototype);   // 👉 true
console.log(mang1.__proto__.__proto__ === Object.prototype);   // 👉 true
console.log(mang1.__proto__.__proto__ == Array.prototype.__proto__); //true
console.log(mang1.__proto__.__proto__.__proto__);              // 👉 null
```

> ✔️ Mảng kế thừa từ `Array.prototype`, rồi `Object.prototype`, rồi `null`.

#### 📌 `toString()` của mảng

```js
//nếu mang1.toString() thì nó xài toString của Array hay Object (nó sẽ lấy của array lấy thằng gần nhất)
console.log(mang1.toString()); // 👉 "1,2,3"
```

> Hàm `toString()` được lấy từ `Array.prototype` vì đó là prototype gần nhất.

---

### 🔢 Prototype của kiểu dữ liệu nguyên thủy (primitive)

```js
let a = 5;
console.log(a.__proto__ === Number.prototype); // 👉 true
```

> Khi gọi `a.__proto__`, JavaScript **tạm thời bọc số 5 thành object**, nên có thể truy cập `Number.prototype`.

---

### ✅ Tổng kết

| Điều cần nhớ | Ý nghĩa |
|--------------|--------|
| `Object.prototype.__proto__ === null` | Gốc của mọi prototype chain |
| `Array`, `Number`, `String`, `Boolean` đều là constructor | Có `prototype` riêng |
| Kiểu primitive có thể truy cập prototype tạm thời | JS tự "boxing" sang object |

---

Nếu bạn muốn mình xuất file `.md` để dùng ngay trong Obsidian, hoặc thêm sơ đồ minh họa **Prototype chain**, mình có thể làm ngay nhé!
