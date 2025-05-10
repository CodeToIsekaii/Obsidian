---
date: 2025-04-21T12:32:00
---
Related : [[JavaScript]]
Tag: #js #project #studentmanagerment
___
[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part04-oop\09-studentManagement\index.html)


# 🎓 Quản lý sinh viên bằng Constructor Function và Prototype

## 🧱 1. Constructor Function: `Student`

```javascript
function Student(name, birthday) {
  this.name = name;
  this.birthday = birthday;
  this.id = new Date().toISOString(); // ID duy nhất dựa vào thời gian
}
```

## 📦 2. Store - Quản lý dữ liệu `localStorage`

```javascript
function Store() {}//method làm trong đây sẽ thành thuộc tính và ko thuộc sở hữu của object ,ko hay
```

### 📄 Lấy danh sách sinh viên từ `localStorage`
```javascript
//getStudents: lấy danh sách sinh viên (students) từ localstorage
Store.prototype.getStudents = function () {
  return JSON.parse(localStorage.getItem("students")) || []; //vô localstore lấy students ra và ép kiểu thành mảng chưa có thì đua mảng rông
};
```

### ➕ Thêm sinh viên mới và nhét newStudent vào danh sách sinh viên
```javascript
Store.prototype.add = function (newStudent) {
  // lấy danh sách sinh viên từ localStorage
  const students = this.getStudents(); //vì đang ở trong store nên this là store ko cần new object
  //nhét newStudent vào students
  students.push(newStudent);
  //lưu students vào localStorage
  localStorage.setItem("students", JSON.stringify(students));
};```

### 🔍 Lấy thông tin sinh viên theo `id`
```javascript
Store.prototype.getStudent = function (id) {
  //lấy student về
  const students = this.getStudents();
  //tìm
  return students.find((student) => student.id == id); //find tìm 1 thằng còn filter sàng lọc nhiều thằng //`student` ở đây là tên biến đại diện cho mỗi phần tử trong mảng `students` khi `.find()` đang duyệt qua.
};
```

### ❌ Xóa sinh viên theo `id`
```javascript
Store.prototype.remove = function (id) {
  const students = this.getStudents();
  const indexRemove = students.findIndex((student) => student.id == id);
  students.splice(indexRemove, 1);//`splice()` là một phương thức của mảng trong JavaScript.Nó được dùng để thêm xóa, hoặc thay thế phần tử ngay tại vị trí xác định.
  localStorage.setItem("students", JSON.stringify(students));
};
```

---

## 🎨 3. RenderUI - Hiển thị giao diện

```javascript
function RenderUI() {}
```

### 🧩 Hiển thị sinh viên vừa thêm
```javascript
//add: thêm newStudent vào giao diện
RenderUI.prototype.add = function (newStudent) {
  //lấy danh sách sinh viên từ localstorage từ đó biết số thứ tự bao nhiêu để hiện thị
  const students = new Store().getStudents(); //this ko đc vì là UI nên phải tạo object từ store để truy cập vào cái kho
  //tạo tr
  const newTr = document.createElement("tr");
  const { name, birthday, id } = newStudent;
  newTr.innerHTML = `
    <td>${students.length}</td>
    <td>${name}</td>
    <td>${birthday}</td>
    <td>
        <button class="btn btn-danger btn-sm btn-remove" data-id="${id}">
        Xóa
        </button>
    </td>
    `;
  // nhét vào tbody
  document.querySelector("tbody").appendChild(newTr);
  //xóa giá trị trên các ô input sau khi ấn thêm
  document.querySelector("#name").value = "";
  document.querySelector("#birthday").value = "";
};
```

### 💬 Hiển thị thông báo (alert)
```javascript
RenderUI.prototype.alert = function (msg, type = "success") {
  //tạo div thông báo
  let divAlert = document.createElement("div");
  divAlert.className = `alert alert-${type}`;
  divAlert.innerHTML = msg;
  //nhét vào div notification
  document.querySelector("#notification").appendChild(divAlert);
  //sau 2 giây thì xóa div thông báo
  setTimeout(() => {
    divAlert.remove();
  }, 2000);
};
```

### 📋 Hiển thị toàn bộ sinh viên lên giao diện
```javascript
//renderUI: là hàm lấy students từ localStorage và render tất cả sinh viên lên ui
RenderUI.prototype.renderAll = function () {
  //1. Lấy danh sách sinh viên students từ localStorage
  const students = new Store().getStudents();
  //2. render tất cả student trong students lên ui
  //truy cập vào tbody và xóa trắng
  //duyệt students và biến student thành tr (reduce)
  const htmlContent = students.reduce(
    (total, currentStudent, indexCurrentStudent) => {
      const { name, birthday, id } = currentStudent;
      return (
        total +
        `
      <tr>
        <td>${indexCurrentStudent + 1}</td>
        <td>${name}</td>
        <td>${birthday}</td>
        <td>
          <button class="btn btn-danger btn-sm btn-remove" data-id="${id}">
            Xóa
          </button>
        </td>
      </tr>
      `
      );
    },
    ""
  );
  //3.nhét các tr vào tbody
  document.querySelector("tbody").innerHTML = htmlContent;
};
```

---

## 🎯 4. Xử lý sự kiện

### 📌 Thêm sinh viên khi submit form
```javascript
document.querySelector("form").addEventListener("submit", (event) => {
  event.preventDefault(); //chặn reset trang
  //lấy data từ input
  const name = document.querySelector("#name").value;
  const birthday = document.querySelector("#birthday").value;
  //từ giá trị thu đc tạo newStudent
  const newStudent = new Student(name, birthday);
  let store = new Store();
  let ui = new RenderUI();
  //lưu newStudent vào danh sách các sinh viên(students) trong localStorage
  store.add(newStudent);
  //hiện thị newStudent lên giao diện
  ui.add(newStudent);
  //hiện thông báo thêm thành công
  ui.alert(`Bạn vừa thêm thành công ${name}`);
});
```

### 🔁 Render danh sách sinh viên khi load trang
```javascript
//sự kiện load trang thì render tất cả student từ localstorage
document.addEventListener("DOMContentLoaded", (event) => {
  //renderAll: render tất cả sinh viên trong students lên ui
  const ui = new RenderUI();
  ui.renderAll();
});
```

### ❌ Xử lý xóa sinh viên
```javascript
document.querySelector("tbody").addEventListener("click", (event) => {
  if (event.target.classList.contains("btn-remove")) {
    //lấy data-id đi kèm trên nút
    const idRemove = event.target.dataset.id;
    //getStudent: từ id remove tìm thông tin student tương ứng trong store
    const store = new Store();
    const ui = new RenderUI();
    const student = store.getStudent(idRemove);
    //hiện confirm
    const isConfirmed = confirm(`Bạn có chắc là muốn xóa ${student.name} ko?`);
    if (isConfirmed) {
      //1.xóa bên loclStorage(lun xóa này trước ròi xóa ui)
      store.remove(idRemove);
      //2. xóa bên ui
      ui.renderAll();
      //3. thông báo xóa thành công
      ui.alert(`Bạn đã xóa thành công ${student.name}`);
    }
  }
});
```

---

## 🧠 Ghi nhớ:

- Dùng `prototype` để chia sẻ method giữa các instance (Store, RenderUI).
- `localStorage` giúp lưu dữ liệu giữa các lần reload trang.
- `constructor function` kết hợp `new` để tạo đối tượng mới.
- `reduce()` dùng để chuyển mảng thành HTML chuỗi hiệu quả.
---
**chuyển từ function sang class**

## 🧾 Quản lý sinh viên bằng Class (OOP + localStorage)

### 🧠 Mục tiêu:
Xây dựng app quản lý sinh viên cơ bản:
- Thêm sinh viên mới
- Hiển thị danh sách
- Xóa sinh viên
- Lưu vào LocalStorage
- Sử dụng OOP với `class`, kế thừa, chia tách trách nhiệm rõ ràng

---

## 🧱 1. Class `Student`: Dữ liệu mỗi sinh viên

```js
class Student {
  constructor(name, birthday) {
    this.name = name;
    this.birthday = birthday;
    this.id = new Date().toISOString(); // tạo id duy nhất
  }
}
```

> Ghi nhớ: Mỗi sinh viên là một object có `name`, `birthday` và `id`.

---

## 💾 2. Class `Store`: Làm việc với localStorage

```js
class Store {
  getStudents() {
    return JSON.parse(localStorage.getItem("students")) || [];
  }

  add(newStudent) {
    const students = this.getStudents();
    students.push(newStudent);
    localStorage.setItem("students", JSON.stringify(students));
  }

  getStudent(id) {
    return this.getStudents().find(student => student.id === id);
  }

  remove(id) {
    const students = this.getStudents();
    const indexRemove = students.findIndex(student => student.id === id);
    students.splice(indexRemove, 1); // xóa 1 phần tử tại vị trí tìm được
    localStorage.setItem("students", JSON.stringify(students));
  }
}
```

> Class `Store` chịu trách nhiệm **lấy / thêm / tìm / xóa** sinh viên trong localStorage.

---

## 🖼 3. Class `RenderUI`: Quản lý giao diện

```js
class RenderUI {
  add(newStudent) {
    const students = new Store().getStudents();
    const newTr = document.createElement("tr");
    const { name, birthday, id } = newStudent;
    newTr.innerHTML = `
      <td>${students.length}</td>
      <td>${name}</td>
      <td>${birthday}</td>
      <td>
        <button class="btn btn-danger btn-sm btn-remove" data-id="${id}">
          Xóa
        </button>
      </td>
    `;
    document.querySelector("tbody").appendChild(newTr);
    document.querySelector("#name").value = "";
    document.querySelector("#birthday").value = "";
  }

  alert(msg, type = "success") {
    const divAlert = document.createElement("div");
    divAlert.className = `alert alert-${type}`;
    divAlert.innerHTML = msg;
    document.querySelector("#notification").appendChild(divAlert);
    setTimeout(() => divAlert.remove(), 2000);
  }

  renderAll() {
    const students = new Store().getStudents();
    const htmlContent = students.reduce((total, current, index) => {
      const { name, birthday, id } = current;
      return total + `
        <tr>
          <td>${index + 1}</td>
          <td>${name}</td>
          <td>${birthday}</td>
          <td>
            <button class="btn btn-danger btn-sm btn-remove" data-id="${id}">
              Xóa
            </button>
          </td>
        </tr>
      `;
    }, "");
    document.querySelector("tbody").innerHTML = htmlContent;
  }
}
```

> Class `RenderUI` lo việc **hiển thị sinh viên**, **thêm vào bảng**, **cảnh báo** và **render toàn bộ danh sách**.

---

## 🎯 4. Các sự kiện chính

### ✅ Khi submit form

```js
document.querySelector("form").addEventListener("submit", (event) => {
  event.preventDefault();
  const name = document.querySelector("#name").value;
  const birthday = document.querySelector("#birthday").value;
  const newStudent = new Student(name, birthday);
  const store = new Store();
  const ui = new RenderUI();
  store.add(newStudent);
  ui.add(newStudent);
  ui.alert(`Bạn vừa thêm thành công ${name}`);
});
```

### 🔁 Khi trang load, hiển thị toàn bộ danh sách

```js
document.addEventListener("DOMContentLoaded", () => {
  const ui = new RenderUI();
  ui.renderAll();
});
```

### ❌ Khi bấm nút "Xóa"

```js
document.querySelector("tbody").addEventListener("click", (event) => {
  if (event.target.classList.contains("btn-remove")) {
    const idRemove = event.target.dataset.id;
    const store = new Store();
    const ui = new RenderUI();
    const student = store.getStudent(idRemove);
    const isConfirmed = confirm(`Bạn có chắc là muốn xóa ${student.name} ko?`);
    if (isConfirmed) {
      store.remove(idRemove);
      ui.renderAll();
      ui.alert(`Bạn đã xóa thành công ${student.name}`);
    }
  }
});
```

---

## 📌 Ghi chú tổng kết:

| Thành phần | Vai trò |
|------------|---------|
| `Student`  | Tạo sinh viên mới |
| `Store`    | Lưu, lấy, xóa sinh viên từ localStorage |
| `RenderUI` | Hiển thị sinh viên lên bảng, thông báo |
| `submit`   | Thêm sinh viên mới |
| `DOMContentLoaded` | Hiển thị danh sách khi tải trang |
| `click btn-remove` | Xóa sinh viên |
