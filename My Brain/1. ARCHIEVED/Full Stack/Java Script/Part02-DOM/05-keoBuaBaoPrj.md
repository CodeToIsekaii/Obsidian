---
date: 2025-04-01T13:05:00
---
Related:[[JavaScript]]
Tag: #project #rockpaperscissor 
___

[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part02-dom\05-keoBuaBaoPrj\index.html)

## 1️⃣ Bảng giá trị (Table)
```js
const VALUES = [
  { id: "scissors", value: "✌️" }, // 0 - Kéo
  { id: "rock", value: "✊" }, // 1 - Búa
  { id: "paper", value: "🖐" }, // 2 - Bao
];
```

## 2️⃣ Quy luật của game
- Khi nào **thắng**:
  - `0 - 2 = -2`  ✅ (Kéo thắng Bao)
  - `1 - 0 = 1`  ✅ (Búa thắng Kéo)
  - `2 - 1 = 1`  ✅ (Bao thắng Búa)
  - **Công thức**: `indexPlayer - indexComputer = -2 || 1` → **win** (return `1`)

- Khi nào **hòa**:
  - `indexPlayer - indexComputer = 0` → **draw** (return `0`)

- Khi nào **thua**:
  - Trường hợp còn lại → **lose** (return `-1`)

## 3️⃣ Hàm thay đổi giá trị của máy tính
```js
let i = 0;
const handleChange = () => {
  let computer = document.querySelector("#computer");
  computer.textContent = VALUES[i].value;
  computer.dataset.id = VALUES[i].id;
  i = i == VALUES.length - 1 ? 0 : ++i;
};
let interval = setInterval(handleChange, 85);
```
📌 **Ghi chú:**
- `textContent` chỉ nhập được chữ, còn `innerHTML` hiển thị cả HTML cao cấp hơn.
- `setInterval` giúp đổi giá trị liên tục, cần lưu lại biến để sau này dừng lại (`clearInterval`).
### 🚀 Giải thích: `++i` thay vì `i++` trong `i = i == VALUES.length - 1 ? 0 : ++i;`

#### 🔍 Sự khác biệt giữa `++i` và `i++`
- **`++i` (Tiền tố - Pre-increment)**: Tăng giá trị `i` trước rồi mới sử dụng.
- **`i++` (Hậu tố - Post-increment)**: Sử dụng giá trị `i` trước rồi mới tăng.

#### 📌 Áp dụng vào câu lệnh:
```js
i = i == VALUES.length - 1 ? 0 : ++i;
```
- Nếu `i` đạt giá trị lớn nhất (`VALUES.length - 1`), đặt lại `i = 0` (reset vòng lặp).
- Nếu không, **tăng `i` trước rồi mới gán lại cho `i`** (`++i`).

#### 🤔 Nếu dùng `i++` thì sao?
```js
i = i == VALUES.length - 1 ? 0 : i++;
```
- Khi `i` chưa đạt giới hạn, `i++` **trả về giá trị cũ rồi mới tăng**, nghĩa là giá trị mới của `i` **không được gán ngay**.
- Điều này dẫn đến `i` luôn giữ nguyên giá trị hiện tại **chứ không tăng**.

#### ✅ Vì vậy, `++i` đảm bảo `i` được tăng ngay lập tức trước khi gán lại!
## 4️⃣ Hàm so sánh giá trị (Xác định thắng - thua - hòa)
```js
const compare = (valuePlayer, valueComputer) => {
  let indexPlayer = VALUES.findIndex((item) => item.id == valuePlayer);
  let indexComputer = VALUES.findIndex((item) => item.id == valueComputer);
  let result = indexPlayer - indexComputer;
  if ([-2, 1].includes(result)) return 1; // Thắng
  else if (result == 0) return 0; // Hòa
  else return -1; // Thua
};
```

## 5️⃣ Xử lý sự kiện khi người chơi bấm chọn
```js
let playerItem = document.querySelectorAll(".user");
//duyệt qua các nút
playerItem.forEach((item) => {
  //nếu có 1 nút nào bị click thì
  item.addEventListener("click", (event) => {
    clearInterval(interval); //dừng máy lại
    //lấy giá trị
    let valuePlayer = event.target.id;
    let computer = document.querySelector("#computer");
    let valueComputer = computer.dataset.id;
    let result = compare(valuePlayer, valueComputer);
    //xóa actived
    playerItem.forEach((_item) => {
      _item.classList.remove("actived");
      _item.style.pointerEvents = "none"; //từ chối sự kiện chuột
    });
    //thêm actived
    event.target.classList.add("actived");
    //báo kết quả
    const alertDiv = document.createElement("div");
    alertDiv.classList.add("alert");
    let msg = "";
    if (result == 1) {
      msg = "You Win";
      alertDiv.classList.add("alert-success");
    } else if (result == 0) {
      msg = "You Draw";
      alertDiv.classList.add("alert-warning");
    } else {
      msg = "You Lose";
      alertDiv.classList.add("alert-danger");
    }
    alertDiv.textContent = msg;
    document.querySelector(".notification").appendChild(alertDiv);
    document.querySelector("#play-again").classList.remove("d-none"); //xóa d-none
  });
});
```
📌 **Ghi chú:**
- Khi bấm chọn, **máy ngừng chạy** (`clearInterval`).
- **Xóa class active** trước khi thêm mới.
- **Tạo thông báo kết quả** hiển thị trên giao diện.

## 6️⃣ Sự kiện chơi lại
```js
document.querySelector(".btn-play-again").addEventListener("click", (event) => {
  clearInterval(interval); //ngăn ko cho ấn play again làm tăng tốc độ lên
  //cho máy chơi lại
  interval = setInterval(handleChange, 85);
  //xóa active
  playerItem.forEach((_item) => {
    _item.classList.remove("actived");
    _item.style.pointerEvents = ""; //khôi phục sự kiện chuột
  });
  //xóa thông báo
  document.querySelector(".notification").innerHTML = "";
  //ẩn nút chơi lại
  document.querySelector("#play-again").classList.add("d-none");
});
```
📌 **Ghi chú:**
- **Ngăn lỗi** nhấn "Chơi lại" nhiều lần làm tăng tốc độ game.
- **Reset trạng thái người chơi và thông báo**.

---
📌 **Kết luận:**
- **Đây là game kéo - búa - bao cơ bản** sử dụng JavaScript.
- **Sử dụng `setInterval` để tạo hiệu ứng chọn ngẫu nhiên cho máy**.
- **So sánh giá trị để xác định thắng - thua - hòa**.
- **Xử lý sự kiện click của người chơi & chơi lại dễ dàng**.

