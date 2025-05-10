---
date: 2025-04-11T22:20:00
---
Related : [[JavaScript]]
Tag: #js #prototypal #inheritance
___

## 📘 Prototypal Inheritance – Kế thừa nguyên mẫu trong JavaScript

### 🔍 Khái niệm

- **Prototypal Inheritance** là việc **một object có thể kế thừa thuộc tính và phương thức từ một object khác** thông qua thuộc tính ẩn `[[Prototype]]`.nghĩa là thằng con nuốt thằng cha thì nó sẽ kế thừa tất cả thuộc tính của thằng cha
- Đây là cách mà JavaScript tổ chức và chia sẻ thuộc tính giữa các object.

> ⚠️ Object không kế thừa lẫn nhau theo kiểu `extends` như class, mà **liên kết thông qua prototype chain (chuỗi nguyên mẫu)**.

---

### 🔗 `[[Prototype]]` và `__proto__`

- Mọi object trong JavaScript (trừ `null`) đều có một thuộc tính ẩn `[[Prototype]]`. nó chứa thông tin tiền thân của nó
- Truy cập vào `[[Prototype]]` có thể thực hiện thông qua:
  - `__proto__` (trình duyệt cung cấp – không chuẩn, dần bị loại bỏ)
  - `Object.getPrototypeOf(obj)`
  - `Object.setPrototypeOf(obj, proto)`

---

### 🧪 Ví dụ cơ bản: Chuỗi kế thừa nhiều cấp

```js
let longEar = {
  ear: "long",
};
// object là ko có kế thừa chỉ có class là kế thừa thông qua extend impliment
let rabbitPink = {
  jumps: true,
   //   __proto__: longEar,
};

// Thiết lập kế thừa
rabbitPink.__proto__ = longEar;
console.log(rabbitPink);
console.log(rabbitPink.ear); //long

let congido = {
  eats: true,
  walk() {
    console.log("Tui chạy bộ nè");
  },
   //   __proto__: rabbitPink,
};

// Thiết lập chuỗi kế thừa: congido → rabbitPink → longEar
congido.__proto__ = rabbitPink;
congido.__proto__.__proto__ = longEar;
console.log(congido);
```

**Minh hoạ chuỗi kế thừa:**

```
congido → rabbitPink → longEar
```

---

### 🔎 Truy cập và kế thừa thuộc tính

```js
rabbitPink.height = 10;

console.log(congido.height);       // 👉 10 (kế thừa từ rabbitPink)
console.log(longEar.height);       // 👉 undefined (vì không có)
```

---

### ⚠️ Ghi đè và cập nhật thuộc tính

```js
// Truy cập trực tiếp longEar từ congido và cập nhật giá trị
//từ congido cập nhật ear của longEar thành "short"
// congido.ear = "short"; //js sẽ tránh cập nhật cha = cách tạo 1 prop ear cho riêng congido
//      lúc này trong congido sẽ có 2 ear
congido.__proto__.__proto__.ear = "short";//mình trâu bò, xoáy vào tìm longEar trong congido để cập nhật

console.log(congido.ear);          // 👉 short (cập nhật từ longEar)
console.log(longEar.ear);          // 👉 short
console.log(congido);
```

> Nếu dùng `congido.ear = "short"`, JS sẽ **tạo thuộc tính mới trong congido**, không thay đổi longEar.

---

### 📛 Lưu ý về `__proto__`

| ⚠️ Trước ES6        | ✔️ Sau ES6                         |
|--------------------|-----------------------------------|
| `__proto__` dùng phổ biến, nhưng không chuẩn | Dùng `Object.getPrototypeOf(obj)` và `Object.setPrototypeOf(obj, proto)` |
- trước ES6(2015) ko có cách chính thống nào để truy cập vào [[Prototype]] của 1 object
- hầu hết các trình duyệt đều thêm 1 accessor property là__proto__, __proto__ko phải là thuộc tính của [[Prototype]] , nó là accessor của [[Prototype]]
- __proto__tính đến năm 2023 vẫn đang đc loại bỏ khỏi js
- __proto__sau này đc thay =<mark style="background: #FFB86CA6;"> Object.getPrototypeOf(obj); | Object.setPrototypeOf(obj, obj2);</mark>
- 
---

### 🧠 Ví dụ nâng cao: Accessor Property (Getter/Setter)

```js
let student = {
  firstName: "Lê",//value property
  lastName: "Điệp",//value property

  get fullName() {
    return this.firstName + " " + this.lastName;
  }, //accessor property

  set fullName(newName) {
    [this.firstName, this.lastName] = newName.split(" ");
  }, //accessor property
};

let user = {
  isUser: true,
  __proto__: student,
};

// Gọi setter fullName từ user (this là user)
user.fullName = "Khủng Long";
//nếu bây giờ muốn cập nhật firstname của Student = user thì sao
// Gọi setter từ user nhưng cập nhật trực tiếp student
user.__proto__.firstName = "Nguyễn";
```

#### Kết quả:

```js
console.log(user.fullName);      // 👉 "Khủng Long"
console.log(student.firstName); // 👉 "Nguyễn"
```

---

### ✅ Tổng kết

- JavaScript sử dụng mô hình **kế thừa nguyên mẫu** thông qua `[[Prototype]]`.
- Có thể tạo chuỗi kế thừa sâu nhiều cấp.
- Nên sử dụng `Object.getPrototypeOf()` và `Object.setPrototypeOf()` thay vì `__proto__`.
- Hiểu rõ về **getter/setter** giúp kiểm soát truy cập và cập nhật dữ liệu tốt hơn khi kế thừa.

---

