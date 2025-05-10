---
date: 2025-04-18T14:05:00
---
Related : [[JavaScript]]
Tag: #js #class #classinheritance
___

## 🧠 JavaScript Class & Kế thừa Class

> **Class** là một cú pháp "kẹo đường" (*syntactic sugar*) trong JavaScript giúp viết OOP dễ hơn. Trước khi có class, người ta dùng **constructor function** và `prototype`.

---

### I. ✅ Kiến thức cơ bản về Clas
- class là (cái khuôn)
- bên trong class có constructor(cái phễu), thuộc tính, method
- class sẽ dùng constructor để tạo ra đối tượng
#### 1. Khai báo class (class declaration)

```js
class User {
  constructor(fullName) {
    [this.firstName, this.lastName] = fullName.split("");
  }

  show() {
    console.log(`FirstName của tui là ${this.firstName}
    và lastName của tui là ${this.lastName}`);
  }
}
```

#### 2. Tạo object từ class

```js
let diep = new User("Lê Điệp");
// diep{
//     firstName:"Lê",
//     lastName:"Điệp",
//     // show(),override
//     [[Prototype]]:class User{
//         constructor,
//         show(),
//     }
// }
console.log(diep);
```

#### 3. Kiểm tra prototype và constructor

```js
console.log(diep);
console.log(diep.__proto__ == User.prototype); //true //User.prototype constructor user . prototype đc class User
console.log(typeof User); //function //khi đề cập User là constructor function
console.log(User.prototype.constructor == User); //true
```

> **Class** thật ra là một hàm constructor.

#### 4. Constructor function truyền thống

```js
function Student(fullName) {
  [this.firstName, this.lastName] = fullName.split(" ");
  // this.show = function () { này đang là thuộc tinh phải dấu nó vô prototype của Student
  //   console.log(`FirstName của tui là ${this.firstName}
  //     và lastName của tui là ${this.lastName}`);
  // };
}
Student.prototype.show = function () {
  console.log(`FirstName của tui là ${this.firstName}
      và lastName của tui là ${this.lastName}`);
};
let tuan = new Student("Phạm Tuấn"); //ko new thì chỉ là cái hàm mà hàm ko return nên giá trị nhận đc là undefined
console.log(tuan);
```

---

### II. 🔍 So sánh Class vs Constructor Function
```js
//1. constructor function ko cần toán tự new
// let hung = new User("Hùng Khùng"); //class ko thiếu new đc
let hung = Student("Hùng Khùng"); //chạy đc, nhưng
//function constructor có new thì hiểu là constructor,
//thiếu new thì hiểu là function ko có return -> undefined

//2.về hình ảnh
console.log(User); //class
console.log(Student); //hàm

//3. code bên trong class phải luôn use Strict, ko có hoisting

```

| Tiêu chí                    | Class                 | Constructor Function        |
|----------------------------|------------------------|-----------------------------|
| Có thể thiếu `new`?        | ❌ Không               | ✅ Có, nhưng kết quả là `undefined` |
| Dạng cú pháp               | `class`               | `function`                  |
| Use strict mặc định        | ✅ Có                  | ❌ Không                    |
| Hoisting                   | ❌ Không               | ✅ Có                       |

---

### III. 🧪 Class Expression

```js
let User1 = class Ahihi {
  constructor(fullName) {
    [this.firstName, this.lastName] = fullName.split("");
  }
  show() {
    console.log(`FirstName của tui là ${this.firstName}
      và lastName của tui là ${this.lastName}`);
  }
};
//Ahihi là tên gọi thân ở nhà của User1 nên chỉ xài đc ở khu vực trong class Ahihi ra ngoài chỉ xài đc User1
```

> `Ahihi` chỉ dùng được bên trong class, bên ngoài chỉ gọi `User1`.

---

### IV. 🧬 Class từ function

```js
function makeClass() {
  class Ahihi {
    constructor(fullName) {
      [this.firstName, this.lastName] = fullName.split("");
    }
    show() {
      console.log(`FirstName của tui là ${this.firstName}
            và lastName của tui là ${this.lastName}`);
    }
  }
  return Ahihi;
}

let user3 = makeClass();
```

---

### V. 🧠 Computed method name

```js
//** Compute Name[]:tên mà có công thức
class User5 {
  firstName = "Nguyễn";
  ["show" + "Name"]() {
    console.log("Hello");
  }
}

let hue = new User5();
hue.showName(); //hello
```

---

### VI. ⚠️ Cảnh giác với `this` trong class

#### Trường hợp sai:

```js
class Button {
  constructor(value) {
    this.value = value;
  }
  click() {
    console.log("giá trị là " + this.value);
  }
}

let btn = new Button("ahihi");
// btn{
//   value: "ahihi",
//   [[Prototype]]:Button.prototype=>class Button{constructor,click()}
// }
btn.click(); //giá trị là ahihi
//điều gì sẽ xảy ra nếu như dùng hàm click trong callback
// setTimeout(btn.click(), 3000); //ko đúng cách xài vì lúc này hàm này chạy luôn ròi nên ko đợi 3s
//click() hàm đang chạy ,click hàm chưa đc chạy
// setTimeout(btn.click, 3000);//giá trị là undefined vì btn ko chạy mà đưa hàm click cho settimeout và settimeout giữ hàm click này sau 3s thì window sẽ chạy hàm click thay cho btn

```

#### Cách 1: Dùng arrow function

```js
setTimeout(() => {
  //sau 3s thì chạy hàm này và hàm này làm hành động kêu btn chạy hàm click
  btn.click();
}, 3000);
//giá trị là ahihi
```

#### Cách 2: Dùng `bind`

```js
class Button1 {
  constructor(value) {
    this.value = value;
    this.click = this.click.bind(this); //độ lại hàm click// khi  bind cái this vào thì nghĩa là đang nhắc nó this là ai
  }
  click() {
    console.log("giá trị là" + this.value);
  }
}
// btn{
//   value:"ahuhu"
//   click()
//   [[Prototype]]:Button1.prototype=>class Button1{constructor,click()}
// }
btn = new Button1("ahuhu");
setTimeout(btn.click, 3000); //nên hàm click này sẽ tự động biết this là ai //bind là đưa this từ ngoài vào
```
#### Cách 3: thay thế method = arrow
```js
class Button2 {
  constructor(value) {
    this.value = value;
  }
  click = () => {
    //dùng arrow thì cái this ko bị giam mà đi bộ ra ngoài mà đi bộ ra ngoài thì biết this là ai
    console.log("giá trị là " + this.value);
  };
}
btn = new Button2("ahehe");
setTimeout(btn.click, 3000);
```

---

## VII. 🧬 Kế thừa class – `extends`

```js
class Animal {
  constructor(name) {
    this.name = name;
    this.speed = 0;
  }
  //method
  run(speed) {
    this.speed = speed;
    console.log(`${this.name} runs with speed ${this.speed}`);
  }
  //method
  stop() {
    this.speed = 0;
    console.log(`${this.name} stands still`);
  }
}
let ani = new Animal("My Animal");
/*
ani{
  name: My Animal
  speed:0
  [[Prototype]]:Animal.prototype=>class animal{
    constructor
    run()
    stop()
  }
}
*/
```

### Class con kế thừa:

```js
class Rabbit extends Animal {
  constructor(name) {
    super(name); //new Cha | new Animal
  } //ko có vẫn đúng
  hide() {
    console.log(`${this.name} hides!!`);
  }
  stop() {
    setTimeout(() => {
      super.stop();
    }, 1000);
  }
}

let yellowRabbit = new Rabbit("YellowRabbit");
yellowRabbit.hide();
yellowRabbit.run(6);
yellowRabbit.stop(); //trong yellowRabbit sẽ có 2 stop của rabbit và animal nếu yellowRabbit.stop() thì sẽ chạy stop của rabbit override
// ani.hide(); lỗi bất hiếu

// yellowRabbit{
//   name: "YellowRabbit"
//   speed: 6;
//   [[Prototype]]: Rabbit.prototype=> class Rabbit
//   class Rabbit.__proto__= Animal.prototype=>class Animal
//   class Animal.__proto__=Object.prototype=> class Object
//   class Object.__proto__=null
// }
console.log(yellowRabbit);
```

---

### VIII. ⚠️ class field: trong class field ko cho kế thừa, ko cho override,

```js
class Animal2 {
  name = "isAnimal"; //class field
  constructor() {
    console.log(this.name);
  }
} //chạy log hết :)

class Rabbit2 extends Animal2 {
  name = "isRabbit"; //class field
  constructor() {
    super();
  } //ko thích khỏi viết vì như nhau
}
```

```js
let ani2 = new Animal2(); //isAnimal
let rb2 = new Rabbit2(); //khi new cái này thì nó chưa có tạo thì nó sẽ lấy giá trị name đầu tiên của nó là isAnimal
//gọi new là đang tạo nên chưa có
//class field: ko kế thừa, ko vượt mặt
//chỉ overwrite:ghi đè nên trong object ko có  2 thằng trùng tên
```

> Class field không kế thừa mà **overwrite**. Khi `super()` chạy, `this.name` vẫn lấy giá trị từ `Animal2`.

---

## 📌 Tổng kết

- `class` là cú pháp kẹo đường cho việc tạo constructor function.
- `extends` cho phép kế thừa giữa các class.
- `super()` dùng trong constructor của class con để gọi constructor của cha.
- Cẩn thận với `this` trong callback — dùng `bind` hoặc arrow function để giữ ngữ cảnh.
- `class field` không kế thừa, mà được ghi đè trong object.

