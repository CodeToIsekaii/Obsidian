---
date: 2025-07-29T17:06:00
---
Related : [[]]
Tag: #
___


Tôi sẽ giải thích từng phần của mã nguồn mà bạn cung cấp một cách chi tiết, dễ hiểu bằng tiếng Việt, dành cho người mới học code. Tôi sẽ chia nhỏ các file và giải thích từng dòng hoặc khối mã để bạn dễ theo dõi. Mục tiêu là giúp bạn hiểu cách hoạt động của hệ thống xác thực (authentication) trong ứng dụng này, bao gồm đăng ký, đăng nhập, làm mới token, đổi mật khẩu, và các chức năng liên quan.

---

### **1. File `apiClient` (Quản lý kết nối API với Axios)**

File này tạo ra một **instance** (bản thể) của Axios, một thư viện dùng để gửi các yêu cầu HTTP (như GET, POST) tới server. Nó được cấu hình để thêm token xác thực và xử lý làm mới token khi hết hạn.

#### **Giải thích từng phần:**

```typescript
import axios, { type AxiosInstance, type AxiosResponse, type InternalAxiosRequestConfig } from 'axios'
```
- **Dòng này**: Nhập thư viện `axios` và một số kiểu dữ liệu (`AxiosInstance`, `AxiosResponse`, `InternalAxiosRequestConfig`) từ Axios để TypeScript biết cách kiểm tra kiểu dữ liệu khi lập trình.
- **Dễ hiểu**: Hãy tưởng tượng `axios` như một người đưa thư, giúp gửi và nhận thông tin từ server. Các kiểu dữ liệu giống như hướng dẫn để người đưa thư biết cách xử lý thư đúng cách.

```typescript
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL || 'http://localhost:5000/api'
```
- **Dòng này**: Định nghĩa URL gốc của API (địa chỉ server). Nó lấy giá trị từ biến môi trường (`VITE_API_BASE_URL`) hoặc dùng mặc định là `http://localhost:5000/api` nếu không có biến môi trường.
- **Dễ hiểu**: Đây giống như địa chỉ nhà của server mà ứng dụng sẽ liên lạc. Nếu không tìm thấy địa chỉ cụ thể, nó mặc định là một địa chỉ cục bộ trên máy tính.

```typescript
export const apiClient: AxiosInstance = axios.create({
  baseURL: API_BASE_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
})
```
- **Dòng này**: Tạo một instance của Axios gọi là `apiClient` với các cấu hình:
  - `baseURL`: Đặt URL gốc cho mọi yêu cầu (như `/login` sẽ thành `http://localhost:5000/api/login`).
  - `timeout`: Giới hạn thời gian chờ là 10 giây. Nếu server không trả lời trong 10 giây, yêu cầu sẽ bị hủy.
  - `headers`: Đặt kiểu dữ liệu gửi đi là JSON.
- **Dễ hiểu**: Giống như bạn thiết lập một chiếc điện thoại thông minh để gọi đến một số tổng đài cụ thể, với thời gian chờ tối đa và định dạng tin nhắn là JSON.

```typescript
apiClient.interceptors.request.use(
  (config: InternalAxiosRequestConfig) => {
    const token = localStorage.getItem('accessToken')
    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  (error) => {
    return Promise.reject(error)
  }
)
```
- **Dòng này**: Thêm một **interceptor** (bộ chặn) cho các yêu cầu gửi đi. Trước khi gửi yêu cầu:
  - Lấy `accessToken` từ `localStorage` (nơi lưu trữ dữ liệu tạm thời trên trình duyệt).
  - Nếu có token, thêm nó vào tiêu đề `Authorization` với định dạng `Bearer <token>` (chuẩn xác thực phổ biến).
  - Nếu có lỗi, trả về lỗi đó.
- **Dễ hiểu**: Giống như bạn gắn một thẻ nhận dạng (token) vào mỗi lá thư trước khi gửi đi để server biết bạn là ai. Nếu không có thẻ, thư vẫn được gửi nhưng không có thông tin xác thực.

```typescript
apiClient.interceptors.response.use(
  (response: AxiosResponse) => {
    return response
  },
  async (error) => {
    const originalRequest = error.config
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true
      try {
        const refreshToken = localStorage.getItem('refreshToken')
        if (refreshToken) {
          const { data } = await axios.post(`${API_BASE_URL}/auth/refresh-token`, {
            refreshToken
          })
          const { accessToken } = data
          localStorage.setItem('accessToken', accessToken)
          console.log(localStorage.getItem('accessToken')) // 👉 "new_token"
          originalRequest.headers.Authorization = `Bearer ${accessToken}`
          return apiClient(originalRequest)
        }
      } catch (refreshError) {
        localStorage.removeItem('accessToken')
        localStorage.removeItem('refreshToken')
        window.location.href = '/login'
        return Promise.reject(refreshError)
      }
    }
    return Promise.reject(error)
  }
)
```
- **Dòng này**: Thêm interceptor cho các phản hồi từ server:
  - Nếu phản hồi thành công, trả về phản hồi.
  - Nếu có lỗi với mã trạng thái `401` (Unauthorized) và yêu cầu chưa được thử lại (`_retry` là `false`):
    - Đặt `_retry` thành `true` để tránh lặp vô hạn.
    - Lấy `refreshToken` từ `localStorage`.
    - Gửi yêu cầu tới `/auth/refresh-token` để lấy `accessToken` mới.
    - Lưu `accessToken` mới vào `localStorage` và cập nhật tiêu đề `Authorization`.
    - Thử lại yêu cầu gốc với token mới.
    - Nếu làm mới token thất bại, xóa token và chuyển hướng người dùng đến trang đăng nhập (`/login`).
- **Dễ hiểu**: Nếu server trả lời "Bạn không được phép vì thẻ nhận dạng (token) hết hạn", hệ thống sẽ tự động dùng một thẻ dự phòng (refreshToken) để xin thẻ mới. Nếu xin thẻ mới thành công, nó tiếp tục gửi thư. Nếu thất bại, bạn bị "đá" về trang đăng nhập.

```typescript
export default apiClient
```
- **Dòng này**: Xuất `apiClient` để các file khác có thể sử dụng.
- **Dễ hiểu**: Gói chiếc điện thoại thông minh này lại để các bộ phận khác trong ứng dụng có thể dùng nó để liên lạc với server.

---

### **2. File `AuthContext.tsx` (Quản lý trạng thái xác thực trong React)**

File này tạo ra một **React Context** để quản lý trạng thái xác thực (như thông tin người dùng, trạng thái đăng nhập) và cung cấp các hàm như đăng nhập, đăng ký, đăng xuất, đổi mật khẩu.

#### **Giải thích từng phần:**

```typescript
import React, { createContext, useEffect, useState } from 'react'
import { useNavigate } from 'react-router-dom'
import { authService } from '../services/authService'
import type { LoginRequest, RegisterRequest, ChangePasswordRequest, User, RegisterResponse } from '../utils/types'
```
- **Dòng này**: Nhập các thành phần từ React (`createContext`, `useEffect`, `useState`) để quản lý trạng thái, `useNavigate` từ `react-router-dom` để điều hướng, `authService` để gọi các hàm xác thực, và các kiểu dữ liệu từ `types`.
- **Dễ hiểu**: Đây là bước chuẩn bị, giống như bạn lấy nguyên liệu để nấu một món ăn: React để quản lý giao diện, công cụ điều hướng, và dịch vụ xác thực.

```typescript
interface AuthContextType {
  user: User | null
  loading: boolean
  login: (data: LoginRequest) => Promise<void>
  register: (data: RegisterRequest) => Promise<RegisterResponse>
  logout: () => Promise<void>
  changePassword: (data: ChangePasswordRequest) => Promise<void>
  refreshUserData: () => Promise<void>
  isAuthenticated: boolean
  isAdmin: boolean
  isManager: boolean
  isStaff: boolean
  isCustomer: boolean
}
```
- **Dòng này**: Định nghĩa kiểu dữ liệu `AuthContextType` cho Context, bao gồm:
  - `user`: Thông tin người dùng (hoặc `null` nếu chưa đăng nhập).
  - `loading`: Trạng thái tải (đang kiểm tra đăng nhập hay không).
  - `login`, `register`, `logout`, `changePassword`, `refreshUserData`: Các hàm xử lý xác thực.
  - `isAuthenticated`, `isAdmin`, `isManager`, `isStaff`, `isCustomer`: Các biến kiểm tra trạng thái và vai trò.
- **Dễ hiểu**: Đây như một danh sách các món ăn bạn có thể phục vụ: thông tin người dùng, các thao tác (đăng nhập, đăng ký, v.v.), và các câu hỏi (đã đăng nhập chưa? Là admin không?).

```typescript
export const AuthContext = createContext<AuthContextType | undefined>(undefined)
```
- **Dòng này**: Tạo Context để lưu trữ trạng thái xác thực. Giá trị mặc định là `undefined`.
- **Dễ hiểu**: Tạo một chiếc hộp để chứa tất cả thông tin xác thực, nhưng chưa có gì trong hộp.

```typescript
export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
```
- **Dòng này**: Tạo component `AuthProvider` để bọc các thành phần con (`children`) và cung cấp Context.
- **Dễ hiểu**: Tưởng tượng `AuthProvider` là một người quản lý, phân phát thông tin xác thực cho các thành phần con (như các trang trong ứng dụng).
#### 📌 Nó có ý nghĩa như sau:

- **`AuthProvider`** là một **React Component**.
- Nó nhận props có tên là **`children`** — tức là **các component con** nằm bên trong khi bạn dùng `<AuthProvider>...</AuthProvider>`.
- Mục tiêu của nó là **cung cấp thông tin xác thực (auth)** cho toàn bộ ứng dụng, thông qua **React Context API**.
#### 🎯 Vậy `children` là gì?
- `children` là **những thứ nằm trong cặp thẻ `AuthProvider`** khi bạn dùng nó trong app.
##### Ví dụ:
``<AuthProvider>   <App />  ← đây chính là `children` </AuthProvider>``
#### 📦 Vai trò của `AuthProvider`
- Giống như **một người quản lý đăng nhập** cho cả ứng dụng.
- Bất kỳ component nào bên trong nó đều có thể **truy cập thông tin người dùng**, **token**, hoặc gọi `login()` mà không cần truyền thủ công từng cấp.
#### 🧠 Hình dung đơn giản:
##### 🏢 Một văn phòng:
- `AuthProvider` là **người quản lý văn phòng** (cung cấp thẻ nhân viên, kiểm tra danh tính,...).
- `children` là **những nhân viên** làm việc trong văn phòng đó (component con).
- Nhờ có `AuthProvider`, mọi nhân viên **đều biết ai là người đăng nhập, có quyền gì, và có thể logout/login bất cứ lúc nào**.

```typescript
const [user, setUser] = useState<User | null>(null)
const [loading, setLoading] = useState(true)
const navigate = useNavigate()
```
- **Dòng này**: Khởi tạo trạng thái:
  - `user`: Lưu thông tin người dùng (bắt đầu là `null`).
  - `loading`: Bắt đầu là `true` (đang tải dữ liệu).
  - `navigate`: Hàm để điều hướng giữa các trang.
- **Dễ hiểu**: Tạo một biến để lưu thông tin người dùng, một biến kiểm tra xem đang tải hay không, và một công cụ để chuyển hướng người dùng.

```typescript
useEffect(() => {
  const currentUser = authService.getCurrentUser()
  if (currentUser && authService.isAuthenticated()) {
    setUser(currentUser)
  }
  setLoading(false)
}, [])
```
- **Dòng này**: Khi ứng dụng khởi động, kiểm tra xem có người dùng nào đã đăng nhập chưa (bằng cách gọi `authService.getCurrentUser` và `authService.isAuthenticated`). Nếu có, cập nhật trạng thái `user`. Sau đó, đặt `loading` thành `false`.
- **Dễ hiểu**: Khi bạn mở ứng dụng, nó kiểm tra xem bạn đã đăng nhập trước đó chưa. Nếu có, nó lưu thông tin người dùng; sau đó báo rằng đã tải xong.

```typescript
const login = async (data: LoginRequest) => {
  try {
    const response = await authService.login(data)
    localStorage.setItem('accessToken', response.accessToken)
    localStorage.setItem('refreshToken', response.refreshToken)
    localStorage.setItem('user', JSON.stringify(response.user))
    setUser(response.user)
    const role = response.user.role
    switch (role) {
      case 'Admin':
        navigate('/admin/dashboard')
        break
      case 'Manager':
        navigate('/manager/dashboard')
        break
      case 'Staff':
        navigate('/staff/dashboard')
        break
      case 'Customer':
      default:
        navigate('/customer/dashboard')
        break
    }
  } catch (error) {
    console.error('Login error:', error)
    throw error
  }
}
```
- **Dòng này**: Hàm `login`:
  - Gọi `authService.login` để đăng nhập.
  - Lưu `accessToken`, `refreshToken`, và thông tin người dùng vào `localStorage`.
  - Cập nhật trạng thái `user`.
  - Điều hướng đến trang phù hợp dựa trên vai trò (`role`) của người dùng.
  - Nếu có lỗi, in lỗi ra console và ném lỗi.
- **Dễ hiểu**: Khi người dùng đăng nhập, hệ thống gửi thông tin đăng nhập đến server, nhận lại token và thông tin người dùng, lưu chúng vào trình duyệt, và đưa người dùng đến trang đúng với vai trò của họ (admin, khách hàng, v.v.).

```typescript
const register = async (data: RegisterRequest) => {
  try {
    const response = await authService.register(data)
    return response
  } catch (error) {
    console.error('Register error:', error)
    throw error
  }
}
```
- **Dòng này**: Hàm `register` gọi `authService.register` để đăng ký người dùng và trả về kết quả. Nếu có lỗi, in lỗi và ném lỗi.
- **Dễ hiểu**: Khi người dùng đăng ký, hệ thống gửi thông tin đến server và trả về kết quả (thành công hay thất bại).

```typescript
const logout = async () => {
  try {
    await authService.logout()
  } catch (error) {
    console.error('Logout error:', error)
  } finally {
    localStorage.removeItem('accessToken')
    localStorage.removeItem('refreshToken')
    localStorage.removeItem('user')
    localStorage.removeItem('role')
    setUser(null)
    navigate('/login')
  }
}
```
- **Dòng này**: Hàm `logout`:
  - Gọi `authService.logout` để đăng xuất.
  - Xóa tất cả thông tin (token, người dùng, vai trò) khỏi `localStorage`.
  - Đặt `user` thành `null` và chuyển hướng về trang đăng nhập.
- **Dễ hiểu**: Khi đăng xuất, hệ thống xóa hết thông tin người dùng và đưa bạn về trang đăng nhập.

```typescript
const changePassword = async (data: ChangePasswordRequest) => {
  try {
    await authService.changePassword(data)
  } catch (error) {
    console.error('Change password error:', error)
    throw error
  }
}
```
- **Dòng này**: Hàm `changePassword` gọi `authService.changePassword` để đổi mật khẩu. Nếu có lỗi, in lỗi và ném lỗi.
- **Dễ hiểu**: Gửi yêu cầu đổi mật khẩu đến server và báo lỗi nếu có vấn đề.

```typescript
const refreshUserData = async () => {
  try {
    const userData = await authService.getProfile()
    setUser(userData)
  } catch (error) {
    console.error('Refresh user data error:', error)
    throw error
  }
}
```
- **Dòng này**: Hàm `refreshUserData` lấy thông tin người dùng mới nhất từ server và cập nhật trạng thái `user`.
- **Dễ hiểu**: Tải lại thông tin người dùng từ server để đảm bảo dữ liệu luôn mới.

```typescript
const isAuthenticated = !!user
const isAdmin = user?.role === 'Admin'
const isManager = user?.role === 'Manager'
const isStaff = user?.role === 'Staff'
const isCustomer = user?.role === 'Customer'
```
- **Dòng này**: Kiểm tra trạng thái:
  - `isAuthenticated`: Kiểm tra xem có người dùng hay không.
  - `isAdmin`, `isManager`, `isStaff`, `isCustomer`: Kiểm tra vai trò của người dùng.
- **Dễ hiểu**: Các câu hỏi để kiểm tra xem người dùng đã đăng nhập chưa và họ thuộc vai trò nào.

```typescript
const value: AuthContextType = {
  user,
  loading,
  login,
  register,
  logout,
  changePassword,
  refreshUserData,
  isAuthenticated,
  isAdmin,
  isManager,
  isStaff,
  isCustomer
}
return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>
```
- **Dòng này**: Gói tất cả trạng thái và hàm vào một đối tượng `value` và cung cấp nó qua `AuthContext.Provider` để các thành phần con sử dụng.
- **Dễ hiểu**: Đưa tất cả thông tin và công cụ vào một chiếc hộp và chia sẻ nó cho các phần khác của ứng dụng.

---

### **3. File `authMiddleware.ts` (Middleware xác thực và phân quyền)**

File này định nghĩa các middleware để kiểm tra token xác thực và vai trò người dùng trên server.

#### **Giải thích từng phần:**

```typescript
import { config } from '../config/config'
import type { Request, Response, NextFunction } from 'express'
import jwt from 'jsonwebtoken'
```
- **Dòng này**: Nhập cấu hình (`config`), các kiểu của Express (`Request`, `Response`, `NextFunction`), và thư viện `jsonwebtoken` để xử lý token.
- **Dễ hiểu**: Chuẩn bị các công cụ để kiểm tra thẻ nhận dạng (token) và xử lý yêu cầu từ client.

```typescript
export type Role = 'Admin' | 'Manager' | 'Staff' | 'Customer'
export interface AuthUser {
  accountId: number
  email: string
  role: Role
}
```
- **Dòng này**: Định nghĩa kiểu `Role` (vai trò) và giao diện `AuthUser` (thông tin người dùng trong token).
- **Dễ hiểu**: Xác định các vai trò có thể có và cấu trúc thông tin người dùng.

```typescript
declare global {
  namespace Express {
    interface Request {
      user?: AuthUser
    }
  }
}
```
- **Dòng này**: Mở rộng giao diện `Request` của Express để thêm thuộc tính `user` chứa thông tin người dùng.
- **Dễ hiểu**: Thêm một ô trống vào yêu cầu để lưu thông tin người dùng sau khi xác thực.

```typescript
export const authenticate = (req: AuthRequest, res: Response, next: NextFunction) => {
  const authHeader = req.headers['authorization']
  if (!authHeader) {
    res.status(401).json({ message: 'Authorization header is missing' })
    return
  }
  const token = authHeader.split(' ')[1]
  if (!token) {
    res.status(401).json({ message: 'Token is missing in the Authorization header' })
    return
  }
  try {
    const payload = jwt.verify(token, config.jwt.secret) as AuthUser
    req.user = payload
    next()
  } catch (error) {
    res.status(401).json({ message: 'Invalid or expired token' })
  }
}
```
- **Dòng này**: Hàm `authenticate`:
  - Kiểm tra tiêu đề `Authorization` trong yêu cầu.
  - Lấy token từ tiêu đề (định dạng `Bearer <token>`).
  - Xác minh token bằng `jwt.verify` với khóa bí mật (`config.jwt.secret`).
  - Nếu token hợp lệ, lưu thông tin người dùng vào `req.user` và chuyển tiếp yêu cầu (`next`).
  - Nếu có lỗi (token không hợp lệ hoặc hết hạn), trả về lỗi 401.
- **Dễ hiểu**: Kiểm tra thẻ nhận dạng trong yêu cầu. Nếu thẻ hợp lệ, cho phép tiếp tục; nếu không, từ chối.

```typescript
export const authorize = (allowedRoles: string[]) => {
  return (req: AuthRequest, res: Response, next: NextFunction): void => {
    const user = req.user
    if (!user) {
      res.status(401).json({ message: 'Unauthorized: No user found in request' })
      return
    }
    const userRole = user.role.toLowerCase()
    const normalizedAllowedRoles = allowedRoles.map((role) => role.toLowerCase())
    if (!normalizedAllowedRoles.includes(userRole)) {
      res.status(403).json({
        message: `Forbidden: User role '${user.role}' is not allowed to access this resource`
      })
      return
    }
    next()
  }
}
```
- **Dòng này**: Hàm `authorize`:
  - Nhận danh sách các vai trò được phép (`allowedRoles`).
  - Kiểm tra xem người dùng (`req.user`) có vai trò nằm trong danh sách hay không (so sánh không phân biệt hoa thường).
  - Nếu không có quyền, trả về lỗi 403; nếu có, tiếp tục.
- **Dễ hiểu**: Kiểm tra xem người dùng có vai trò phù hợp để truy cập một tài nguyên hay không. Ví dụ, chỉ admin mới vào được trang admin.

---

### **4. File `authRoutes.ts` (Định tuyến API xác thực)**

File này định nghĩa các tuyến đường (routes) API cho xác thực, như đăng ký, đăng nhập, làm mới token, v.v.

#### **Giải thích từng phần:**

```typescript
import { Router } from 'express'
import {
  forgotPasswordHandler,
  loginHandler,
  logoutHandler,
  PasswordChangeHandler,
  refreshAccessTokenHandler,
  registerHandler,
  resetPasswordHandler
} from '../controllers/authController'
import { authenticate, authorize } from '../middlewares/authMiddleware'
import { userController } from '../controllers/userController'
```
- **Dòng này**: Nhập `Router` từ Express, các hàm xử lý từ `authController`, middleware từ `authMiddleware`, và `userController`.
- **Dễ hiểu**: Chuẩn bị các công cụ để định nghĩa các đường dẫn API (như `/login`, `/register`) và các hàm xử lý yêu cầu.

```typescript
const router = Router()
```
- **Dòng này**: Tạo một router để định nghĩa các tuyến đường.
- **Dễ hiểu**: Tạo một bảng chỉ đường để server biết xử lý yêu cầu nào.

```typescript
router.post('/register', registerHandler)
router.post('/login', loginHandler)
router.post('/refresh-token', refreshAccessTokenHandler)
router.delete('/logout', authenticate, logoutHandler)
router.post('/forgot-password', forgotPasswordHandler)
router.post('/reset-password', resetPasswordHandler)
router.get('/profile', authenticate, userController.getProfile)
router.put('/change-password', authenticate, PasswordChangeHandler)
router.put('/profile', authenticate, userController.updateProfile)
```
- **Dòng này**: Định nghĩa các tuyến đường:
  - `POST /register`: Xử lý đăng ký.
  - `POST /login`: Xử lý đăng nhập.
  - `POST /refresh-token`: Làm mới token.
  - `DELETE /logout`: Đăng xuất (yêu cầu xác thực).
  - `POST /forgot-password`: Quên mật khẩu.
  - `POST /reset-password`: Đặt lại mật khẩu.
  - `GET /profile`: Lấy thông tin người dùng (yêu cầu xác thực).
  - `PUT /change-password`: Đổi mật khẩu (yêu cầu xác thực).
  - `PUT /profile`: Cập nhật hồ sơ (yêu cầu xác thực).
- **Dễ hiểu**: Mỗi tuyến đường như một địa chỉ cụ thể trên server, ví dụ `/login` là nơi xử lý đăng nhập.

---

### **5. File `authController.ts` (Xử lý logic cho các tuyến đường xác thực)**

File này chứa các hàm xử lý logic cho các tuyến đường xác thực.

#### **Giải thích từng phần:**

```typescript
export const registerHandler = async (req: AuthRequest, res: Response): Promise<void> => {
  const { Email, PasswordHash, ConfirmPassword, FullName, PhoneNumber, Address, DateOfBirth, SignatureImage } = req.body
  if (!Email || !PasswordHash || !ConfirmPassword) {
    res.status(400).json({ message: 'Thiếu email hoặc mật khẩu' })
    return
  }
  const passwordregex = /^.{6,12}$/
  if (!passwordregex.test(PasswordHash)) {
    res.status(400).json({ message: 'Mật khẩu phải có độ dài từ 6 đến 12 ký tự' })
    return
  }
  const emailRegex = /^[a-zA-Z0-9._%+-]+@gmail\.com$/
  if (!emailRegex.test( alacsony
    res.status(400).json({ message: 'Email không hợp lệ' })
    return
  }
  const phoneRegex = /^0\d{9}$/
  if (!phoneRegex.test(PhoneNumber)) {
    res.status(400).json({ message: 'Phone Number không hợp lệ' })
    return
  }
  const pool = await getDbPool()
  const phoneNumber1 = await pool.request().input('phoneNumber', PhoneNumber).query(`
    SELECT PhoneNumber FROM UserProfiles WHERE PhoneNumber = @phoneNumber
  `)
  if (phoneNumber1.recordset.length > 0) {
    res.status(409).json({ message: 'Phone Number đã bị đứng' })
    return
  }
  try {
    const user = await register(
      Email,
      PasswordHash,
      ConfirmPassword,
      FullName,
      PhoneNumber,
      Address,
      DateOfBirth,
      SignatureImage
    )
    if (!user) {
      res.status(409).json({ message: 'Email đã tồn tại' })
      return
    }
    res.status(201).json({ message: 'Đăng ký thành công', success: true })
  } catch (error: unknown) {
    console.log(error)
    if (error instanceof Error) {
      res.status(500).json({ message: error.message })
    } else {
      res.status(500).json({ message: 'Đã xảy ra lỗi không xác định' })
    }
  }
}
```
- **Dòng này**: Hàm `registerHandler`:
  - Lấy dữ liệu từ yêu cầu (`req.body`).
  - Kiểm tra xem có thiếu email, mật khẩu, hoặc xác nhận mật khẩu không.
  - Kiểm tra mật khẩu (6-12 ký tự), email (phải là Gmail), và số điện thoại (bắt đầu bằng 0, 10 chữ số).
  - Kiểm tra số điện thoại có bị trùng trong cơ sở dữ liệu không.
  - Gọi hàm `register` từ `authService` để tạo tài khoản.
  - Trả về thông báo thành công hoặc lỗi.
- **Dễ hiểu**: Kiểm tra thông tin người dùng nhập khi đăng ký (email, mật khẩu, số điện thoại, v.v.) có hợp lệ không, sau đó gọi hàm đăng ký và trả kết quả.

```typescript
export const loginHandler = async (req: AuthRequest, res: Response): Promise<void> => {
  const { Email, PasswordHash } = req.body
  if (!Email || !PasswordHash) {
    res.status(400).json({ message: 'Thiếu email hoặc mật khẩu' })
    return
  }
  try {
    const tokens = await login(Email, PasswordHash)
    if (!tokens) {
      res.status(401).json({ message: 'Thông tin đăng nhập không hợp lệ' })
      return
    }
    res.json({
      accessToken: tokens.accessToken,
      refreshToken: tokens.refreshToken,
      success: true,
      user: tokens.payload
    })
  } catch (error: unknown) {
    if (error instanceof Error) {
      console.log(Error)
      res.status(500).json({ message: error.message })
    } else {
      res.status(500).json({ message: 'Đã xảy ra lỗi không xác định' })
    }
  }
}
```
- **Dòng này**: Hàm `loginHandler`:
  - Kiểm tra email và mật khẩu.
  - Gọi hàm `login` từ `authService`.
  - Trả về token và thông tin người dùng nếu thành công, hoặc lỗi nếu thất bại.
- **Dễ hiểu**: Xử lý đăng nhập bằng cách kiểm tra thông tin và trả về token để người dùng tiếp tục sử dụng.

Các hàm khác như `PasswordChangeHandler`, `refreshAccessTokenHandler`, `logoutHandler`, `forgotPasswordHandler`, và `resetPasswordHandler` hoạt động tương tự, xử lý các yêu cầu như đổi mật khẩu, làm mới token, đăng xuất, quên mật khẩu, và đặt lại mật khẩu.

---

### **6. File `authService.ts` (Logic chính của xác thực)**

File này chứa logic cốt lõi cho các chức năng xác thực, tương tác với cơ sở dữ liệu và tạo token.

#### **Giải thích từng phần:**

```typescript
export const register = async (
  email: string,
  password: string,
  confirmpassword: string,
  fullname: string,
  phoneNumber: string,
  address: string,
  dateOfBirth: string,
  signatureImage: string
) => {
  if (password !== confirmpassword) {
    throw new Error('Mật khẩu không trùng khớp')
  }
  const pool = await getDbPool()
  const result = await pool.request().input('email', email).query(`
    SELECT Email FROM Accounts WHERE Email = @Email
  `)
  if (result.recordset.length > 0) {
    throw new Error('Email đã tồn tại')
  }
  const roleAcc = await pool.request().input('name', 'Customer').query(`
    SELECT * FROM Roles WHERE RoleName = @name
  `)
  if (roleAcc.recordset.length === 0) {
    throw new Error('Role không tồn tại')
  }
  const role = roleAcc.recordset[0]
  const passwordHash = await bcrypt.hash(password, 10)
  const i = await pool
    .request()
    .input('email', email)
    .input('password', passwordHash)
    .input('role_id', role.RoleID)
    .query(`
      INSERT INTO Accounts (Email, PasswordHash, RoleID)
      VALUES (@email, @password, @role_id)
    `)
  const createdAccount = await pool.request().input('email', email).query(`
      SELECT * FROM Accounts WHERE Email = @email`)
  await pool
    .request()
    .input('Acid', createdAccount.recordset[0].AccountID)
    .input('name', fullname)
    .input('Phone', phoneNumber)
    .input('Address', address)
    .input('DateOfBirth', dateOfBirth)
    .input('SignatureImage', signatureImage)
    .query(
      'Insert into UserProfiles(AccountID,FullName,PhoneNumber,Address,DateOfBirth,SignatureImage) VALUES (@Acid,@name,@Phone,@Address,@DateOfBirth,@SignatureImage)'
    )
  return createdAccount.recordset[0]
}
```
- **Dòng này**: Hàm `register`:
  - Kiểm tra mật khẩu và xác nhận mật khẩu có khớp không.
  - Kiểm tra email đã tồn tại trong cơ sở dữ liệu chưa.
  - Lấy ID của vai trò `Customer`.
  - Mãprompt hóa mật khẩu bằng `bcrypt`.
  - Thêm tài khoản vào bảng `Accounts` và thông tin người dùng vào bảng `UserProfiles`.
  - Trả về thông tin tài khoản vừa tạo.
- **Dễ hiểu**: Tạo tài khoản mới bằng cách kiểm tra thông tin, mã hóa mật khẩu, và lưu vào cơ sở dữ liệu.

```typescript
export const login = async (email: string, password: string) => {
  const pool = await getDbPool()
  const result = await pool.request().input('email', email).query(`
    SELECT a.AccountID, a.Email, a.PasswordHash, r.RoleName AS role_name 
    FROM Accounts a
    JOIN Roles r ON a.RoleID = r.RoleID
    WHERE a.Email = @email
  `)
  if (result.recordset.length === 0) return null
  const user = result.recordset[0]
  const valid = await bcrypt.compare(password, user.PasswordHash)
  if (!valid) return null
  const payload = {
    accountId: user.AccountID,
    email: user.Email,
    role: user.role_name
  }
  const accessToken = generateAccessToken(payload)
  const refreshToken = await generateRefreshToken(payload)
  return { accessToken, refreshToken, payload }
}
```
- **Dòng này**: Hàm `login`:
  - Tìm tài khoản theo email.
  - So sánh mật khẩu nhập vào với mật khẩu đã mã hóa.
  - Tạo `accessToken` và `refreshToken` nếu mật khẩu đúng.
  - Trả về token và thông tin người dùng.
- **Dễ hiểu**: Kiểm tra email và mật khẩu, nếu đúng thì cấp token để người dùng truy cập ứng dụng.

Các hàm khác trong file này (`generateAccessToken`, `generateRefreshToken`, `verifyRefreshToken`, `PasswordChange`, `forgotPassword`, `resetPassword`) xử lý các chức năng như tạo token, làm mới token, đổi mật khẩu, và khôi phục mật khẩu.

---

### **Tóm tắt cách hệ thống hoạt động:**

1. **Client-side (React)**:
   - `AuthContext` quản lý trạng thái người dùng và cung cấp các hàm như đăng nhập, đăng xuất.
   - `apiClient` gửi yêu cầu đến server, tự động thêm token xác thực và làm mới token khi cần.

2. **Server-side (Express)**:
   - `authMiddleware` kiểm tra token và vai trò người dùng.
   - `authRoutes` định nghĩa các tuyến đường API.
   - `authController` xử lý logic cho các tuyến đường (gọi `authService`).
   - `authService` tương tác với cơ sở dữ liệu, mã hóa mật khẩu, tạo token, và gửi email.

3. **Quy trình xác thực**:
   - Người dùng đăng nhập → Nhận `accessToken` và `refreshToken`.
   - `accessToken` dùng để xác thực các yêu cầu; nếu hết hạn, `refreshToken` được dùng để lấy token mới.
   - Token được xác minh bằng `jwt.verify` và lưu thông tin người dùng vào yêu cầu.
   - Các chức năng như đổi mật khẩu, quên mật khẩu được bảo vệ bằng token hoặc email xác thực.

---

### **Dành cho người mới học code:**

Hãy tưởng tượng hệ thống này như một ngân hàng:
- **Client (React)**: Là bạn, người muốn rút tiền, gửi thông tin (email, mật khẩu) đến ngân hàng.
- **Server (Express)**: Là ngân hàng, kiểm tra thông tin của bạn và cấp thẻ (token) để bạn sử dụng.
- **Token**: Như thẻ ngân hàng, dùng để chứng minh bạn là ai khi yêu cầu dịch vụ.
- **Middleware**: Như bảo vệ ngân hàng, kiểm tra thẻ của bạn trước khi cho bạn vào.
- **Database**: Như két sắt, lưu thông tin tài khoản và mật khẩu của bạn.

Hy vọng giải thích này giúp bạn hiểu rõ cách hệ thống hoạt động! Nếu bạn cần giải thích chi tiết hơn về một phần cụ thể, hãy cho tôi biết.