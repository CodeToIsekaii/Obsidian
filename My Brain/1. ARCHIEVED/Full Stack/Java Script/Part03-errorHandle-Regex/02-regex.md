---
date: 2025-04-02T13:32:00
---
Related:[[JavaScript]]
Tag: #js #regex 
___

# Regular Expression (Regex) trong JavaScript

## 1. Regex là gì?
- Regex (Regular Expression) hay biểu thức chính quy là một mẫu dùng để tìm kiếm hoặc thao tác chuỗi.
- Nó giống như `LIKE` trong SQL nhưng mạnh mẽ hơn.
- Trong JavaScript, regex là một **object**.
> [!caution] 
> js thì mình dùng method  .test() |thay vì matches() java 
## 2. Cách sử dụng regex trong JavaScript
### Tạo regex
```js
let regex1 = /nam/; // tìm chuỗi chứa "nam"
let str = "Điệp is my name";
console.log(regex1.test(str)); // true
```

### Các flag thường dùng
- `i`: **ignore case** (không phân biệt chữ hoa, chữ thường) <mark style="background: #FFF3A3A6;">trong java thì ? ở đầu</mark>
- `g`: **global** (tìm tất cả kết quả thay vì chỉ 1)
- `m`: **multiline** (xử lý trên nhiều dòng)

```js
let regex1 = /Nam/i;
console.log(regex1.test("Điệp is my name")); // true
```

### Các phương thức dùng với regex
```js
console.log(regex1.exec(str)); // ['nam', index: 11, input: 'Điệp is my name', groups: undefined]
console.log(str.match(regex1)); // ['nam', index: 11, input: 'Điệp is my name', groups: undefined]
console.log(str.search(regex1)); // 11

// Thay thế chuỗi
let regex2 = /Điệp/gi;
let newStr = "Điệp is my name, my name is Điệp".replace(regex2, "Tuấn");
console.log(newStr); // "Tuấn is my name, my name is Tuấn"
```

---
## 3. Metacharacters (Ký tự đặc biệt)
| Ký tự  | Ý nghĩa |
|--------|---------|
| `^` | Bắt đầu chuỗi (`/^hello/i` → "hello world" ✅, "world hello" ❌) |
| `$` | Kết thúc chuỗi (`/world$/i` → "hello world" ✅, "world hello" ❌) |
| `.` | Đại diện cho 1 ký tự bất kỳ ngoại trừ xuống dòng (`/m.y/i` → "may", "moy" ✅, "my" ❌) |
| `*` | Lặp lại ký tự trước từ 0 đến nhiều lần (`/m*y/i` → "my", "mmy" ✅) |
| `+` | Lặp lại ký tự trước từ 1 đến nhiều lần (`/m+y/i` → "my", "mmy" ✅, "y" ❌) |
| `?` | Ký tự trước có hoặc không (`/ma?y?h?o?r?n?y/i` → "mary", "mhoany" ✅) |
| `\` | Dùng để **escape** ký tự đặc biệt (`/\.com/` → khớp với `.com`) |

---
## 4. Character Sets và Quantifiers
### Nhóm ký tự `[ ]`
- `[abc]`: Chứa một trong các ký tự `a`, `b`, `c`.
- `[^abc]`: Không chứa `a`, `b`, `c`.
- `[a-z]`, `[A-Z]`, `[a-zA-Z]` , `[0-9]`: Phạm vi chữ hoặc số.

```js
console.log(/m[an]/i.test("ma")); // true
console.log(/m[^an]/i.test("mo")); // true
```

### Giới hạn số lượng ký tự `{}`
```js
console.log(/me{2}t/.test("meet")); // true
console.log(/me{2,5}t/.test("meeeet")); //  met false   meet|meeet|meeeet|meeeeet true
console.log(/me{0,5}t/.test("meeeet")); //  mt | met |  meet|meeet|meeeet|meeeeet true
console.log(/me{2,}t/.test("meeeeeet")); // true
```

### Nhóm ký tự `()`
```js
console.log(/(me){2,}t/.test("mememememet")); // true
```

### Hoặc `|`
```js
console.log(/(Hồ|Lê) Điệp/.test("Hồ Điệp")); // true
console.log(/(Hồ|Lê) Điệp/.test("Lê Điệp")); // true
```

---
## 5. Shorthand Character Classes

|Ký tự|Ý nghĩa|
|---|---|
|`\w`|Chữ cái, số hoặc `_` (`/Die\w/` → "Die1", "Die_", "Dien" ✅)|
|`\w+`|Một hoặc nhiều chữ cái, số hoặc `_` (`/Die\w+/` → "Die123", "Die_test" ✅)|
|`\W`|Không phải chữ cái, số hoặc `_`|
|`\d`|Số (`/\d/` → "1" ✅)|
|`\d+`|Một hoặc nhiều số (`/\d+/` → "123", "4567" ✅)|
|`\D`|Không phải số|
|`\s`|Khoảng trắng|
|`\S`|Không phải khoảng trắng|

---
## 6. Lookahead & Lookbehind
- **Positive Lookahead (`?=`)**: Khớp nếu **có** chuỗi theo sau.
- **Negative Lookahead (`?!`)**: Khớp nếu **không có** chuỗi theo sau.

```js
console.log(/a(?=b)/.test("ab")); // true (khớp "a" nếu có "b" sau)
console.log(/a(?!b)/.test("ac")); // true (khớp "a" nếu không có "b" sau)
```

---
## 7. Word Boundaries `\b`
- `\b` xác định ranh giới từ.
- `an\b` khớp với "an" trong "an" hoặc "an tốt", nhưng không khớp "anh trai".

```js
console.log(/an\b/.test("an")); // true
console.log(/an\b/.test("an tốt")); // true
console.log(/an\b/.test("anh trai")); // false
```

---
## 8. Test Regex Online
- **Trang web hữu ích**: [https://regexr.com/](https://regexr.com/)

---
### Tổng kết
- Regex là công cụ mạnh mẽ giúp tìm kiếm và thay thế chuỗi.
- Nắm vững các ký tự đặc biệt (`^`, `$`, `.`) và nhóm ký tự (`[]`, `{}`, `()`).
- Hiểu về **Lookahead, Lookbehind** để tìm kiếm nâng cao.
- Test trên **regexr.com** để dễ dàng kiểm tra biểu thức chính quy.





