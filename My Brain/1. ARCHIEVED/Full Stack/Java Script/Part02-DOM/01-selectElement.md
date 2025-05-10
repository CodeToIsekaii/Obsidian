---
date: 2025-03-28T23:11:00
---
Related:[[JavaScript]]
Tag: #js #dom #selectelement
___
[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part02-dom\01-selectElement.html)

** #dom:** document object model
- liên kết(DOM) | móc 1 biến vào 1 đối tượng trong DOM
## 1. Liên kết DOM - Móc một biến vào một đối tượng trong DOM

Có hai cách để DOM một đối tượng:

```js
let inputNode = document.getElementById("name"); // Lấy phần tử theo id
inputNode = document.querySelector("#name"); // Lấy phần tử theo CSS selector
console.log(inputNode);
```

- `getElementById("id")`: Lấy phần tử dựa trên ID
- `querySelector("#id")`: Lấy phần tử đầu tiên thỏa mãn selector (CSS selector)

Trong CSS selector và JavaScript DOM, dấu `#` và dấu `.` có sự khác biệt quan trọng:  

- `#id` → Chọn phần tử có **id cụ thể**  
  - Ví dụ: `#name` chọn phần tử có `id="name"`  
  - **Chỉ có duy nhất một phần tử với một `id` cụ thể**  
  - Dùng với:  
    ```js
    document.getElementById("name");
    document.querySelector("#name");
    ```

- `.class` → Chọn tất cả các phần tử có **class cụ thể**  
  - Ví dụ: `.card` chọn tất cả phần tử có `class="card"`  
  - **Một class có thể gán cho nhiều phần tử**  
  - Dùng với:  
    ```js
    document.getElementsByClassName("card");
    document.querySelectorAll(".card");
    ```

👉 **Tóm lại:**  
- `#` dùng để chọn ID (duy nhất).  
- `.` dùng để chọn Class (có thể nhiều).  

Bạn cần thêm ví dụ thực tế nào không? 🚀
## 2. nếu muốn lấy nhiều phần tử thỏa selector thì sao?

```js
let cardList = document.getElementsByClassName("card"); //HTMLCollection
cardList = [...document.getElementsByClassName("card")]; //mai sau thấy mấy ông già xài còn biết chứ ko xài
cardList = document.querySelectorAll(".card"); // Trả về NodeList

cardList.forEach((element) => {
  console.log(element);
});
console.log(cardList);
```

- `getElementsByClassName("class")`: Trả về HTMLCollection (giống array, ko hỗ trợ mấy method phổ biến một trong số đó là .forEach)
- `querySelectorAll(".class")`: Trả về NodeList ( giống array, nhưng có hầu hết các method phổ biến)

## 3. Tìm hiểu một node có gì

```js
let firstCard = document.querySelector(".card");
console.log(firstCard);
console.log(firstCard.childNodes); // NodeList(5) [text, h2, text, p, text]
console.log(firstCard.children); // HTMLCollection(2) [h2, p]
console.log(firstCard.parentElement); // Trả về node cha của node hiện tại
console.log(firstCard.nextElementSibling); // Truy cập vào thằng cùng cấp phía dưới
console.log(firstCard.firstChild); // Trả về phần tử đầu tiên của childNodes (text)
console.log(firstCard.firstElementChild); // Trả về phần tử đầu tiên của children (h2)
console.log(firstCard.classList); // ["card", "p-2"]
console.log(firstCard.className); // "card p-2"
```

## 4. Tạo một phần tử mới trong DOM

```js
let newCard = document.createElement("div");
newCard.className = "card p-2"; // Thêm class

let fname = "Tui đc tạo ra từ JS";
newCard.innerHTML = `
  <h2>${fname}</h2>
  <p>Tui là một node fake</p>
`;

let cardGroup = document.querySelector(".card-group");
// cardGroup.appendChild(newCard); // Nhét vào cuối danh sách
cardGroup.replaceChild(newCard, cardGroup.children[1]); // Thay thế phần tử thứ 2
```

- `createElement("tag")`: Tạo phần tử mới
- `className`: Thêm class vào phần tử
- `innerHTML`: Gán nội dung HTML vào phần tử
- `appendChild(node)`: Thêm node vào cuối danh sách con
- `replaceChild(newNode, oldNode)`: Thay thế node con

## 5. Làm việc với Attribute

```js
firstCard.setAttribute("data-id", "1"); // Set hoặc thêm attribute
console.log(firstCard.getAttribute("data-id")); // Lấy giá trị attribute
firstCard.removeAttribute("data-id"); // Xóa attribute
```

- `setAttribute("attr", "value")`: Thêm hoặc cập nhật attribute
- `getAttribute("attr")`: Lấy giá trị của attribute
- `removeAttribute("attr")`: Xóa attribute khỏi phần tử

