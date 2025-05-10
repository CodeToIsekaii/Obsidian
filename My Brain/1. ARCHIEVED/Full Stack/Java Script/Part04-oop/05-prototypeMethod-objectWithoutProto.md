---
date: 2025-04-18T13:49:00
---
Related : [[JavaScript]]
Tag: #js #prototypemethod #objectwithoutproto
___

## 🧬 Quản lý `[[Prototype]]` trong JavaScript (không dùng `__proto__`)

Trong thời hiện đại (JS 2023), ta **hạn chế dùng `__proto__`** do nó không nằm trong chuẩn chính thức ban đầu và gây ra nhiều vấn đề về hiệu năng, thay vào đó, dùng các phương thức được tiêu chuẩn hóa:

---

### 🔧 Các phương thức làm việc với `[[Prototype]]`

```js
Object.getPrototypeOf(obj);       // giống obj.__proto__ – Lấy [[Prototype]]
Object.setPrototypeOf(obj, proto); // gán [[Prototype]]
Object.create(proto, {descriptors}); // tạo object với proto và thuộc tính kèm bộ cờ
```
- proto thường dùng để kế thừa nguyên mẫu là kế thừa giữa 2 cái object
---

### 🐾 Ví dụ về kế thừa nguyên mẫu (Prototypal Inheritance)

```js
let animal = {
  eat: true,
  //__proto__: Object.prototype
};
//trong animal ngoài eat ra thì còn có [[Prototype]] - __proto__
console.log(animal.__proto__ == Object.prototype); //true
//vì animal đc tạo từ constructor Object
//nên animal.[[Prototype]] = prototype của Object constructor
//mà Object.prototype == class Object
```

---

### 📦 Prototypal Inheritance: kế thừa nguyên mẫu (giữa 2 object )
**Tạo `rabbitYellow` kế thừa từ `animal`**
**Cách 1:** (Cũ – dùng `__proto__`)
```js
let rabbitYellow = {
  jumps: true,
  __proto__: animal,
};
```

**Cách 2:** (Dùng `Object.setPrototypeOf`)
```js
let rabbitYellow = {
  jumps: true,
};
Object.setPrototypeOf(rabbitYellow, animal); //thay rabbitYellow.__proto__ = animal;
```

**Cách 3:** (Dùng `Object.create`)
```js
rabbitYellow = Object.create(animal);
//tạo object rỗng {} có [[Prototype]] = animal
rabbitYellow.jumps = true; //thiếu thuộc tính jumps nên thêm vào
```

**Cách 4:** (Tạo luôn cả thuộc tính với bộ cờ)
```js
rabbitYellow = Object.create(animal, {
  jumps: {
    value: true,
    writable: false,
    enumerable: false,
    configurable: true,
  },
});
//vừa tạo object {}, vừa set [[Prototype]] = animal, vừa tạo property kèm bộ cờ
```

---

### 📋 Clone `rabbitYellow` như thế nào?

**Cách 1:** (Dùng spread `...`)
```js
let objClone = { ...rabbitYellow };
```
> ❌ Không clone được bộ cờ:
> - ko lấy đc Thuộc tính `enumerable` là false
> - ko lấy đc `[[Prototype]]` ko ăn cáp đc tiền thân

---

**Cách 2:** (Dùng `Object.defineProperties`)
```js
objClone = Object.defineProperties(
  {},
  Object.getOwnPropertyDescriptors(rabbitYellow)
);
```
> ✅ Lấy được bộ cờ   nên lấy đc prop có enumerable là false
> ❌ Không lấy được `[[Prototype]]`

---

**Cách 3:** (Dùng `Object.create(newProto,{descriptors})` để clone toàn bộ)
```js
objClone = Object.create(
  Object.getPrototypeOf(rabbitYellow),//rabbitYellow.__proto__ cũng đc
  Object.getOwnPropertyDescriptors(rabbitYellow)
);
```
> ✅ Clone đầy đủ cả `[[Prototype]]` và bộ cờ

---

## 📜 Lịch sử `__proto__` và lý do nên tránh

| Năm | Sự kiện |
|-----|---------|
| Xưa  | `constructor.prototype` là cách kế thừa phổ biến |
| 2012 | `Object.create` ra đời – tạo object có `[[Prototype]]` |
| 2015 | `Object.getPrototypeOf` và `Object.setPrototypeOf` được chuẩn hóa |
| `__proto__` | Từng được thêm tạm thời để tiện thao tác, nhưng **không chuẩn** và nên tránh |

---

### ❗ Lý do `__proto__` bị thay thế

- Không an toàn, dễ gây lỗi.
- Làm chậm hiệu năng do phá tối ưu hóa nội bộ của JS engine.
- Không phải là thành phần chính thức của chuẩn ECMAScript (về sau mới hỗ trợ như "legacy accessor").

> 📌 **Tốt nhất**: chỉ set prototype **một lần khi khởi tạo**, không thay đổi sau đó.

---

## ⚙️ "Very plain Object" – Object siêu phẳng

> [!note] note
>`[[Prototype]]` là Object, Class, null, nhưng ko đc là string

```js
let obj = {}; //tạo ra object rỗng
// let key="name";
// obj[key]= "giá trị demo";//obj.["name"]//obj.name
let key = "__proto__";
obj[key] = "giá trị demo"; //ko là string
console.log(obj);
```
> ❗ Vì `[[Prototype]]` mặc định là `Object.prototype`, nên khi gán `"__proto__"` thì JS engine sẽ hiểu đó là thao tác với prototype, không phải key bình thường.

---

### ✅ Tạo object siêu phẳng (không prototype)
<mark style="background: #FFB86CA6;">- object siêu phẳng (tức là bên trong ko có gì hết, ko tầng ở phía dưới chỉ có 1 mặt thôi)</mark>
```js
let obj = Object.create(null); // [[Prototype]] === null
// Object.setPrototypeOf(obj, Object.prototype);//biến lại thành object ko phẳng
key = "__proto__";
obj[key] = animal;
console.log(obj);
//khi đề cập __proto__ thì nó ko hiểu này là [[Prototype]]
//mà nó cho em tạo 1 thuộc tính __proto__
```

> ✔️ `obj.__proto__` lúc này là **key bình thường**, không dính dáng gì đến `[[Prototype]]`

---

## 📌 Ghi nhớ

- Tránh dùng `__proto__`
- Dùng `Object.create`, `getPrototypeOf`, `setPrototypeOf`
- Tránh thay đổi `[[Prototype]]` sau khi object đã tạo
- Dùng `Object.create(null)` khi cần object "trơn" không dính gì
