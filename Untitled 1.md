Tuyệt vời, bạn chọn hướng dùng Yarn là hợp lý cho một monorepo hiện đại. Dưới đây là hướng dẫn chi tiết để bạn tạo môi trường cho **frontend (React + Vite)** và **backend (Node.js + TypeScript + Express)** theo cấu trúc module, cùng với file `README.md` mẫu để thành viên dễ làm theo.

---

## 🌱 1. Khởi tạo Monorepo với Yarn

Tại thư mục `bloodline-dna-monorepo`:

```bash
yarn init -y
```

Thêm `workspaces` vào `package.json`:

```json
{
  "private": true,
  "workspaces": [
    "bloodline-dna-frontend",
    "bloodline-dna-backend"
  ]
}
```

---

## 📦 2. Backend Setup (Node.js + TS + Express)

Tại `bloodline-dna-backend/`:

### 🔧 Khởi tạo và cài gói:

```bash
yarn init -y

yarn add express dotenv reflect-metadata
yarn add typescript ts-node tsc-alias tsconfig-paths @types/node @types/express -D
yarn add eslint prettier eslint-config-prettier eslint-plugin-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser -D
yarn add rimraf nodemon -D
```

### 📁 Cấu trúc thư mục đề xuất:

```
src/
  ├── common/
  │   ├── config/
  │   ├── constants/
  │   ├── middlewares/
  │   └── utils/
  ├── modules/
  │   └── index.ts
  ├── app.ts
  └── server.ts
```

### ⚙️ File cấu hình

**tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "baseUrl": "./src",
    "paths": {
      "@common/*": ["common/*"],
      "@modules/*": ["modules/*"]
    }
  }
}
```

**nodemon.json**

```json
{
  "watch": ["src"],
  "ext": "ts",
  "ignore": ["dist"],
  "exec": "ts-node -r tsconfig-paths/register src/server.ts"
}
```

**.eslintrc.json**

```json
{
  "parser": "@typescript-eslint/parser",
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],
  "plugins": ["@typescript-eslint"],
  "env": {
    "node": true,
    "es2021": true
  }
}
```

**.prettierrc**

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

---

## ⚛️ 3. Frontend Setup (React + Vite + TS + Tailwind)

Tại `bloodline-dna-frontend/`:

### 🚀 Khởi tạo với Vite:

```bash
yarn create vite . --template react-ts
```

### 🌬️ Cài Tailwind:

```bash
yarn add -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

**tailwind.config.js**

```js
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

**src/index.css**

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### ✨ Thêm Prettier + ESLint:

```bash
yarn add -D eslint prettier eslint-plugin-react eslint-config-prettier eslint-plugin-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

**.eslintrc.json**

```json
{
  "parser": "@typescript-eslint/parser",
  "extends": [
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],
  "plugins": ["react", "@typescript-eslint"],
  "settings": {
    "react": {
      "version": "detect"
    }
  },
  "env": {
    "browser": true,
    "es2021": true
  }
}
```

---

## 📘 4. Tạo README.md cho mỗi repo

### ✅ `bloodline-dna-frontend/README.md`

````md
# Bloodline DNA Frontend

## Cài đặt
```bash
yarn install
````

## Phát triển

```bash
yarn dev
```

## Build production

```bash
yarn build
```

## Format và lint

```bash
yarn lint
yarn format
```

````

### ✅ `bloodline-dna-backend/README.md`
```md
# Bloodline DNA Backend

## Cài đặt
```bash
yarn install
````

## Chạy server

```bash
yarn dev
```

## Scripts:

* `yarn build`: Compile TypeScript
* `yarn lint`: Kiểm tra lint
* `yarn format`: Format code

## Cấu trúc thư mục

* `src/common`: Các cấu hình chung
* `src/modules`: Các module chức năng

```

---

Bạn muốn mình tạo luôn tất cả file và gửi dạng `.zip`, hay bạn muốn từng bước copy code để tự chạy local?
```