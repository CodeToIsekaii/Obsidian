---
date: ""
---
Related : [[]]
Tag: #
___

POST /api/auth/forgot-password 200 3321.838 ms - 80Error in resetPassword: JsonWebTokenError: invalid token at Object.module.exports [as verify] (D:\aFPT\SWP\bloodline-dna-monorepo\bloodline-dna-backend\node_modules\jsonwebtoken\verify.js:75:17) at resetPassword (D:\aFPT\SWP\bloodline-dna-monorepo\bloodline-dna-backend\src\services\authService.ts:320:25) at processTicksAndRejections (node:internal/process/task_queues:105:5) at async resetPasswordHandler (D:\aFPT\SWP\bloodline-dna-monorepo\bloodline-dna-backend\src\controllers\authController.ts:271:20)POST /api/auth/reset-password 200

### 1. **Backend - authService.ts**:

- **Encode token trong URL**: Sử dụng `encodeURIComponent(resetToken)` khi tạo URL reset password để tránh lỗi với các ký tự đặc biệt trong JWT token
- **Decode token khi xử lý**: Trong hàm `resetPassword`, thêm logic để decode token nếu cần thiết
- **Xử lý lỗi JWT tốt hơn**: Thêm try-catch để xử lý trường hợp token không cần decode


### 2. **Frontend - ResetPassword.tsx**:

- **Decode token từ URL**: Thêm logic để decode token khi lấy từ URL parameters
- **Xử lý lỗi decode**: Nếu decode lỗi, sử dụng token gốc


## Nguyên nhân lỗi:

JWT token chứa các ký tự đặc biệt như dấu chấm (.), dấu gạch ngang (-), và ký tự base64 có thể gây lỗi khi truyền qua URL. Khi không được encode/decode đúng cách, token bị thay đổi và không thể verify được.

## Cách hoạt động sau khi sửa:

1. **Forgot Password**: Token được encode trước khi đưa vào URL email
2. **Email**: URL chứa token đã được encode an toàn
3. **Reset Password**: Token được decode trước khi verify và cập nhật mật khẩu
4. **Database**: Mật khẩu mới được hash và lưu đúng cách

   -------- 


   ## Thay đổi trong Backend:

6. **authService.ts**:

7. Thêm console.log để track token trong `forgotPassword` và `resetPassword`
8. Đơn giản hóa logic xử lý token - không encode trong service, chỉ encode trong email
9. Xóa bỏ logic decode phức tạp



10. **email.ts**:

11. Encode token trong URL bằng `encodeURIComponent(token)`
12. Thêm console.log để track URL được tạo
13. Cải thiện HTML email với button và link backup





## Thay đổi trong Frontend:

3. **ResetPassword.tsx**:

4. Thêm console.log để track token từ URL và sau khi decode
5. Decode token bằng `decodeURIComponent()`
6. Thêm error handling tốt hơn cho việc decode token





## Cách hoạt động:

1. **Forgot Password**: Tạo JWT token → Gửi email với token được encode trong URL
2. **Email**: Chứa URL với token đã encode: `?token=encoded_jwt_token`
3. **Frontend**: Lấy token từ URL → Decode → Gửi về backend
4. **Backend**: Nhận token đã decode → Verify JWT → Cập nhật password
