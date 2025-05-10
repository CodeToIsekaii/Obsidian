---
date: 2025-04-28T15:20:00
---
Related : [[JavaScript]]
Tag: #js #project #province
___
[HTML](D:\PiedTeam\FE-BE-Nodejs\ch03-js\part06-json-ajax-fetch\04-provinceProject\index.html)

# 📜 Code phân 3 cấp tỉnh, huyện, xã - Fetch API, Promise, Class

## 1. Cấu hình base URL

```javascript
//..../path/?query parameter=
const baseUrl = "https://provinces.open-api.vn/api";
```
> **Ghi chú**: API lấy dữ liệu tỉnh/huyện/xã.

---

## 2. Lớp `Http` – Gửi request HTTP

```javascript
class Http {
  get(url) {
     //ai gọi get() thì ko chấm then đc ,chỉ nhận data,ko .then xử lý data tiếp đc, nên phải return
    return fetch(url)
      .then((response) => {
        if (response.ok) {
          return response.json();
        } else {
          throw new Error(response.statusText);
        }
      });
  }
}
```

- Dùng `fetch` để gửi GET request.
- Nếu thành công (`response.ok`), trả về JSON.
- Nếu lỗi, **throw Error** để `.catch` bắt.

---

## 3. Lớp `Store` – Quản lý dữ liệu

```javascript
class Store {
  constructor() {
    this.http = new Http();
  }

  // Lấy danh sách các tỉnh/thành
  getProvinces(code = "") {
    //code = "" là default parameter nếu ko truyền gì hết thì nó rỗng
    return this.http
      .get(`${baseUrl}/p/${code}`)
      .then((provinces) => {
        return provinces;
      })
      .catch((error) => {
        console.log(error);
      });
  }

  // Lấy danh sách quận/huyện dựa theo mã tỉnh
  getDistrictsByProvinceCode(provinceCode) {
    return this.http
      .get(`${baseUrl}/p/${provinceCode}/?depth=2`)
      .then((province) => province.districts)
      .catch((error) => console.log(error));
  }

  // Lấy danh sách phường/xã dựa theo mã huyện
  getWardsByDistrictCode(districtCode) {
    return this.http
      .get(`${baseUrl}/d/${districtCode}/?depth=2`)
      .then((district) => district.wards)
      .catch((error) => console.log(error));
  }
}
```

---

## 4. Lớp `RenderUI` – Hiển thị dữ liệu lên giao diện

```javascript
class RenderUI {
  //renderProvinces: hàm nhận vào danh sách provinces và render lên UI
  renderProvinces(provinces) {
    let htmlContent = "";
    provinces.forEach((provinceItem) => {
      const { code, name } = provinceItem; //destructuring
      htmlContent += `<option value="${code}">${name}</option>`; //nếu để = nó sẽ ghi đè lên giá trị hiện có của biến cho từng vòng lặp
    }); //còn += cho phép nối giá trị vào mới vào cuối
    //nhét vào select#province
    document.querySelector("#province").innerHTML = htmlContent;
  }
  //renderDistricts: hàm nhận vào danh sách districts và render lên UI
  renderDistricts(districts) {
    let htmlContent = "";
    districts.forEach((districtItem) => {
      const { code, name } = districtItem;
      htmlContent += `<option value="${code}">${name}</option>`;
    });
    //nhét vào select#district
    document.querySelector("#district").innerHTML = htmlContent;
  }
  //renderWards: hàm nhận vào danh sách wards và render lên UI
  renderWards(wards) {
    let htmlContent = "";
    wards.forEach((wardItem) => {
      const { code, name } = wardItem;
      htmlContent += `<option value="${code}">${name}</option>`;
    });
    //nhét vào select#ward
    document.querySelector("#ward").innerHTML = htmlContent;
  }

  renderInformation(information) {
    const { address, district, ward, province } = information; //phân rã
    let htmlContent = `${address}, ${ward}, ${district}, ${province}`;
    document.querySelector("#information").innerHTML = htmlContent;
  }
}
```

---

## 5. Các sự kiện trong ứng dụng

### 5.1 Load ban đầu khi `DOMContentLoaded`

```javascript
document.addEventListener("DOMContentLoaded", (event) => {
  let store = new Store();
  let ui = new RenderUI();

  store
    .getProvinces()
    .then((provinces) => {
      //render danh sách provinces lên ui
      ui.renderProvinces(provinces);
      //lấy provinceCode hiện tại
      let provinceCode = document.querySelector("#province").value; //.value lấy đc mã lúc render

      //dùng provinceCode getDistrictsByProvinceCode
      // store.getDistrictsByProvinceCode(provinceCode).then((districts) => {}); này lấy quận tí lấy huyện nữa sẽ bị promise hell

      //tìm quận = provinceCode
      return store.getDistrictsByProvinceCode(provinceCode);
    })
    .then((districts) => {
      ui.renderDistricts(districts);
      //lấy districtCode hiện tại
      let districtCode = document.querySelector("#district").value;
      //tìm ward bằng districtCode
      return store.getWardsByDistrictCode(districtCode);
    })
    .then((wards) => {
      //render wards vừa thu đc lên ui
      ui.renderWards(wards);
    });
});
```

> **Ý nghĩa**:  
> Load danh sách tỉnh → lấy quận đầu tiên của tỉnh đó → lấy xã/phường đầu tiên của quận đó → render.

---

### 5.2 Khi chọn tỉnh (`change` sự kiện)

```javascript
//sự province bị thay đổi                           sự kiện thay đổi giá trị là change
document.querySelector("#province").addEventListener("change", (event) => {
  let store = new Store();
  let ui = new RenderUI();

  //lấy provinceCode hiện tại
  let provinceCode = document.querySelector("#province").value;
  //lấy các quận = provinceCode
  store
    .getDistrictsByProvinceCode(provinceCode)
    .then((districts) => {
      ui.renderDistricts(districts);
      //lấy districtCode hiện tại
      let districtCode = document.querySelector("#district").value;
      //tìm ward bằng districtCode
      return store.getWardsByDistrictCode(districtCode);
    })
    .then((wards) => {
      ui.renderWards(wards);
    });
});
```

> **Ý nghĩa**:  
> Khi đổi tỉnh → load lại quận, rồi load xã tương ứng với quận đầu tiên.

---

### 5.3 Khi chọn quận/huyện (`change` sự kiện)

```javascript
//sự kiện district bị thay đổi
document.querySelector("#district").addEventListener("change", (event) => {
  let store = new Store();
  let ui = new RenderUI();

  //lấy districtCode hiện tại
  let districtCode = document.querySelector("#district").value;
  //tìm ward bằng districtCode
  store.getWardsByDistrictCode(districtCode).then((wards) => {
    //render ward vừa lấy
    ui.renderWards(wards);
  });
});
```

> **Ý nghĩa**:  
> Khi đổi quận → load lại danh sách phường/xã tương ứng.

---

### 5.4 Khi submit form đặt hàng

```javascript
//khi submit đặt hàng
document.querySelector("form").addEventListener("submit", (event) => {
  event.preventDefault();
  let province = document.querySelector("#province option:checked").innerHTML; //vào option bị checked
  let district = document.querySelector("#district option:checked").innerHTML;
  let ward = document.querySelector("#ward option:checked").innerHTML;
  let address = document.querySelector("#address").value; //input
  //làm cái hàm information
  let information = {
    //object
    address,
    ward,
    district,
    province,
  };

  //
  let ui = new RenderUI();
  ui.renderInformation(information); //phân rã để xài
});
```

> **Ý nghĩa**:  
> Khi submit form, lấy toàn bộ thông tin địa chỉ người dùng nhập và hiển thị ra.

---

# 📚 Tổng kết

- **Fetch API** + **Promise** để gọi API.
- **Class** để chia nhỏ thành các khối `Http`, `Store`, `RenderUI`.
- **Event DOM** để xử lý thay đổi tỉnh/quận/xã và submit form.
- Cách viết **chuyên nghiệp**, dễ mở rộng.


# 📜 Tổng hợp: `Promise` ➔ `async/await` trong fetch + class

---

## **1. Gốc code (dùng Promise `.then`)**

- `Http.get()` trả về `fetch(url).then(response => response.json())`
- Các hàm như `getProvinces()`, `getDistrictsByProvinceCode()`, `getWardsByDistrictCode()` **dùng `.then()`** để xử lý kết quả.
- Các xử lý khi load trang (`DOMContentLoaded`) và các sự kiện (`change`, `submit`) cũng dùng `.then()` xâu chuỗi.

---

## **2. Đã sửa thành `async/await` ở những chỗ nào?**

### ✏️ 2.1. **Class `Http`**

| Trước | Sau |
|:-----|:----|
| `get(url)` return `fetch(url).then(...)` | `async get(url)` dùng `await fetch(url)` |

> **Thay đổi:**  
> - Thêm `async` vào `get(url)`.
> - Dùng `await fetch(url)`.
> - Trả về `await response.json()`.

```javascript
// Cũ:
get(url) {
  return fetch(url).then((response) => {
    if (response.ok) return response.json();
    else throw new Error(response.statusText);
  });
}

// Mới:
async get(url) {
  const response = await fetch(url);
  if (response.ok) {
    return response.json();
  } else {
    throw new Error(response.statusText);
  }
}
```

---

### ✏️ 2.2. **Class `Store`**

| Trước | Sau |
|:-----|:----|
| Các hàm `getProvinces()`, `getDistrictsByProvinceCode()`, `getWardsByDistrictCode()` return Promise `.then` | Các hàm này chuyển thành `async`, xử lý bằng `await` |

> **Thay đổi:**  
> - Thêm `async` vào trước các hàm.
> - Gọi `await this.http.get(url)`.
> - Xài `try...catch` để bắt lỗi.

```javascript
// Cũ:
getProvinces(code = "") {
  return this.http.get(`${baseUrl}/p/${code}`).then((provinces) => provinces);
}

// Mới:
async getProvinces(code = "") {
  try {
    const provinces = await this.http.get(`${baseUrl}/p/${code}`);
    return provinces;
  } catch (error) {
    console.log(error);
  }
}
```

**→ Các hàm còn lại (`getDistrictsByProvinceCode`, `getWardsByDistrictCode`) làm tương tự.**

---

### ✏️ 2.3. **Khi load trang `DOMContentLoaded`**

| Trước | Sau |
|:-----|:----|
| `.then().then().then()` nối Promise | Gọi `await` từng bước |

> **Thay đổi:**  
> - Thêm `async (event) => {}` vào `DOMContentLoaded`.
> - Dùng `await` lấy dữ liệu lần lượt.

```javascript
// Cũ:
store.getProvinces().then((provinces) => {
  ui.renderProvinces(provinces);
  let provinceCode = document.querySelector("#province").value;
  return store.getDistrictsByProvinceCode(provinceCode);
}).then((districts) => { ... })

// Mới:
document.addEventListener("DOMContentLoaded", async (event) => {
  const provinces = await store.getProvinces();
  ui.renderProvinces(provinces);

  let provinceCode = document.querySelector("#province").value;
  const districts = await store.getDistrictsByProvinceCode(provinceCode);
  ui.renderDistricts(districts);

  let districtCode = document.querySelector("#district").value;
  const wards = await store.getWardsByDistrictCode(districtCode);
  ui.renderWards(wards);
});
```

---

### ✏️ 2.4. **Sự kiện `change` của `#province` và `#district`**

| Trước | Sau |
|:-----|:----|
| `.then()` | `async/await` |

> **Thay đổi:**  
> - Viết `async (event) => {}` cho các `addEventListener`.
> - Gọi `await` lấy dữ liệu.

```javascript
// Cũ:
document.querySelector("#province").addEventListener("change", (event) => {
  store.getDistrictsByProvinceCode(provinceCode).then((districts) => {
    ui.renderDistricts(districts);
  });
});

// Mới:
document.querySelector("#province").addEventListener("change", async (event) => {
  const districts = await store.getDistrictsByProvinceCode(provinceCode);
  ui.renderDistricts(districts);
});
```

---

### ✏️ 2.5. **Submit form (`submit` event)**

| Trước | Sau |
|:-----|:----|
| Không thay đổi lớn | Vẫn dùng đồng bộ, chỉ lấy dữ liệu form |

✅ Không cần đổi `async` vì xử lý trong `submit` là đồng bộ (không fetch API).

---

## **3. Tổng kết lý do chuyển từ `.then` sang `async/await`**

- Đọc code **dễ hơn**, **mạch lạc** hơn (giống như đọc từ trên xuống thay vì xâu chuỗi `.then`).
- Tránh tình trạng **Promise Hell** (lồng `.then()` quá nhiều).
- Code **ít lỗi hơn** khi có `try...catch`.

---

# 🧠 Ghi nhớ nguyên tắc:

| `.then` | `async/await` |
|:--------|:--------------|
| `.then(data => {...})` | `const data = await promise` |
| `.catch(err => {...})` | `try { await ... } catch (err) {}` |
