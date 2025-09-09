---
date: 2025-09-08T13:18:00
---
Related : [[React]]
Tag: #react
___
### Object

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

### Array
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

### Function
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
    