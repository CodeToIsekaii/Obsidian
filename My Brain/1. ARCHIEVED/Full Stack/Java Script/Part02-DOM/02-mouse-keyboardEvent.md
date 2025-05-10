---
date: 2025-03-29T23:32:00
---
Related:[[JavaScript]]
Tag: #js #mouse #event #keyboard
___
[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part02-dom\02-mouse-keyboardEvent.html)

## 1. Lắng nghe sự kiện nút bấm
```javascript
let btnAdd = document.querySelector("#btn-add");

btnAdd.addEventListener("click", (event) => {
  console.log(event);
  console.log(event.clientX, event.clientY); //tham chiếu là viewpoint tính từ góc trái trên cùng của phần trắng
  console.log(event.offsetX, event.offsetY); //tham chiếu là element(cái nút) diễn ra sự kiện
  console.log(event.target); //return object vừa diễn ra sự kiện (điều khiển những dom fake)

  //thực hiện hoa ước mơ
  let inputNode = document.querySelector("#name");
  let newItem = document.createElement("li");
  newItem.className = "card p-2 mb-3";
  newItem.innerHTML = `
    <p>${inputNode.value}</p>
  `;
  let list = document.querySelector("#list");
  list.appendChild(newItem);
  inputNode.value = "";
});
```
#event. #target: Chỉ đến phần tử HTML mà người dùng đã click vào.
### Các sự kiện chuột phổ biến:
- `click`: Nhấp chuột
- `mouseover`: Di chuột vào phần tử
- `mouseout`: Di chuột ra khỏi phần tử
- `dblclick`: Nhấp chuột hai lần liên tiếp

### Các sự kiện bàn phím phổ biến:
- `keydown`: Nhấn phím xuống (có thể mất 1 nhịp)
- `keypress`: Nhấn phím nhưng không nhận ALT, ESC, SHIFT, CTRL (có thể mất 1 nhịp)
- `keyup`: Nhả phím ra (không nhận ALT, ESC, SHIFT, CTRL)
- `input`: Tổng hợp cả 3 sự kiện trên (không nhận ALT, ESC, SHIFT, CTRL)
- `change`: Kích hoạt khi bấm ra ngoài phần tử nhập liệu

Ví dụ bắt sự kiện bàn phím:
```javascript
let inputNode = document.querySelector("#name");
inputNode.addEventListener("keyup", (event) => {
  console.log(inputNode.value);
  console.log(event);
});
```

## 2. Tìm hiểu về Cookies
Cookies cho phép lưu trữ thông tin người dùng trên máy tính.

### Thiết lập Cookie:
```javascript
const date = new Date(2023, 11, 28).toString();
document.cookie = `username=diep; expires=${date}; path=/;`;
console.log(document.cookie);
```
⚠️ **Lưu ý:** Trên thực tế, người ta thường thao tác với cookies thông qua thư viện như [js-cookie](https://github.com/js-cookie/js-cookie).

## 3. Tìm hiểu về LocalStorage
`localStorage` giúp lưu trữ dữ liệu ở local và có hiệu lực vĩnh viễn (trừ khi bị xóa).

### Lưu dữ liệu vào `localStorage`:
```javascript
localStorage.setItem("name", "Điệp Cookie");
```

### Lưu Object vào `localStorage`:
```javascript
const profile = {
  fname: "Anh Điệp đẹp trai",
  age: 24,
};
console.log(profile);

// Chuyển object thành chuỗi JSON
let str = JSON.stringify(profile);
console.log(str);

// Lưu vào LocalStorage
localStorage.setItem("profile", str);
```

### Lấy dữ liệu từ `localStorage`:
```javascript
let data = localStorage.getItem("profile");
data = JSON.parse(data);
console.log(data);
```

---
✍️ **Ghi chú:**
- `localStorage` và `cookies` chỉ lưu được **chuỗi hoặc JSON string**.
- `localStorage` có ưu điểm là lưu trữ dữ liệu vĩnh viễn mà không cần gửi request lên server như `cookies`.

