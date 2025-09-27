# Tailwind CSS 完整安裝指南

這是一個詳細的指南，教你如何在新的 React + Vite 專案中正確安裝和配置 Tailwind CSS。

## 目錄

- [快速開始](#快速開始)
- [詳細步驟](#詳細步驟)
- [配置文件](#配置文件)
- [測試配置](#測試配置)
- [常見問題](#常見問題)
- [版本差異](#版本差異)

## 快速開始

### 一鍵命令（推薦）

```bash
# 建立新專案
npm create vite@latest my-tailwind-project -- --template react
cd my-tailwind-project
npm install

# 安裝 Tailwind CSS v4（最新版）
npm install --save-dev @tailwindcss/vite @tailwindcss/postcss tailwindcss autoprefixer postcss
```

## 詳細步驟

### 步驟 1：建立 React + Vite 專案

```bash
# 建立新專案
npm create vite@latest my-project -- --template react
cd my-project
npm install
```

### 步驟 2：選擇 Tailwind CSS 版本並安裝

#### 方法一：Tailwind CSS v4（推薦，最新版）

```bash
# 安裝 Tailwind CSS v4 相關套件
npm install --save-dev @tailwindcss/vite @tailwindcss/postcss tailwindcss autoprefixer postcss
```

**優點：**

- 更簡潔的配置
- 更好的性能
- 更多的新功能

#### 方法二：Tailwind CSS v3（穩定版）

```bash
# 安裝 Tailwind CSS v3 相關套件
npm install --save-dev tailwindcss postcss autoprefixer
# 初始化配置文件
npx tailwindcss init -p
```

**優點：**

- 更穩定
- 社群支援更完整
- 文檔更豐富

### 步驟 3：配置文件設定

#### Tailwind CSS v4 配置

1. **創建或修改 `postcss.config.js`**

```javascript
export default {
  plugins: {
    '@tailwindcss/postcss': {},
    autoprefixer: {},
  },
}
```

2. **修改 `vite.config.js`**

```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

#### Tailwind CSS v3 配置

1. **`tailwind.config.js`**（通過 `npx tailwindcss init -p` 自動生成）

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

2. **`postcss.config.js`**（通過 `npx tailwindcss init -p` 自動生成）

```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

### 步驟 4：設定 CSS 文件

#### 對於 Tailwind CSS v4

在 `src/index.css` 中**替換**所有內容為：

```css
@import 'tailwindcss';
```

#### 對於 Tailwind CSS v3

在 `src/index.css` 中**替換**所有內容為：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 步驟 5：確認 CSS 文件導入

確認 `src/main.jsx` 有導入 CSS 文件：

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css' // 確認這行存在

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

## 測試配置

### 測試用的 App.jsx

在 `src/App.jsx` 中加入以下測試代碼：

```jsx
function App() {
  return (
    <div className="min-h-screen bg-gradient-to-r from-blue-500 to-purple-600 flex items-center justify-center">
      <div className="bg-white p-8 rounded-lg shadow-lg max-w-md mx-auto">
        <h1 className="text-3xl font-bold text-gray-800 mb-4 text-center">
          Hello Tailwind CSS!
        </h1>
        <p className="text-gray-600 mb-6 text-center">
          如果你看到這個樣式，表示 Tailwind CSS 已經成功配置！
        </p>
        <div className="flex justify-center">
          <button className="px-6 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition-colors duration-200 shadow-md hover:shadow-lg">
            測試按鈕
          </button>
        </div>

        {/* 其他測試元素 */}
        <div className="mt-6 grid grid-cols-3 gap-4">
          <div className="h-12 bg-red-400 rounded"></div>
          <div className="h-12 bg-green-400 rounded"></div>
          <div className="h-12 bg-yellow-400 rounded"></div>
        </div>

        <div className="mt-4 text-sm text-gray-500 text-center">
          <p>響應式測試：</p>
          <p className="sm:text-base md:text-lg lg:text-xl">
            在不同螢幕尺寸下文字會改變大小
          </p>
        </div>
      </div>
    </div>
  )
}

export default App
```

### 啟動開發伺服器

```bash
npm run dev
```

如果配置成功，你應該會看到一個漂亮的藍紫色漸變背景，中間有一個白色卡片。

## 常見問題

### 問題 1：`autoprefixer` 模組找不到

**錯誤訊息：**

```
Error: Loading PostCSS Plugin failed: Cannot find module 'autoprefixer'
```

**解決方案：**

```bash
npm install --save-dev autoprefixer
```

### 問題 2：PostCSS 配置重複或衝突

**錯誤訊息：**

```
[plugin:vite:css] Failed to load PostCSS config
```

**解決方案：**

- **v4**: 在 `postcss.config.js` 中只使用 `@tailwindcss/postcss`，不要同時使用 `tailwindcss`
- **v3**: 在 `postcss.config.js` 中只使用 `tailwindcss`，不要使用 `@tailwindcss/postcss`

### 問題 3：樣式沒有生效

**檢查清單：**

1. ✅ 確認 CSS 文件有正確的 `@import` 或 `@tailwind` 指令
2. ✅ 確認 `main.jsx` 有導入 CSS 文件 (`import './index.css'`)
3. ✅ 確認 Vite 配置有包含 Tailwind 插件
4. ✅ 檢查瀏覽器開發者工具看是否有 CSS 載入
5. ✅ 嘗試重新啟動開發伺服器 (`Ctrl+C` 然後 `npm run dev`)

### 問題 4：Vite 配置錯誤

**對於 v4，確認 `vite.config.js` 包含：**

```javascript
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

### 問題 5：CSS 類別沒有作用

1. 檢查是否有拼寫錯誤
2. 檢查是否使用了正確的 Tailwind 語法
3. 在瀏覽器開發者工具中檢查元素是否有對應的 CSS 規則

## 版本差異

### Tailwind CSS v4 vs v3

| 特點       | v4                          | v3                                                           |
| ---------- | --------------------------- | ------------------------------------------------------------ |
| 配置複雜度 | 簡單                        | 較複雜                                                       |
| 性能       | 更好                        | 良好                                                         |
| CSS 導入   | `@import "tailwindcss"`     | `@tailwind base; @tailwind components; @tailwind utilities;` |
| 配置檔案   | 不需要 `tailwind.config.js` | 需要 `tailwind.config.js`                                    |
| 穩定性     | 較新，可能有 bug            | 穩定                                                         |
| 社群支援   | 較少                        | 豐富                                                         |

### 建議選擇

- **新專案且追求最新功能**：選擇 v4
- **生產環境或需要穩定性**：選擇 v3
- **團隊合作且其他人熟悉 v3**：選擇 v3

## 進階配置

### 自定義顏色和字體

#### v4 方式

在 `src/index.css` 中：

```css
@import 'tailwindcss';

@theme {
  --color-primary: #3b82f6;
  --color-secondary: #8b5cf6;
  --font-family-custom: 'Noto Sans TC', sans-serif;
}
```

#### v3 方式

在 `tailwind.config.js` 中：

```javascript
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: '#3b82f6',
        secondary: '#8b5cf6',
      },
      fontFamily: {
        custom: ['Noto Sans TC', 'sans-serif'],
      },
    },
  },
  plugins: [],
}
```

### VS Code 擴充功能推薦

安裝以下擴充功能提升開發體驗：

1. **Tailwind CSS IntelliSense** - 自動完成和語法高亮
2. **PostCSS Language Support** - PostCSS 語法支援
3. **Headwind** - 自動排序 Tailwind 類別

## 結語

按照這個指南，你應該能夠在任何新專案中成功設定 Tailwind CSS。記住最重要的幾點：

1. 🔑 **正確安裝所有依賴項目**（特別是 `autoprefixer`）
2. 🔑 **選擇合適的版本配置**（v4 或 v3）
3. 🔑 **確保 CSS 文件正確導入**
4. 🔑 **不要混用不同版本的配置**

如果遇到問題，請檢查這個指南中的常見問題部分，或者重新按照步驟操作。

---

**最後更新：** 2025 年 9 月 27 日  
**測試環境：** Node.js 18+, Vite 7.x, React 19.x
