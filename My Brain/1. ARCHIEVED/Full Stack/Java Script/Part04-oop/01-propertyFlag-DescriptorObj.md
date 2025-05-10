---
date: 2025-04-04T00:38:00
---
Related:[[JavaScript]]
Tag: #js #oop #propertyflag #descriptorobj
___

# Property Flag trong JavaScript

## 1. Property Descriptor
Mọi thuộc tính (property) trong một object đều có một tập hợp các cờ (flags) gọi là **Property Descriptor**, bao gồm:
- **value**: Giá trị của thuộc tính.
- **writable**:
  - `true`: Có thể thay đổi giá trị.
  - `false`: Chỉ đọc (read-only).
- **enumerable**:
  - `true`: Có thể duyệt trong vòng lặp.
  - `false`: Không thể duyệt.
- **configurable**:
  - `true`: Có thể cập nhật các cờ.
  - `false`: Không thể cập nhật `enumerable` và chỉ có thể thay đổi `writable` từ `true` → `false`.

### Ví dụ
```js
let profile = {
  fname: "Điệp",
  age: 18,
};
console.log(Object.getOwnPropertyDescriptor(profile, "fname"));
// {value: 'Điệp', writable: true, enumerable: true, configurable: true}
```

## 2. Object.defineProperty
Dùng để định nghĩa hoặc cập nhật bộ cờ của một property.

### 2.1 Cập nhật bộ cờ của thuộc tính có sẵn
```js
Object.defineProperty(profile, "fname", {
  writable: false,
});
profile.fname = "Tuấn"; // Không thay đổi giá trị do writable: false
```

### 2.2 Tạo mới một thuộc tính kèm bộ cờ
```js
Object.defineProperty(profile, "job", {
  value: "yangho",
  writable: true,
});
console.log(Object.getOwnPropertyDescriptor(profile, "job"));
// {value: 'yangho', writable: true, enumerable: false, configurable: false}
```

> **Lưu ý:** Khi `enumerable: false`, thuộc tính không xuất hiện trong vòng lặp `for...in`.

## 3. Non-configurable (Không thể cấu hình)
`configurable: false` nghĩa là:
1. Không thể thay đổi `configurable`.
2. Không thể thay đổi `enumerable`.
3. Không thể thay đổi `writable` từ `false` → `true` (nhưng `true` → `false` thì được).
4. Không thể thay đổi getter và setter (accessor property).

Ví dụ:
```js
Object.defineProperty(profile, "age", {
  configurable: false,
});
```

## 4. Object.defineProperties
Dùng để định nghĩa nhiều thuộc tính cùng lúc.
```js
Object.defineProperties(profile, {
  point: { value: 9, writable: true },
  student_id: { value: "SE111", writable: true },
});
console.log(Object.getOwnPropertyDescriptors(profile));
```

## 5. Clone Object với Property Descriptor
### Cách 1: Spread Operator (Không clone bộ cờ)
```js
let objClone = { ...profile };
console.log(Object.getOwnPropertyDescriptors(objClone));
```

### Cách 2: Clone đầy đủ bằng `Object.defineProperties`
```js
objClone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(profile));
console.log(Object.getOwnPropertyDescriptors(objClone));
```

## 6. Sealing an Object (Niêm phong Object)
- `Object.preventExtensions(obj)`: Ngăn thêm thuộc tính mới.
- `Object.seal(obj)`: Ngăn thêm/xóa thuộc tính (`configurable: false`).
- `Object.freeze(obj)`: Ngăn thêm/xóa/thay đổi thuộc tính (`configurable: false, writable: false`).

### Kiểm tra trạng thái Object
- `Object.isExtensible(obj)`: Kiểm tra có bị `preventExtensions` không.
- `Object.isSealed(obj)`: Kiểm tra có bị `seal` không.
- `Object.isFrozen(obj)`: Kiểm tra có bị `freeze` không.

## 7. Value Property vs. Accessor Property
Trong object có 2 loại property:
- **Value Property**: Chứa giá trị trực tiếp.
- **Accessor Property**: Dùng getter và setter.

Ví dụ:
```js
let student = {
  lastName: "Điệp",
  firstName: "Lê",
  get fullName() {
    return this.firstName + " " + this.lastName;
  },
  set fullName(newName) {
    [this.firstName, this.lastName] = newName.split(" ");
  },
};
console.log(student.fullName);
student.fullName = "Trà Long";
console.log(student);
```
### `this` trong `getter` và `setter`
Trong JavaScript, `this` đại diện cho **đối tượng hiện tại** khi một phương thức được gọi. Trong `getter` và `setter`, `this` trỏ đến object mà thuộc tính đó thuộc về.

### Nếu không có `this` thì sao?
Hãy phân tích đoạn code:

```js
let student = {
  get fname() {
    return this._fname;
  },
  set fname(newName) {
    if (newName.length < 4) {
      alert("Name is too short");
      return;
    }
    this._fname = newName;
  },
};

student.fname = "Huệ";  // ❌ Cảnh báo: "Name is too short"
console.log(student.fname); // ❌ undefined (do không được gán)
```

#### Vì sao phải dùng `this._fname`?
- Khi gọi `student.fname = "Huệ"`, JavaScript sẽ tự động gọi `set fname(newName)`.
- `this._fname = newName;` lưu giá trị vào **thuộc tính `_fname` của object `student`**.
- Khi truy cập `student.fname`, `get fname()` chạy và **trả về giá trị của `this._fname`**.

---

### ❌ Nếu bỏ `this` đi:
Giả sử ta viết thế này:

```js
let student = {
  get fname() {
    return _fname; // ❌ Lỗi: _fname is not defined
  },
  set fname(newName) {
    if (newName.length < 4) {
      alert("Name is too short");
      return;
    }
    _fname = newName; // ❌ Lỗi: _fname is not defined
  },
};
```

🔴 **Lỗi xảy ra vì** `_fname` không được định nghĩa ở phạm vi toàn cục (global scope) hoặc trong object `student`.

---

### ✅ Cách khắc phục mà không dùng `this`
Nếu muốn tránh dùng `this`, ta có thể định nghĩa `_fname` **bên ngoài object**:

```js
let _fname = ""; // Biến toàn cục

let student = {
  get fname() {
    return _fname;
  },
  set fname(newName) {
    if (newName.length < 4) {
      alert("Name is too short");
      return;
    }
    _fname = newName;
  },
};

student.fname = "Trà Long";
console.log(student.fname); // ✅ "Trà Long"
```

📌 **Nhược điểm của cách này**:
- `_fname` là **biến toàn cục**, **không gắn liền với object `student`**, nên nó không an toàn nếu có nhiều object khác nhau.
- Nếu có nhiều object `student`, tất cả chúng sẽ dùng chung `_fname`.

---

### 🔥 Tổng kết:
- **Có `this`** → `_fname` là **thuộc tính của object**, mỗi object có một `_fname` riêng.
- **Không có `this`** → `_fname` là biến toàn cục, không an toàn khi dùng nhiều object.

📢 **Lời khuyên**: Luôn dùng `this` trong `getter` và `setter` để giữ trạng thái trong object. 🚀
## 8. Ứng dụng Accessor Property
Dùng để kiểm soát dữ liệu trước khi gán.
```js
let student = {
  get fname() {
    return this._fname;
  },
  set fname(newName) {
    if (newName.length < 4) {
      alert("Name is too short");
      return;
    }
    this._fname = newName;
  },
};

student.fname = "Huệ"; // Alert: Name is too short
```


Đoạn code này sử dụng **getter và setter** để kiểm soát giá trị của thuộc tính `fname` trong object `student`. Cách nó hoạt động như sau:

1. **Getter (`get fname()`)**:
   - Khi truy cập `student.fname`, JavaScript sẽ gọi hàm `get fname()`.
   - Hàm này trả về giá trị của thuộc tính `_fname`.

2. **Setter (`set fname(newName)`)**:
   - Khi gán `student.fname = "Huệ"`, JavaScript sẽ gọi hàm `set fname(newName)`.
   - Trong setter, có một điều kiện kiểm tra độ dài của `newName`:
     - Nếu `newName.length < 4`, hiển thị cảnh báo `"Name is too short"` và kết thúc hàm (`return`).
     - Nếu hợp lệ, giá trị của `_fname` được cập nhật bằng `newName`.

### Chạy chương trình:
```js
student.fname = "Huệ"; 
```
- `"Huệ"` có độ dài **3 ký tự**, nhỏ hơn 4.
- Điều kiện `if (newName.length < 4)` đúng → Hiển thị `alert("Name is too short")`.
- Quá trình gán giá trị kết thúc sớm, `this._fname` không thay đổi.

💡 **Kết quả**: Không có giá trị nào được lưu vào `_fname`, do điều kiện kiểm tra bị vi phạm.

Bạn có thể thử với chuỗi dài hơn để gán thành công:
```js
student.fname = "Trà Long"; 
console.log(student.fname); // "Trà Long"
```
