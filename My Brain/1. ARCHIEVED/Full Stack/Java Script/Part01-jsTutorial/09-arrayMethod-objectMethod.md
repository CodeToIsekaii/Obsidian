---
date: 2025-03-28T22:09:00
---
Related:[[JavaScript]]
Tag: #js #method #array #object
___

## I. Mảng trong JavaScript
### 1. Mảng không nhất thiết phải cùng kiểu
```js
let arr1 = [1, 2, "a", { lname: "Huệ", age: 10 }, [3, 5]];
console.log(arr1);
```

### 2. `.length`: Lấy độ dài của mảng
```js
console.log(arr1.length); // 5
```

### 3. Kiểm tra một biến có phải là mảng hay không : #Array.isArray() | #instanceof Array
```js
console.log(Array.isArray(arr1)); // true
console.log(arr1 instanceof Array); // true
```

### 4. `.toString()`: Chuyển mảng thành chuỗi kèm dấu `,`
```js
let workerList = ["Huệ", "Lan", "Trà"];
let str1 = workerList.toString();
console.log(str1); // "Huệ,Lan,Trà"
```

### 5. `split()` & `join()`
- `split(separator)`: Chuyển chuỗi thành mảng
- `join(separator)`: Chuyển mảng thành chuỗi

## II. Chèn & Xóa Phần Tử Trong Mảng
> **Array là mutable**, nghĩa là method có thể tác động vào object| nó vẫn chứa các method return ra object mới

### 6. `.push(item)`: Thêm phần tử vào cuối mảng, trả về độ dài mới
```js
workerList = ["Huệ", "Lan", "Trà"];
let result = workerList.push("Cúc");
console.log(workerList, result); // ["Huệ", "Lan", "Trà", "Cúc"] 4
```

### 7. `.pop()`: Xóa phần tử cuối cùng, trả về phần tử bị xóa
```js
workerList = ["Huệ", "Lan", "Trà"];
result = workerList.pop();
console.log(workerList, result); // ["Huệ", "Lan"] "Trà"
```

### 8. `.unshift(item)`: Thêm phần tử vào đầu mảng, trả về độ dài mới
```js
workerList = ["Huệ", "Lan", "Trà"];
result = workerList.unshift("Cúc");
console.log(workerList, result); // ["Cúc", "Huệ", "Lan", "Trà"] 4
```

### 9. `.shift()`: Xóa phần tử đầu tiên, trả về phần tử bị xóa
```js
workerList = ["Huệ", "Lan", "Trà"];
result = workerList.shift();
console.log(workerList, result); // ["Lan", "Trà"] "Huệ"
```

### 10. `delete array[index]`: Xóa phần tử tại vị trí `index` - xóa entry trong object để lại khoảng trống ở vị trí đó nếu truy cập vào thì undefined 
> ⚠️ Không nên sử dụng vì tạo ra "lỗ hổng" trong mảng.
```js
workerList = ["Huệ", "Lan", "Trà"];
delete workerList[1];
console.log(workerList); // ["Huệ", empty, "Trà"]
console.log(workerList[1]); // undefined
```

### 11. `.splice(index, số lượng cần xóa, ...các phần tử muốn thêm)`
- Trả về mảng các phần tử bị xóa
```js
//xóa ko thêm
workerList = ["Huệ", "Lan", "Trà"];
result = workerList.splice(1, 1);
console.log(workerList, result); //["Huệ", "Trà"] ["Lan"]

//thêm ko xóa
workerList = ["Huệ", "Lan", "Trà"];
result = workerList.splice(1, 0, "Điệp", "Cường");
console.log(workerList, result); //["Huệ", "Điệp", "Cường","Lan", "Trà"] []

//vừa thêm vừa xóa
workerList = ["Huệ", "Lan", "Trà"];
result = workerList.splice(0, 2, "Điệp", "Cường");
console.log(workerList, result); //["Điệp", "Cường", "Trà"] ["Huệ", "Lan"]
```

### 12. `.slice(start, end)`: Lấy mảng con từ `start` đến `end - 1`

### 13. `.concat()`: Nối mảng
```js
workerBoy = ["Điệp", "Hùng"];
workerGirl = ["Lan", ["Duy", "Tuấn"]];
workerList = workerBoy.concat(workerGirl, "Hồng", ["Tâm", "Phúc"]);
console.log(workerList); //['Điệp', 'Hùng', 'Lan', Array(2), 'Hồng', 'Tâm', 'Phúc']
```
<mark style="background: #ABF7F7A6;"> #shallow copy: copy nông, sao chép nhưng ko cắt đc hết dây mơ rễ má => 2 chàng trỏ 1 nàng</mark>
### 14. Spread Operator (`...`):  destructuring: kỹ thuật phân rã {} | [] (cực mạnh)
```js
workerList = [...workerBoy, ...workerGirl];
console.log(workerList); //['Điệp', 'Hùng', 'Lan', Array(2)]
console.log(workerList[3] == workerGirl[1]); //true
//shallow copy
workerList[3] = [...workerList[3]]; //làm như vậy mới đạt đc deep copy
console.log(workerList[3] == workerGirl[1]); //false
```

### 15. `.forEach(callback)`: Lặp qua mảng
- callback function:(val, index, array) => {}
```js
arr1 = ["Huệ", "Cúc", "Hồng"];
arr1.forEach((val, index, array) => {
  console.log(val, index, array);
});
```

### 16. `.filter(callback)`: lọc các item thỏa điều kiện của callback function và trả ra mảng
- callback function nhận item, nếu trả ra true thì item đó đc báo vào kết quả, false thì bỏ
- callback function: (val, index, array) => {}
```js
arr1 = [1, 2, 3, 4, 5];
result = arr1.filter((item) => item % 2 == 0);
console.log(result); // [2, 4]
```

### 17. `.find(callback)`: return item đầu tiên trong mảng thỏa điều kiện
```js
arr1 = [1, 2, 3, 4, 5];
result = arr1.find((item) => item % 2 == 0);
console.log(result); //2
```

### 18. `.findIndex(callback)`:  return index của item đầu tiên thỏa điều kiện
```js
arr1 = [1, 2, 3, 4, 5];
result = arr1.findIndex((item) => item % 2 == 0);
console.log(result); //1
```

### 19. `.indexOf(value)`: return index đầu tiên của các item có giá trị giống value
```js
arr1 = [1, 2, 3, 4, 5];
result = arr1.indexOf(3); //2
result = arr1.indexOf([3, 5]); //-1
result = arr1.indexOf([3, 4]); //-1
console.log(result);
```
> [!note] 
> array.indexof(searchElement, indexStart):
tìm phần tử từ vị trí indexStart trở lên
nên là indexOf không có khả năng tìm vị trí của mảng con trong mảng cha 

> [!tip] 
> find(cf) thỏa thì trả item
findIndex(cf) thỏa thì trả index của item đầu tiên
indexOf(value)thỏa thì trả index đầu có val thỏa điều kiện 
### 20. `.every(callback)`: giống như All trong DBI
- tất cả các phần tử trong mảng phải thỏa điều kiện callback function thì mới true, 1 thằng ko thỏa thì false
```js
arr1 = [1, 2, 3, 4, 5];
result = arr1.every((item) => item > 1); //false, vì số 1 ko thỏa
result = arr1.every((item) => item < 6); //true
console.log(result);
```

### 21. `.some(callback)`: giống như In trong DBI
- chỉ cần 1 phần tử thỏa điều kiện callback function thì true, ko ai thỏa thì false
```js
result = arr1.every((item) => item > 1); //true
result = arr1.every((item) => item > 6); //false
console.log(result);
```

### 22. `.includes(value)`: Kiểm tra xem phần tử có tồn tại trong mảng không (true | false)
```js
result = arr1.includes(4); // true
console.log(result);
```

### 23. `.reverse()`: Đảo ngược mảng

### 24. `.sort()`: Sắp xếp mảng
```js
// Sắp xếp chuỗi
arr1 = ["Điệp", "An", "Bảo"];
arr1 = arr1.sort();
console.log(arr1); // ['An', 'Bảo', 'Điệp']

// Sắp xếp số (cần cung cấp hàm so sánh)
arr1 = [1, 3, 20, 100];
arr1 = arr1.sort(); //[1, 100, 20, 3]
//phải dạy nó vì js đang coi nó như chuỗi
arr1 = arr1.sort((a, b) => a - b); // [1, 3, 20, 100]
console.log(arr1);
```

### 25. `.map(callback)`: biến đổi các phần tử trong mảng theo 1 công thức nào đó,sau khi biến đổi vẫn là cái mảng
- callback function: (val,index,array) => {}
```js
arr1 = [2, 5, 7];
arr1 = arr1.map((item) => (item + 2) / 3);
console.log(arr1);
```
> [!attention] 
> đừng thiếu return, map sẽ thay thế item = kết quả return đó
nếu thiếu return phần tử sẽ thành undefined 
### 26. `.reduce(callback, initial)`: biến đổi từng phần tử trong mảng theo 1 công thức và dồn giá trị về 1 biến total (vừa biến đổi vừa dồn các kết quả về 1 biến duy nhất)
- callback function : (total, current, currentIndex) => {}
```js
arr1 = [2, 5, 7];
let sum = arr1.reduce((total, current, currentIndex) => {
  return total + (current + 2) / 3;
}, 0);
console.log(sum);
```

### 27. * Biến mảng thành object bằng `.reduce()`
```js
arr1 = ["Điệp", 10, 22];
arr1 = arr1.reduce((total, current, currentIndex) => {
  total[currentIndex] = current;
  return total;
}, {});
console.log(arr1);
```

# Các Phương Thức Của Object

### Khái niệm Object
- Entry của object là `key: value`
- Key luôn là `string` hoặc `number`
- Value có thể là `object | function | number | string`

```js
let worker1 = {
  lname: "Điệp đẹp trai",
  age: 24,
  showInfor() {
    console.log(this.lname + " " + this.age);
  },
};
worker1.showInfor();

// Thêm thuộc tính
worker1.point = 10;
worker1["point"] = 10;

// Cập nhật thuộc tính
worker1.lname = "Điệp PiedTeam";

// Xóa thuộc tính (delete không để lại lỗ như mảng,cái này sinh ra để cho thằng này dùng)
delete worker1.age;
console.log(worker1);
```

### 1. `Object.assign()`: Merge Object
- Nếu đã tồn tại thì ghi đè, chưa có thì thêm vào

```js
let person1 = {
  lname: "Điệp",
  age: 24,
  job: ["Yangho", "Coder"],
};

let person2 = {
  lname: "Lan",
  age: 24,
  company: "PiedTeam",
};

let person3 = Object.assign(person1, person2);
console.log(person1);
/*
let person3 = {
  lname: "Lan",
  age: 24,
  job: ["Yangho", "Coder"],
  company: "PiedTeam",
};
*/
```

#### Shallow Copy với Spread Operator
```js
person3 = { ...person1, ...person2 };
console.log(person3.job == person1.job); // true
person3.job = [...person3.job]; // Tạo bản sao độc lập //cắt đc duyên âm
console.log(person3.job == person1.job); // false
//deep copy
```

### 2. Lấy danh sách key, value, entries của object
**Object.keys(obj): mảng các key của object**
```js
console.log(Object.keys(person3));   // ['lname', 'age', 'job', 'company']
```
**Object.values(obj): mảng các value của object**
```js
console.log(Object.values(person3)); // ['Lan', 24, Array(2), 'PiedTeam']
```
**Object.entries(obj): mảng các entries của object**
```js
console.log(Object.entries(person3)); // [['lname', 'Lan'], ['age', 24], ['job', ['Yangho', 'Coder']], ['company', 'PiedTeam']]
```
### 3. `for-in`: Lặp qua object
```js
for (let key in person3) {
  console.log(key, person3[key]);
}
```



