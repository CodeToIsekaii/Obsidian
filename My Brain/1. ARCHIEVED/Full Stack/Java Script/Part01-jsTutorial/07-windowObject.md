---
date: 2025-03-27T14:11:00
---
Related:[[JavaScript]]
Tag: #window #object #html
___

### **1. Giới thiệu về `window` Object**
- **`window`** là một đối tượng đại diện cho cửa sổ trình duyệt.
- Tất cả các **global object**, **global function**, và **biến** được khai báo bằng `var` đều là các **method** hoặc **properties** của đối tượng `window`.
- **Lưu ý**: Các khai báo với `let` và `const` sẽ không trở thành properties của `window`.

### **2. DOM cũng là một phần của `window`**
- **DOM (Document Object Model)** là một cấu trúc dữ liệu đại diện cho các thành phần trong tài liệu HTML. DOM là một phần của đối tượng `window` trong JavaScript.

### **3. Các Properties và Methods của `window`**

#### **Các thuộc tính (Properties) của `window`:**

- **`window.innerHeight`**: Lấy chiều cao của cửa sổ (phần màu trắng).
  ```javascript
  console.log(window.innerHeight);
  ```
  
- **`window.innerWidth`**: Lấy chiều rộng của cửa sổ (phần màu trắng).
  ```javascript
  console.log(window.innerWidth);
  ```

#### **Mở một tab mới và thay đổi kích thước của nó:**
```javascript
let newTab;
setTimeout(() => {
  newTab = window.open(
    "https://animehay.io/",  // URL để mở
    "_bank",  // Tên tab mới
    "width=500, height=500"  // Kích thước của tab
  );
  newTab.resizeTo(300, 400);  // Thay đổi kích thước tab sau 3 giây
}, 3000);
```

- **`window.close()`**: Đóng cửa sổ hoặc tab hiện tại.

#### **Các thuộc tính của `location` (URL của trang web):**

- **`location.href`**: Trả về toàn bộ URL của trang hiện tại.
  ```javascript
  console.log(location.href);  // Lấy toàn bộ URL
  ```

- **`location.hostname`**: Trả về tên miền của URL.
  ```javascript
  console.log(location.hostname);  // Lấy tên miền
  ```

- **`location.pathname`**: Trả về đường dẫn của URL (phần sau dấu `/`).
  ```javascript
  console.log(location.pathname);  // Lấy đường dẫn
  ```

- **`location.protocol`**: Trả về giao thức của URL (ví dụ: `http:` hoặc `https:`).
  ```javascript
  console.log(location.protocol);  // Lấy giao thức
  ```

#### **Thay đổi URL trang hiện tại:**
- **`location = "url"`**: Thay đổi URL trang web hiện tại.
  ```javascript
  location = "https://example.com";  // Chuyển hướng trang
  ```

- **`location.assign("url")`**: Dùng để điều hướng đến một URL mới.
  ```javascript
  location.assign("https://example.com");
  ```

#### **Duyệt lịch sử trang web:**
- **`history.forward()`**: Di chuyển về phía trước trong lịch sử trình duyệt.
  ```javascript
  history.forward();  // Đi tới trang kế tiếp
  ```

- **`history.back()`**: Di chuyển về phía sau trong lịch sử trình duyệt.
  ```javascript
  history.back();  // Quay lại trang trước
  ```

### **4. Các Popup trong Trình Duyệt**
Trình duyệt cung cấp 3 loại popup cơ bản:

- **`alert()`**: Hiển thị một thông báo.
  ```javascript
  alert("Lộc ngu");  // Hiển thị thông báo
  ```

- **`confirm()`**: Hiển thị hộp thoại xác nhận với hai tùy chọn: OK và Cancel.
  ```javascript
  let result = confirm("Anh Điệp có đẹp trai ko?");
  if (result) {
    alert("Ghét nhất bọn nói thật");
  } else {
    alert("Đừng dối lòng nữa");
  }
  ```

- **`prompt()`**: Hiển thị hộp thoại yêu cầu người dùng nhập thông tin.
  ```javascript
  let result = prompt("Nhập tên đi thằng l");
  let defaultValue = prompt("Nhập tên đi thằng l", "Lộc");  // Có thể đặt giá trị mặc định
  ```

---

### **Tóm Tắt:**

| Phương thức                   | Mô tả                                                   |
|-------------------------------|---------------------------------------------------------|
| `window.innerHeight`           | Chiều cao của cửa sổ trình duyệt                        |
| `window.innerWidth`            | Chiều rộng của cửa sổ trình duyệt                       |
| `window.open()`                | Mở một tab mới trong trình duyệt                         |
| `window.close()`               | Đóng cửa sổ hoặc tab hiện tại                           |
| `location.href`                | Lấy hoặc thay đổi toàn bộ URL của trang hiện tại        |
| `location.hostname`            | Lấy tên miền của trang web                              |
| `location.pathname`            | Lấy đường dẫn của trang web                             |
| `location.protocol`            | Lấy giao thức của URL                                   |
| `location.assign("url")`       | Điều hướng tới một URL mới                              |
| `history.forward()`            | Di chuyển về trang kế tiếp trong lịch sử trình duyệt    |
| `history.back()`               | Quay lại trang trước trong lịch sử trình duyệt          |
| `alert()`                      | Hiển thị một thông báo popup                            |
| `confirm()`                    | Hiển thị hộp thoại xác nhận (OK/Cancel)                 |
| `prompt()`                     | Hiển thị hộp thoại yêu cầu nhập thông tin               |

---

