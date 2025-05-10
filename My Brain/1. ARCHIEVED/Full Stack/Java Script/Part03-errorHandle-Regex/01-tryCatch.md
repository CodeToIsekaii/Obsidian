---
date: 2025-04-02T12:27:00
---
Related:[[JavaScript]]
Tag: #errorhandle #trycatch
___

# Xử lý lỗi trong JavaScript
vì làm backend nên phải xử lý lỗi rất nhiều
- làm cho lỗi tường minh dễ nhìn chứ ko nhìn déo bít lỗi gì
- giấu đi những thông tin nhạy cảm 
## 1. Phân loại lỗi trong JavaScript

- **`RuntimeError`**: Lỗi xảy ra trong quá trình vận hành (do người dùng).
- **`SyntaxError`**: Lỗi sai cấu trúc (do người lập trình).
- **`LogicError`**: Lỗi sai tư duy (do lỗi trong cách nghĩ, không phải do code sai cú pháp).do bạn ngu chứ lỗi éo gì

### Chú ý về `try...catch`
- **`try...catch`** chỉ xử lý được lỗi trong **runtime**. Nếu có **`syntaxError`** thì code sẽ không chạy được, và không có cơ hội để bắt lỗi. (tại lỗi ròi đâu đợi đâu mà bắt)
- **`try...catch`** chỉ hoạt động trong môi trường đồng bộ.

### Ví dụ sử dụng `try...catch`:

```javascript
try {
  diepPiedTeam;  // Đây là lỗi tham chiếu không hợp lệ.
} catch (err) {  // Lỗi sẽ được bắt và xử lý tại đây. //trong pham vi này lỗi sẽ đc tạo thành object lỗi để ném ra ngoài
  console.log(err);  // In ra thông tin lỗi.
}
```

### Lưu ý với bất đồng bộ (`setTimeout`):

### 1. Đoạn mã đầu tiên:

```javascript
try {
  setTimeout(() => {
    diepPieadTeam;  // Đây là lỗi tham chiếu không hợp lệ.
  }, 1000);          // setTimeout sẽ chạy sau 1000ms.
} catch (err) {
  console.log(err);  // Lỗi sẽ không được bắt ở đây.
}
```

- **Lý do không bắt được lỗi**:
  - `setTimeout` là một hàm bất đồng bộ (asynchronous), có nghĩa là nó không chạy ngay lập tức mà sẽ được thực thi sau một khoảng thời gian (ở đây là 1000ms, tương đương 1 giây).
  - Trong JavaScript, `try...catch` chỉ hoạt động đối với các lỗi trong môi trường đồng bộ. Nếu một lỗi xảy ra trong một hàm bất đồng bộ như `setTimeout`, thì lỗi đó sẽ không được bắt bởi `try...catch` trong phạm vi gọi hàm.
  - Mặc dù lỗi phát sinh từ việc truy cập biến `diepPieadTeam` không hợp lệ, nhưng vì `setTimeout` chạy bất đồng bộ, lỗi này sẽ không được bắt bởi `try...catch` trong block chính, vì đoạn mã này đã kết thúc trước khi `setTimeout` thực thi.

### 2. Đoạn mã thứ hai:

```javascript
setTimeout(() => {
  try {
    diepPieadTeam;  // Đây là lỗi tham chiếu không hợp lệ.
  } catch (err) {
    console.log(err);  // Lỗi sẽ được bắt và xử lý ở đây.
    console.log(err.name);  // Tên của lỗi (ví dụ: ReferenceError).
    console.log(err.message);  // Thông điệp lỗi (ví dụ: "diepPieadTeam is not defined").
    console.log(err.stack);  // Thông tin stack trace đầy đủ.
  }
}, 1000);
```

- **Lý do bắt được lỗi**:
  - Trong đoạn mã này, `setTimeout` vẫn được sử dụng để chạy mã bất đồng bộ, nhưng `try...catch` lại nằm **bên trong** callback của `setTimeout`.
  - Khi lỗi xảy ra trong callback của `setTimeout` (trong trường hợp này là lỗi tham chiếu vì `diepPieadTeam` không được định nghĩa), lỗi sẽ được bắt bởi `try...catch` nằm trong phạm vi callback bất đồng bộ.
  - Khi đó, lỗi sẽ được ném ra trong callback của `setTimeout` và sẽ được bắt và xử lý trong `catch` block, giúp bạn có thể kiểm tra các thuộc tính của lỗi như `name`, `message`, và `stack`.

### Tóm lại:
- **Đoạn mã đầu tiên** không bắt được lỗi vì `try...catch` chỉ hoạt động trong phạm vi đồng bộ, và lỗi trong `setTimeout` là bất đồng bộ, nên không được bắt ở đó.
- **Đoạn mã thứ hai** bắt được lỗi vì `try...catch` nằm trong callback của `setTimeout`, giúp bắt lỗi khi nó xảy ra trong phạm vi bất đồng bộ.
## 2. Cấu trúc lỗi trong JavaScript

Khi phát sinh lỗi, JavaScript sẽ tạo ra một **lỗi** dưới dạng một object có các thuộc tính như sau:

- **`name`**: Kiểu của lỗi (ví dụ: `ReferenceError`, `TypeError`, ...).
- **`message`**: Thông tin chi tiết về lỗi.
- **`stack`**: Chi tiết về stack trace (dòng mã gây ra lỗi). (full thông tin kèm dòng phát sinh lỗi)
> [!attention] 
> stack là prop mà mình không muốn người dùng nhìn thấy nhất  

### **Flow 1: Omit `stack`**
Trong trường hợp này, bạn không muốn cho phép người dùng nhìn thấy thông tin chi tiết về lỗi (`stack`). Cấu trúc lỗi sẽ chỉ chứa `name` và `message`, không có `stack` để bảo mật hoặc đơn giản hóa thông tin.

```javascript
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = "CustomError";
    // Không cung cấp stack để giấu thông tin chi tiết về lỗi
    delete this.stack;
  }
}

try {
  throw new CustomError("Đây là lỗi tùy chỉnh!");
} catch (err) {
  console.log(err.name);    // CustomError
  console.log(err.message); // Đây là lỗi tùy chỉnh!
  console.log(err.stack);   // undefined (vì đã xóa stack)
}
```

### **Flow 2: Custom Error với thêm thuộc tính**
Trong trường hợp này, bạn mở rộng `Error` để tạo ra lỗi tùy chỉnh và thêm thuộc tính riêng (như `status`). Điều này giúp dễ dàng mở rộng thêm thông tin lỗi cho mục đích đặc thù của bạn.

```javascript
class ErrorWithStatus extends Error {
  constructor(message, status) {
    super(message);
    this.name = "ErrorWithStatus";
    this.status = status; // Thêm thuộc tính status
  }
}

try {
  throw new ErrorWithStatus("Đã có lỗi xảy ra", 500);
} catch (err) {
  console.log(err.name);    // ErrorWithStatus
  console.log(err.message); // Đã có lỗi xảy ra
  console.log(err.status);  // 500
  console.log(err.stack);   // Thông tin stack (nếu có)
}
```

### **Tóm lại về các flow:**
- **Flow 1**: Bạn tạo một lỗi tùy chỉnh nhưng bỏ qua thông tin `stack` khi xử lý lỗi để bảo mật hoặc đơn giản hóa việc xuất thông tin.
- **Flow 2**: Bạn tạo lỗi tùy chỉnh và thêm các thuộc tính riêng (ví dụ như `status`) để dễ dàng theo dõi hoặc xử lý trong các tình huống phức tạp hơn.

### Ví dụ tạo lỗi với `throw`:
```javascript
let money = 99999999999999999;
try {
  if (money >= 99999999999999999) {
    throw new RangeError("Lỗi chà bá nè");  // Tạo lỗi rõ ràng cho tình huống này.
  }
  console.log(money);
} catch (err) {
  console.log(err);  // In ra lỗi.
}
```

## 3. Các loại lỗi đặc biệt trong JavaScript

Ngoài `throw new Error()`, chúng ta còn có 7 hàm (constructor function) khác phục vụ cho việc **tường minh lỗi** của mình hơn:

- **`EvalError()`**: Tạo 1 instance đại diện cho một lỗi xảy ra liên quan đến hàm toàn cục **`Eval()`**.
- **`InternalError()`**: Tạo 1 instance đại diện cho một lỗi xảy ra khi 1 lỗi bên trong **JavaScript Engine** được ném. Ví dụ: Quá nhiều đệ quy.
- **`RangeError()`**: Tạo 1 instance đại diện cho một lỗi xảy ra khi một biến số hoặc tham chiếu nằm ngoài phạm vi hợp lệ của nó.
- **`ReferenceError`**: Tạo 1 instance đại diện cho một lỗi xảy ra khi hủy tham chiếu của 1 tham chiếu không hợp lệ.
- **`SyntaxError`**: Tạo 1 instance đại diện cho một lỗi xảy ra trong khi phân tích cú pháp mã trong **`Eval()`**.
- **`TypeError`**: Tạo 1 instance đại diện cho một lỗi xảy ra khi một biến hoặc 1 tham số có kiểu không hợp lệ.
- **`URIError`**: Tạo 1 instance đại diện cho một lỗi xảy ra khi **`encodeURI()`** hoặc **`decodeURI()`** truyền các tham số không hợp lệ.
### 4. Sử dụng `finally` trong `try...catch`

Khối `finally` luôn được thực thi sau khi `try` và `catch`, dù có lỗi hay không. Đây là nơi thích hợp để thực hiện các thao tác dọn dẹp, chẳng hạn như thay đổi trạng thái của biến.

```javascript
let loading = true;
try {
    loading = true;
    get(); // Lỗi
    loading = false;
} catch (err) {
    // Xử lý lỗi (nếu có)
} finally {
    loading = false; // Đảm bảo loading luôn được đặt lại
}
console.log(loading);
```

### 5. Tạo lỗi tùy chỉnh với `class`

Bạn có thể tạo lớp (class) riêng để tạo kiểu lỗi tùy chỉnh, kế thừa từ `Error`:

```javascript
class PiedError extends Error {
    constructor(message, student) {
        super(message); // Gọi constructor của lớp cha (Error)
        this.student = student; // Thêm thuộc tính tùy chỉnh
        this.name = "Lỗi từ PiedTeam"; // Đặt tên cho lỗi
    }
}

try {
    throw new PiedError("Ahihi", "Người học"); // Ném lỗi tùy chỉnh
} catch (err) {
      console.log(err.name);  // "Lỗi từ PiedTeam"
	  console.log(err.message);  // "Có lỗi xảy ra"
	  console.log(err.student);  // "Nguyễn Văn A"
}
```

```js
class PiedError extends Error {
  constructor({ message, student }) {
    super(message);
    this.student = student;
    this.name = "Lỗi từ PiedTeam"; // Đặt tên cho lỗi
  }
}

try {
  throw new PiedError({ student: "John Doe", message: "Mày bị ngu" });
} catch (err) {
  console.log(err); // In lỗi tùy chỉnh
}

```

```js
//chế ra 1 kiểu lỗi mới dựa trên Error
class PiedError extends Error {
  constructor({ message, student }) {
    super(message);
    this.student = student;
    this.name = "Lỗi từ PiedTeam";
  }
}

try {
  throw new Error("ahihi");
} catch (err) {
  let newError = new PiedError({ status: 401, message: "Mày bị ngu" });
  console.log(newError);
}

```