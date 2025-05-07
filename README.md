# Micro Frontend Demo with Module Federation

A modern approach to frontend architecture using Webpack Module Federation for building scalable and modular React applications.

## 🚀 Overview

This project demonstrates the Micro Frontend architecture using Webpack Module Federation. The application consists of two separate micro frontends that work seamlessly together:

- **MFE1 (app1)**: Host Application - Product Listing (Port 3001)
- **MFE2 (app2)**: Remote Application - Cart Application (Port 3002)

The architecture allows each micro frontend to be developed, deployed, and scaled independently while sharing components and state when needed.

## 📂 Project Structure

```
├── mfe1 (Port: 3001)
│   ├── src
│   │   ├── App.js
│   │   ├── bootstrap.js
│   │   └── index.js
│   └── webpack.config.js
│
├── mfe2 (Port: 3002)
    ├── src
    │   ├── App.js
    │   ├── bootstrap.js
    │   └── index.js
    └── webpack.config.js
```

## ✨ Key Features

- **Independent Development**: Each micro frontend can be developed by separate teams
- **Runtime Integration**: MFE1 dynamically loads components from MFE2 at runtime
- **Shared Dependencies**: Common libraries (React, React-DOM) are shared to avoid duplication
- **Module Federation**: Webpack 5's Module Federation enables seamless component sharing
- **Isolated Deployment**: Each application can be deployed independently
- **Resilient Architecture**: Applications can fail independently without breaking the entire system

## 🔄 Application Flow

1. **MFE1 (Host Application)**:
   - Runs on port 3001
   - Entry point in `index.js` which imports `bootstrap.js`
   - `bootstrap.js` initializes the React application
   - `App.js` contains the main layout and imports components from MFE2

2. **MFE2 (Remote Application)**:
   - Runs on port 3002
   - Exposes its App component through Module Federation
   - Configuration in `webpack.config.js` exposes components via `remoteEntry.js`

3. **Integration**:
   - MFE1 imports and uses components from MFE2 using: `import RemoteApp from "app2/App"`
   - Shared dependencies prevent duplicate loading of React

## 🛠️ Module Federation Configuration

**MFE1 (Host):**
```javascript
new ModuleFederationPlugin({
  name: "app1",
  remotes: {
    app2: "app2@http://localhost:3002/remoteEntry.js",
  },
  shared: {'react': {singleton: true}, "react-dom": {singleton: true}},
})
```

**MFE2 (Remote):**
```javascript
new ModuleFederationPlugin({
  name: 'app2',
  filename: 'remoteEntry.js',
  exposes: {
    './App': './src/App',
  },
  shared: { react: { singleton: true }, 'react-dom': { singleton: true } },
})
```

## 🚦 Getting Started

1. Start MFE2 (Remote):
```bash
cd mfe2
npm install
npm start
```

2. Start MFE1 (Host):
```bash
cd mfe1
npm install
npm start
```

3. Access the applications:
   - MFE1: http://localhost:3001
   - MFE2: http://localhost:3002

## ⚠️ Important Notes

- Ensure MFE2 is started before MFE1
- Shared dependencies must have compatible versions
- Network connection is required for the host application to load remote modules

## 🔍 Learn More

This project demonstrates key concepts of Micro Frontend architecture:
- Decoupling frontend applications
- Independent development and deployment
- Runtime integration
- Shared dependencies management

---

# Demo Micro Frontend với Module Federation

Một cách tiếp cận hiện đại cho kiến trúc frontend sử dụng Webpack Module Federation để xây dựng các ứng dụng React có khả năng mở rộng và module hóa.

## 🚀 Tổng quan

Dự án này trình bày kiến trúc Micro Frontend sử dụng Webpack Module Federation. Ứng dụng bao gồm hai micro frontend riêng biệt hoạt động liền mạch với nhau:

- **MFE1 (app1)**: Ứng dụng chủ (Host) - Product Listing (Cổng 3001)
- **MFE2 (app2)**: Ứng dụng từ xa (Remote) - Cart Application (Cổng 3002)

Kiến trúc này cho phép mỗi micro frontend được phát triển, triển khai và mở rộng độc lập trong khi vẫn chia sẻ các component và trạng thái khi cần thiết.

## 📂 Cấu trúc dự án

```
├── mfe1 (Cổng: 3001)
│   ├── src
│   │   ├── App.js
│   │   ├── bootstrap.js
│   │   └── index.js
│   └── webpack.config.js
│
├── mfe2 (Cổng: 3002)
    ├── src
    │   ├── App.js
    │   ├── bootstrap.js
    │   └── index.js
    └── webpack.config.js
```

## ✨ Tính năng chính

- **Phát triển độc lập**: Mỗi micro frontend có thể được phát triển bởi các team riêng biệt
- **Tích hợp thời gian chạy**: MFE1 tải động các component từ MFE2 tại thời điểm chạy
- **Chia sẻ dependencies**: Các thư viện chung (React, React-DOM) được chia sẻ để tránh trùng lặp
- **Module Federation**: Module Federation của Webpack 5 cho phép chia sẻ component liền mạch
- **Triển khai độc lập**: Mỗi ứng dụng có thể được triển khai độc lập
- **Kiến trúc linh hoạt**: Các ứng dụng có thể gặp sự cố độc lập mà không làm hỏng toàn bộ hệ thống

## 🔄 Luồng hoạt động

1. **MFE1 (Ứng dụng chủ)**:
   - Chạy trên cổng 3001
   - Điểm khởi đầu trong `index.js` import `bootstrap.js`
   - `bootstrap.js` khởi tạo ứng dụng React
   - `App.js` chứa layout chính và import component từ MFE2

2. **MFE2 (Ứng dụng từ xa)**:
   - Chạy trên cổng 3002
   - Expose component App thông qua Module Federation
   - Cấu hình trong `webpack.config.js` expose component qua `remoteEntry.js`

3. **Tích hợp**:
   - MFE1 import và sử dụng component từ MFE2 thông qua: `import RemoteApp from "app2/App"`
   - Các dependencies được chia sẻ ngăn chặn việc tải trùng lặp React

## 🛠️ Cấu hình Module Federation

**MFE1 (Host):**
```javascript
new ModuleFederationPlugin({
  name: "app1",
  remotes: {
    app2: "app2@http://localhost:3002/remoteEntry.js",
  },
  shared: {'react': {singleton: true}, "react-dom": {singleton: true}},
})
```

**MFE2 (Remote):**
```javascript
new ModuleFederationPlugin({
  name: 'app2',
  filename: 'remoteEntry.js',
  exposes: {
    './App': './src/App',
  },
  shared: { react: { singleton: true }, 'react-dom': { singleton: true } },
})
```

## 🚦 Bắt đầu

1. Khởi động MFE2 (Remote):
```bash
cd mfe2
npm install
npm start
```

2. Khởi động MFE1 (Host):
```bash
cd mfe1
npm install
npm start
```

3. Truy cập ứng dụng:
   - MFE1: http://localhost:3001
   - MFE2: http://localhost:3002

## ⚠️ Lưu ý quan trọng

- Đảm bảo MFE2 được khởi động trước MFE1
- Các dependencies chung phải có phiên bản tương thích
- Cần có kết nối mạng để ứng dụng chủ có thể tải các module từ xa

## 🔍 Tìm hiểu thêm

Dự án này minh họa các khái niệm chính của kiến trúc Micro Frontend:
- Tách biệt các ứng dụng frontend
- Phát triển và triển khai độc lập
- Tích hợp thời gian chạy
- Quản lý dependencies chung
