---
date: 2025-05-02T12:00:00
---
Related : [[NodeJS]]
Tag: #nodejs
___

# 🟢# Node.js là gì? Tại sao phải học Node.js?

## Node.js là gì?

JavaScript là ngôn ngữ lập trình, muốn chạy được cần môi trường.

Môi trường phổ biến: Trình duyệt, Node.js, Deno, Bun,...

* **Node.js** là môi trường chạy JavaScript ngoài trình duyệt, nhờ vậy có thể dùng để viết backend server.
* Miễn phí, mã nguồn mở, chạy được trên Linux, macOS, Windows.
* Node.js dùng **Chrome V8 JavaScript Engine** làm nhân thực thi code JS, được viết bằng C++.
* Ngoài V8, Node.js còn dùng nhiều thư viện như: `libuv`, `c-ares`,...

### Lịch sử Node.js

(Không quan tâm)

## Tại sao chọn Node.js viết Backend?

* Viết bằng JS, FE dev dễ chuyển qua viết BE => Tiết kiệm chi phí nhân sự.
* Node.js nhanh hơn PHP.
* Dù thua Java, .NET, Go về tốc độ, nhưng đủ xài. VD:

  * Stack Exchange chỉ cần 300 req/s
  * Fastify (Node.js) \~45,659 req/s
  * Express \~9,888 req/s
* Nút thắt hiệu năng thường nằm ở DB, không phải ngôn ngữ.

> Nếu cảm thấy Node.js chậm, chỉ cần nâng server! Easy!

* Nhu cầu việc làm cao, tương đương PHP, Java, .NET

## Hiểu lầm về Node.js và JavaScript

* JS là ngôn ngữ **đơn luồng**, nhưng **môi trường chạy** có thể đa luồng.
* VD: Trình duyệt có WebAPI (setTimeout, fetch,...) chạy 1 luồng khác.
* Node.js cũng thế, nó có thể chạy đa luồng => có thể tận dụng được hết sức mạnh CPU ngày nay

# Cài đặt Node.js

Truy cập: [https://nodejs.org/en](https://nodejs.org/en) để tải. nhưng không nên, vì sau này rất khó để chuyển đổi giữa các version khác nhau

Cách cài thông minh hơn là dùng NVM(node version managerment), thích version nào thì chuyển sang version đó, rất tiện

### Dùng NVM (Node Version Management)

Giúc quản lý nhiều phiên bản Node.js.

* **Mac & Linux**: [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)
* **Windows**: [https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)

Cài Node:

```bash
nvm list                 # Để biết cài NVM thành công chưa
nvm install node         # Cài phiên bản mới nhất
nvm install 14.7.0       # Cài phiên bản cụ thể
nvm alias default 14.7.0 # Đặt làm mặc định nếu muốn
```

# Lộ trình học

Theo roadmap: [https://roadmap.sh/nodejs](https://roadmap.sh/nodejs)

* **Module**: Thay vì viết hết trong 1 file, ta chia ra nhiều module.
* **CommonJS** (module cũ) & **ESModule** (module mới)

## Thực hành: Module

Tạo folder `ch01-Nodejs-npm-npx`

### Dùng CommonJS

Tạo folder `test-module`:

* `utils.js`
```js
const sum = (a, b) => a + b;
exports.sum = sum;
```
thằng utils.js này giống như 1 cái object và các thuộc tính mọi người tạo ở đây đang bị private và để public thì ghi **exports** , bây giờ obj utils sẽ có hàm sum

* `main.js`
Để sự dụng hàm sum bên utils thì sẽ phải import như này **require("./utils.js")**
```js
const utils = require("./utils.js");
console.log("main.js nè");
console.log(utils.sum(1, 5));
```

Chạy:

```bash
node main.js 
//main.js nè
//6
```

### Dùng ESModule
- trong `ch01-Nodejs-npm-npx` tạo folder `es-module` để demo `ESModule`

- để dùng `ESModule` ta phải có `package.json`(đây là file lưu thông tin dự án của mọi người trong đây có thể set dự án theo esmodule,thằng này giống danh sách các công nghệ,cấu hình đang sự dụng trong dự án đó) hoặc file phải có đuôi là `mjs`(module js), ở đây mình sẽ dùng `pakage.json` nên ta vào folder `es-module` và:

  ```bash
  npm init :thì nó sẽ khởi tạo cho mình tự setup dự án , thêm -y thì nó tạo sẵn cho mình luôn
  ```

  ```bash
  npm init -y //để tạo gói package.json có sẵn , npm giống thằng chuyên quản lý các đóng gói,công nghệ
  ```

  Hình dung thư mục này như dự án có rất nhiều công nghệ cần phải sự dụng, thì mọi người cần anh chuyên quản lý nhũng cái công nghệ của dự án đó tên là npm

- ta vào file `package.json` config lại type cho module
  ```json
  "type": "module"
  ```

- sau đó ta tạo 2 file để demo module

  - tạo file `helper.js` lưu hàm mình thích
    Y chang có 2 hàm muốn hàm nào public cho mọi người sự dụng thì export khác ở cách gõ
    ```js
    const logName = () => {
      console.log("xin chào mọi người");
    };
    export const sum = (a, b) => a + b;
    export default logName;  default sử dụng lại tên logName (khi import thì ko cần dùng {})
    ```

  - tạo file `index.js` để xài hàm của `helper.js`
    muốn xài thì import khác cách gõ thay vì require thì mình ghi import lun
    ```js
    import logName, { sum } from "./helper.js";
    logName();
    console.log(sum(1, 5));
    ```

  - chạy thử để xem kết quả
    ```bash
      node index.js
    ```
<mark style="background: #FFF3A3A6;"> dù là common hay es thì cái nào không export thì sẽ k được sử dụng ở file khác thường dự án người ta xài esmodule, nhưng những thự viện thường sẽ xài common</mark>
# NPM (Node Package Management)
<mark style="background: #FFB8EBA6;">là 1 trình quản lý thư viện của nodejs</mark>, thay vì ta phải mò lên các trang web để tải src về không chính thống hay copy đường dẫn bỏ vào file thì mới xài đc, thì ta có thể thông qua npm để tại trực tiếp bằng lệnh, nhanh chóng, an toàn, và dể dàng
(mỗi lần cài thì tắt vscode mở lại)

## Demo: Cài lodash

Tạo folder `npmDemo`:
- giờ ta tạo file `package.json`: là file quản lý những công nghệ mình đang dùng
  ```bash
  npm init --giúp tạo package.json
  ```

- giờ cài thử thư viện `lodash`: 1 thư viên chứa rất nhiều hàm tiện ích hộ trợ cho js
  ```bash
  npm i lodash
  npm i axios
  ```

  i: là install
- hãy xem package để xem thay đổi
- `node_modules` là nơi chứa các file công nghệ đã cài đặt
- `package-lock.json` chứa thông tin đầu đủ và chi tiết của các công nghệ trong package.json đó

- giả xử ta muốn xóa bớt thư viện, ta hãy xóa bằng cách
  ```bash
    npm uninstall axios
  ```

  nó sẽ gỡ chính xác những cái npm cài đã vào

- giờ thì vào `package.json` để xem còn `axios` không

# option cài đặt global -g hay local

global là cài đặt trong máy tính => toàn bộ máy tính có nodemon, sử dụng ở đâu trong máy cũng đc
cài thử nodemon global => nó cũng ko xuất hiện trong package.json vì package.json lưu thông tin của dự án chứ ko phải lưu thông tin của máy tính

```bash
npm i nodemon -g
```

mún tắt thì ctrl + c
để dỡ npm uninstall nodemon -g

- `nodemon` là 1 thư viện theo dõi những thay đổi trong dự án và chạy lại các câu lệnh ta yêu cầu nếu có thay đổi

### thực hành cấu hình cho `nodemon`

- tạo file `index.js`

  ```js
  const sum = (a, b) => a + b;

  console.log(sum(1, 2));
  ```

- mỗi lần mình thay đổi code , mình phải chạy lại cái `node index.js` để xem kết quả rất mệt
- nên giờ ta sẽ gõ là `nodemon index.js` để nó theo dõi thay đổi của `index.js` và khi thay đổi , nó sẽ chạy lại

- nhưng trên thực tế thì anh thích cài `local` hơn, tránh ảnh hưởng máy người khác
- `local` có 2 kiểu cài

1. dependence(production):(là nơi bắt buộc phải có để sản phẩm có thể hoạt động) ta sẽ cài các thứ liên quan đến quá trình vận hành của sản phẩm

```bash
npm i nodemon
```

2. devdependence(khu vực development):(công cụ hộ trợ mình trong quá trình viết source) cài tool, thư viện mà nó k nằm trong quá trình production(quá trình ra thành phẩm)
   -vd: nodemon là 1 tool hỗ trợ chạy code khi refesh

   --save-dev là -D

```bash
npm i nodemon -D
```

uninstall -d là xóa ở dependencies
xóa nodemon khỏi global

```bash
npm uninstall nodemon -g
```

giờ ta cài đặt nodemon vào `local` rồi, nên muốn xài phải thông qua `package`
ta vào `package` thêm script(thống nhất lệch)

```json
"start": "nodemon index.js"
```

và chạy bằng cách gọi lên "start"

```bash
npm run start
```

# cách cập nhật package trong dự án

## học cách ký hiệu của version

version có cấu trúc **"Major.minor.patch"**
tương ứng như **"breakchange.newfeature.fixed"**

ta sẽ gặp tiền tố **"^Major.minor.patch"** hoặc **"~Major.minor.patch"**

**"^Major.minor.patch"** cho phép cài lệch minor và patch ví dụ **0.13.1** thì có thể cài thành **0.13.2** hoặc **0.14.0**

**"~Major.minor.patch"** cho phép cài lệch patch, ví dụ **0.13.1** thì có thể cài **0.13.2**, không thể cài **0.14.1** được

hiểu rõ được cách đánh version thì ta mới tiến hành cập nhật
<mark style="background: #ADCCFFA6;">nếu cập nhật cái breakchange thì khả năng rất cao hệ thống bị lỗi,</mark> vì khi 1 thư viện hoạt động nó sẽ hoạt động với nhiều thư viện khác nếu em cập nhật lên nhiều quá thì cái công nghệ đang xài bị lệch so với công nghệ khác => gây ra lỗi

## demo

xóa thử nodemon và cài 1 ver cũ cho nó

```bash
npm uninstall nodemon
npm i nodemon@2.0.2 -D
```

cập nhật nè

kiểm tra(có thằng nào bị lệch phiên bản so với thằng khác ko) package đã lỗi thời

```bash
npm outdated
Package  Current  Wanted  Latest  Location              Depended by
nodemon    2.0.2  2.0.22  2.0.22  node_modules/nodemon  npmDemo
```

cập nhật

```bash
npm update --cập nhật nè   (update đồng loạt lun còn từng cái thì phải gõ ra cập nhật phiên bản mình muốn)
npm outdated --test lại xem còn outdated k nè
```

dù đã cập nhật nhưng: `package` k cập nhật, còn `package-lock` thì có cập nhật
ta không cần quan tâm sự tương thích này, vì ta chỉ cần quan trọng `package-lock` đã cập nhật thôi

### nếu ta muốn cập nhật trong `package.json`(k nên thử phần này) cập nhật khô máu

- ta có thể fix bằng tay
- hoặc dùng `npm-check-updates`

  ```bash
  npm i -g npm-check-updates
  ```

- sau khi cài xong ta chỉ cần gõ:

  ```bash
  ncu //để xem những outdate
  ```

- và ta cập nhật bằng

  ```bash
  ncu -u //để cập nhật
  ```

**_cập nhật cẩn thận, nhiều khi cập nhật lên đến breaking change là nó lỗi ngu người_**

```bash
npm install
```

cài lại cho chắc

### giới thiệu về ignore và npm i khi đã xóa node_module

- phần này dể demo

# NPX là gì?

giống npm nhưng hiệu năng cao hơn
npx hộ trợ nhiều cho react

- NPX là 1 `commandline` mới được cài vào sau npm
- NPX giúp cài 1 gói package nhanh, đỡ phải cài từng thằng

- ta sẽ demo thử môi trường code cho react

  - tạo thử folder tên `NpxDemo`

    ```bash
    npm init -y
    npm i create-react-app
    npx create-react-app
    ```

- nếu ta vào tìm `file node_module/bin/create-react-app` và thêm `console.log`
  thì ta sẽ thấy npx chạy log đó

npx hoạt động bằng cách nếu như package.json k có thì nó vẫn sẽ tự mò lên npm thư viện để tìm và cài đặt create-react-app -> **_npx k phụ thuộc vào package.json_**
=> dễ bị lỗi vì cài đa luồng lun mà ,nó phân luông cài nên lúc hợp các luồng lại thì bị lỗi nhưng vẫ khá ngon
nếu giờ bạn xóa hết tất cả trong npxDemo và gõ

```bash
npx create-react-app my-app
```

thì nó sẽ tự động lên npm tại về dù chẳng có package.json nào cả

ngoài npx còn có yarn(thay thế npm nhưng giờ ko ai xài vì như buồi), pnpm (bọn này giống npm nhưng mà nó hiệu năng cài đa luồng, nhanh hơn)


---

**Lưu ý**: Các package không export sẽ không dùng được bên ngoài. EsModule thường dùng trong project, CommonJS dùng nhiều trong thư viện.
