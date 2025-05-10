---
date: 2025-03-28T11:00:00
---
Related:[[JavaScript]]
Tag: #string #method #js
___

## 1. Thuộc tính của String
- **chuỗi trong js đc bọc = ''  ""**
```js
let str = "ahihi";
```
### 1.1. ` #length`
Trả về độ dài của chuỗi:
```js
let str = "ahihi";
console.log(str.length); // 5
```

## 2. Tìm kiếm trong chuỗi
### 2.1. ` #indexOf()`
Tìm vị trí đầu tiên của một ký tự hoặc chuỗi con:
```js
console.log(str.indexOf("h")); // 1
console.log(str.indexOf("ih")); // 2
console.log(str.indexOf("s")); // -1 (không tìm thấy)
```

## 3. Cắt chuỗi
### 3.1. `#slice(start, end)`
Trả về chuỗi con từ `start` đến `end - 1`:
```js
let x = "xin chào PiedTeam, mình là Điệp";
let result = x.slice(9, 17); //PiedTeam
console.log(result); //PiedTeam
console.log(x); //ko thay đổi
```
> [!important] 
> string là immutable: object có method ko làm thay đổi object mà chỉ trả ra 1 object mới sau xử lý
   string có nhiều method nhưng ko làm thay đổi nó mà chỉ tạo kết quả mới và hứng nó 
   
#### Cắt theo chiều ngược:
```js
console.log(x.slice(-22, -14)); // "PiedTeam"
```

#### Cắt với một tham số:
```js
console.log(x.slice(9));  // "PiedTeam, mình là Điệp"
console.log(x.slice(-12)); // "mình là Điệp"
```

### 3.2. `#substring(start, end)`
Giống `slice`, nhưng không hỗ trợ index âm:
```js
console.log(x.substring(9, 17)); // "PiedTeam"
```

### 3.3. `#substr(start, length)`
Trả về chuỗi con từ `start` với độ dài `length`:
```js
x = "xin chào PiedTeam, mình là Điệp";
result = x.substr(9, 8); //PiedTeam
console.log(result);
```

## 4. Thay thế chuỗi
### 4.1. `#replace()`
Chỉ thay thế lần xuất hiện đầu tiên:
```js
let str1 = "PiedTeam có nhiều bạn rất nhiều tiền";
str1 = str1.replace("nhiều", "ít"); //"PiedTeam có ít bạn rất nhiều tiền"
console.log(str1);
```

### 4.2. `#replaceAll()`
Thay thế tất cả các lần xuất hiện:
```js
str1 = "PiedTeam có nhiều bạn rất nhiều tiền";
str1 = str1.replaceAll("nhiều", "ít");
console.log(str1); //"PiedTeam có ít bạn rất ít tiền"
```

### 4.3. `replace()` + Regex
```js
str1 = "PiedTeam có nhiều bạn rất nhiều tiền";
str1 = str1.replace(/nhiều/g, "ít");
console.log(str1); //"PiedTeam có ít bạn rất ít tiền"
```

## 5. Chuyển đổi chữ hoa - chữ thường
```js
console.log("hello".toUpperCase()); // "HELLO"
console.log("HELLO".toLowerCase()); // "hello"
```

## 6. Nối chuỗi (`#concat()` và `+`)
```js
let str1 = "xin chào";
let str2 = "PiedTeam";
let str3 = str1.concat(" ", "các bạn đến với", " ", str2);
console.log(str3);

// Dùng dấu + cho nhanh
console.log(str1 + " " + "các bạn đến với" + " " + str2);
```

## 7. Xóa khoảng trắng
### 7.1. `#trim()`
Xóa khoảng trắng ở hai đầu chuỗi:
```js
let str1 = "   xin   chào   các   bạn   ";
console.log(str1.trim()); // "xin   chào   các   bạn"
```

### 7.2. Xóa khoảng trắng ở giữa chuỗi
#### Cách 1: Dùng `#replace()` + regex
```js
str1 = "   xin   chào   các   bạn   ";
str1 = str1.replace(/\s+/g, " ").trim();
console.log(str1);
```

#### Cách 2: Dùng `#split()`, `#filter()` và `#join()`
```js
str1 = "   xin   chào   các   bạn   ";
str1 = str1
  .split(" ") //băm
  .filter((item) => item != "") //lọc
  .join("-"); //nối
console.log(str1);
```

## 8. So sánh chuỗi
- == : So sánh giá trị
- === : So sánh cả kiểu dữ liệu và giá trị

## 9. Lấy ký tự trong chuỗi
### 9.1. `#charAt(index)`
Trả về ký tự tại vị trí `index`:
```js
let x = "Lê Mười Điệp";
console.log(x.charAt(3)); // "M"
```

### 9.2. Dùng dấu `[]` để truy cập ký tự
```js
console.log(x[3]); // "M"
```

### 9.3. Chuỗi là **immutable**
Chuỗi không thể thay đổi giá trị bằng cách gán trực tiếp:
```js
x[3] = "L";
console.log(x); // "Lê Mười Điệp" (không thay đổi)
```

<mark style="background: #D2B3FFA6;">vì string là immutable nên có làm gì thì string cũng ko thay đổi</mark>

