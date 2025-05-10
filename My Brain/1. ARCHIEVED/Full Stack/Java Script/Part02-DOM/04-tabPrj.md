---
date: 2025-04-01T11:26:00
---
Related:[[JavaScript]]
Tag: #js #project #tab
___
[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part02-dom\04-tabPrj\04-tabPrj.html)
[CSS](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part02-dom\04-tabPrj\04-tabPrj.css)

---

# 📌 Quản lý Tabs bằng JavaScript

## 🎯 Sự kiện `DOMContentLoaded`
```js
document.addEventListener("DOMContentLoaded", () => {});
```
- Được sử dụng để đảm bảo code chạy **chỉ sau khi trang web đã load xong**. dùng cho mấy khứa đặt script ở đầu
- Dùng khi đặt `<script>` trong `<head>`, tránh lỗi do DOM chưa được tạo đầy đủ.
> [!important] 
> nhược điểm của đặt script lên đầu là trang web chưa load xong đã chạy code trong js lúc đó móc cái gì 
## 🏗️ Code quản lý Tabs  
### 🔹 Ý tưởng
- Duyệt qua từng nút tab (`.navtab-btn`), khi nút được click:
  1. Xóa class `actived` của tất cả nút.
  2. Thêm class `actived` vào nút vừa click.
  3. Xóa class `actived` của tất cả nội dung tab (`.tab-content-item`).
  4. Tìm nội dung tab tương ứng theo `data-id` của nút và thêm `actived`.

### 🔹 Code chi tiết
```js
let btnList = document.querySelectorAll(".navtab-btn");
let contentList = document.querySelectorAll(".tab-content-item");
//duyệt qua từng cái nút
btnList.forEach((btn) => {
  //nếu có nút nào bị nhấn thì
  btn.addEventListener("click", (event) => {
    //xóa hết actived của các nút
    btnList.forEach((_btn) => {
      _btn.classList.remove("actived");
    });
    //cái nút vừa bị nhấn phải thêm actived
    event.target.classList.add("actived");
    //xóa hết actived của các content
    contentList.forEach((content) => {
      content.classList.remove("actived");
    });
    //lấy id từ các nút vừa nhấn
    let id = event.target.id; 
    //móc dom trực tiếp vào luôn            
    let contentChecked = document.querySelector(
      `.tab-content-item[data-id="${id}"]`
    );
    contentChecked.classList.add("actived");
  });
});
```

## 📌 Giải thích một số phần quan trọng
- **`event.target`** → Chính là nút vừa được click.
- **`getAttribute("...")`** → Lấy giá trị từ thuộc tính ,giá trị từ mấy cái tên là lạ mà mình đặt
- **`.classList.add("actived")` / `.classList.remove("actived")`** → Thêm / Xóa class để hiển thị tab tương ứng.
-  <mark style="background: #FFB8EBA6;">(dataset.id hay.id)data-id hay id là những thuộc tính đặc biệt có sẵn</mark>
### 🔍 Giải thích đoạn **`.tab-content-item[data-id="${id}"]`**  

#### **1️⃣ Cách hoạt động**
Câu lệnh này sử dụng **CSS Selector** để tìm phần tử có thuộc tính `data-id` khớp với giá trị của biến `id`.  

Cụ thể:
```js
let contentChecked = document.querySelector(`.tab-content-item[data-id="${id}"]`);
```
- `document.querySelector(...)`: Tìm **phần tử đầu tiên** trong tài liệu HTML khớp với bộ chọn CSS.
- `.tab-content-item[data-id="${id}"]`:
  - `.tab-content-item` → Chỉ chọn các phần tử có class **`tab-content-item`**.
  - `[data-id="${id}"]` → Chỉ chọn phần tử có **thuộc tính `data-id`** với giá trị bằng **`id`** của nút vừa click.

#### **2️⃣ Quá trình chạy**
Ví dụ với HTML sau:
```html
<div class="tab-content-item" data-id="tab1">Nội dung Tab 1</div>
<div class="tab-content-item" data-id="tab2">Nội dung Tab 2</div>
<div class="tab-content-item" data-id="tab3">Nội dung Tab 3</div>
```
Khi người dùng click vào:
```html
<button id="tab2" class="navtab-btn">Tab 2</button>
```
- `event.target.id` sẽ lấy giá trị `"tab2"`.
- Biến `id = "tab2"`, nên chuỗi template string:
  ```js
  `.tab-content-item[data-id="${id}"]`
  ```
  trở thành:
  ```js
  `.tab-content-item[data-id="tab2"]`
  ```
- `document.querySelector(...)` sẽ tìm:
  ```html
  <div class="tab-content-item" data-id="tab2">Nội dung Tab 2</div>
  ```
  và lưu vào biến `contentChecked`.

#### **3️⃣ Tóm gọn lại**
- **Lấy `id` của nút click.**
- **Dùng `querySelector` tìm phần tử `.tab-content-item` có `data-id` khớp với `id`.**
- **Thêm class `actived` để hiển thị nội dung đúng.**

---

🔥 **Mẹo**:  
✅ Có thể dùng `getAttribute("data-id")` thay vì `id` nếu không muốn ràng buộc ID với HTML.
## 🎨 Ví dụ HTML
```html
<div class="tabs">
  <button id="tab1" class="navtab-btn actived">Tab 1</button>
  <button id="tab2" class="navtab-btn">Tab 2</button>
  <button id="tab3" class="navtab-btn">Tab 3</button>
</div>

<div class="tab-content">
  <div class="tab-content-item actived" data-id="tab1">Nội dung Tab 1</div>
  <div class="tab-content-item" data-id="tab2">Nội dung Tab 2</div>
  <div class="tab-content-item" data-id="tab3">Nội dung Tab 3</div>
</div>
```

## 🎨 CSS (Thêm hiệu ứng đẹp)
```css
.navtab-btn {
  padding: 10px 20px;
  cursor: pointer;
  border: none;
  background: lightgray;
  margin: 5px;
}
.navtab-btn.actived {
  background: blue;
  color: white;
}
.tab-content-item {
  display: none;
  padding: 15px;
  border: 1px solid #ccc;
  margin-top: 10px;
}
.tab-content-item.actived {
  display: block;
}
```

---

📌 **Ghi chú**:  
- **Cần có `.actived` ban đầu trên Tab 1** để tránh lỗi khi load trang.
- **CSS quan trọng**: `.tab-content-item { display: none; }` giúp ẩn tab không được chọn.

💡 **Ứng dụng thực tế**:
- Quản lý nhiều nội dung trên cùng một trang mà không cần tải lại.
- Dùng trong website, dashboard, hoặc ứng dụng web.

---

Ghi chú này gọn gàng và dễ đọc trong **Obsidian**. Bạn có thể paste vào ngay 🎯