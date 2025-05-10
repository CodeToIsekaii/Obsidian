---
date: 2025-04-03T00:03:00
---
Related:[[JavaScript]]
Tag: #project #validform
___

[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part03-errorHandle-Regex\03-validFormPrj\index.html)

# JavaScript Form Validation - Giải Thích Luồng Hoạt Động

## 1. Quy Tắc Validation

Chúng ta có một số quy tắc để kiểm tra dữ liệu nhập vào:
- **Email**: Bắt buộc, phải là email hợp lệ.
- **Name**: Bắt buộc, có thể là tiếng Anh hoặc tiếng Việt, tối đa 50 ký tự.
- **Gender**: Bắt buộc.
- **Country**: Bắt buộc.
- **Password**: Bắt buộc, tối thiểu 8 ký tự, tối đa 30 ký tự.
- **Confirmed Password**: Bắt buộc, tối thiểu 8 ký tự, tối đa 30 ký tự, phải khớp với password.
- **Agree (Checkbox đồng ý)**: Bắt buộc.

## 2. Biểu Thức Chính Quy (Regex) Cho Email Và Name

```javascript
const REG_EMAIL =
  /^[a-zA-Z\d\.\-_]+(\+\d+)?@[a-zA-Z\d\.\-_]{1,65}\.[a-zA-Z]{1,5}$/;
const REG_NAME =
  /^[a-zA-Z\u00C0-\u024F\u1E00-\u1EFF]+((\s[a-zA-Z\u00C0-\u024F\u1E00-\u1EFF]+)+)?$/;
```
- **REG_EMAIL**: Kiểm tra email hợp lệ.
- **REG_NAME**: Chấp nhận tên có chứa cả ký tự tiếng Việt.

## 3. Các Hàm Kiểm Tra Giá Trị

```javascript
const isRequired = (value) => (value !== "" ? "" : "That field is required");
const isEmail = (value) => (REG_EMAIL.test(value) ? "" : "Email is invalid");
const isName = (value) => (REG_NAME.test(value) ? "" : "Name is invalid");
const min = (num) => (value) => value.length >= num ? "" : `Min is ${num}`;
const max = (num) => (value) => value.length <= num ? "" : `Max is ${num}`;
const isSame = (paramValue, fieldName1, fieldName2) => (value) =>
  paramValue == value ? "" : `${fieldName1} ko khớp ${fieldName2}`;
```
- **isRequired**: Kiểm tra xem giá trị có bị bỏ trống không.
- **isEmail**: Kiểm tra định dạng email.
- **isName**: Kiểm tra định dạng tên.
- **min(num)**: Đảm bảo độ dài tối thiểu.
- **max(num)**: Đảm bảo độ dài tối đa.
- **isSame(paramValue, fieldName1, fieldName2)**: Kiểm tra hai giá trị có khớp nhau không (ví dụ: Password và Confirmed Password).

## 4. Hàm Hiển Thị Thông Báo Lỗi

```javascript
const createMsg = (parentNode, controlNodes, msg) => {
  let invalidDiv = document.createElement("div");
  invalidDiv.className = "invalid-feedback";
  invalidDiv.innerHTML = msg;
  parentNode.appendChild(invalidDiv);
  controlNodes.forEach((inputNode) => {
    inputNode.classList.add("is-invalid");
  });
};
```
- **createMsg** tạo một thông báo lỗi trong **parentNode** và thêm lớp CSS `is-invalid` cho các input có lỗi.

## 5. Hàm Kiểm Tra Giá Trị Và Gán Lỗi

```javascript
const isValid = (paramOject) => {
  let { value, funcs, parentNode, controlNodes } = paramOject;
  for (const funcCheck of funcs) {
    let msg = funcCheck(value);
    if (msg != "") {
      createMsg(parentNode, controlNodes, msg);
      return msg;
    }
  }
  return "";
};
```
- **isValid** duyệt qua danh sách các hàm kiểm tra (**funcs**), nếu có lỗi thì hiển thị thông báo bằng `createMsg`.
Hàm `isValid` có nhiệm vụ kiểm tra giá trị nhập vào dựa trên các quy tắc đã định nghĩa trước đó. Dưới đây là cách nó hoạt động:
### 🔍 Luồng Hoạt Động của `isValid`

1. **Nhận vào một đối tượng `paramOject`** chứa các thông tin cần kiểm tra:
   - `value`: Giá trị nhập vào của input.
   - `funcs`: Danh sách các hàm kiểm tra (validation functions).
   - `parentNode`: Phần tử cha của input để có thể chèn thông báo lỗi.
   - `controlNodes`: Danh sách các input cần thêm class lỗi.

2. **Duyệt qua từng hàm kiểm tra trong `funcs`**:
   - Mỗi hàm kiểm tra sẽ được gọi với giá trị `value`.
   - Nếu `value` không thỏa mãn quy tắc kiểm tra, hàm sẽ trả về một chuỗi báo lỗi.

3. **Xử lý khi có lỗi**:
   - Nếu bất kỳ hàm nào trả về một chuỗi lỗi (`msg` khác `""`), hàm sẽ:
     1. Gọi `createMsg(parentNode, controlNodes, msg)` để hiển thị lỗi trên giao diện.
     2. Trả về `msg` để báo hiệu input này không hợp lệ.

4. **Trả về chuỗi rỗng nếu hợp lệ**:
   - Nếu tất cả các kiểm tra đều không báo lỗi, hàm trả về `""`, nghĩa là giá trị nhập vào hợp lệ.

#### 🔗 Ví dụ Minh Họa

Giả sử chúng ta kiểm tra trường `password`:
```javascript
isValid({
  value: "12345",
  funcs: [isRequired, min(8), max(30)],
  parentNode: passwordNode.parentElement,
  controlNodes: [passwordNode],
});
```
- `isRequired("12345")` → Hợp lệ, trả về `""`
- `min(8)("12345")` → Không hợp lệ, trả về `"Min is 8"`
- **Hàm `isValid` dừng ngay tại đây và hiển thị lỗi `"Min is 8"` lên giao diện.**

⚡ **Lưu ý**: Hàm sẽ dừng ngay khi gặp lỗi đầu tiên, không chạy tiếp các kiểm tra còn lại.

Bạn có cần giải thích thêm chỗ nào không? 🚀
`funcCheck` là một biến đại diện cho từng hàm kiểm tra (validation function) bên trong vòng lặp `for ... of`. 
### 🛠 Cách Hoạt Động:
Dòng code này:
```javascript
for (const funcCheck of funcs) {
```
có nghĩa là: **lặp qua từng hàm trong mảng `funcs`**, và `funcCheck` sẽ đại diện cho từng hàm trong mỗi vòng lặp.

Ví dụ, nếu `funcs` có các hàm kiểm tra như sau:
```javascript
[isRequired, isEmail]
```
thì vòng lặp sẽ chạy hai lần:
1. `funcCheck = isRequired`
2. `funcCheck = isEmail`

Sau đó, mỗi `funcCheck` được gọi với `value`:
```javascript
let msg = funcCheck(value);
```
- Nếu `isRequired(value)` trả về chuỗi lỗi, ta dừng lại và hiển thị lỗi.
- Nếu `isRequired(value)` không có lỗi, tiếp tục kiểm tra với `isEmail(value)`.

💡 **Tóm lại:** `funcCheck` chính là một trong các hàm validation (`isRequired`, `isEmail`, `min(8)`, v.v.), được gọi tuần tự để kiểm tra `value`.



```javascript
let { value, funcs, parentNode, controlNodes } = paramOject;
```

là **destructuring assignment** trong JavaScript. Nó có tác dụng **trích xuất** các thuộc tính `value`, `funcs`, `parentNode` và `controlNodes` từ đối tượng `paramOject` và gán chúng vào các biến cùng tên.

### 🛠 Cách hoạt động:
- `paramOject` là một object chứa thông tin về input cần kiểm tra.
- Dòng code trên giúp truy cập trực tiếp các thuộc tính của `paramOject` mà không cần phải viết dài dòng như:
  ```javascript
  let value = paramOject.value;
  let funcs = paramOject.funcs;
  let parentNode = paramOject.parentNode;
  let controlNodes = paramOject.controlNodes;
  ```

### 🔍 Ví dụ:

Giả sử ta có object:
```javascript
let paramOject = {
  value: "test@example.com",
  funcs: [isRequired, isEmail],
  parentNode: document.querySelector("#email").parentElement,
  controlNodes: [document.querySelector("#email")]
};
```
Khi chạy:
```javascript
let { value, funcs, parentNode, controlNodes } = paramOject;
```
Thì các biến sẽ có giá trị:
- `value = "test@example.com"`
- `funcs = [isRequired, isEmail]`
- `parentNode = <div class="form-group">...</div>` (cha của input email)
- `controlNodes = [<input type="email" id="email">]`

💡 **Mục đích**: Giúp code gọn hơn, dễ đọc và truy xuất nhanh các giá trị từ object `paramOject`.
## 6. Hàm Xóa Lỗi

```javascript
const clearMsg = () => {
  document.querySelectorAll(".is-invalid").forEach((inputNode) => {
    inputNode.classList.remove("is-invalid");
  });
  document.querySelectorAll(".invalid-feedback").forEach((divMsg) => {
    divMsg.remove();
  });
};
```
- **clearMsg** xóa tất cả thông báo lỗi và các lớp CSS `is-invalid`.

## 7. Xử Lý Sự Kiện Khi Submit Form

```javascript
document.querySelector("form").addEventListener("submit", (event) => {
  event.preventDefault(); //chặn reset trang
  clearMsg(); //xóa các thông báo lỗi
  let emailNode = document.querySelector("#email"); //.value là tới giá trị của ô input
  let nameNode = document.querySelector("#name");
  let genderNode = document.querySelector("#gender");

  let countryNode = document.querySelector("input[name='country']:checked"); //có thể người dùng ko chọn, ta bị null
  let passwordNode = document.querySelector("#password");
  let confirmedPasswordNode = document.querySelector("#confirmedPassword");
  let agreeNode = document.querySelector("input#agree:checked");

  //xử lý
  const errorMsg = [
    //email
    isValid({
      value: emailNode.value,
      funcs: [isRequired, isEmail],
      parentNode: emailNode.parentElement,
      controlNodes: [emailNode],
    }),
    //name
    isValid({
      value: nameNode.value,
      funcs: [isRequired, isName, max(50)],
      parentNode: nameNode.parentElement,
      controlNodes: [nameNode],
    }),
    //gender
    isValid({
      value: genderNode.value,
      funcs: [isRequired],
      parentNode: genderNode.parentElement,
      controlNodes: [genderNode],
    }),
    // password
    isValid({
      value: passwordNode.value,
      funcs: [isRequired, min(8), max(30)],
      parentNode: passwordNode.parentElement,
      controlNodes: [passwordNode],
    }),
    //confirmedPassword
    isValid({
      value: confirmedPasswordNode.value,
      funcs: [
        isRequired,
        min(8),
        max(30),
        isSame(passwordNode.value, "Password", "confirmed-password"),
      ],
      parentNode: confirmedPasswordNode.parentElement,
      controlNodes: [confirmedPasswordNode],
    }),
    //country
    isValid({
      value: countryNode ? countryNode.value : "",
      funcs: [isRequired],
      parentNode: document.querySelector(".form-check-country").parentElement,
      controlNodes: document.querySelectorAll("input[name='country']"), //all là mảng
    }),
    //agree
    isValid({
      value: agreeNode ? agreeNode.value : "",
      funcs: [isRequired],
      parentNode: document.querySelector("input#agree").parentElement,
      controlNodes: [document.querySelector("input#agree")],
    }),
  ];
  const isValidForm = errorMsg.every((item) => !item); //chuỗi rỗng là false
  if (isValidForm) {
    clearMsg();
    alert("Form is Valid");
  }
});
```
- **clearMsg()**: Xóa lỗi trước khi kiểm tra.
- **errorMsg**: Lưu danh sách lỗi của từng input.
- **isValidForm**: Kiểm tra xem tất cả các trường có hợp lệ không.
- Nếu **hợp lệ**, thông báo thành công; nếu **không**, hiển thị lỗi tương ứng.

---

## 8. Luồng Chạy Của Form Validation
1. Khi submit, gọi **clearMsg()** để xóa lỗi cũ.
2. Duyệt qua từng input, kiểm tra bằng **isValid()**.
3. Nếu có lỗi, dùng **createMsg()** để hiển thị lỗi.
4. Nếu tất cả trường hợp lệ, hiển thị thông báo thành công.

