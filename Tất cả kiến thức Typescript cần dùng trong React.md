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

Bạn muốn mình viết lại theo kiểu **chỉ dùng array** hay **dùng union (array hoặc string)** cho tiện?