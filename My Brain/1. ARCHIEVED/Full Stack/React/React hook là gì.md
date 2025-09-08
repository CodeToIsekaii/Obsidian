---
date: ""
---
Related : [[React]]
Tag: #reacthook
___
Tôi sẽ giải thích **React Hooks** một cách chi tiết, dễ hiểu bằng tiếng Việt, dành cho người mới học code. Tôi sẽ giải thích từng hook phổ biến, cách chúng hoạt động, và tại sao chúng quan trọng trong React, với ngôn ngữ đơn giản và ví dụ thực tế. Các thuật ngữ chuyên ngành sẽ được giải thích rõ ràng để bạn không bị bối rối.

---

## **React Hooks là gì?**

**React Hooks** là một tính năng được giới thiệu trong **React 16.8** (năm 2018), cho phép bạn sử dụng các tính năng của React (như quản lý trạng thái, xử lý vòng đời) trong **functional components** mà không cần viết **class components**. Trước Hooks, để quản lý trạng thái hoặc xử lý các tác vụ phức tạp, bạn phải dùng class components, nhưng chúng phức tạp và khó đọc hơn. Hooks giúp code ngắn gọn, dễ hiểu, và dễ tái sử dụng.

- **Dễ hiểu**: Hãy tưởng tượng Hooks như những công cụ đặc biệt trong hộp đồ nghề của React. Chúng giúp bạn thêm các tính năng "thông minh" (như lưu trữ dữ liệu, phản ứng với sự thay đổi) vào các thành phần giao diện (components) mà không cần viết code dài dòng như trước.

---

## **Tại sao cần Hooks?**

1. **Đơn giản hóa code**: Hooks cho phép viết code ngắn gọn hơn so với class components.
2. **Tái sử dụng logic**: Hooks giúp bạn tách biệt logic (như lấy dữ liệu, xử lý sự kiện) để dùng lại ở nhiều nơi.
3. **Dễ hiểu hơn**: Code với Hooks thường dễ đọc và ít lỗi hơn so với class components (tránh các vấn đề như `this` trong JavaScript).
4. **Thay thế class components**: Functional components với Hooks có thể làm mọi thứ mà class components làm, nhưng dễ dàng hơn.

- **Dễ hiểu**: Trước đây, để làm một căn nhà (component) thông minh, bạn phải xây bằng gạch (class components) và tốn nhiều công sức. Hooks giống như các khối Lego, giúp bạn xây nhanh hơn, đẹp hơn, và dễ sửa chữa.

---

## **Các React Hooks phổ biến**

Dưới đây, tôi sẽ giải thích từng Hook phổ biến, cách chúng hoạt động, và ví dụ minh họa. Tôi sẽ giải thích cả các thuật ngữ chuyên ngành để bạn hiểu rõ.

### **1. useState**

**useState** là Hook dùng để **quản lý trạng thái** (state) trong functional components. Trạng thái là dữ liệu mà component lưu trữ và có thể thay đổi, ví dụ như giá trị của một ô input hoặc trạng thái "đang tải".

- **Thuật ngữ**:
  - **State**: Dữ liệu mà component giữ và có thể thay đổi (như tên người dùng, số lượng sản phẩm trong giỏ hàng).
  - **Functional Component**: Một hàm JavaScript trả về giao diện (JSX), đơn giản hơn class components.

**Cách hoạt động**:
- `useState` nhận một giá trị ban đầu (initial state).
- Trả về một mảng gồm 2 phần:
  1. Giá trị trạng thái hiện tại.
  2. Hàm để cập nhật trạng thái.
- Khi bạn gọi hàm cập nhật, React sẽ **render lại** (vẽ lại) component với giá trị mới.

**Cú pháp**:
```javascript
const [state, setState] = useState(initialState)
```

**Ví dụ**:
```javascript
import React, { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0) // Khởi tạo state count = 0

  return (
    <div>
      <p>Bạn đã nhấn {count} lần</p>
      <button onClick={() => setCount(count + 1)}>Nhấn tôi</button>
    </div>
  )
}
```

- **Giải thích**:
  - `useState(0)`: Tạo trạng thái `count` với giá trị ban đầu là `0`.
  - `count`: Giá trị hiện tại của trạng thái (bắt đầu là `0`).
  - `setCount`: Hàm để cập nhật giá trị của `count`.
  - Khi nhấn nút, `setCount(count + 1)` tăng `count` lên 1, và React vẽ lại giao diện với giá trị mới.
- **Dễ hiểu**: Tưởng tượng `count` như một tờ giấy ghi số `0`. Mỗi lần nhấn nút, bạn xóa số cũ và viết số mới (tăng lên 1). React sẽ hiển thị tờ giấy mới lên màn hình.

---

### **2. useEffect**

**useEffect** dùng để xử lý **side effects** (hiệu ứng phụ) trong functional components. Side effects là các tác vụ không liên quan trực tiếp đến việc vẽ giao diện, như gọi API, cập nhật tiêu đề trang, hoặc lắng nghe sự kiện.

- **Thuật ngữ**:
  - **Side Effect**: Các tác vụ xảy ra ngoài việc render giao diện, như lấy dữ liệu từ server, thay đổi DOM, hoặc đăng ký sự kiện.
  - **Cleanup**: Dọn dẹp các side effects (như hủy đăng ký sự kiện) để tránh lỗi hoặc rò rỉ bộ nhớ.

**Cách hoạt động**:
- `useEffect` nhận một hàm (chứa side effect) và một mảng phụ thuộc (dependency array).
- Hàm trong `useEffect` chạy sau mỗi lần render (hoặc khi các giá trị trong mảng phụ thuộc thay đổi).
- Nếu trả về một hàm từ `useEffect`, hàm đó sẽ chạy để dọn dẹp trước khi component bị xóa hoặc trước khi `useEffect` chạy lại.

**Cú pháp**:
```javascript
useEffect(() => {
  // Side effect code
  return () => {
    // Cleanup code (tùy chọn)
  }
}, [dependencies])
```

**Ví dụ**:
```javascript
import React, { useState, useEffect } from 'react'

function Timer() {
  const [seconds, setSeconds] = useState(0)

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(seconds => seconds + 1)
    }, 1000)

    return () => clearInterval(interval) // Cleanup
  }, []) // Mảng rỗng: chạy một lần khi component được tạo

  return <p>Thời gian: {seconds} giây</p>
}
```

- **Giải thích**:
  - `useState(0)`: Tạo trạng thái `seconds` để đếm số giây.
  - `useEffect`: Thiết lập một bộ đếm (setInterval) để tăng `seconds` mỗi giây.
  - Mảng phụ thuộc `[]`: Chỉ chạy một lần khi component được tạo.
  - Hàm cleanup `clearInterval`: Dừng bộ đếm khi component bị xóa để tránh lỗi.
- **Dễ hiểu**: Tưởng tượng `useEffect` như một người quản lý đồng hồ. Khi bạn bật đồng hồ (component được tạo), nó bắt đầu đếm giây. Khi bạn tắt đồng hồ (component bị xóa), nó dừng lại để tiết kiệm pin.

---

### **3. useContext**

**useContext** dùng để truy cập **Context** (ngữ cảnh) trong React, giúp chia sẻ dữ liệu giữa các components mà không cần truyền props qua nhiều cấp.

- **Thuật ngữ**:
  - **Context**: Một cơ chế của React để chia sẻ dữ liệu (như thông tin người dùng, theme) cho nhiều components mà không cần truyền thủ công.
  - **Props**: Dữ liệu được truyền từ component cha sang component con.

**Cách hoạt động**:
- Bạn tạo một Context bằng `React.createContext`.
- Dùng `Context.Provider` để cung cấp dữ liệu.
- Dùng `useContext` để lấy dữ liệu từ Context trong bất kỳ component nào.

**Cú pháp**:
```javascript
const value = useContext(MyContext)
```

**Ví dụ** (từ mã bạn cung cấp - `AuthContext.tsx`):
```javascript
import React, { createContext, useContext, useState } from 'react'

const ThemeContext = createContext()

function App() {
  const [theme, setTheme] = useState('light')

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Toolbar />
    </ThemeContext.Provider>
  )
}

function Toolbar() {
  const { theme, setTheme } = useContext(ThemeContext)

  return (
    <div>
      <p>Theme hiện tại: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Đổi theme
      </button>
    </div>
  )
}
```

- **Giải thích**:
  - `createContext`: Tạo một Context để lưu trữ thông tin theme.
  - `ThemeContext.Provider`: Cung cấp `theme` và `setTheme` cho các component con.
  - `useContext(ThemeContext)`: Lấy giá trị từ Context để sử dụng trong `Toolbar`.
- **Dễ hiểu**: Tưởng tượng Context như một máy phát wifi, gửi tín hiệu (dữ liệu) đến mọi thiết bị (components) trong nhà. `useContext` là cách một thiết bị kết nối để nhận tín hiệu.

---

### **4. useReducer**

**useReducer** là một Hook thay thế cho `useState` khi bạn cần quản lý trạng thái phức tạp hoặc có nhiều hành động liên quan.

- **Thuật ngữ**:
  - **Reducer**: Một hàm nhận trạng thái hiện tại và một hành động (action), trả về trạng thái mới.
  - **Action**: Một đối tượng mô tả hành động cần thực hiện (như `{ type: 'increment' }`).
  - **Dispatch**: Hàm để gửi action đến reducer.

**Cách hoạt động**:
- `useReducer` nhận một hàm reducer và trạng thái ban đầu.
- Trả về mảng gồm trạng thái hiện tại và hàm `dispatch` để gửi hành động.

**Cú pháp**:
```javascript
const [state, dispatch] = useReducer(reducer, initialState)
```

**Ví dụ**:
```javascript
import React, { useReducer } from 'react'

const initialState = { count: 0 }

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 }
    case 'decrement':
      return { count: state.count - 1 }
    default:
      return state
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState)

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Tăng</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Giảm</button>
    </div>
  )
}
```

- **Giải thích**:
  - `initialState`: Trạng thái ban đầu với `count = 0`.
  - `reducer`: Hàm xử lý hành động (`increment` tăng count, `decrement` giảm count).
  - `dispatch`: Gửi hành động đến reducer để cập nhật trạng thái.
- **Dễ hiểu**: Tưởng tượng `useReducer` như một nhân viên ngân hàng. Bạn gửi yêu cầu (action) như "rút tiền" hoặc "gửi tiền", và nhân viên cập nhật số dư (state) theo yêu cầu.

---

### **5. useRef**

**useRef** tạo một **tham chiếu** (reference) có thể lưu trữ giá trị không bị reset khi component render lại. Nó thường được dùng để truy cập DOM hoặc lưu trữ giá trị tạm thời.

- **Thuật ngữ**:
  - **Ref**: Một đối tượng có thuộc tính `current` để lưu giá trị bền vững qua các lần render.
  - **DOM**: Cấu trúc HTML của trang web mà bạn có thể truy cập và thay đổi.

**Cách hoạt động**:
- `useRef` tạo một đối tượng ref với thuộc tính `current`.
- Giá trị của `current` không bị reset khi component render lại.

**Cú pháp**:
```javascript
const ref = useRef(initialValue)
```

**Ví dụ**:
```javascript
import React, { useRef } from 'react'

function TextInput() {
  const inputRef = useRef(null)

  const focusInput = () => {
    inputRef.current.focus()
  }

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Tập trung vào input</button>
    </div>
  )
}
```

- **Giải thích**:
  - `useRef(null)`: Tạo một ref để lưu tham chiếu đến phần tử input.
  - `inputRef.current`: Truy cập phần tử input trong DOM để gọi phương thức `focus()`.
- **Dễ hiểu**: Tưởng tượng `useRef` như một chiếc bút đánh dấu trang trong sách. Bạn có thể dùng nó để đánh dấu một ô input trên giao diện và quay lại bất cứ lúc nào.

---

### **6. useMemo**

**useMemo** dùng để **tối ưu hóa hiệu suất** bằng cách ghi nhớ (memoize) kết quả của một phép tính nặng và chỉ tính lại khi các giá trị phụ thuộc thay đổi.

- **Thuật ngữ**:
  - **Memoization**: Lưu kết quả của một phép tính để tránh tính lại nếu đầu vào không thay đổi.

**Cách hoạt động**:
- `useMemo` nhận một hàm tính toán và mảng phụ thuộc.
- Chỉ chạy lại hàm nếu các giá trị trong mảng phụ thuộc thay đổi.

**Cú pháp**:
```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])
```

**Ví dụ**:
```javascript
import React, { useState, useMemo } from 'react'

function ExpensiveCalculation() {
  const [count, setCount] = useState(0)
  const [other, setOther] = useState(0)

  const expensiveResult = useMemo(() => {
    console.log('Tính toán nặng...')
    return count * 1000
  }, [count])

  return (
    <div>
      <p>Kết quả: {expensiveResult}</p>
      <button onClick={() => setCount(count + 1)}>Tăng count</button>
      <button onClick={() => setOther(other + 1)}>Tăng other</button>
    </div>
  )
}
```

- **Giải thích**:
  - `useMemo`: Chỉ tính lại `expensiveResult` khi `count` thay đổi.
  - Nếu `other` thay đổi, `useMemo` không chạy lại hàm tính toán.
- **Dễ hiểu**: Tưởng tượng bạn phải tính toán một bài toán phức tạp. Thay vì làm lại mỗi lần, bạn ghi đáp án ra giấy và chỉ tính lại khi số liệu thay đổi.

---

### **7. useCallback**

**useCallback** giống `useMemo`, nhưng dùng để ghi nhớ **hàm** thay vì giá trị, giúp tránh tạo lại hàm khi component render.

**Cách hoạt động**:
- `useCallback` nhận một hàm và mảng phụ thuộc.
- Trả về một phiên bản ghi nhớ của hàm, chỉ thay đổi khi phụ thuộc thay đổi.

**Cú pháp**:
```javascript
const memoizedCallback = useCallback(() => {
  doSomething(a, b)
}, [a, b])
```

**Ví dụ**:
```javascript
import React, { useState, useCallback } from 'react'

function Button({ onClick }) {
  console.log('Button rendered')
  return <button onClick={onClick}>Nhấn tôi</button>
}

function App() {
  const [count, setCount] = useState(0)

  const handleClick = useCallback(() => {
    console.log('Button clicked', count)
  }, [count])

  return (
    <div>
      <Button onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>Tăng count</button>
    </div>
  )
}
```

- **Giải thích**:
  - `useCallback`: Ghi nhớ hàm `handleClick`, chỉ tạo lại khi `count` thay đổi.
  - Ngăn `Button` render lại không cần thiết.
- **Dễ hiểu**: Tưởng tượng bạn có một nút bấm. Thay vì tạo mới nút mỗi lần, bạn giữ nguyên nút cũ trừ khi có lý do cần thay đổi.

---

## **Quy tắc sử dụng Hooks**

1. **Chỉ gọi Hooks ở cấp cao nhất**: Không gọi Hooks trong vòng lặp, điều kiện, hoặc hàm lồng nhau.
2. **Chỉ gọi Hooks trong functional components hoặc custom Hooks**: Không dùng trong hàm thông thường.
3. **Tên custom Hooks bắt đầu bằng `use`**: Ví dụ, `useCustomHook`.

- **Dễ hiểu**: Hooks giống như công cụ đặc biệt chỉ hoạt động trong những căn phòng đặc biệt (functional components). Bạn phải sử dụng chúng đúng cách để tránh làm hỏng ngôi nhà (ứng dụng).

---

## **Ví dụ tổng hợp (kết hợp nhiều Hooks)**

```javascript
import React, { useState, useEffect, useContext } from 'react'
import { AuthContext } from './AuthContext'

function UserProfile() {
  const { user, login } = useContext(AuthContext)
  const [message, setMessage] = useState('')

  useEffect(() => {
    if (user) {
      setMessage(`Chào mừng ${user.email}`)
    } else {
      setMessage('Vui lòng đăng nhập')
    }
  }, [user])

  return (
    <div>
      <p>{message}</p>
      <button onClick={() => login({ email: 'test@gmail.com', password: '123456' })}>
        Đăng nhập
      </button>
    </div>
  )
}
```

- **Giải thích**:
  - `useContext`: Lấy thông tin người dùng từ `AuthContext`.
  - `useState`: Quản lý thông điệp hiển thị.
  - `useEffect`: Cập nhật thông điệp khi `user` thay đổi.
- **Dễ hiểu**: Component này như một nhân viên lễ tân, kiểm tra thông tin người dùng (từ Context), hiển thị lời chào, và cho phép đăng nhập.

---

## **Tóm tắt**

- **useState**: Quản lý dữ liệu thay đổi (như số đếm, nội dung ô input).
- **useEffect**: Xử lý các tác vụ phụ (như lấy dữ liệu, chạy đồng hồ).
- **useContext**: Chia sẻ dữ liệu cho nhiều components (như thông tin người dùng).
- **useReducer**: Quản lý trạng thái phức tạp với nhiều hành động.
- **useRef**: Lưu trữ tham chiếu hoặc giá trị bền vững.
- **useMemo**: Tối ưu hóa phép tính nặng.
- **useCallback**: Tối ưu hóa hàm để tránh tạo lại không cần thiết.

Hooks giống như các công cụ trong hộp đồ nghề, giúp bạn xây dựng ứng dụng React dễ dàng, linh hoạt, và hiệu quả hơn. Nếu bạn cần giải thích thêm về bất kỳ Hook nào hoặc muốn ví dụ cụ thể hơn, hãy cho tôi biết!

