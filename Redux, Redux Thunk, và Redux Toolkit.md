---
date: ""
---
Related : [[]]
Tag: #
___
Để giải thích **Redux**, **Redux Thunk**, và **Redux Toolkit** một cách dễ hiểu, mình sẽ chia nhỏ từng khái niệm, giải thích bằng ngôn ngữ đơn giản, kèm theo các thuật ngữ chuyên ngành và ví dụ thực tế. Mục tiêu là giúp người mới học code hiểu rõ bản chất và cách sử dụng của từng công cụ này trong việc quản lý trạng thái (state) trong ứng dụng JavaScript, đặc biệt là với React.

---

### 1. Redux là gì?

**Redux** là một thư viện **quản lý trạng thái (state management)** mã nguồn mở, được sử dụng phổ biến trong các ứng dụng JavaScript, đặc biệt là với React. Nó giúp quản lý trạng thái của ứng dụng một cách **dự đoán được (predictable)**, tập trung và dễ bảo trì.

#### Ý tưởng chính:
- Trong một ứng dụng, trạng thái (state) là toàn bộ dữ liệu mà ứng dụng cần để hoạt động (ví dụ: danh sách người dùng, trạng thái đăng nhập, dữ liệu giỏ hàng).
- Nếu không có Redux, việc quản lý trạng thái trong các ứng dụng lớn có thể trở nên lộn xộn, vì dữ liệu được truyền qua nhiều component (gọi là **prop drilling**) hoặc khó đồng bộ giữa các phần của ứng dụng.
- Redux giải quyết vấn đề này bằng cách lưu trữ **toàn bộ trạng thái của ứng dụng trong một nơi duy nhất** gọi là **store**, và cung cấp các quy tắc nghiêm ngặt để thay đổi trạng thái.

#### Các khái niệm cốt lõi trong Redux:
Redux dựa trên ba nguyên tắc chính:
1. **Single Source of Truth (Nguồn sự thật duy nhất)**:
   - Toàn bộ trạng thái của ứng dụng được lưu trong một **store** (một object JavaScript). Ví dụ: `{ users: [], cart: [], isLoggedIn: false }`.
   - Điều này giúp dễ dàng theo dõi và kiểm soát trạng thái.

2. **State is Read-Only (Trạng thái chỉ đọc)**:
   - Bạn không thể trực tiếp thay đổi state. Muốn thay đổi, bạn phải gửi một **action** (một object mô tả thay đổi, ví dụ: `{ type: 'ADD_ITEM', payload: { id: 1, name: 'Sản phẩm A' } }`).
   - **Reducer** là một hàm xử lý action và tạo ra state mới (không sửa state cũ).

3. **Changes are Made with Pure Functions (Thay đổi được thực hiện bởi hàm thuần túy)**:
   - Reducer là các hàm **thuần túy (pure functions)**, nghĩa là cùng input sẽ luôn cho cùng output, không có tác dụng phụ (side effects).
   - Ví dụ reducer:
     ```javascript
     const cartReducer = (state = [], action) => {
       switch (action.type) {
         case 'ADD_ITEM':
           return [...state, action.payload];
         default:
           return state;
       }
     };
     ```

#### Các thành phần chính trong Redux:
- **Store**: Nơi lưu trữ trạng thái toàn cục (global state) của ứng dụng.
- **Action**: Object mô tả sự thay đổi (có thuộc tính `type` và thường có `payload` chứa dữ liệu).
- **Reducer**: Hàm nhận state hiện tại và action, trả về state mới.
- **Dispatch**: Hàm dùng để gửi action đến store để cập nhật state.

#### Ví dụ đơn giản:
Giả sử bạn xây dựng một ứng dụng giỏ hàng:
- **State**: `cart: [{ id: 1, name: 'Sản phẩm A' }]`.
- **Action**: `{ type: 'ADD_ITEM', payload: { id: 2, name: 'Sản phẩm B' } }`.
- **Reducer**: Xử lý action để thêm sản phẩm mới vào giỏ hàng.
- **Store**: Lưu trữ giỏ hàng và cung cấp state cho toàn bộ ứng dụng.

#### Khi nào dùng Redux?
- Khi ứng dụng có nhiều trạng thái phức tạp, cần chia sẻ giữa nhiều component.
- Khi cần theo dõi lịch sử thay đổi trạng thái hoặc debug dễ dàng.
- Ví dụ: Ứng dụng thương mại điện tử, mạng xã hội, hoặc ứng dụng quản lý dữ liệu lớn.

---

### 2. Redux Thunk là gì?

**Redux Thunk** là một **middleware** (phần mềm trung gian) cho Redux, giúp xử lý các **tác vụ bất đồng bộ (asynchronous tasks)** như gọi API, setTimeout, hoặc các tác vụ cần thời gian.

#### Vấn đề Redux Thunk giải quyết:
- Redux cơ bản chỉ xử lý các action là **object đồng bộ**. Ví dụ, bạn không thể gửi một action để gọi API trực tiếp, vì API là bất đồng bộ (phải chờ dữ liệu trả về).
- Redux Thunk cho phép action không chỉ là object mà còn có thể là **hàm (function)**, giúp bạn thực hiện các logic bất đồng bộ trước khi gửi action đến reducer.

#### Cách Redux Thunk hoạt động:
- Bình thường, action là một object: `{ type: 'FETCH_USERS_SUCCESS', payload: users }`.
- Với Redux Thunk, action có thể là một hàm (gọi là **thunk**), bên trong hàm này bạn có thể:
  - Gọi API.
  - Dispatch các action khác khi API hoàn tất.
  - Truy cập state hiện tại thông qua hàm `getState`.

#### Ví dụ:
Giả sử bạn muốn lấy danh sách người dùng từ API:
```javascript
// Action creator sử dụng Redux Thunk
const fetchUsers = () => {
  return async (dispatch, getState) => {
    dispatch({ type: 'FETCH_USERS_REQUEST' }); // Bắt đầu gọi API
    try {
      const response = await fetch('https://api.example.com/users');
      const users = await response.json();
      dispatch({ type: 'FETCH_USERS_SUCCESS', payload: users });
    } catch (error) {
      dispatch({ type: 'FETCH_USERS_FAILURE', payload: error.message });
    }
  };
};

// Cách sử dụng
store.dispatch(fetchUsers());
```

#### Các thuật ngữ:
- **Middleware**: Một lớp trung gian giữa việc dispatch action và reducer, cho phép can thiệp vào quá trình xử lý action.
- **Thunk**: Một hàm trả về một hàm khác, thường được dùng để trì hoãn thực thi logic.
- **Action Creator**: Hàm tạo ra action (có thể là object hoặc hàm với Redux Thunk).

#### Khi nào dùng Redux Thunk?
- Khi cần xử lý các tác vụ bất đồng bộ như gọi API, setTimeout, hoặc Promise.
- Khi muốn viết logic phức tạp trước khi dispatch action.

---

### 3. Redux Toolkit là gì?

**Redux Toolkit** (RTK) là bộ công cụ chính thức được tạo ra bởi đội ngũ Redux để **đơn giản hóa** việc sử dụng Redux. Nó giảm bớt các đoạn mã lặp lại (boilerplate code), cung cấp các API tiện lợi và tích hợp các best practices.

#### Vấn đề Redux Toolkit giải quyết:
- Redux cơ bản yêu cầu viết nhiều mã lặp lại (boilerplate code) như định nghĩa action types, action creators, và reducer.
- Redux Thunk phải được cài đặt và cấu hình riêng.
- Viết reducer dễ gây lỗi vì phải tự xử lý immutability (bất biến) của state.
- Redux Toolkit cung cấp các công cụ để:
  - Giảm mã lặp.
  - Tích hợp Redux Thunk mặc định.
  - Tự động xử lý immutability.
  - Cung cấp các API tiện lợi như `createSlice`, `createAsyncThunk`.

#### Các tính năng chính của Redux Toolkit:
1. **createSlice**:
   - Kết hợp action types, action creators và reducer thành một hàm duy nhất.
   - Tự động xử lý immutability (bạn có thể viết mã như thể sửa trực tiếp state, nhưng RTK dùng thư viện **Immer** để đảm bảo state không bị thay đổi trực tiếp).
   - Ví dụ:
     ```javascript
     import { createSlice } from '@reduxjs/toolkit';

     const cartSlice = createSlice({
       name: 'cart',
       initialState: [],
       reducers: {
         addItem(state, action) {
           state.push(action.payload); // Immer tự động đảm bảo immutability
         },
         removeItem(state, action) {
           return state.filter(item => item.id !== action.payload.id);
         },
       },
     });

     export const { addItem, removeItem } = cartSlice.actions;
     export default cartSlice.reducer;
     ```

2. **createAsyncThunk**:
   - Đơn giản hóa việc xử lý các tác vụ bất đồng bộ (tương tự Redux Thunk).
   - Tự động tạo ra các action cho trạng thái `pending`, `fulfilled`, và `rejected`.
   - Ví dụ:
     ```javascript
     import { createAsyncThunk, createSlice } from '@reduxjs/toolkit';

     const fetchUsers = createAsyncThunk('users/fetch', async () => {
       const response = await fetch('https://api.example.com/users');
       return await response.json();
     });

     const userSlice = createSlice({
       name: 'users',
       initialState: { users: [], status: 'idle', error: null },
       extraReducers: builder => {
         builder
           .addCase(fetchUsers.pending, state => {
             state.status = 'loading';
           })
           .addCase(fetchUsers.fulfilled, (state, action) => {
             state.status = 'succeeded';
             state.users = action.payload;
           })
           .addCase(fetchUsers.rejected, (state, action) => {
             state.status = 'failed';
             state.error = action.error.message;
           });
       },
     });

     export { fetchUsers };
     export default userSlice.reducer;
     ```

3. **configureStore**:
   - Thay thế cho `createStore` của Redux, tự động tích hợp Redux Thunk và các middleware khác, hỗ trợ Redux DevTools.
   - Ví dụ:
     ```javascript
     import { configureStore } from '@reduxjs/toolkit';
     import cartReducer from './cartSlice';
     import userReducer from './userSlice';

     const store = configureStore({
       reducer: {
         cart: cartReducer,
         users: userReducer,
       },
     });
     ```

#### Các thuật ngữ:
- **Slice**: Một phần của Redux state, bao gồm reducer và action creators liên quan.
- **Immutability**: Tính bất biến, đảm bảo state không bị thay đổi trực tiếp.
- **createAsyncThunk**: API để tạo action bất đồng bộ, trả về các action cho các trạng thái khác nhau (pending, fulfilled, rejected).
- **Redux DevTools**: Công cụ debug để theo dõi state và action trong Redux.

#### Khi nào dùng Redux Toolkit?
- Hầu hết các dự án Redux hiện nay đều nên dùng Redux Toolkit vì nó:
  - Giảm mã lặp.
  - Tích hợp các best practices.
  - Dễ học hơn cho người mới.
- Redux Toolkit là cách tiêu chuẩn để viết Redux hiện nay.

---

### So sánh Redux, Redux Thunk, và Redux Toolkit:

| **Tiêu chí**            | **Redux**                              | **Redux Thunk**                       | **Redux Toolkit**                     |
|--------------------------|---------------------------------------|---------------------------------------|---------------------------------------|
| **Chức năng chính**      | Quản lý trạng thái toàn cục          | Xử lý action bất đồng bộ             | Đơn giản hóa Redux, tích hợp Thunk   |
| **Độ phức tạp**          | Cao, nhiều mã lặp                   | Trung bình, cần cấu hình middleware   | Thấp, giảm mã lặp, API tiện lợi      |
| **Dùng khi nào?**        | Ứng dụng lớn, cần kiểm soát state   | Cần xử lý API hoặc tác vụ bất đồng bộ| Hầu hết các trường hợp dùng Redux    |
| **Ví dụ ứng dụng**       | Gửi action đồng bộ                  | Gọi API lấy dữ liệu                  | Quản lý state và API với ít mã hơn  |

---

### Lời khuyên cho người mới:
1. **Bắt đầu với Redux Toolkit**: Đừng học Redux cơ bản trước, vì Redux Toolkit là cách hiện đại và dễ học hơn. Nó bao gồm tất cả những gì bạn cần (kể cả Redux Thunk).
2. **Hiểu rõ luồng dữ liệu**: Trong Redux, dữ liệu đi theo luồng: **Component → Dispatch Action → Reducer → Store → Component**.
3. **Thực hành với dự án nhỏ**: Hãy thử xây dựng một ứng dụng như danh sách công việc (todo list) hoặc giỏ hàng để làm quen với Redux Toolkit.
4. **Sử dụng Redux DevTools**: Cài đặt extension Redux DevTools trên trình duyệt để dễ dàng debug state và action.

---

### Ví dụ tổng hợp với Redux Toolkit:
Dưới đây là một ví dụ nhỏ về ứng dụng quản lý giỏ hàng sử dụng Redux Toolkit:
```javascript
// cartSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// Thunk để lấy dữ liệu sản phẩm từ API
export const fetchProducts = createAsyncThunk('cart/fetchProducts', async () => {
  const response = await fetch('https://api.example.com/products');
  return await response.json();
});

const cartSlice = createSlice({
  name: 'cart',
  initialState: { items: [], status: 'idle', error: null },
  reducers: {
    addItem(state, action) {
      state.items.push(action.payload);
    },
    removeItem(state, action) {
      state.items = state.items.filter(item => item.id !== action.payload.id);
    },
  },
  extraReducers: builder => {
    builder
      .addCase(fetchProducts.pending, state => {
        state.status = 'loading';
      })
      .addCase(fetchProducts.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.items = action.payload;
      })
      .addCase(fetchProducts.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.error.message;
      });
  },
});

export const { addItem, removeItem } = cartSlice.actions;
export default cartSlice.reducer;

// store.js
import { configureStore } from '@reduxjs/toolkit';
import cartReducer from './cartSlice';

const store = configureStore({
  reducer: {
    cart: cartReducer,
  },
});

export default store;

// Trong component React
import { useDispatch, useSelector } from 'react-redux';
import { addItem, fetchProducts } from './cartSlice';

function Cart() {
  const dispatch = useDispatch();
  const cartItems = useSelector(state => state.cart.items);
  const status = useSelector(state => state.cart.status);

  const handleAddItem = () => {
    dispatch(addItem({ id: Date.now(), name: 'Sản phẩm mới' }));
  };

  useEffect(() => {
    dispatch(fetchProducts());
  }, [dispatch]);

  return (
    <div>
      <h2>Giỏ hàng</h2>
      {status === 'loading' && <p>Đang tải...</p>}
      <ul>
        {cartItems.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      <button onClick={handleAddItem}>Thêm sản phẩm</button>
    </div>
  );
}
```

