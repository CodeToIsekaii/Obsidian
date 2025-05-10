---
date: 2025-03-25T13:06:00
---
Related:[[JavaScript]]
Tag: #js #method #object #hof #this #bind
___

## **1. Object (Đối tượng) trong JavaScript**

### **1.1 Định nghĩa**

- Object là một tập hợp các **thuộc tính (properties)** và **phương thức (methods)**.

- Tất cả những gì có thể sờ được, đếm được đều là đối tượng.

- Các đối tượng (object) có thể được miêu tả bằng **thuộc tính (properties)**.

- Các đối tượng có những hành động đặc trưng được gọi là **phương thức (methods)**.

- Các đối tượng có thể chứa các **hàm (functions)**.

- Một **function** nằm trong **object** thì được gọi là **method**, còn function nằm ngoài object thì chỉ là một **function bình thường**.

### **1.2 Tạo một object và khai báo properties & methods**

```javascript
let promotionBoy1 = {
  nickname: "Lê Mười Điệp",
  age: 25,

  // Phương thức
  sayHi() {
    console.log("Ahihi quẹo lựa quẹo lựa");
  },
  sayHi1: function () {
    console.log("Ahihi quẹo lựa quẹo lựa");
  },
  sayHi2: () => {
    console.log("Ahihi quẹo lựa quẹo lựa");
  }
};
```



Cách tạo method bằng function expression (FE) hoặc function declaration (FD) về mặt lý thuyết có sự khác biệt trên cơ sở kế thừa, nhưng sự khác biệt này rất nhỏ và không đáng kể.

Thông thường, người ta thích viết method bằng function declaration (FD) để tối ưu hóa cú pháp và dễ đọc hơn.

Function thường được viết bằng function expression (FE) hoặc function arrow (FA), tuy nhiên, method không nên sử dụng arrow function vì nó không có `this` riêng.

### **1.3 Thêm properties hoặc methods sau khi khởi tạo object**

```javascript
promotionBoy1.money = 1000;
promotionBoy1.chuiKhanh = function () {
  console.log("under the hook, ko được thì cook");
};
```

---

## **2. Xác định ********************************************************************`this`******************************************************************** trong Method**

### **2.1 ********************************************************************`this`******************************************************************** trong phương thức của object**

//(object > method > this)

```javascript
let promotionBoy2 = {
  nickname: "Lê Mười Điệp", //properties
  //method
  showName() {
    //mfd
    console.log("Nicknmae nè " + this.nickname); //this là undefined
  },
  showName1: function () {
    //mfe
    console.log("Nicknmae nè " + this.nickname); //this là undefined
  },
  showName2: () => {
    //mfa
    console.log("Nicknmae nè " + this.nickname); //this là undefined
  },
};
```

### **Kết quả khi gọi method**

```javascript
promotionBoy2.showName(); //mfd //Nicknmae nè Lê Mười Điệp
promotionBoy2.showName1(); //mfe //Nicknmae nè Lê Mười Điệp
promotionBoy2.showName2(); //mfa //Nicknmae nè undefined//this là window >window.nicname>undefined
```

### **Lưu ý về ********************************************************************`this`******************************************************************** trong method**

- `this` chỉ được xác định tại runtime, khi hàm được gọi.
- `this` trong một method trỏ đến object gọi nó.
- Không nên dùng arrow function để khai báo method vì `this` sẽ không trỏ đến object.

---

## **3. ********************************************************************` #this `******************************************************************** trong function bên trong method**

//(object > method > function > this)

```javascript
let promotionBoy5 = {
  nickname: "Lê Mười Điệp", //properties
  //method
  showName() {
    //mfd
    let arrow = () => {
      //fa
      console.log("Nickname nè " + this.nickname);
    };
    arrow();
  },

  showName2() {
    //mfd
    let expression = function () {
      //fe
      console.log("Nickname nè " + this.nickname);
    };
    expression();
  },
};
```

### **Kết quả khi gọi method**

```javascript
promotionBoy5.showName();
//promotionBoy5 gọi showName: mfd giam this và xác định this là obj gọi showName
//                  arrow:fa : dù use strict hay normal thì this vẫn đc thả đi bộ từ từ ra ngoài
//                                  và bị chặn bởi showName: mfd nên nó xác định this là obj gọi method showName
//                                              this là promotionBoy5 -> promotionBoy5.nickname
//Nickname nè Lê Mười Điệp

promotionBoy5.showName2();
//promotionBoy5 gọi showName2: mfd giam this và xác định this là obj gọi showName2
//      expression: fe
//  normal: sút this ra window   | use strict: giam this
//          this là window       |          this ko tìm đc cha -> this là undefined
// window.nickname -> undefined  |          undefined.nickname -> lỗi

//=>nếu cần xài function bên trong method thì nên xài arrow function
```

✅ **Kết luận:** Nếu cần dùng function bên trong method, nên sử dụng **arrow function** để `this` không bị thay đổi.
## 3.1  (object > method > fuction(fuction > this))
//giống y chang thoi
- <mark style="background: #BBFABBA6;">setTimeout xài callback function như đang xài ở lớp chứa nó</mark>
```js
let promotionBoy6 = {
  nickname: "Lê Mười Điệp", //properties
  //method
  showName() {
    //mfd
    let arrow = () => {
      //fa
      console.log("Nicknmae nè " + this.nickname);
    };
    setTimeout(arrow, 3000);
  },

  showName2() {
    //mfd
    let expression = function () {
      //fe
      console.log("Nicknmae nè " + this.nickname);
    };
    setTimeout(expression, 3000);
  },
};

```

## tại sao cần có #this?

```js
let promotionBoy3 = {
  nickname: "Lê Mười Điệp",
  showName() {
    //fd
    console.log("nickname: " + promotionBoy3.nickname);
  },
};
let promotionBoy4 = promotionBoy3;
promotionBoy3 = null;
```
   <mark style="background: #ADCCFFA6;">promotionBoy4.showName() //lỗi đó vì promotionBoy3 là null sao . nickname</mark>

---

## Higher Order Functions (HOF)

### 1. ** #Callback Functions**
- **Callback** là một hàm nhận vào một hàm khác như là một tham số.
- **Ví dụ**: Dưới đây là ví dụ sử dụng `forEach` để lặp qua một mảng và in các giá trị ra màn hình:

```javascript
const array1 = [1, 2, 3, 4, 5];
array1.forEach((val) => {
  console.log(val);
});
```

### 2. ** #Closure**
- **Lexical Scoping**: Hàm con có thể truy cập các biến từ hàm cha.
- **Closure**: Một hàm có thể trả về một hàm khác, và hàm trả về đó có thể "nhớ" được các biến của hàm cha.

#### Ví dụ tạo **Identity Generator**:

```javascript
const initIdentity = () => {
  let newId = 0;
  return () => ++newId;
};

//cách dùng sai
console.log(initIdentity()); //tạo newId = 0 và trả ra hàm () => ++newId;
console.log(initIdentity()()); //1
console.log(initIdentity()()); //1

//xài đúng
let demoClosure = initIdentity(); //() => ++newId;
console.log(demoClosure()); //1
console.log(demoClosure()); //2
console.log(demoClosure()); //3
```

### 3. ** #Currying**
- **Currying** là kỹ thuật biến đổi một hàm nhận nhiều tham số thành các hàm con liên tiếp, mỗi hàm nhận một tham số.

#### Ví dụ về **Currying**:
- viết 1 hàm có thể xử lý 3 bài toán sau
`tìm các số từ 0 -10 là số chẵn
`tìm các số từ 0 -20 là số lẽ
`tìm các số từ 0 -30 là số chia 3 dư 2

```javascript
let handle = (end, checkNumberFunc) => {
  let result = [];
  for (let i = 0; i <= end; i++) {
    if (checkNumberFunc(i)) result.push(i);
  }
  return result;
};
console.log(handle(10, (number) => number % 2 == 0));
handle(20, (number) => number % 2 == 1);
handle(30, (number) => number % 3 == 2);

//curring
handle = (end) => (checkNumberFunc) => {
  let result = [];
  for (let i = 0; i <= end; i++) {
    if (checkNumberFunc(i)) result.push(i);
  }
  return result;
};
handle(10)((number) => number % 2 == 0);
```

### 4. **Ứng dụng #HOF: Filter Number**
- Ví dụ áp dụng HOF để lọc các số theo điều kiện từ 0 đến một số cụ thể.

```javascript
let handle = (end, checkNumberFunc) => {
  let result = [];
  for (let i = 0; i <= end; i++) {
    if (checkNumberFunc(i)) result.push(i);
  }
  return result;
};

console.log(handle(10, (number) => number % 2 === 0)); // Số chẵn từ 0 - 10
```

---

## #Call, #Apply, #Bind

### 1. **Call**
- `call` cho phép gọi một hàm với `this` là đối tượng mà bạn chỉ định, đồng thời có thể truyền các tham số cho hàm.
<mark style="background: #FFB86CA6;">call(obj,...paremeter cũ);</mark>
```javascript
const people = {
  fullname: "ahihi", // ko lấy ahihi vì bị  call hoặc apply bẻ qua diep ròi
  print(age, location) {
    //mfd
    console.log(this.fullname + " " + age + " " + location);
  },
};
people.print(10, "TP HCM"); //undefined 10 TP HCM
//this là people -> people.fullname -> undefined

//ta có thể bẻ đường dẫn của this = cách như sau
const diep = { fullname: "Lê Mười Điệp" };
```

### 2. **Apply**
- `apply` cũng giống như `call` nhưng các tham số được truyền dưới dạng mảng.
<mark style="background: #FFB86CA6;">apply(obj, [...parameter cũ]);</mark>
```javascript
people.print.apply(diep, [10, "TP HCM"]); // Lê Mười Điệp TP HCM
```

### 3. **Bind**
- `bind` tạo ra một bản sao của hàm với `this` đã được gắn sẵn.
<mark style="background: #FFB86CA6;">bind(obj,...parameter cũ)() => closures</mark>
```javascript
let boundFunc = people.print.bind(diep);
boundFunc(10, "TP HCM"); // Lê Mười Điệp TP HCM
```
<mark style="background: #FFB86CA6;">bind(obj)(...parameter cũ) => currying</mark>
```js
people.print.bind(diep)(10, "TP HCM");
```

```js
people.print = people.print.bind(diep);
people.print(10, "TP HCM");
```
### 4. **Ứng dụng Bind trong Closure**
- **Bind** có thể kết hợp với closure để giữ `this` ổn định trong các hàm callback.

```javascript
let promotionBoy7 = {
  nickname: "Lê Mười Điệp", //properties
  //method
  showName2() {
    //mfd
    let expression = function () {
      //fe
      console.log("Nicknmae nè " + this.nickname);
    }.bind(this); // this bên ngoài xác định đc và đưa cho this bên trong đang bị giam
    setTimeout(expression, 3000);
  },
};
```

---

### Tóm Tắt:
- **Callback**: Hàm nhận vào hàm khác làm tham số.
- **Closure**: Hàm trả về một hàm khác và có thể nhớ được các biến của hàm cha.
- **Currying**: Biến đổi một hàm nhiều tham số thành các hàm con nhận một tham số.
- **Call, Apply, Bind**: Quản lý `this` trong các hàm và callback.

Dưới đây là cách trình bày đẹp và đầy đủ nội dung về `Date` trong JavaScript, phù hợp để ghi vào Obsidian:

---

## ** #Datetime trong JavaScript (Date Object)**

### **1. Thời gian trong JavaScript**
- Thời gian trong JavaScript được biểu diễn dưới dạng **object**.
- Thời gian được tính dựa trên **mili giây** (milliseconds) từ **1/1/1970** (theo chuẩn UTC).

### **2. Cách khởi tạo đối tượng `Date`**
Có 4 cách để tạo một đối tượng `Date`:

```javascript
let a = new Date(); // Khởi tạo thời gian hiện tại
a = new Date(1691849729913); // Khởi tạo từ mốc thời gian theo mili giây (epoch time)
a = new Date("2023-8-12"); // Khởi tạo từ chuỗi thời gian theo định dạng YYYY-MM-DD
a = new Date(2023, 7, 12, 21, 1, 0, 0); // Khởi tạo từ các tham số năm, tháng, ngày, giờ, phút, giây, mili giây
```

### **3. Các phương thức của đối tượng `Date`**

- **getDate()**: Lấy ngày trong tháng (1-31)
  ```javascript
  let a = new Date();
  console.log(a.getDate()); // Lấy ngày trong tháng
  ```

- **getDay()**: Lấy ngày trong tuần (0: Chủ Nhật - 6: Thứ 7)
  ```javascript
  console.log(a.getDay()); // Lấy ngày trong tuần (0 - 6)
  ```

- **getFullYear()**: Lấy năm đầy đủ (4 chữ số)
  ```javascript
  console.log(a.getFullYear()); // Lấy năm
  ```

- **getHours()**: Lấy giờ (0 - 23)
  ```javascript
  console.log(a.getHours()); // Lấy giờ trong ngày
  ```

- **getMilliseconds()**: Lấy mili giây (0 - 999)
  ```javascript
  console.log(a.getMilliseconds()); // Lấy mili giây
  ```

- **getMinutes()**: Lấy phút (0 - 59)
  ```javascript
  console.log(a.getMinutes()); // Lấy phút
  ```

- **getMonth()**: Lấy tháng (0 - 11, 0 = Tháng 1)
  ```javascript
  console.log(a.getMonth()); // Lấy tháng (0 - 11)
  ```

- **getSeconds()**: Lấy giây (0 - 59)
  ```javascript
  console.log(a.getSeconds()); // Lấy giây
  ```

- **toISOString()**: Lấy thời gian theo định dạng chuẩn ISO (YYYY-MM-DDTHH:mm:ss.sssZ)
  - Được sử dụng để lưu vào cơ sở dữ liệu hoặc trao đổi giữa các hệ thống, vì định dạng ISO là chuẩn quốc tế.
  ```javascript
  console.log(a.toISOString()); // Lấy định dạng ISO
  ```

### **4. Ví dụ**

```javascript
let a = new Date();
a = new Date(1691849729913); // Khởi tạo từ mốc mili giây
a = new Date("2023-8-12");   // Khởi tạo từ chuỗi thời gian
a = new Date(2023, 7, 12, 21, 1, 0, 0); // Khởi tạo từ các tham số năm, tháng, ngày, giờ, phút, giây, mili giây

console.log(a); // In đối tượng Date
console.log(a.toISOString()); // In thời gian theo định dạng ISO
```

### **Tóm Tắt Các Phương Thức Chính**

| Phương thức             | Mô tả                                |
|-------------------------|--------------------------------------|
| `getDate()`             | Lấy ngày trong tháng                |
| `getDay()`              | Lấy ngày trong tuần (0 - 6)         |
| `getFullYear()`         | Lấy năm đầy đủ                      |
| `getHours()`            | Lấy giờ (0 - 23)                    |
| `getMilliseconds()`     | Lấy mili giây (0 - 999)            |
| `getMinutes()`          | Lấy phút (0 - 59)                   |
| `getMonth()`            | Lấy tháng (0 - 11)                  |
| `getSeconds()`          | Lấy giây (0 - 59)                   |
| `toISOString()`         | Lấy định dạng ISO chuẩn             |

---

### **Lưu Ý**:
- **Date** trong JavaScript luôn dựa vào thời gian UTC (Coordinated Universal Time).
- Các phương thức như `getMonth()` trả về giá trị tháng bắt đầu từ 0 (tháng 0 là tháng 1, tháng 11 là tháng 12).

---
## **Tóm tắt**

- Hiểu cách sử dụng `this` trong method và function trong object.
- Sử dụng `call`, `apply`, `bind` để kiểm soát `this`.
- Hiểu về HOF, Callback, Closure, Currying.
- Làm việc với `Date` trong JavaScript.

