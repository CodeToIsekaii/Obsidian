---
date: 2025-10-15T11:42:00
---
Related : [[]]
Tag: #
___
# Giải thích chi tiết & ví dụ (từ cơ bản → nâng cao) — #map, #filter, #reduce (và khi kết hợp)

Mục tiêu: bạn có thể hiểu sâu từng hàm, biết các biến thể/edge-case, thấy cách dùng trong React / functional JS

---

# 1) `Array.prototype.map()`

## 🔎 Ý nghĩa ngắn gọn

#map lặp qua từng phần tử của mảng, chạy một hàm chuyển đổi trên mỗi phần tử và trả về **một mảng mới** cùng độ dài (mỗi phần tử là kết quả của hàm chuyển đổi).

## Cú pháp

```js
const newArr = arr.map((element, index, array) => {
  // trả về giá trị mới cho phần tử tương ứng
});
```

- `element`: phần tử hiện tại.
    
- `index`: chỉ số.
    
- `array`: mảng gốc.
    

## Ví dụ cơ bản

```js
const nums = [1, 2, 3];
const doubled = nums.map(n => n * 2); // [2, 4, 6]
```

## Ví dụ React (thực tế)

```jsx
const names = ["An","Bảo","Huy"];
return (
  <ul>
    {names.map((name, i) => <li key={i}>{name}</li>)}
  </ul>
);
```

## Ví dụ nâng cao

- Chuyển mảng object thành mảng khác:
    

```js
const users = [{id:1,name:'A'},{id:2,name:'B'}];
const names = users.map(u => u.name); // ['A','B']
```

- Map trả về component phức tạp:
    

```jsx
users.map(u => <UserCard key={u.id} user={u} />)
```

## Lưu ý & lỗi thường gặp

- `map` luôn trả về mảng cùng chiều — không dùng để “lọc” (dùng `filter`).
    
- Trong React: luôn cung cấp `key` duy nhất nếu map ra JSX list.
    
- Nếu hàm `map` không return (ví dụ dùng `{}` mà quên `return`), kết quả là mảng `undefined` hoặc `null`.
    

## Bài tập thực hành ngắn

- Dùng `map` để biến mảng số thành chuỗi `"#1: 1", "#2: 2", ..."`
    
- Trong React: render bảng `<table>` từ mảng object users.
    

---

# 2) `Array.prototype.filter()`

## 🔎 Ý nghĩa ngắn gọn

#filter lặp mảng, chạy hàm kiểm tra (predicate) cho mỗi phần tử, và trả về **mảng mới chỉ chứa các phần tử thỏa điều kiện**.

## Cú pháp

```js
const filtered = arr.filter((element, index, array) => booleanExpression);
```

## Ví dụ cơ bản

```js
const nums = [1,2,3,4,5];
const evens = nums.filter(n => n % 2 === 0); // [2,4]
```

## Ví dụ nâng cao

- Lọc object theo thuộc tính:
    

```js
const products = [{id:1,price:50},{id:2,price:200}];
const cheap = products.filter(p => p.price < 100);
```

- Lọc unique (loại bỏ trùng):
    

```js
const arr = [1,2,2,3];
const unique = arr.filter((v,i,self) => self.indexOf(v) === i);
```

## Lưu ý & lỗi thường gặp

- `filter` trả về mảng rỗng nếu không phần tử thỏa điều kiện.
    
- Predicate phải trả về boolean; nếu trả về giá trị truthy/falsy JS vẫn chấp nhận.
    
- `filter` không thay đổi mảng gốc (trong trường hợp predicate thuần).
    

## Bài tập

- Lọc danh sách student chỉ lấy `age >= 18`.
    
- Viết hàm `filterByQuery(arr, q)` trả các phần tử có chuỗi chứa `q` (case-insensitive).
    

---

# 3) `Array.prototype.reduce()`

## 🔎 Ý nghĩa ngắn gọn

#reduce lặp mảng và **gộp/tính toán** thành **một giá trị duy nhất** (có thể là số, object, mảng, v.v). Dùng accumulator (biến tích lũy) để lưu kết quả giữa các vòng lặp.

## Cú pháp

```js
const result = arr.reduce((accumulator, currentValue, index, array) => {
  // trả về accumulator mới
}, initialValue);
```

- `initialValue` là giá trị khởi tạo của accumulator; nếu bỏ, accumulator mặc định là phần tử đầu tiên và vòng chạy bắt đầu từ phần tử thứ hai.
    

## Ví dụ cơ bản (tổng)

```js
const nums = [1,2,3,4];
const sum = nums.reduce((acc, n) => acc + n, 0); // 10
```

### Bước chạy (để giảng):

- Lần 1: acc = 0, n = 1 → acc = 1
    
- Lần 2: acc = 1, n = 2 → acc = 3
    
- ...
    

## Ví dụ nâng cao

- Đếm occurrences:
    

```js
const arr = ['a','b','a'];
const counts = arr.reduce((acc, x) => {
  acc[x] = (acc[x] || 0) + 1;
  return acc;
}, {}); // {a:2, b:1}
```

- Flatten mảng 1 cấp:
    

```js
const nested = [[1,2],[3,4]];
const flat = nested.reduce((acc, arr) => acc.concat(arr), []); // [1,2,3,4]
```

- Nhóm theo thuộc tính:
    

```js
const people = [{age:21},{age:30},{age:21}];
const group = people.reduce((acc,p) => {
  acc[p.age] = acc[p.age] || [];
  acc[p.age].push(p);
  return acc;
}, {});
```

## Giá trị khởi tạo (initialValue)

- Nếu `initialValue = 0` → acc bắt đầu ở 0.
    
- Nếu `initialValue = 1` → acc bắt đầu ở 1 → tổng cuối cùng tăng 1 (ví dụ).
    
- Nếu không truyền `initialValue` → acc = arr[0], vòng bắt đầu từ arr[1]. Cẩn thận với mảng rỗng (sẽ lỗi).
    

## Lưu ý & lỗi thường gặp

- Không truyền `initialValue` trên mảng rỗng → `TypeError`.
    
- Nếu accumulator là object/array, trả về **một object/array mới** hoặc mutate? Thường nên **trả về giá trị mới** (immutability) để an toàn, nhất là trong React/functional programming.
    
- `reduce` có thể khó đọc nếu logic phức tạp — tốt khi tách thành hàm nhỏ.
    

## Bài tập

- Dùng `reduce` tính tổng giá trị `price` trong giỏ hàng.
    
- Viết `groupBy(arr, fn)` dùng `reduce` để nhóm phần tử.
    

---

# 4) Kết hợp `map`, `filter`, `reduce` — Patterns & ví dụ thực tế

## Pattern 1 — Lọc → Chuyển đổi → Tổng hợp

Ví dụ: tổng giá tiền các sản phẩm đang có trong kho

```js
const products = [
  {id:1,price:100,inStock:true},
  {id:2,price:200,inStock:false},
  {id:3,price:50,inStock:true}
];

const total = products
  .filter(p => p.inStock)         // [p1, p3]
  .map(p => p.price)              // [100, 50]
  .reduce((s, v) => s + v, 0);    // 150
```

## Pattern 2 — map + filter (chỉ map những gì cần)

Bạn có thể `filter` trước rồi `map`, hoặc trong `map` trả `null`/`undefined` cho phần tử không cần rồi `filter(Boolean)` để loại bỏ — nhưng `filter` trước thường rõ ràng hơn.

## Pattern 3 — chaining với performance consideration

Chuỗi nhiều phương thức tạo nhiều mảng tạm; nếu mảng lớn, cân nhắc:

- Dùng `reduce` để làm tất cả trong một lần duyệt.
    

```js
const total = products.reduce((acc, p) => {
  if (!p.inStock) return acc;
  return acc + p.price;
}, 0);
```

→ 1 lần duyệt, không tạo mảng tạm.

## Pattern 4 — dùng `map` để tạo component list, `filter` để chọn hiển thị

Trong React:

```jsx
items
  .filter(i => i.visible)
  .map(i => <Item key={i.id} {...i} />)
```
Rất tốt 👏 — đoạn bạn hỏi:

```jsx
items
  .filter(i => i.visible)
  .map(i => <Item key={i.id} {...i} />)
```

là **một cách rất phổ biến trong React** để **lọc và hiển thị danh sách component**.  
Giờ mình sẽ **giải thích từng phần cực kỳ chi tiết**, kèm ví dụ minh họa dễ hiểu nhé 👇

---

## 🧩 Cấu trúc tổng thể

Đây là **2 hàm mảng nối chuỗi (chaining)**:

1️⃣ `.filter()` → **lọc dữ liệu**, chỉ lấy những phần tử thỏa điều kiện  
2️⃣ `.map()` → **hiển thị (render)** từng phần tử đó bằng JSX

---

## ⚙️ Giả sử ta có dữ liệu:

```js
const items = [
  { id: 1, name: "Táo", visible: true },
  { id: 2, name: "Cam", visible: false },
  { id: 3, name: "Xoài", visible: true },
];
```

---

## 📘 Phân tích từng phần của biểu thức:

### 1️⃣ `items.filter(i => i.visible)`

- `.filter()` lặp qua toàn bộ mảng `items`
    
- Với mỗi phần tử `i`, nó kiểm tra điều kiện `i.visible`  
    (nếu `true` thì giữ lại, nếu `false` thì bỏ đi)
    

👉 Kết quả sau filter:

```js
[
  { id: 1, name: "Táo", visible: true },
  { id: 3, name: "Xoài", visible: true }
]
```

---

### 2️⃣ `.map(i => <Item key={i.id} {...i} />)`

- `.map()` sẽ **biến đổi mỗi object còn lại** thành **một component React `<Item />`**
    
- Dấu `{...i}` là **spread props** → nghĩa là truyền toàn bộ thuộc tính của `i` vào component.  
    Ví dụ `i = { id:1, name:"Táo", visible:true }`  
    thì `<Item {...i} />` tương đương với:
    
    ```jsx
    <Item id={1} name="Táo" visible={true} />
    ```
    
- `key={i.id}` là **bắt buộc trong React** khi render list, giúp React nhận diện từng component riêng biệt để tối ưu cập nhật.
    

---

### ✅ Kết quả cuối cùng (trong React):

Biểu thức trên sẽ render ra **2 component** tương ứng với các phần tử `visible: true`.

Ví dụ component `Item` của bạn là:

```jsx
function Item({ name }) {
  return <p>{name}</p>;
}
```

Thì React sẽ hiển thị:

```
Táo
Xoài
```

---

## 🧠 Tóm tắt ý nghĩa từng phần:

|Phần|Ý nghĩa|Kết quả trung gian|
|---|---|---|
|`.filter(i => i.visible)`|Lọc những item có `visible = true`|Mảng rút gọn|
|`.map(i => <Item ... />)`|Tạo component JSX từ từng item|Mảng các `<Item />`|
|`key={i.id}`|Giúp React tối ưu render list|Không ảnh hưởng hiển thị|
|`{...i}`|Spread toàn bộ thuộc tính vào component|`Item` nhận props tự động|

---

## 💡 Ví dụ đầy đủ bạn có thể chạy thử:

```jsx
function Item({ id, name }) {
  return <li>{id}. {name}</li>;
}

export default function App() {
  const items = [
    { id: 1, name: "Táo", visible: true },
    { id: 2, name: "Cam", visible: false },
    { id: 3, name: "Xoài", visible: true },
  ];

  return (
    <ul>
      {items
        .filter(i => i.visible)
        .map(i => <Item key={i.id} {...i} />)}
    </ul>
  );
}
```

➡️ Kết quả trên giao diện:

```
1. Táo
2. Xoài
```

---

## 🔁 Tại sao phải nối `.filter()` rồi `.map()`?

Nếu bạn viết tách ra, cũng được:

```jsx
const visibleItems = items.filter(i => i.visible);
const list = visibleItems.map(i => <Item key={i.id} {...i} />);
return <ul>{list}</ul>;
```

Còn viết liền:

```jsx
return (
  <ul>
    {items
      .filter(i => i.visible)
      .map(i => <Item key={i.id} {...i} />)}
  </ul>
);
```

👉 Hai cách hoàn toàn giống nhau — cách viết liền **ngắn hơn, sạch hơn**, và thường dùng trong JSX.

---

## 🚀 Gợi ý để bạn giảng lại dễ hiểu:

- Nói `.filter()` là “**bộ lọc dữ liệu**”, chỉ giữ phần tử hợp lệ.
    
- `.map()` là “**bộ chuyển đổi**, biến từng phần tử thành component hiển thị.”
    
- Trong React, 2 thằng này thường **đi cặp** để xử lý danh sách.
    

---

Bạn có muốn mình vẽ **sơ đồ luồng chạy (flow chart)** thể hiện rõ dữ liệu ban đầu → sau `.filter()` → sau `.map()` → hiển thị ra màn hình không?  
Nó giúp bạn giảng lại cực dễ hiểu.
---

# 5) Các ví dụ nâng cao & trick hữu dụng

## A. Avoid mutation (immutable pattern)

Không làm:

```js
arr.map(x => { x.v *= 2; return x; }) // mutate object gốc — nguy hiểm
```

Thay bằng:

```js
arr.map(x => ({ ...x, v: x.v * 2 }));
```
Tuyệt vời 👏 — câu hỏi này cực kỳ hay, vì nó chạm đúng vào **lỗi nguy hiểm nhất khi dùng `.map()` trong React hoặc JS nói chung**:  
👉 **“Mutation” — làm thay đổi trực tiếp dữ liệu gốc.**

Giờ mình sẽ giải thích **rõ ràng**: lỗi này là gì, **tại sao nó nguy hiểm**, **khi nào gặp phải**, và **cách khắc phục đúng** nhé 👇

---

## 🧩 1. Lỗi “Mutation” nghĩa là gì?

**Mutation (đột biến)** tức là **bạn thay đổi trực tiếp dữ liệu gốc trong bộ nhớ**, thay vì tạo một bản sao mới.

Trong JavaScript, **object và array là kiểu tham chiếu (reference type)** → nghĩa là khi bạn gán `let b = a`, thì `b` và `a` cùng trỏ về **một vùng nhớ**.

---

## ⚠️ 2. Ví dụ lỗi mutation khi dùng `.map()`

Giả sử bạn có:

```js
const arr = [{ v: 1 }, { v: 2 }, { v: 3 }];

const newArr = arr.map(x => {
  x.v *= 2;     // ❌ THAY ĐỔI trực tiếp object gốc
  return x;
});

console.log("arr:", arr);
console.log("newArr:", newArr);
```

### 💥 Kết quả:

```
arr: [ { v: 2 }, { v: 4 }, { v: 6 } ]
newArr: [ { v: 2 }, { v: 4 }, { v: 6 } ]
```

➡️ Cả `arr` **và** `newArr` đều bị đổi giá trị!  
Mặc dù bạn tưởng `map()` tạo ra mảng mới, nhưng **các phần tử bên trong vẫn là cùng object** — nên thay đổi một cái là thay đổi cả hai.

---

## 😱 3. Vì sao lỗi này nguy hiểm?

### Trong **JavaScript thuần**:

- Có thể gây sai logic, vì bạn tưởng dữ liệu cũ vẫn giữ nguyên.
    
- Dữ liệu bị “nhiễm” (thay đổi ngầm) → debug khó.
    

### Trong **React**:

Cực kỳ nguy hiểm, vì React **dựa vào cơ chế so sánh (shallow compare)** để biết khi nào cần re-render.

Nếu bạn **mutate state** (thay đổi trực tiếp mảng/object), React **sẽ không nhận ra sự thay đổi** → UI không cập nhật, hoặc cập nhật sai.

---

## 🧠 4. Ví dụ lỗi trong React

```jsx
function App() {
  const [users, setUsers] = useState([
    { id: 1, name: "Bảo" },
    { id: 2, name: "Trang" },
  ]);

  const updateNames = () => {
    users.map(u => {
      u.name = u.name.toUpperCase(); // ❌ mutate
      return u;
    });
    setUsers(users); // ⚠️ React thấy state cũ và mới cùng tham chiếu → không re-render
  };

  return (
    <>
      <button onClick={updateNames}>Chuyển in hoa</button>
      {users.map(u => (
        <p key={u.id}>{u.name}</p>
      ))}
    </>
  );
}
```

### ❌ Khi click:

Không có gì thay đổi trên UI, dù dữ liệu trong state **đã bị đổi**.  
React không render lại vì nó thấy `users` cũ === `users` mới (cùng vùng nhớ).

---

## ✅ 5. Cách sửa đúng (immutable pattern)

Thay vì thay đổi trực tiếp object, bạn **tạo bản sao mới**.

```js
const newArr = arr.map(x => ({ ...x, v: x.v * 2 }));
```

### Kết quả:

```
arr: [ { v: 1 }, { v: 2 }, { v: 3 } ]
newArr: [ { v: 2 }, { v: 4 }, { v: 6 } ]
```

→ Mỗi phần tử trong `newArr` là **một object mới độc lập**.

---

## 🧠 6. Nguyên tắc nhớ nhanh:

> ❌ Không bao giờ sửa trực tiếp object/array từ state  
> ✅ Luôn **tạo bản sao mới** trước khi sửa

Ví dụ trong React:

```jsx
setUsers(users.map(u => (
  u.id === 2 ? { ...u, name: "Trang VIP" } : u
)));
```

→ Không mutate `users`, mà tạo state mới hoàn toàn → React nhận ra thay đổi → UI cập nhật đúng.

---

## 🧩 7. Tóm tắt bảng so sánh

|Cách làm|Hành vi|Hậu quả|Cách đúng|
|---|---|---|---|
|`x.v *= 2`|Thay đổi object gốc|UI không re-render, dữ liệu sai|Dùng `{ ...x, v: x.v * 2 }`|
|`arr.push(...)`|Thay đổi mảng gốc|React không nhận ra thay đổi|Dùng `[...arr, newItem]`|
|`arr.splice()`|Thay đổi mảng gốc|Sai logic khi render|Dùng `.filter()` hoặc `.slice()`|

---

## 🧩 8. Khi nào bạn gặp lỗi này trong thực tế

|Tình huống|Dấu hiệu|
|---|---|
|Update state trong React bằng `.map()` mà không clone object|UI không cập nhật|
|Dữ liệu bị đổi chỗ sau khi log ra console|Dữ liệu bị “nhiễm”|
|Dùng `.push()` hoặc `.splice()` trong Redux/React state|State bị mutate|
|Object cũ & mới có cùng reference (`===`)|React bỏ qua render|

---

## ✅ 9. Tổng kết: Cách nhớ dễ

> - `.map()` = tạo **mảng mới**, nhưng **phần tử bên trong vẫn là tham chiếu cũ**
>     
> - Muốn “mới hoàn toàn” → phải **clone từng phần tử**
>     
> - Trong React: luôn dùng **immutable update pattern**
>     

---

Nếu bạn muốn, mình có thể **minh họa trực quan bằng biểu đồ vùng nhớ (memory reference)**:

- Một bản vẽ cho thấy `arr` và `newArr` trỏ cùng hoặc khác vùng nhớ  
    → Giúp bạn giảng lại cực dễ cho người học.
    

Bạn có muốn mình vẽ sơ đồ đó cho phần “mutation” này không?
## B. Map với async (chú ý)

`map` không hiểu `async` — nếu bạn dùng `async` hàm trong `map`, kết quả là mảng Promise:

```js
const promises = arr.map(async x => await fetchData(x));
const results = await Promise.all(promises);
```

## C. Short-circuit trong `map` (khi không muốn render một vài phần tử)

Trong React, thay vì `map` trả `null`:

```jsx
arr.map(x => condition ? <Comp/> : null)
```

Hoặc filter trước.

## D. reduceRight

Có `reduceRight` để gộp từ phải sang trái — hiếm dùng, nhưng cần khi thứ tự là yếu tố.

---

# 6) Phân tích độ phức tạp & hiệu năng

- `map`, `filter`, `reduce` mỗi hàm đều O(n) thời gian (một lần duyệt).
    
- Chaining `filter().map().reduce()` = nhiều lần duyệt (tương ứng số hàm). Đối với mảng lớn, cân nhắc hợp gộp bằng `reduce` duy nhất.
    
- Memory: mỗi `map`/`filter` tạo mảng mới → tốn bộ nhớ tạm.
    

---

# 7) Các lỗi phổ biến khi dạy/giảng

- Quên `return` trong `map` khi dùng `{ }` thay vì `()` → `undefined`.
    
- Trong React: quên `key` hoặc dùng `index` trong list có thể gây re-render bất lợi.
    
- Không truyền `initialValue` cho `reduce` khi mảng có thể rỗng → lỗi.
    
- Mutating original array/object dẫn đến bug khó debug.
    

---

# 8) Checklist giảng bài — slide / demo bạn nên trình bày

1. Giải thích ý nghĩa ngắn gọn của từng hàm.
    
2. Live-coding ví dụ cơ bản (console.log show).
    
3. Giải thích step-by-step (đặc biệt cho `reduce`: show acc/n thay đổi từng bước).
    
4. Demo React: render list bằng `map`.
    
5. Cho ví dụ kết hợp (filter→map→reduce) và biến thể dùng `reduce` 1-pass.
    
6. Nêu lỗi phổ biến và cách tránh.
    
7. Kết thúc bằng bài tập thực hành + đáp án.
    

---

# 9) Bài tập (để bạn giao & đáp án)

## Bài 1 (cơ bản)

**Yêu cầu:** Từ mảng `['a','b','a','c']` tạo object đếm occurrences.  
**Gợi ý:** dùng `reduce`.  
**Đáp án:**

```js
const arr = ['a','b','a','c'];
const counts = arr.reduce((acc, x) => {
  acc[x] = (acc[x] || 0) + 1;
  return acc;
}, {});
// {a:2, b:1, c:1}
```

## Bài 2 (trung bình)

**Yêu cầu:** Tính tổng `price` của các sản phẩm `inStock === true` trong một lần duyệt (không dùng `.filter().map().reduce()`).  
**Đáp án:**

```js
const total = products.reduce((acc, p) => p.inStock ? acc + p.price : acc, 0);
```

## Bài 3 (ứng dụng React)

**Yêu cầu:** Render danh sách tasks & nút “Xóa” cho mỗi task (sử dụng `map`). Khi click Xóa, remove item khỏi state.  
**Đáp án (ngắn):**

```jsx
function Todo() {
  const [tasks, setTasks] = useState(['a','b','c']);
  return (
    <ul>
      {tasks.map((t,i) => (
        <li key={i}>
          {t} <button onClick={() => setTasks(tasks.filter((_,j) => j !== i))}>Xóa</button>
        </li>
      ))}
    </ul>
  );
}
```

## Bài 4 (nâng cao)

**Yêu cầu:** Flatten mảng 2 cấp và nhóm theo thuộc tính `type`, trả về object `{type1:[...], type2:[...]}`.  
**Đáp án:**

```js
const nested = [[{type:'a',v:1}], [{type:'b',v:2}, {type:'a',v:3}]];
const flat = nested.reduce((acc, arr) => acc.concat(arr), []);
const grouped = flat.reduce((acc, item) => {
  (acc[item.type] = acc[item.type] || []).push(item);
  return acc;
}, {});
```

---

# 10) Tips ngắn để bạn giảng dễ hiểu

- Dùng analogies:
    
    - `map` = “băng chuyền biến đổi từng món”
        
    - `filter` = “bộ lọc, chỉ cho món thỏa điều kiện đi qua”
        
    - `reduce` = “máy ép, bỏ tất cả vào và thu được 1 kết quả”
        
- Show step-by-step bằng bảng (acc / current) cho `reduce`.
    
- Luôn demo console logs để học viên thấy giá trị trung gian.
    
- Khuyến khích viết hàm nhỏ, đặt tên rõ ràng (`toPrice`, `isInStock`) để `reduce`/`map` dễ đọc.
    

---

Nếu bạn muốn, mình có thể:

- Soạn **slides ngắn** (markdown) để bạn dùng dạy (có ví dụ + bước chạy `reduce`).
    
- Tạo **project mẫu** (code runnable) chứa các ví dụ + small UI để minh hoạ — và đóng gói **.zip** để bạn tải về.
    

Bạn muốn mình tạo **slides** hay **project .zip** trước?

