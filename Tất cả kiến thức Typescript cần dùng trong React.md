---
date: 2025-09-08T13:18:00
---
Related : [[React]]
Tag: #react
___
### **Object**

Trong hình bạn khai báo:

```ts
let house: {
  address: string;
  color?: string;
}
```

👉 Đây **chỉ là khai báo kiểu (type annotation)** chứ chưa gán giá trị khởi tạo cho `house`.  
Do đó, biến `house` lúc này là `undefined`. Khi bạn gọi:

```ts
house.address = "Da Nang";
```

TypeScript báo lỗi:

```
Variable 'house' is used before being assigned. ts(2454)
```

Vì `house` chưa được gán object ban đầu nào để có thể chứa `address`.

---

### Cách sửa ✅

Bạn cần khởi tạo `house` với một object rỗng (hoặc đầy đủ thuộc tính):

```ts
let house: {
  address: string;
  color?: string;
} = {
  address: "",   // khởi tạo trước
};
house.address = "Da Nang";
```

Hoặc nếu muốn khởi tạo rỗng rồi mới gán sau thì dùng optional (`?`) cho tất cả fields:

```ts
let house: {
  address?: string;
  color?: string;
} = {}; // object rỗng hợp lệ

house.address = "Da Nang";
```

---

👉 Tóm lại: lỗi này là do bạn **chỉ khai báo kiểu mà chưa gán giá trị ban đầu cho biến `house`**.

### **Array**
Trong code bạn có:

```ts
let products: any[] = [];
products.push(1);
products.push("VietNam");

products = "VietNam"; // ❌ lỗi ở đây
```

---

### Nguyên nhân 🔎

- `products` được khai báo kiểu `any[]` (mảng chứa nhiều phần tử bất kỳ).
    
- Nhưng sau đó bạn lại gán trực tiếp:
    
    ```ts
    products = "VietNam";
    ```
    
    👉 `"VietNam"` là một **string**, không phải **array**.  
    TypeScript báo lỗi:
    
    ```
    Type 'string' is not assignable to type 'any[]'. ts(2322)
    ```
    

---

### Cách sửa ✅

1. Nếu bạn muốn `products` **luôn là mảng** → chỉ được dùng `push`, không gán string:
    
    ```ts
    let products: any[] = [];
    products.push(1);
    products.push("VietNam");
    ```
    
2. Nếu bạn muốn `products` có thể là **string hoặc mảng** → dùng union type:
    
    ```ts
    let products: any[] | string = [];
    products.push(1);
    products.push("VietNam");
    
    products = "VietNam"; // hợp lệ
    ```
    

---

👉 Tóm lại: lỗi này là do bạn khai báo `products` là **array**, nhưng lại gán cho nó một **string**.

### **Function**
### 🔹 Cách 1: Khai báo function và type ngay trong phần định nghĩa

```ts
const sum = (num1: number, num2: number): number => {
  return num1 + num2;
};
```

Ở đây:

- `num1: number, num2: number`: kiểu tham số.
    
- `: number` sau dấu `)` → kiểu trả về.
    
- Tất cả đều được viết ngay trong function.
    

👉 Ưu điểm: ngắn gọn, dễ đọc.  
👉 Nhược điểm: không tái sử dụng được "chữ ký" (kiểu function).

---

### 🔹 Cách 2: Khai báo kiểu function trước (function type), rồi gán function vào biến

```ts
const sub: (num1: number, num2: number) => number = (
  num1: number,
  num2: number
) => num1 - num2;
```

Ở đây tách ra thành 2 phần:

1. **Khai báo kiểu của biến `sub`**:
    
    ```ts
    (num1: number, num2: number) => number
    ```
    
    nghĩa là "một function nhận vào 2 số, trả về 1 số".
    
2. **Gán function thực tế cho `sub`**:
    
    ```ts
    (num1, num2) => num1 - num2
    ```
    

👉 Ưu điểm:

- Khi có nhiều function cùng "chữ ký", bạn chỉ cần khai báo **1 type** rồi dùng lại.  
    Ví dụ:
    
    ```ts
    type MathOperation = (a: number, b: number) => number;
    
    const add: MathOperation = (a, b) => a + b;
    const subtract: MathOperation = (a, b) => a - b;
    const multiply: MathOperation = (a, b) => a * b;
    ```
    
    → Tất cả các function `add`, `subtract`, `multiply` đều cùng kiểu `MathOperation`.
    

👉 Nhược điểm: viết dài hơn một chút nếu chỉ dùng 1 lần.

---

✅ Tóm gọn:

- **Cách 1**: Viết gọn, dùng trực tiếp.
- **Cách 2**: Tách riêng phần **type function** → dễ tái sử dụng, code chuẩn hơn khi có nhiều hàm tương tự.


---
### **Class**
## 🔹 1. Class `Person1`

```ts
class Person1 {
  private name: string;     // chỉ dùng bên trong class
  age: number;              // mặc định là public
  readonly money: number = 40; // chỉ đọc, gán giá trị lúc tạo

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  handle() {
    let value = this.money; // đọc được
  }
}

const alex = new Person1("Alex", 27);

alex.money = 200; // ❌ lỗi: money là readonly
```

### Giải thích:

- `private name`: chỉ truy cập được trong class, bên ngoài không gọi `alex.name` được.
    
- `age`: mặc định là `public`, có thể truy cập từ ngoài class.
    
- `readonly money`: chỉ đọc, không thể gán lại sau khi khởi tạo.
    

👉 Vì vậy dòng `alex.money = 200;` sẽ báo lỗi.

---

## 🔹 2. Class `Person` (cách viết 1)

```ts
class Person {
  public name: string;
  public age: number;

  constructor(name: string, age: number) {
    this.age = age;
    this.name = name;
  }
}
```

### Giải thích:

- `public`: có thể truy cập ở mọi nơi (thật ra mặc định `public` là không cần viết cũng được).
    
- Trong constructor, bạn phải gán `this.name` và `this.age` thủ công.
    

---

## 🔹 3. Class `Person` (cách viết rút gọn)

```ts
class Person {
  constructor(public name: string, public age: number) {}
}
```

### Giải thích:

- Đây là **shorthand của TypeScript**.
    
- Khi bạn khai báo `constructor(public name: string, public age: number)`, TS sẽ:
    
    1. Tự tạo thuộc tính `name` và `age` trong class.
        
    2. Tự gán giá trị khi new object.
        

👉 Nghĩa là 2 class `Person` này **hoàn toàn giống nhau**, chỉ khác cách viết.

## 🔹 So sánh nhanh
- `private`: chỉ dùng được **trong chính class đó**, class con cũng không truy cập được.
- `protected`: dùng được trong **class đó và các class con**, nhưng không thể gọi từ bên ngoài.
## 🔹 Ví dụ minh họa

```ts
class Animal {
  private privateName = "Private";     // chỉ dùng trong Animal
  protected protectedName = "Protected"; // dùng được trong Animal + class con

  public sayHello() {
    console.log("Hello, I am an Animal");
    console.log(this.privateName);   // ✅ OK
    console.log(this.protectedName); // ✅ OK
  }
}

class Dog extends Animal {
  public showNames() {
    // console.log(this.privateName);  // ❌ Lỗi: private chỉ có Animal dùng được
    console.log(this.protectedName);   // ✅ OK: protected dùng trong class con
  }
}

const d = new Dog();

d.sayHello();   // OK
d.showNames();  // OK

// ❌ Lỗi: gọi trực tiếp từ ngoài class thì không được
// console.log(d.protectedName);
// console.log(d.privateName);
```

## ✅ Tóm gọn

- **`private`** → kín nhất, chỉ trong chính class đó.
- **`protected`** → cho phép **class con** kế thừa sử dụng, nhưng bên ngoài không truy cập được.
- **`public`** → ai cũng dùng được (mặc định).
- `readonly`: chỉ đọc, không gán lại sau khi khởi tạo.
- `constructor(public name: string, ...)` → cách viết **ngắn gọn** để vừa khai báo vừa gán.

---

## 🔹 1. Union (`|`)

```ts
let price: string | number | boolean;
price = "10";
price = 20;
price = false;
```

- `string | number | boolean` nghĩa là `price` có thể nhận **nhiều kiểu khác nhau**.
    
- Union rất hữu ích khi dữ liệu có thể thay đổi nhiều dạng (ví dụ: từ API).
    

Ví dụ phức tạp hơn:

```ts
let body: { name: string | number } | { firstName: string } = {
  name: 100,
};
```

- Ở đây `body` có thể là:
    
    - `{ name: string | number }`, hoặc
        
    - `{ firstName: string }`.
        

---

## 🔹 2. Enum

```ts
enum Sizes {
  S = "S",
  M = "M",
  L = "L",
  XL = "XL",
}

let size = Sizes.S;
```

- `enum` tạo ra một tập hằng số có tên → giúp code **dễ đọc hơn** thay vì dùng string tự do.
    nếu không để giá trị thì nó tự động thêm số bắt đầu từ ko từ trên xuống
- `size` chỉ có thể nhận `"S" | "M" | "L" | "XL"`.
    

Ví dụ:

```ts
function getDiscount(size: Sizes) {
  if (size === Sizes.L) return 10;
  return 0;
}
```

---

## 🔹 3. Interface

```ts
// interface State {
//   name: string
//   isLoading: boolean
// }

// interface State {
//   age: number
// }

// let state: State = {
//   name: 'Dang',
//   isLoading: false,
//   age: 100
// }
```

- Điểm hay của `interface`: **có thể khai báo nhiều lần và tự động gộp lại** (declaration merging).
    
- Trong ví dụ trên, `State` sẽ có:
    
    ```ts
    { name: string; isLoading: boolean; age: number }
    ```
    

---

⚠️ Nhưng `interface` **không dùng được Union trực tiếp**:

```ts
// ❌ Sai
// interface Person = Name | Age
```

---

## 🔹 4. Type

```ts
// type State = {
//   name: string
//   isLoading: boolean
// }
```

- `type` thì **không gộp được** như `interface`, nhưng có thể dùng **Union** hoặc **Intersection** rất linh hoạt.
    

Ví dụ:

```ts
type Name = { name: string };
type Age = { age: number };

type Person = Name | Age;   // ✅ được
type PersonFull = Name & Age; // ✅ được
```

👉 Đây là lý do bạn thấy:

- `interface` hợp để **mô tả object/struct**,
    
- `type` hợp để **xây dựng kiểu linh hoạt, union, intersection**.
    

---

## 🔹 5. Generic function

```ts
const handleClick = <Type>(value: Type) => value;

let value = 100;
handleClick<string>("100");
```

- `<Type>` là **generic type parameter** (tham số kiểu).
    
- Nó cho phép bạn viết hàm mà **chưa cần biết trước kiểu dữ liệu**.
    
- Khi gọi:
    
    - `handleClick<string>("100")` → ép `Type` thành `string`.
        
    - `handleClick(100)` → TS tự suy luận `Type = number`.
        

👉 Giúp hàm **linh hoạt** nhưng vẫn **an toàn kiểu**.

---

## ✅ Tóm tắt

- **Union (`|`)** → biến có thể nhận nhiều loại giá trị.
- **Enum** → tập hằng số có tên, dễ đọc, tránh “magic string”.
- **Interface** → mô tả object, có thể mở rộng/gộp.
- **Type** → mô tả kiểu, mạnh khi cần union/intersection.
- **Generic** → viết hàm/class có thể dùng cho nhiều kiểu khác nhau.
