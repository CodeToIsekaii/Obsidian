---
date: 2025-04-22T14:50:00
---
Related : [[JavaScript]]
Tag: #js #promise 
___

# 🧠 Ghi chú: JavaScript Promise Cơ Bản

## 🔰 Khái niệm Promise

- **Promise** là một *lời hứa* rằng một việc gì đó sẽ xảy ra trong tương lai.
- Được giới thiệu từ **ES6**.
- **Promise là eager (chạy liền khi được tạo), không lazy** như function (function chỉ chạy khi được gọi).

---

## 📌 3 trạng thái của Promise:
- vd: lời hứa: anh sẽ đi vũng tàu anh mua cho em bánh bông lan trứng muối vào tháng 10

| Trạng thái     | Diễn giải thực tế                          | Miêu tả                |
|----------------|--------------------------------------------|------------------------|
| `pending`      | Đang chờ kết quả                           | Lời hứa đang chờ xử lý |
| `fulfilled`    | Thành công: `resolve("1 nụ hôn")`          | Lời hứa được thực hiện |
| `rejected`     | Thất bại: `reject("1 sự thất vọng")`       | Lời hứa bị thất hứa    |

---
#### <mark style="background: #FFB86CA6;">cú pháp Promise</mark>
- new Promise((resolve, reject) => {});
- new Promise(function (resolve, reject) {});
## 🧪 Ví dụ 1: Anh hứa mua cà rá

```js
//tạo bối cảnh: chúa tạo
let wallet = 1000;

//anh trai đó hứa với cô gái đó
//"anh sẽ mua cho em chiếc cà rá 5000$"
let p1 = new Promise((resolve, reject) => {
  if (wallet >= 5000) {
    resolve("1 nụ hôn");
  } else {
    reject("1 sự thất vọng");
  }
});
```

### ✅ Cách 1: Xử lý lỗi chung bằng `.catch()` ưu tiên hơn

```js
p1.then((value) => {
  console.log("Nếu tôi ở đay cso nghĩa là lời hứa thành công rồi");
  console.log("ảnh sẽ có " + value);
}).catch((error) => {      //có nhiều then nhưng có 1 catch để lỗi là dồn về cuối xử lý 1 lần
  console.log("code mà lọt vào đây là thằng chó đó ko có tiền");       //mô hình handler và controller xử lý về middlewhere ồn lỗi về 1 error handler
  console.log("và thứ mà nó nhận đc là " + error);
});
//resolve thì vào then reject vào catch
```

### ✅ Cách 2: Xử lý lỗi riêng biệt từng bước
- cứ mỗi 1 bước có lỗi là xử lý lỗi, sau này xử lý cái đó xong  .then tiếp xử lý cái tiếp theo ,thằng này thay thế callback m làm việc này xong  đợi làm việc khác, mỗi lần then là mỗi lần làm việc khác
```js
p1.then((value) => {
  console.log("Nếu tôi ở đay cso nghĩa là lời hứa thành công rồi");
  console.log("ảnh sẽ có " + value);
}, (error) => {      //làm thế này là chia lỗi theo từng bước từng bước mỗi bước có chỗ xử lý lỗi riêng
  console.log("code mà lọt vào đây là thằng chó đó ko có tiền");
  console.log("và thứ mà nó nhận đc là " + error);
});
```

---

## ⏳ Ví dụ 2: Xử lý bất đồng bộ với `setTimeout`
- thử chuyển 1 async callback thành promise
```js
let data;

//server mất 3 giây để lấy dữ liệu => data undefined
setTimeout(() => {
  data = { name: "Điệp", age: 24 };
}, 3000);
console.log(data);
```

### ✅ Dùng Promise để xử lý:

```js
//server ơi hứa với mình rằng sau 3 giây thì đưa cho mình data nhé
let p2 = new Promise ((resolve, reject) => {  //nhược điểm là vừa tạo  thì code chạy luôn
  setTimeout(() => {
    resolve({ name: "Điệp", age: 24 });
  }, 3000); sau 3s resolve
})();
//kiểm chứng
p2 (mày cứ pending đi sau 3s tao biết là onFulfilled lun).then((value) => {
  DataTransfer.value;
  console.log("Data nè " + data);
}); //chỉ có khu vực này đợi data bên ngoài ko đợi
//từ 0s đến 3s là pending
//nếu sau đó resolve thì em sẽ có trạng thái onFulfilled
//nếu reject thì em sẽ có onReject
```

---

## ⚠️ Promise là eager: Tạo ra là chạy liền
- Promise sẽ chạy code bên trong ngay khi vừa khai báo ko giống hàm
- còn hàm thì khai báo chưa chạy, gọi mới chạy
```js
//vd: cách cùi:
let a = 1;
let p3 = new Promise((resolve,reject)=>{//tạo là chạy luôn
    a++;
})
p3.then((value)=>{  //ko resolve reject ,pending  hoài luôn ko xác định đc trạng thái
 console.log(value)
})
console.log(a); chạy lun ra 2
```

### ✅ Kiểm soát khi nào Promise chạy bằng function

```js
//cách 2 : hơi cùi: dùng function
function ahihi(params) {
  let p3 = new Promise((resolve, reject) => {
    a++;
  });
  return p3;
}
ahihi().then();  //kiểm soát đc khi nào đc quyền chạy khi nào ko
console.log(a);//2
```

### ✅ Viết gọn hơn với arrow function

```js
//cách 3: arrow
let p3 = () =>
  new Promise((resolve, reject) => {
    a++;
  });
p3().then();  //curraing, closure

console.log(a);
```

---

## ⚡ Thứ tự của `resolve` và `reject`
- 1 promise chỉ có thể rơi vào 1 trong 3 trạng thái
	(pending | onFulfilled | onReject)
	            resolve     reject
 - resolve  => then và reject => catch: 2 thằng này khá giống lệnh return (còn này chọn chỗ để đến)
- resolve ném giá trị cho then dưới dạng value
- reject ném giá trị cho catch dưới dạng error

- resolve và reject ko làm code dừng lại khác return
- resolve | reject ai đến trước thì sẽ quyết định promise ở trạng thái nào

```js
let p4 = new Promise.resolve("ahihi");
let p4 = new Promise((resolve, reject) => {
  resolve("ahihi"); //này thì chạy nè vì ở trước
  reject("Lỗi rồi nè"); //vô dụng bị lơ đi vì ở sau
  console.log("Xin chào mọi ngưới"); //ko dừng lại chạy luôn nè
});
```

### ✅ Xử lý kết quả

```js
//xác thực
p4.then((value) => {
  console.log("Thành công " + value);
}).catch((error) => {
  console.log("Thất bại " + error);
});

//cách viết thứ 2
p4.then(
  (value) => {
    console.log("Thành công " + value);
  },
  (error) => {
    console.log("Thất bại " + error);
  }
);
```

---

## 🔁 Tổng kết Promise

- Một Promise **chỉ có thể ở 1 trong 3 trạng thái**:
  - `pending`
  - `fulfilled` → `.then()`
  - `rejected` → `.catch()`
- `resolve()` → đưa giá trị qua `then(value)`
- `reject()` → đưa giá trị qua `catch(error)`
- Câu lệnh `resolve` và `reject` không dừng code như `return`.
Dưới đây là phần trình bày lại nội dung đoạn code Promise nâng cao, được trình bày đẹp, dễ hiểu và rõ ràng để bạn chép vào **Obsidian**. Nội dung này đi từ cách xử lý lỗi, đến luồng bất đồng bộ có quan hệ nguyên nhân – kết quả.

---

# 🚀 Promise nâng cao trong JavaScript!

![[z6407750116321_c1d659442f183438f7496cd7c932e440.jpg]]
## 🔁 1. `return` trong `.then()` hoặc `.catch()` sẽ đưa Promise về trạng thái **onFulfilled**

```js
let p5 = new Promise((resolve, reject) => {
  reject("Lỗi chà bá");
});

p5.then((value = {}))
  .catch((error) => {
    console.log("P5 đã thất bại nên bị lỗi là " + error);
    return "Lê hồ Điệp"; //hứa thêm cái nữa//return= Promise.resolve("Lê Hồ Điệp") khi gặp return thì nguyên khúc phía trên
  })                                                                      // biến thành onfulfilled
  .then((value) => {
    console.log("Lần này anh ấy đã có đc " + value);
  });
```

✅ **Giải thích**:
- Khi `catch` trả về một giá trị, **Promise chain chuyển sang onFulfilled**.
- `return` luôn đẩy kết quả qua `.then()` tiếp theo.

---

## ⚠️ 2. `throw` trong `.then()` hoặc `.catch()` sẽ đưa Promise về trạng thái **onRejected**

```js
p5 = new Promise((resolve, reject) => {
  resolve("Vui ghê");
});

p5.then((value) => {
  console.log("value: ahihi " + value);
  throw "ahahaha"; //Promise.reject("ahahaha")
})
  .catch((error) => {
    console.log("P5 đã thất bại nên bị lỗi là " + error);
    return "Lê hồ Điệp"; //return Promise.resolve("Lê Hồ Điệp")      rớt vào catch gần nhất
  })
  .then((value) => {
    console.log("Lần này anh ấy đã có đc " + value);
  })
  .catch((error) => {
    console.log("error nè " + error);
  });
```

💡 **Nguyên tắc**:
- `throw` sẽ **rơi vào `.catch()` gần nhất**.
- `return` trong `.catch()` lại đưa flow tiếp tục vào `.then()` tiếp theo.

> [!attention] nhớ nè cu
>throw vào catch gần nhất return vào then gần nhất

---

## ⏱ 3. Promise với bất đồng bộ: chuỗi tác vụ phụ thuộc nhau

### 🧩 Mô phỏng:
- Task 1: Lấy profile từ server → 3s
- Task 2: Lấy danh sách bài viết → 2s

```js
// getProfile: hàm mô phỏng lên database lấy dữ liệu mất 3s
let getProfile = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ name: "Điệp đẹp trai", age: 22 });
    }, 3000);
  });
// getArticle: hàm mô phỏng lên database lấy dữ liệu mất 2s
let getArticle = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(["Hoàng Tử Bé", "Nhà Giả KIm", "Mèo dạy hải âu bay"]);
    }, 2000);
  });

//nếu 2 tác vụ độc lập
// getProfile().then((value) => {
//   console.log(value);
// });

// getArticle().then((value) => {
//   console.log(value);
// });

//nhưng nếu mình muốn 2 cái task là nguyên nhân kết quả : 5s
```

---

### ❌ Cách sai (gà): promise hell

```js
getProfile().then((value) => {
  console.log(value); // 3s sau
  getArticle().then((value) => {//ròi pending cho lời hứa này
    console.log(value); // 2s sau đó → tổng 5s
  });
});
```

➡️ Rất khó đọc khi lồng nhau nhiều tầng.

---

### ✅ Cách chuẩn: chain `.then()`

```js
getProfile()
  .then((profile) => {
    console.log(profile); // sau 3s
    return getArticle(); // trả về promise
  })
  .then((articles) => {
    console.log(articles); // sau 2s nữa
  });
```

📌 **Ưu điểm**:
- Gọn gàng.
- Dễ đọc.
- Tránh callback hell.
- Giữ được mối quan hệ nguyên nhân – kết quả.

---

## 🧠 Tổng kết

| Hành động               | Kết quả                         |
|------------------------|----------------------------------|
| `return` trong `.then`/`.catch` | ➜ chuyển flow sang `.then()` tiếp theo |
| `throw` trong `.then`/`.catch`  | ➜ chuyển flow sang `.catch()` gần nhất |

> ⚠️ Cẩn thận khi mix `throw` và `return`, vì chúng điều hướng Promise theo hướng khác nhau!


## ⏱ Chuỗi bất đồng bộ có phụ thuộc

- Task 1 → `.then()` trả về Task 2 → `.then()` tiếp

```js
getProfile()
  .then(...)
  .then(...);
```

