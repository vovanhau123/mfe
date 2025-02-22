# Micro Frontend Demo với Module Federation

## Tổng quan
Đây là một ví dụ về kiến trúc Micro Frontend sử dụng Webpack Module Federation. Ứng dụng bao gồm hai micro frontend riêng biệt:
- MFE1 (app1): Ứng dụng chủ (host) - Product Listing
- MFE2 (app2): Ứng dụng từ xa (remote) - Cart Application

## Cấu trúc dự án

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

## Luồng hoạt động

### 1. Khởi động ứng dụng MFE1 (Host Application - Port 3001)
- `index.js` là điểm khởi đầu, import file `bootstrap.js`
- `bootstrap.js` khởi tạo React application và render component App chính
- `App.js` chứa layout chính và import component từ MFE2 (app2/App)
- Webpack config thiết lập Module Federation để có thể import các module từ MFE2

### 2. Khởi động ứng dụng MFE2 (Remote Application - Port 3002)
- Chạy trên port 3002
- Expose component App thông qua Module Federation
- `webpack.config.js` cấu hình để expose App component qua `remoteEntry.js`
- Component được chia sẻ có thể được import và sử dụng bởi MFE1

### 3. Tích hợp và Chia sẻ
- MFE1 import và sử dụng component từ MFE2 thông qua cú pháp: `import RemoteApp from "app2/App"`
- Các dependencies chung (React, React-DOM) được chia sẻ để tránh duplicate
- Module Federation đảm bảo việc load các remote modules một cách động

### 4. Cấu hình Module Federation

#### MFE1 (Host):
```javascript
new ModuleFederationPlugin({
  name: "app1",
  remotes: {
    app2: "app2@http://localhost:3002/remoteEntry.js",
  },
  shared: {'react': {singleton: true}, "react-dom": {singleton: true}},
})
```

#### MFE2 (Remote):
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

## Cách chạy ứng dụng

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

## Lưu ý quan trọng
- Đảm bảo MFE2 được khởi động trước MFE1
- Các shared dependencies phải có version tương thích
- Cần có kết nối mạng để host application có thể tải remote modules