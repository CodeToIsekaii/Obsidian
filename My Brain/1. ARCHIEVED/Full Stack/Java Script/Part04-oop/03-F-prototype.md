---
date: 2025-04-11T22:45:00
---
Related : [[JavaScript]]
Tag: #js #f-prototype
___

## 🚗 Function Constructor & Prototype trong JavaScript

### 🧱 1. Tạo Object bằng Constructor Function (Không cần `class`)
- trong js người ta thích dùng function hơn class
- bên java nếu muốn tạo 1 object(bức tượng), em phải thông qua class(cái khuôn) > constructor(phễu)
- bên js: ta có constructor độc lập tức là 1 function dùng để tạo object (mà ko cần class bao bọc nó)

```js
//constructor hay function Car cũng đc
function Car(name, price) { //thằng này là constructor tạo ra chiếc xe, tao sẽ lấy thằng này tạo ra chiếc xe luôn ko cần class bọc
  this.name = name;//vì có hoisting nên tự tạo method cũng vậy tự bay ra ngoài tạo
  this.price = price;
  // thuộc tính ẩn  prototype:class Car(class chứa nó)
}
```

> Hàm `Car` đóng vai trò như **constructor**. Dùng `new` để tạo ra object mới.
```js
//đây là hình dạng thật sau khi có class
class Car{ 
     khai báo prop
    constructor(name, price) {
        this.name = name;
        this, (price = price);
      }
     khai báo method
}
```
### 🧪 Ví dụ

```js
let audi = new Car("Audi", "2 tỷ");
console.log(audi);
// {
//   name: "Audi",
//   price: "2 tỷ",
//   [[Prototype]]: prototype của Function Car => class Car
// }
```

---

### 🏗️ 2. Thay đổi `prototype` của Constructor Function

```js
let factory = {
  date: "2023",
};

Car.prototype = factory;

let rollRoyce = new Car("rollRoyce", "1 tỷ 2");//new cái nào là đang sử dụng constructor của cái đó để tạo ra chiếc xe
console.log(rollRoyce);
// rollRoyce{
//   name: "rollRoyce",
//   price: "1 tỷ 2",
//   [[Prototype]]: factory (không còn là Car.prototype ban đầu) không ảnh hưởng đến thằng audi
//   date:"2023",
// }
```

> ⚠️ Việc **gán lại `Car.prototype` sẽ chỉ ảnh hưởng đến các object được tạo **sau khi gán**, như `rollRoyce`. Object trước đó như `audi` **không bị ảnh hưởng**.
> =>js ko đảm bảo đúng constructor mà ta cần, nếu như ta chủ động thay thế prototype của constructor
---

### 🧠 Ghi nhớ: `Function.prototype`

> [!NOTE] Note
>F.prototype mặc định là thuộc tính của constructor function

| Mô tả | Ghi chú |
|------|--------|
| Mỗi function trong JS đều có thuộc tính `prototype` | 👉 Là **mặc định**, kể cả khi bạn không gán gì |
| `prototype` mặc định là một object | 👉 Có thuộc tính `constructor` trỏ về chính function đó |

---

### 🐾 Ví dụ với `Animal` Constructor

```js
function Animal(name) {
  this.name = name;
    //   prototype=class Animal{
  //     constructor(name){//comstructor cũng chính là function Animal
  //         this.name=name
  //         prototype=class Animal{...Animal
  //         }
  //     }
  //   }
}

console.log(Animal.prototype); // class Animal👉 { constructor: Animal }
console.log(Animal.prototype.constructor === Animal); // 👉 true
```

---

### 🔄 `constructor` vs `__proto__`

```js
let dog = new Animal("Milo");

console.log(dog.__proto__);                   // 👉 Animal.prototype  //class Animal
console.log(dog.__proto__.constructor);       // 👉 function Animal
console.log(dog.constructor);                 // 👉 function Animal
console.log(dog.constructor == Animal);      // 👉 true
console.log(dog.__proto__.constructor== Animal); // 👉 true
```
- vẫn truy cập vào constructor, nó sẽ đi vô các lớp nó tìm nên vẫn sẽ true
> Object không có `prototype`, chỉ có `[[Prototype]]` (truy cập qua `__proto__`).
> Còn function thì có `prototype`.

---

### 🧬 Tạo object giống hệt thông qua `constructor`
- 1 object có constructor mà constructor là cái tạo ra object,
- từ đây js sản sinh ra 1 cách tạo object mới nó ra đường thấy con nào đẹp nó kêu con đó đưa cho nó cái constructor để nó tạo ra 1 con giống y chang con đó 
```js
let chihuahau = new dog.constructor();
console.log(chihuahau);
```

> 💡 JavaScript có thể **dùng constructor của một object để tạo ra object mới cùng loại**. Cách này gọi là **"cloning theo constructor"**.

---

### ✅ Tổng kết

| Kiến thức | Ý nghĩa |
|----------|--------|
| `function` có `prototype` | Dùng để kế thừa các phương thức, thuộc tính |
| Object tạo bằng `new` | Có `[[Prototype]]` trỏ về `Function.prototype` |
| `constructor` | Dùng để xác định hàm tạo ra object đó |
| Có thể gán lại `prototype` | Nhưng làm vậy sẽ cắt đứt mối liên hệ `constructor` gốc |


