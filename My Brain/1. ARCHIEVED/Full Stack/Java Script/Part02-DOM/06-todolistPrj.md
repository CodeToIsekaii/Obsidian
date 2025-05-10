---
date: 2025-04-01T23:36:00
---
Related:[[JavaScript]]
Tag: #project #todolist 
___
[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part02-dom\06-todolistPrj\index.html)

# Quản Lý Danh Sách (LocalStorage & UI)

## 1. Mô Tả Tác Dụng

- **Thêm một item**: Nhập tên và nhấn Submit. Item sẽ hiển thị trên giao diện và lưu vào `localStorage`.
- **Xóa một item**: Nhấn nút "Remove" trên item cần xóa.
- **Xóa tất cả**: Nhấn nút "Remove All".
- **Lọc danh sách**: Nhập văn bản vào ô filter.

---

## 2. Code Chi Tiết

### 2.1. Xử Lý Sự Kiện Submit Form

```js
document.querySelector("form").addEventListener("submit", (event) => {
  event.preventDefault();// Ngăn chặn reload trang khi submit form
  let name = document.querySelector("#name").value.trim();
  let item = { id: new Date().toISOString(), name };//lưu rất nhiều item nên phải có id nên lấy ngầy  giờ tạo luôn 
  console.log(item);
  addItemToUI(item);  //hàm nhận vào item và hiện thị lên UI:
  addItemToLS(item);  //hàm nhận vào item và lưu vào localStorage:
  document.querySelector("#name").value = "";//làm trống chỗ nhập
});
```

### 2.2. Lấy Danh Sách  các item Từ LocalStorage

```js
const getList = () => JSON.parse(localStorage.getItem("list")) || [];
```
### Giải thích:
- **`localStorage.getItem("list")`**: Lấy giá trị của khóa `"list"` từ `localStorage`. Nếu khóa này chưa tồn tại, nó sẽ trả về `null`.
- **`JSON.parse(...)`**: Chuyển đổi chuỗi JSON thành một mảng JavaScript. Nếu `localStorage` có dữ liệu hợp lệ, nó sẽ được chuyển thành một danh sách (mảng) các item.
- **`|| []`**: Nếu `localStorage.getItem("list")` trả về `null` (tức là chưa có dữ liệu), đoạn code sẽ trả về một mảng rỗng `[]` để tránh lỗi.

### Tóm lại:
Hàm `getList()` lấy danh sách item từ `localStorage` và đảm bảo rằng nếu danh sách chưa tồn tại, nó sẽ trả về một mảng rỗng thay vì `null`. Điều này giúp chương trình luôn có một danh sách hợp lệ để làm việc.
### 2.3. Thêm Item Vào UI

```js
const addItemToUI = (item) => {
  let newCard = document.createElement("div");  //tạo card từ item
  newCard.className = "card d-flex flex-row justify-content-between align-items-center p-2 mb-3";
  newCard.innerHTML = `
    <span>${item.name}</span>
    <button data-id="${item.id}" type="button" class="btn btn-danger btn-sm btn-remove">Remove</button>
  `;
  document.querySelector(".list").appendChild(newCard);
};
```

### 2.4. hàm nhận vào item và lưu item đó vào localStorage

```js
const addItemToLS = (item) => {
  let list = getList(); //lấy danh sách các item từ ls
  list.push(item); //lấy danh sách các item từ ls
  localStorage.setItem("list", JSON.stringify(list));  //lưu danh sách lên lại ls
};
```

### Tại sao phải lấy danh sách (`getList()`) rồi mới thêm vào?
1. **Lấy danh sách hiện tại từ `localStorage`**:  
   - `getList()` lấy danh sách các item đã lưu trong `localStorage`.
   - Nếu danh sách chưa có, nó trả về một mảng rỗng `[]`.

2. **Thêm item mới vào danh sách**:  
   - `list.push(item);` thêm `item` mới vào danh sách.

3. **Ghi đè danh sách cũ bằng danh sách mới**:  
   - `localStorage.setItem("list", JSON.stringify(list));`  
     → Chuyển danh sách thành chuỗi JSON rồi lưu lại vào `localStorage`.

### Tại sao không lưu trực tiếp `item` vào `localStorage`?
- `localStorage` chỉ lưu dữ liệu dưới dạng chuỗi, không thể tự động cập nhật danh sách.
- Nếu ghi đè trực tiếp:
  ```js
  localStorage.setItem("list", JSON.stringify(item)); 
  ```
  → Chỉ lưu **một item duy nhất**, các item trước đó sẽ bị mất.

### Có cách tối ưu hơn không?
Thay vì lấy cả danh sách về rồi ghi đè lại, có thể chỉ thêm item trực tiếp bằng cách:
```js
const addItemToLS = (item) => {
  let list = JSON.parse(localStorage.getItem("list") || "[]");
  localStorage.setItem("list", JSON.stringify([...list, item]));
};
```
✅ Lợi ích:
- Không cần khai báo biến `list` riêng.
- Đọc `localStorage` một lần và cập nhật ngay lập tức.

`localStorage.getItem("list")` là một phương thức của `localStorage` trong JavaScript. Nó dùng để **lấy dữ liệu** từ `localStorage` dựa trên một **key** (`"list"` trong trường hợp này).  

Cụ thể:  
- Khi bạn gọi `localStorage.getItem("list")`, trình duyệt sẽ trả về giá trị đã lưu dưới key `"list"`.  
- Vì `localStorage` chỉ lưu trữ dữ liệu dưới dạng **chuỗi (string)**, nên ta cần dùng `JSON.parse(...)` để chuyển đổi nó thành **mảng JavaScript**.  

Ví dụ hoạt động của nó:  
```js
localStorage.setItem("list", JSON.stringify([{ id: 1, name: "Item 1" }]));

let data = localStorage.getItem("list");
console.log(data); 
// Kết quả: '[{"id":1,"name":"Item 1"}]' (chuỗi JSON)

let parsedData = JSON.parse(data);
console.log(parsedData);
// Kết quả: [{ id: 1, name: "Item 1" }] (mảng JavaScript)
```

Trong code của bạn, `getList()` sử dụng `getItem` để **lấy danh sách từ `localStorage`**:  
```js
const getList = () => JSON.parse(localStorage.getItem("list")) || [];
```
- Nếu `"list"` tồn tại, nó sẽ lấy dữ liệu và chuyển về dạng mảng.
- Nếu `"list"` **chưa tồn tại** (`null`), nó sẽ trả về một mảng rỗng `[]` để tránh lỗi.

Bạn cần giải thích thêm phần nào không? 😊
### 2.5. init: hàm render các item trong danh sách vì khi reload sẽ mất list trên màn hình nên cần hàm này

```js
const init = () => {
  getList().forEach(addItemToUI);
};
init();
```

### 2.6. Xóa Item Khỏi UI và LocalStorage
> [!warning] 
> ko dom vô mấy thằng tạo sau như nút remove , mấy thằng fake 

```js
document.querySelector(".list").addEventListener("click", (event) => {
  if (event.target.classList.contains("btn-remove")) {
    let nameItem = event.target.previousElementSibling.innerHTML;
    let isConfirmed = confirm(`Bạn có chắc là muốn xóa item: ${nameItem}`);
    if (isConfirmed) {
      //xóa
      //xóa UI
      let card = event.target.parentElement;
      card.remove();
      //xóa lS
      let idRemove = event.target.dataset.id; //lấy data-id của btn-remove
      //hàm nhận vào cái id của item cần xóa và xóa item khỏi danh sách trong ls:removeItemFromLS(idRemove)
      removeItemFromLS(idRemove);
    }
  }
});

const removeItemFromLS = (idRemove) => {
  let list = getList();
  //filter:lọc ra những item có mã khác idRemove
  list = list.filter((item) => item.id != idRemove);
  //lưu lại lên ls
  localStorage.setItem("list", JSON.stringify(list));
};
```
### Giải thích code
#### 1. **Sự kiện lắng nghe click trên danh sách (`.list`)**
```js
document.querySelector(".list").addEventListener("click", (event) => {
```
- Khi người dùng click vào bất kỳ phần tử nào bên trong danh sách (`.list`), sự kiện này sẽ kích hoạt.
- `event.target` là phần tử mà người dùng vừa click vào.

---

#### 2. **Kiểm tra xem có phải nút "Remove" không**
```js
if (event.target.classList.contains("btn-remove")) {
```
- Kiểm tra nếu phần tử được click có class `"btn-remove"`, tức là nút "Remove".
- Nếu đúng, tiếp tục xử lý xóa.

---

#### 3. **Lấy tên của item để hiển thị trong hộp thoại xác nhận**
```js
let nameItem = event.target.previousElementSibling.innerHTML;
```
- `event.target.previousElementSibling`: Lấy phần tử liền trước nút bấm.
- Do cấu trúc HTML:
  ```html
  <span>Item Name</span>
  <button class="btn-remove">Remove</button>
  ```
  → `previousElementSibling` của `<button>` chính là `<span>Item Name</span>`.
- `.innerHTML`: Lấy nội dung bên trong `<span>`, chính là tên của item.

---

#### 4. **Xác nhận với người dùng trước khi xóa**
```js
if (confirm(`Bạn có chắc muốn xóa item: ${nameItem}`)) {
```
- Hiển thị hộp thoại xác nhận: `"Bạn có chắc muốn xóa item: [Tên Item]?"`.
- Nếu người dùng chọn "OK" → Tiếp tục xóa.
- Nếu chọn "Cancel" → Hủy thao tác.

---

#### 5. **Xóa item khỏi UI**
```js
let card = event.target.parentElement;
card.remove();
```
- `event.target.parentElement`: Lấy phần tử cha của nút bấm, chính là `div` chứa item.
- `.remove()`: Xóa phần tử khỏi giao diện.

---

#### 6. **Xóa item khỏi `localStorage`**
```js
removeItemFromLS(event.target.dataset.id);
```
- `event.target.dataset.id`: Lấy `data-id` từ nút "Remove", chính là `id` của item.
- Gọi hàm `removeItemFromLS(idRemove)` để xóa item khỏi `localStorage`.

---

#### 7. **Hàm xóa item khỏi `localStorage`**
```js
const removeItemFromLS = (idRemove) => {
  let list = getList().filter((item) => item.id !== idRemove);
  localStorage.setItem("list", JSON.stringify(list));
};
```
- `getList()`: Lấy danh sách item hiện tại từ `localStorage`.
- `.filter((item) => item.id !== idRemove)`: 
  - Giữ lại tất cả item **không** có `id` bằng `idRemove` (tức là loại bỏ item cần xóa).
- `localStorage.setItem("list", JSON.stringify(list))`: Cập nhật danh sách mới lên `localStorage`.

---

#### **Tóm tắt hoạt động**
1. Người dùng bấm vào nút "Remove".
2. Kiểm tra nếu đúng là nút "Remove".
3. Lấy tên item hiển thị trong hộp thoại xác nhận.
4. Nếu người dùng đồng ý, xóa item khỏi UI.
5. Xóa item khỏi `localStorage`.


### 2.7. Xóa Hết Danh Sách

```js
document.querySelector("#btn-remove-all").addEventListener("click", (event) => {
  const isConfirmed = confirm("Bạn có chác muốn xóa hết ko?");
  if (isConfirmed) {
    document.querySelector(".list").innerHTML = ""; //xóa hết ui
    localStorage.removeItem("list"); //xóa hết ls
  }
});
```
- removeItem của localstorage
### 2.8. Lọc Danh Sách

```js
document.querySelector("#filter").addEventListener("keyup", (event) => {
  //giá trị từ ô filter
  let inputValue = document.querySelector("#filter").value;
  let list = getList(); //lấy danh sách list
  //item nào có name chứa giá trị đang gõ thì lấy ra
  let filterList = list.filter((item) => item.name.includes(inputValue));
  document.querySelector(".list").innerHTML = ""; //xóa danh sách cũ trước khi render danh sách mới lọc
  //render danh sách mới lọc
  filterList.forEach((item) => {
    addItemToUI(item);
  });
});
```
### Giải thích `name` trong đoạn code:
```js
let filterList = getList().filter((item) => item.name.includes(inputValue));
```

#### **1. `item.name` là gì?**
- `getList()` trả về danh sách các item từ `localStorage`, mỗi item có cấu trúc:
  ```js
  { id: "2024-04-02T12:00:00.000Z", name: "Học JavaScript" }
  ```
- `item.name` chính là **tên của item**, ví dụ: `"Học JavaScript"`.

#### **2. `item.name.includes(inputValue)` làm gì?**
- Kiểm tra xem `name` của item có chứa chuỗi `inputValue` (giá trị người dùng nhập vào ô filter) hay không.
- Nếu **có**, item đó sẽ được giữ lại trong `filterList`.
- Nếu **không**, item đó bị loại bỏ.

#### **3. Ví dụ minh họa**
Giả sử danh sách trong `localStorage`:
```js
[
  { id: "1", name: "Học JavaScript" },
  { id: "2", name: "Luyện React" },
  { id: "3", name: "Làm dự án nhỏ" }
]
```
Người dùng nhập `"Học"` vào ô filter:
```js
let inputValue = "Học";
let filterList = getList().filter((item) => item.name.includes(inputValue));
```
👉 **Kết quả `filterList`**:
```js
[
  { id: "1", name: "Học JavaScript" }
]
```
Vì chỉ có `"Học JavaScript"` chứa `"Học"`, còn các item khác không khớp.


---

## 3. Tổng Kết

- **Lưu trữ danh sách**: `localStorage`
- **Thêm item**: Hiển thị lên UI và lưu vào `localStorage`
- **Xóa item**: Xóa khỏi UI và `localStorage`
- **Xóa tất cả**: Xóa danh sách trong UI và `localStorage`
- **Lọc danh sách**: Hiển thị item phù hợp với tài khoản

---

**Ghi Chú**:
- Sử dụng `localStorage` để lưu trữ danh sách ngay cả khi reload trang.
- Mọi thao tác thêm, xóa, lọc đều được cập nhật ngay trên UI và `localStorage`.
- Hợp lý với các danh sách nhỏ gọn cần quản lý nhanh.


