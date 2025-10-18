Thought for 2s### Giải Thích Chi Tiết Hệ Thống User Management

Tôi sẽ giải thích cách toàn bộ hệ thống hoạt động từ frontend → backend → database.

---

## **1. KIẾN TRÚC TỔNG QUAN**

```plaintext
┌─────────────────────────────────────────────────────────────┐
│                    FRONTEND (React + Vite)                  │
│  - Login/Register Pages                                     │
│  - User Management (CRUD)                                   │
│  - State Management (React Hooks)                           │
│  - API Calls (Fetch)                                        │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP Requests (JSON)
                         ↓
┌─────────────────────────────────────────────────────────────┐
│              BACKEND (.NET Core REST API)                   │
│  - AuthController (Login/Register)                          │
│  - UsersController (CRUD Operations)                        │
│  - JwtService (Token Generation)                            │
│  - Entity Framework (ORM)                                   │
└────────────────────────┬────────────────────────────────────┘
                         │ SQL Queries
                         ↓
┌─────────────────────────────────────────────────────────────┐
│              DATABASE (PostgreSQL)                          │
│  - Users Table                                              │
│  - Indexes, Constraints                                     │
└─────────────────────────────────────────────────────────────┘
```

---

## **2. FRONTEND - CẤU TRÚC VÀ HOẠT ĐỘNG**

### **2.1 Cấu Trúc Thư Mục**

```plaintext
src/
├── pages/
│   ├── Login.tsx          # Trang đăng nhập
│   ├── Register.tsx       # Trang đăng ký
│   ├── UserList.tsx       # Danh sách người dùng
│   ├── AddUser.tsx        # Thêm người dùng mới
│   └── EditUser.tsx       # Chỉnh sửa người dùng
├── components/
│   └── Navbar.tsx         # Navigation bar
├── models/
│   ├── user.ts            # Interface User
│   └── auth.ts            # Interface Auth
├── App.tsx                # Main component + routing
└── main.tsx               # Entry point
```

### **2.2 Models (TypeScript Interfaces)**

**src/models/user.ts**

```typescript
export interface User {
  id: number;
  fullName: string;
  email: string;
  phone: string;
  role: string;
  createdAt: string;
}
```

**src/models/auth.ts**

```typescript
export interface AuthResponse {
  token: string;
  user: {
    id: number;
    fullName: string;
    email: string;
    role: string;
  };
}
```

### **2.3 Luồng Đăng Ký (Register Flow)**

```plaintext
User nhập form (fullName, email, password, phone)
         ↓
Click "Register" button
         ↓
Frontend gọi API: POST /api/auth/register
{
  "fullName": "Phạm Gia Bảo",
  "email": "bao@gmail.com",
  "password": "123456",
  "phone": "0341273343"
}
         ↓
Backend nhận request
         ↓
Kiểm tra email đã tồn tại chưa
         ↓
Hash password (SHA256)
         ↓
Lưu user vào database
         ↓
Tạo JWT token
         ↓
Trả về response:
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": 1,
    "fullName": "Phạm Gia Bảo",
    "email": "bao@gmail.com",
    "role": "User"
  }
}
         ↓
Frontend lưu token vào localStorage
         ↓
Redirect tới trang UserList
```

**Code Frontend (Register.tsx)**

```typescript
const handleRegister = async (e) => {
  e.preventDefault();
  
  try {
    const response = await fetch('http://localhost:5298/api/auth/register', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        fullName,
        email,
        password,
        phone
      })
    });
    
    const data = await response.json();
    
    // Lưu token vào localStorage
    localStorage.setItem('token', data.token);
    localStorage.setItem('user', JSON.stringify(data.user));
    
    // Redirect
    navigate('/users');
  } catch (error) {
    setError('An error occurred. Please try again.');
  }
};
```

### **2.4 Luồng Đăng Nhập (Login Flow)**

```plaintext
User nhập email + password
         ↓
Click "Login" button
         ↓
Frontend gọi API: POST /api/auth/login
{
  "email": "bao@gmail.com",
  "password": "123456"
}
         ↓
Backend kiểm tra email có tồn tại không
         ↓
So sánh password (hash password nhập vào với hash trong DB)
         ↓
Nếu đúng: Tạo JWT token
Nếu sai: Trả về lỗi 401 Unauthorized
         ↓
Frontend lưu token + user info vào localStorage
         ↓
Redirect tới UserList
```

### **2.5 Luồng Lấy Danh Sách Người Dùng (Get Users)**

```plaintext
User truy cập trang /users
         ↓
Frontend gọi API: GET /api/users?page=1&orderBy=fullName&orderType=asc
(Kèm theo Authorization header: Bearer {token})
         ↓
Backend kiểm tra token hợp lệ không (JWT validation)
         ↓
Nếu token không hợp lệ: Trả về 401 Unauthorized
Nếu token hợp lệ: Tiếp tục
         ↓
Backend lấy dữ liệu từ database:
- Sắp xếp theo fullName (ascending)
- Phân trang (10 users/trang)
         ↓
Trả về response:
{
  "users": [
    {
      "id": 1,
      "fullName": "Phạm Gia Bảo",
      "email": "bao@gmail.com",
      "phone": "0341273343",
      "role": "User",
      "createdAt": "2024-10-18T10:30:00"
    },
    ...
  ],
  "amount": 25,        // Tổng số users
  "pagesAmount": 3     // Tổng số trang
}
         ↓
Frontend hiển thị danh sách users
```

**Code Frontend (UserList.tsx)**

```typescript
useEffect(() => {
  const fetchUsers = async () => {
    const token = localStorage.getItem('token');
    
    const response = await fetch(
      `http://localhost:5298/api/users?page=${page}&orderBy=${orderBy}&orderType=${orderType}`,
      {
        headers: {
          'Authorization': `Bearer ${token}`
        }
      }
    );
    
    const data = await response.json();
    setUsers(data.users);
    setPagesAmount(data.pagesAmount);
  };
  
  fetchUsers();
}, [page, orderBy, orderType]);
```

### **2.6 Luồng Thêm Người Dùng (Create User)**

```plaintext
Admin nhập form (fullName, email, password, phone)
         ↓
Click "Add User" button
         ↓
Frontend gọi API: POST /api/users
{
  "fullName": "Nguyễn Văn A",
  "email": "nguyenvana@gmail.com",
  "password": "123456",
  "phone": "0987654321"
}
(Kèm Authorization header)
         ↓
Backend kiểm tra token
         ↓
Kiểm tra email đã tồn tại chưa
         ↓
Hash password
         ↓
Lưu user mới vào database
         ↓
Trả về 201 Created
         ↓
Frontend refresh danh sách users
```

### **2.7 Luồng Chỉnh Sửa Người Dùng (Update User)**

```plaintext
Admin click "Edit" trên user
         ↓
Trang EditUser load dữ liệu user hiện tại
         ↓
Frontend gọi API: GET /api/users/{id}
         ↓
Backend trả về dữ liệu user
         ↓
Frontend hiển thị form với dữ liệu cũ
         ↓
Admin chỉnh sửa thông tin
         ↓
Click "Update User" button
         ↓
Frontend gọi API: PUT /api/users/{id}
{
  "fullName": "Phạm Gia Bảo Updated",
  "email": "bao.updated@gmail.com",
  "phone": "0341273343",
  "role": "Admin"
}
         ↓
Backend kiểm tra token
         ↓
Kiểm tra email không bị trùng với user khác
         ↓
Update user trong database
         ↓
Trả về 200 OK
         ↓
Frontend redirect tới UserList
```

**Code Frontend (EditUser.tsx)**

```typescript
const handleUpdate = async (e) => {
  e.preventDefault();
  const token = localStorage.getItem('token');
  
  const response = await fetch(
    `http://localhost:5298/api/users/${id}`,
    {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      body: JSON.stringify({
        fullName,
        email,
        phone,
        role
      })
    }
  );
  
  if (response.ok) {
    navigate('/users');
  }
};
```

### **2.8 Luồng Xóa Người Dùng (Delete User)**

```plaintext
Admin click "Delete" trên user
         ↓
Confirm dialog hiện lên
         ↓
Frontend gọi API: DELETE /api/users/{id}
(Kèm Authorization header)
         ↓
Backend kiểm tra token
         ↓
Xóa user khỏi database
         ↓
Trả về 200 OK
         ↓
Frontend refresh danh sách users
```

---

## **3. BACKEND - CẤU TRÚC VÀ HOẠT ĐỘNG**

### **3.1 Cấu Trúc Thư Mục**

```plaintext
UserManagementAPI/
├── Models/
│   ├── User.cs                    # Entity User
│   ├── LoginRequest.cs            # DTO
│   ├── RegisterRequest.cs         # DTO
│   ├── UpdateUserRequest.cs       # DTO
│   └── AuthResponse.cs            # DTO
├── Data/
│   └── ApplicationDbContext.cs    # DbContext (Entity Framework)
├── Services/
│   └── JwtService.cs              # JWT Token generation
├── Controllers/
│   ├── AuthController.cs          # Login/Register endpoints
│   └── UsersController.cs         # CRUD endpoints
├── Program.cs                     # Configuration
└── appsettings.json               # Settings
```

### **3.2 Models (C# Classes)**

**Models/User.cs** - Entity (đại diện cho bảng Users trong database)

```csharp
public class User
{
    public int Id { get; set; }
    public string FullName { get; set; }
    public string Email { get; set; }
    public string PasswordHash { get; set; }  // Mật khẩu được hash
    public string Phone { get; set; }
    public string Role { get; set; } = "User";
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
}
```

**Models/RegisterRequest.cs** - DTO (Data Transfer Object)

```csharp
public class RegisterRequest
{
    public string FullName { get; set; }
    public string Email { get; set; }
    public string Password { get; set; }      // Plain text password
    public string Phone { get; set; }
}
```

### **3.3 Entity Framework DbContext**

**Data/ApplicationDbContext.cs**

```csharp
public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) 
        : base(options) { }

    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>(entity =>
        {
            entity.HasKey(e => e.Id);
            entity.Property(e => e.Email).IsRequired().HasMaxLength(255);
            entity.Property(e => e.FullName).IsRequired().HasMaxLength(255);
            entity.HasIndex(e => e.Email).IsUnique();  // Email phải unique
        });
    }
}
```

**Giải thích:**

- `DbSet<User>` - Đại diện cho bảng Users
- `HasKey(e => e.Id)` - Id là primary key
- `HasIndex(e => e.Email).IsUnique()` - Email phải unique (không trùng)


### **3.4 JWT Service - Tạo Token**

**Services/JwtService.cs**

```csharp
public class JwtService : IJwtService
{
    public string GenerateToken(User user)
    {
        // Tạo secret key từ appsettings.json
        var key = new SymmetricSecurityKey(
            Encoding.UTF8.GetBytes(_configuration["Jwt:SecretKey"])
        );
        
        // Tạo signing credentials
        var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

        // Tạo claims (thông tin user được encode trong token)
        var claims = new[]
        {
            new Claim(ClaimTypes.NameIdentifier, user.Id.ToString()),
            new Claim(ClaimTypes.Email, user.Email),
            new Claim(ClaimTypes.Name, user.FullName),
            new Claim(ClaimTypes.Role, user.Role)
        };

        // Tạo JWT token
        var token = new JwtSecurityToken(
            issuer: _configuration["Jwt:Issuer"],
            audience: _configuration["Jwt:Audience"],
            claims: claims,
            expires: DateTime.UtcNow.AddHours(24),  // Token hết hạn sau 24 giờ
            signingCredentials: credentials
        );

        return new JwtSecurityTokenHandler().WriteToken(token);
    }
}
```

**JWT Token Structure:**

```plaintext
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

Gồm 3 phần:

1. **Header** - Thuật toán (HS256)
2. **Payload** - Claims (user info)
3. **Signature** - Chữ ký (để verify token)


### **3.5 AuthController - Đăng Ký & Đăng Nhập**

**Controllers/AuthController.cs**

```csharp
[HttpPost("register")]
public async Task<IActionResult> Register([FromBody] RegisterRequest request)
{
    // 1. Kiểm tra email đã tồn tại chưa
    if (await _context.Users.AnyAsync(u => u.Email == request.Email))
        return BadRequest(new { message = "Email already exists" });

    // 2. Tạo user mới
    var user = new User
    {
        FullName = request.FullName,
        Email = request.Email,
        Phone = request.Phone,
        PasswordHash = HashPassword(request.Password),  // Hash password
        Role = "User"
    };

    // 3. Lưu vào database
    _context.Users.Add(user);
    await _context.SaveChangesAsync();

    // 4. Tạo JWT token
    var token = _jwtService.GenerateToken(user);
    
    // 5. Trả về response
    return Ok(new AuthResponse
    {
        Token = token,
        User = new UserDto { ... }
    });
}

[HttpPost("login")]
public async Task<IActionResult> Login([FromBody] LoginRequest request)
{
    // 1. Tìm user theo email
    var user = await _context.Users.FirstOrDefaultAsync(u => u.Email == request.Email);
    
    // 2. Kiểm tra user tồn tại và password đúng
    if (user == null || !VerifyPassword(request.Password, user.PasswordHash))
        return Unauthorized(new { message = "Invalid email or password" });

    // 3. Tạo JWT token
    var token = _jwtService.GenerateToken(user);
    
    // 4. Trả về response
    return Ok(new AuthResponse { Token = token, User = ... });
}

private string HashPassword(string password)
{
    using (var sha256 = SHA256.Create())
    {
        var hashedBytes = sha256.ComputeHash(Encoding.UTF8.GetBytes(password));
        return Convert.ToBase64String(hashedBytes);
    }
}

private bool VerifyPassword(string password, string hash)
{
    var hashOfInput = HashPassword(password);
    return hashOfInput == hash;
}
```

**Giải thích Password Hashing:**

```plaintext
Plain password: "123456"
         ↓
SHA256 hash: "8d969eef6ecad3c29a3a873fba8a4f7f3c8c3c3c3c3c3c3c3c3c3c3c3c3c3c"
         ↓
Lưu hash vào database (KHÔNG lưu plain password)

Khi login:
Plain password: "123456"
         ↓
Hash lại: "8d969eef6ecad3c29a3a873fba8a4f7f3c8c3c3c3c3c3c3c3c3c3c3c3c3c3c"
         ↓
So sánh với hash trong database
         ↓
Nếu giống → Login thành công
```

### **3.6 UsersController - CRUD Operations**

**Controllers/UsersController.cs**

```csharp
[ApiController]
[Route("api/[controller]")]
[Authorize]  // Tất cả endpoints đều yêu cầu JWT token
public class UsersController : ControllerBase
{
    [HttpGet]
    public async Task<IActionResult> GetUsers(
        [FromQuery] int page = 1,
        [FromQuery] string orderBy = "fullName",
        [FromQuery] string orderType = "asc")
    {
        const int pageSize = 10;
        var query = _context.Users.AsQueryable();

        // Sorting
        if (orderType.ToLower() == "desc")
        {
            query = orderBy.ToLower() switch
            {
                "email" => query.OrderByDescending(u => u.Email),
                "fullname" => query.OrderByDescending(u => u.FullName),
                _ => query.OrderByDescending(u => u.FullName)
            };
        }
        else
        {
            query = orderBy.ToLower() switch
            {
                "email" => query.OrderBy(u => u.Email),
                "fullname" => query.OrderBy(u => u.FullName),
                _ => query.OrderBy(u => u.FullName)
            };
        }

        // Pagination
        var totalCount = await query.CountAsync();
        var pagesAmount = (int)Math.Ceiling((double)totalCount / pageSize);

        var users = await query
            .Skip((page - 1) * pageSize)  // Bỏ qua (page-1)*10 records
            .Take(pageSize)                // Lấy 10 records
            .Select(u => new { u.Id, u.FullName, u.Email, u.Phone, u.Role, u.CreatedAt })
            .ToListAsync();

        return Ok(new { users, amount = totalCount, pagesAmount });
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetUser(int id)
    {
        var user = await _context.Users.FindAsync(id);
        if (user == null)
            return NotFound(new { message = "User not found" });

        return Ok(new { user.Id, user.FullName, user.Email, user.Phone, user.Role, user.CreatedAt });
    }

    [HttpPost]
    public async Task<IActionResult> CreateUser([FromBody] RegisterRequest request)
    {
        if (await _context.Users.AnyAsync(u => u.Email == request.Email))
            return BadRequest(new { message = "Email already exists" });

        var user = new User
        {
            FullName = request.FullName,
            Email = request.Email,
            Phone = request.Phone,
            PasswordHash = HashPassword(request.Password),
            Role = "User"
        };

        _context.Users.Add(user);
        await _context.SaveChangesAsync();

        return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> UpdateUser(int id, [FromBody] UpdateUserRequest updateRequest)
    {
        var user = await _context.Users.FindAsync(id);
        if (user == null)
            return NotFound(new { message = "User not found" });

        // Kiểm tra email không bị trùng với user khác
        if (updateRequest.Email != user.Email && 
            await _context.Users.AnyAsync(u => u.Email == updateRequest.Email))
            return BadRequest(new { message = "Email already exists" });

        user.FullName = updateRequest.FullName;
        user.Email = updateRequest.Email;
        user.Phone = updateRequest.Phone;
        user.Role = updateRequest.Role;

        _context.Users.Update(user);
        await _context.SaveChangesAsync();

        return Ok(new { message = "User updated successfully" });
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> DeleteUser(int id)
    {
        var user = await _context.Users.FindAsync(id);
        if (user == null)
            return NotFound(new { message = "User not found" });

        _context.Users.Remove(user);
        await _context.SaveChangesAsync();

        return Ok(new { message = "User deleted successfully" });
    }
}
```

### **3.7 Program.cs - Configuration**

```csharp
var builder = WebApplication.CreateBuilder(args);

// 1. Add DbContext
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection"))
);

// 2. Add JWT Service
builder.Services.AddScoped<IJwtService, JwtService>();

// 3. Add CORS (cho phép frontend gọi API)
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowReactApp", policy =>
    {
        policy.WithOrigins("http://localhost:5173")  // Frontend URL
              .AllowAnyMethod()
              .AllowAnyHeader();
    });
});

// 4. Add Authentication (JWT)
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(secretKey),
            ValidateIssuer = true,
            ValidIssuer = jwtSettings["Issuer"],
            ValidateAudience = true,
            ValidAudience = jwtSettings["Audience"],
            ValidateLifetime = true
        };
    });

// 5. Add Controllers
builder.Services.AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = JsonNamingPolicy.CamelCase;
    });

var app = builder.Build();

app.UseCors("AllowReactApp");
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

---

## **4. DATABASE - POSTGRESQL**

### **4.1 Schema (Cấu Trúc Bảng)**

```sql
CREATE TABLE "Users" (
    "Id" SERIAL PRIMARY KEY,
    "FullName" VARCHAR(255) NOT NULL,
    "Email" VARCHAR(255) NOT NULL UNIQUE,
    "PasswordHash" TEXT NOT NULL,
    "Phone" VARCHAR(20),
    "Role" VARCHAR(50) DEFAULT 'User',
    "CreatedAt" TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON "Users"("Email");
```

**Giải thích:**

- `Id SERIAL PRIMARY KEY` - Auto-increment ID
- `Email VARCHAR(255) NOT NULL UNIQUE` - Email không được trùng
- `PasswordHash TEXT NOT NULL` - Lưu hash password
- `CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP` - Tự động ghi lại thời gian tạo


### **4.2 Dữ Liệu Mẫu**

```sql
INSERT INTO "Users" ("FullName", "Email", "PasswordHash", "Phone", "Role", "CreatedAt")
VALUES 
(
    'Phạm Gia Bảo',
    'bao@gmail.com',
    '8d969eef6ecad3c29a3a873fba8a4f7f3c8c3c3c3c3c3c3c3c3c3c3c3c3c3c',
    '0341273343',
    'Admin',
    NOW()
);
```

---

## **5. LUỒNG HOÀN CHỈNH - VÍ DỤ THỰC TẾ**

### **Scenario: User đăng ký tài khoản mới**

```plaintext
1. USER NHẬP FORM
   ┌─────────────────────────────────────┐
   │ Full Name: Nguyễn Văn A             │
   │ Email: nguyenvana@gmail.com         │
   │ Password: 123456                    │
   │ Phone: 0987654321                   │
   │ [Register Button]                   │
   └─────────────────────────────────────┘

2. FRONTEND GỬI REQUEST
   POST http://localhost:5298/api/auth/register
   Content-Type: application/json
   
   {
     "fullName": "Nguyễn Văn A",
     "email": "nguyenvana@gmail.com",
     "password": "123456",
     "phone": "0987654321"
   }

3. BACKEND XỬ LÝ
   a) Kiểm tra email đã tồn tại?
      SELECT * FROM "Users" WHERE "Email" = 'nguyenvana@gmail.com'
      → Không tồn tại ✓
   
   b) Hash password
      SHA256("123456") = "8d969eef6ecad3c29a3a873fba8a4f7f3c8c3c3c3c3c3c3c3c3c3c3c3c3c"
   
   c) Lưu user vào database
      INSERT INTO "Users" ("FullName", "Email", "PasswordHash", "Phone", "Role", "CreatedAt")
      VALUES ('Nguyễn Văn A', 'nguyenvana@gmail.com', '8d969eef...', '0987654321', 'User', NOW())
      → User ID = 2
   
   d) Tạo JWT token
      Claims:
      - NameIdentifier: "2"
      - Email: "nguyenvana@gmail.com"
      - Name: "Nguyễn Văn A"
      - Role: "User"
      
      Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

4. BACKEND TRẢ VỀ RESPONSE
   HTTP 200 OK
   
   {
     "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
     "user": {
       "id": 2,
       "fullName": "Nguyễn Văn A",
       "email": "nguyenvana@gmail.com",
       "role": "User"
     }
   }

5. FRONTEND LƯU DỮ LIỆU
   localStorage.setItem('token', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...')
   localStorage.setItem('user', '{"id":2,"fullName":"Nguyễn Văn A",...}')

6. REDIRECT
   navigate('/users')
   → Trang UserList load
```

### **Scenario: User đăng nhập**

```plaintext
1. USER NHẬP FORM
   ┌─────────────────────────────────────┐
   │ Email: nguyenvana@gmail.com         │
   │ Password: 123456                    │
   │ [Login Button]                      │
   └─────────────────────────────────────┘

2. FRONTEND GỬI REQUEST
   POST http://localhost:5298/api/auth/login
   
   {
     "email": "nguyenvana@gmail.com",
     "password": "123456"
   }

3. BACKEND XỬ LÝ
   a) Tìm user theo email
      SELECT * FROM "Users" WHERE "Email" = 'nguyenvana@gmail.com'
      → Tìm thấy user ID = 2
   
   b) Hash password nhập vào
      SHA256("123456") = "8d969eef6ecad3c29a3a873fba8a4f7f3c8c3c3c3c3c3c3c3c3c3c3c3c3c"
   
   c) So sánh với hash trong database
      "8d969eef..." == "8d969eef..." ✓
   
   d) Tạo JWT token
      Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

4. BACKEND TRẢ VỀ RESPONSE
   HTTP 200 OK
   {
     "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
     "user": { ... }
   }

5. FRONTEND LƯU TOKEN VÀ REDIRECT
   localStorage.setItem('token', '...')
   navigate('/users')
```

### **Scenario: Lấy danh sách users**

```plaintext
1. FRONTEND GỬI REQUEST
   GET http://localhost:5298/api/users?page=1&orderBy=fullName&orderType=asc
   Headers: {
     "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
   }

2. BACKEND KIỂM TRA TOKEN
   a) Lấy token từ Authorization header
      Token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
   
   b) Verify token
      - Kiểm tra signature (chữ ký)
      - Kiểm tra expiration (hết hạn chưa)
      - Kiểm tra issuer, audience
      → Token hợp lệ ✓
   
   c) Extract claims từ token
      - User ID: 2
      - Email: nguyenvana@gmail.com
      - Role: User

3. BACKEND QUERY DATABASE
   SELECT * FROM "Users"
   ORDER BY "FullName" ASC
   LIMIT 10 OFFSET 0
   
   → Trả về 10 users đầu tiên

4. BACKEND TRẢ VỀ RESPONSE
   HTTP 200 OK
   {
     "users": [
       {
         "id": 1,
         "fullName": "Nguyễn Văn A",
         "email": "nguyenvana@gmail.com",
         "phone": "0987654321",
         "role": "User",
         "createdAt": "2024-10-18T10:30:00"
       },
       ...
     ],
     "amount": 25,
     "pagesAmount": 3
   }

5. FRONTEND HIỂN THỊ DANH SÁCH
   ┌─────────────────────────────────────────────────────┐
   │ User Management System                              │
   ├─────────────────────────────────────────────────────┤
   │ ID │ Full Name      │ Email              │ Actions │
   ├────┼────────────────┼────────────────────┼─────────┤
   │ 1  │ Nguyễn Văn A   │ nguyenvana@...     │ Edit Del│
   │ 2  │ Phạm Gia Bảo   │ bao@gmail.com      │ Edit Del│
   │ 3  │ Trần Thị B     │ tranb@gmail.com    │ Edit Del│
   └─────────────────────────────────────────────────────┘
```

---

## **6. SECURITY - BẢO MẬT**

### **6.1 Password Hashing**

- Mật khẩu được hash bằng SHA256 trước khi lưu
- Không bao giờ lưu plain password
- Khi login, hash password nhập vào rồi so sánh


### **6.2 JWT Token**

- Token được ký bằng secret key
- Token có thời gian hết hạn (24 giờ)
- Mỗi request cần kèm token trong Authorization header
- Backend verify token trước khi xử lý request


### **6.3 CORS**

- Chỉ cho phép frontend từ `http://localhost:5173` gọi API
- Ngăn chặn cross-origin attacks


### **6.4 Database Constraints**

- Email phải unique (không trùng)
- Email, FullName là required
- Có index trên Email để tăng tốc độ query


---

## **7. TÓNG TẮT LUỒNG DỮ LIỆU**

```plaintext
┌──────────────────────────────────────────────────────────────┐
│                    FRONTEND (React)                          │
│  - User nhập form                                            │
│  - Gọi API (Fetch)                                           │
│  - Lưu token vào localStorage                               │
│  - Hiển thị dữ liệu                                          │
└────────────────────┬─────────────────────────────────────────┘
                     │ HTTP Request (JSON)
                     │ Authorization: Bearer {token}
                     ↓
┌──────────────────────────────────────────────────────────────┐
│                  BACKEND (.NET Core)                         │
│  - Nhận request                                              │
│  - Verify JWT token                                          │
│  - Validate dữ liệu                                          │
│  - Query/Update database                                     │
│  - Trả về response (JSON)                                    │
└────────────────────┬─────────────────────────────────────────┘
                     │ SQL Query
                     ↓
┌──────────────────────────────────────────────────────────────┐
│              DATABASE (PostgreSQL)                           │
│  - Lưu/Lấy dữ liệu                                           │
│  - Enforce constraints                                       │
│  - Trả về kết quả                                            │
└──────────────────────────────────────────────────────────────┘
```

---

Đó là toàn bộ cách hoạt động của hệ thống! Mỗi phần đều liên kết với nhau để tạo thành một ứng dụng quản lý người dùng hoàn chỉnh.