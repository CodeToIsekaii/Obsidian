---
date: 2025-04-21T10:32:00
---
Related : [[JavaScript]]
Tag: #js #static #propertymethod
___

## 🧠 JavaScript - Static Property và Static Method

### 🔹 `static` là gì?

- `static` dùng để khai báo **thuộc tính (property)** hoặc **phương thức (method)** **tĩnh** — tức là **không thuộc về instance (đối tượng)**, mà thuộc **về class**.
- Khi một property hoặc method là `static`, ta **không thể truy cập nó thông qua object được tạo từ class**, mà phải truy cập **thông qua chính class đó**.

---

### 📌 Ví dụ 1: `static` property

```js
class User9 {
  name = "Điệp";              // thuộc tính thường
  static name2 = "Lan";       // thuộc tính tĩnh
}

let obj = new User9();

console.log(obj.name);      // ✅ Kết quả: "Điệp"
console.log(obj.name2);     // ❌ undefined - vì name2 là static, không thuộc về obj //sai (nhưng đúng bên java)
console.log(User9.name2);   // ✅ Kết quả: "Lan" //truy cập = class
```

➡️ **Giải thích:**  
- `name2` được khai báo là `static`, nên nó không gắn liền với từng đối tượng `obj`, mà gắn liền với `User9` class.  
- Do đó, ta **truy cập qua `User9.name2`** chứ không dùng `obj.name2`.

---

### 📌 Ví dụ 2: `static` method - hàm so sánh trong class `Article`

```js
class Article {
  constructor(title, date) {
    this.title = title;
    this.date = date;
  }

  static compare(articleA, articleB) {
    return articleA.date - articleB.date;
  }//trong js ko có interface comparable nên phải tự tạo hàm compare
}
```

➡️ Nếu `compare()` **không phải** là static method thì mỗi `Article` object sẽ có một bản sao riêng — tốn tài nguyên nếu có nhiều object.  
➡️ Bằng cách khai báo `compare` là `static`, ta chỉ tạo **một lần duy nhất**, dùng chung cho tất cả.

---

### 🗞 Ví dụ sử dụng: sắp xếp danh sách bài báo

```js
let articleList = [
  new Article("Hoài Linh để quên 14 tỷ trong ngân hàng", new Date(2022, 2, 4)),
  new Article("Jack bán áo Mesi để từ thiện", new Date(2022, 0, 6)),
  new Article("Người mua áo Messi dùng tiền từ thiện cho con của Jack", new Date(2033, 8, 20))
];

// Dùng phương thức static để so sánh và sắp xếp
articleList.sort(Article.compare);

console.log(articleList);
```

➡️ `Article.compare` được truyền vào `sort()` để làm tiêu chí so sánh theo ngày (`date`).

---

### ✅ Tóm tắt

| Thành phần    | Có `static`           | Không có `static`      |
|---------------|------------------------|--------------------------|
| Thuộc về      | Class                  | Instance (object)        |
| Truy cập qua  | `ClassName.property`   | `object.property`        |
| Số lần tạo    | 1 lần duy nhất         | Mỗi object 1 lần         |

---

### 📍 Ghi nhớ:

> Khi một hàm không phụ thuộc vào dữ liệu cụ thể của instance, hãy dùng `static` để tiết kiệm tài nguyên và làm rõ ý định thiết kế.
