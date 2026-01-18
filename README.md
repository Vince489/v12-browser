# VIRT Browser (V12) 🖥️

<div align="center">

![VIRT Browser Logo](src/assets/favicon.ico)

**The Developer's Walled Garden** - A revolutionary browser that transforms GitHub repositories into instantly browsable websites through the VIRT:// protocol.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Electron](https://img.shields.io/badge/Electron-40.x-blue.svg)](https://electronjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-20.x-green.svg)](https://nodejs.org/)

[Download for Windows](#download) • [View Demo](#demo) • [Documentation](#documentation)

</div>

---

## 🌟 What is VIRT Browser?

VIRT Browser is a desktop web browser built specifically for developers, featuring a revolutionary custom protocol that transforms any internet-accessible server into an instantly browsable website. Unlike traditional browsers that rely on DNS and hosting providers, VIRT Browser creates a **decentralized ecosystem** where developers can register custom domains pointing to any HTTPS URL or IP address.

### 🎯 Key Innovation

The VIRT:// protocol enables:
- **Instant Deployment**: Push code to any server and access it via `virt://your-domain.vc`
- **True Decentralization**: Register domains pointing to any internet-accessible server
- **Developer-First**: Built by developers for developers with full transparency
- **Privacy-Focused**: Zero tracking, zero ads, zero data collection

---

## ✨ Key Features & Benefits

### 🚀 Instant Deployment
- **Zero Build Pipeline**: No hosting fees, no deployment scripts
- **Real-Time Updates**: Changes appear instantly when pushed to your server
- **Version Control Integration**: Direct connection to your Git repositories

### 🔒 Privacy & Anonymity
- **No Data Collection**: Absolutely zero telemetry or tracking
- **Anonymous Registration**: Secret-key based domain management (no personal accounts)
- **Secure Sandbox**: Each VIRT site runs in complete isolation
- **No Ads**: Clean, ad-free browsing experience

### 🛡️ Security First
- **Sandboxed Execution**: Chromium sandbox + VIRT isolation layers
- **Direct Connections**: No intermediary servers or proxies
- **Key-Based Authentication**: Secure domain management with bcrypt-hashed keys
- **Input Validation**: Comprehensive validation of domains, targets, and requests

### 🏗️ Developer Experience
- **Full DevTools**: Chrome DevTools integration for debugging
- **Tabbed Browsing**: Modern tab management with navigation controls
- **Bookmark System**: Chrome-compatible bookmark storage
- **Search Engine**: Built-in discovery via `lookin.at`
- **Open Source**: Inspect, modify, and contribute to the browser itself

### 🌐 Decentralized Architecture
- **Custom Protocol**: VIRT:// scheme for domain resolution
- **Arbitrary Hosting**: Support for any HTTPS URL or IP:port combination
- **Domain Registry**: MongoDB-backed domain registration system
- **API-First**: RESTful backend for all domain operations

---

## 🏛️ Technical Architecture

### Frontend (Electron Application)
- **Framework**: Electron 40.x with Chromium rendering engine
- **Protocol Handler**: Custom VIRT:// scheme implementation
- **UI**: Modern web-based interface with Tailwind CSS
- **Security**: Sandboxed webviews and context isolation

### Backend (Node.js API)
- **Runtime**: Node.js 20.x LTS with Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: Secret-key based system with bcrypt hashing
- **Search**: Full-text search with weighted MongoDB text indexes

### Domain Resolution Flow
```
1. User enters: virt://mysite.vc
2. Electron protocol handler intercepts request
3. HTTP call to: /api/lookup/mysite/vc
4. MongoDB query returns target server info
5. Direct connection to registered IP/server
6. Content served through sandboxed webview
```

### Supported Target Types
- **HTTPS URLs**: `https://my-server.com`
- **IP Addresses**: `192.168.1.50:8080`
- **GitHub Repos**: Auto-converted to raw.githubusercontent.com

---

## 📦 Installation

### Prerequisites
- **Windows 10/11 (64-bit)** or **macOS/Linux**
- **Node.js 20.x** and **npm**
- **MongoDB** (local or cloud instance)
- **4GB RAM minimum** (8GB recommended)

### Quick Start

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Vince489/vrt-browser-2.git
   cd vrt-browser-2
   ```

2. **Install dependencies:**
   ```bash
   # Install backend dependencies
   cd backend
   npm install

   # Install frontend dependencies (root directory)
   cd ..
   npm install
   ```

3. **Configure environment:**
   ```bash
   cd backend
   cp .env.example .env
   # Edit .env with your MongoDB URI and settings
   ```

4. **Start the backend:**
   ```bash
   cd backend
   npm run dev
   ```

5. **Start the browser:**
   ```bash
   # In another terminal (root directory)
   npm start
   ```

### Docker Setup (Alternative)
```bash
# Backend with MongoDB
docker-compose up -d

# Frontend
npm start
```

---

## 🚀 Usage Guide

### Registering Your First Domain

1. **Open VIRT Browser** and navigate to `virt://register.at`
2. **Choose a domain** (e.g., `myproject.vc`)
3. **Enter target URL/IP** (e.g., `https://github.com/user/myrepo` or `192.168.1.50:3000`)
4. **Receive secret key** (save this securely!)
5. **Access your site** at `virt://myproject.vc`

### Domain Management

```bash
# Check domain availability
GET /api/check/mydomain?vld=vc

# Register new domain
POST /api/register
{
  "domain": "mydomain",
  "tld": "vc",
  "target": "https://my-server.com",
  "title": "My Project",
  "description": "A cool web app"
}

# Update domain (requires secret key)
PUT /api/update
{
  "domain": "mydomain",
  "tld": "vc",
  "secretKey": "v12-xxxxxxxxxxxx",
  "target": "https://new-server.com"
}
```

### Available TLDs
- **`.vc`** - Venture/Company projects
- **`.vmc`** - Virtual Machine/Container apps
- **`.at`** - Applications/Tools
- **`.lit`** - Literature/Documentation

---

## 🔍 API Reference

### Domain Operations

#### Check Availability
```http
GET /api/check/:domain?tld=vc
Response: { "available": true, "message": "Domain is available" }
```

#### Domain Registration
```http
POST /api/register
Content-Type: application/json

{
  "domain": "mysite",
  "tld": "vc",
  "target": "https://github.com/user/repo",
  "title": "My Site",
  "description": "Description"
}
```

#### Domain Lookup (DNS Resolution)
```http
GET /api/lookup/:domain/:tld
Response: {
  "domain": "mysite",
  "tld": "vc",
  "target": "https://github.com/user/repo",
  "rawBase": "https://raw.githubusercontent.com/user/repo/main/",
  "title": "My Site",
  "verified": false
}
```

#### Update Domain
```http
PUT /api/update
{
  "domain": "mysite",
  "tld": "vc",
  "secretKey": "v12-xxxxxxxxxxxx",
  "target": "https://new-target.com"
}
```

#### Search Sites
```http
GET /api/search?q=javascript
Response: [{ domain, tld, title, description }, ...]
```

---

## 🏗️ Development

### Project Structure
```
vrt-browser-2/
├── main.js                    # Electron main process
├── package.json              # Frontend dependencies
├── preload.js                # Context bridge security
├── src/
│   ├── renderer/             # Browser UI (HTML/CSS/JS)
│   └── assets/               # Static assets and pages
└── backend/
    ├── server.js             # Express API server
    ├── routes/               # API route handlers
    ├── models/               # MongoDB schemas
    └── package.json          # Backend dependencies
```

### Building for Production

```bash
# Build backend (if needed)
cd backend
npm run build

# Build Electron app
cd ..
npm run build
npm run build:win  # Windows
npm run build:mac  # macOS
npm run build:linux # Linux
```

### Testing
```bash
# Backend tests
cd backend
npm test

# Frontend (manual testing)
npm start
```

---

## 🤝 Contributing

We welcome contributions from the developer community!

### How to Contribute

1. **Fork the repository**
2. **Create a feature branch:** `git checkout -b feature/amazing-feature`
3. **Make your changes** and test thoroughly
4. **Commit your changes:** `git commit -m 'Add amazing feature'`
5. **Push to the branch:** `git push origin feature/amazing-feature`
6. **Open a Pull Request**

### Development Guidelines

- **Code Style**: Follow existing patterns and use ESLint
- **Security**: All changes must maintain security boundaries
- **Testing**: Add tests for new features
- **Documentation**: Update README for significant changes

### Areas for Contribution

- **Protocol Extensions**: New TLDs, enhanced resolution
- **UI/UX Improvements**: Better user experience
- **Security Enhancements**: Additional validation layers
- **Performance**: Optimization and caching
- **Cross-Platform**: macOS/Linux support

---

## 📊 System Requirements

### Minimum Requirements
- **OS**: Windows 10 (64-bit), macOS 10.15+, Ubuntu 18.04+
- **RAM**: 4GB
- **Storage**: 500MB free space
- **Network**: Internet connection for domain resolution

### Recommended Specifications
- **OS**: Windows 11 (64-bit), macOS 12+, Ubuntu 20.04+
- **RAM**: 8GB
- **Storage**: 1GB free space
- **Network**: Stable broadband connection

---

## 🔐 Privacy & Security

### Privacy Commitments
- **Zero Data Collection**: No analytics, no crash reporting, no usage tracking
- **Anonymous Operation**: No user accounts or personal information required
- **Local Storage Only**: Bookmarks and settings stored locally
- **No Third-Party Tracking**: Completely self-contained ecosystem

### Security Features
- **Sandboxed Execution**: Each site runs in isolated Chromium context
- **Protocol Isolation**: VIRT protocol separated from standard web browsing
- **Input Validation**: Comprehensive validation of all user inputs
- **Key-Based Security**: Bcrypt-hashed secret keys for domain management

### Data Handling
- **Domain Registry**: MongoDB stores only domain mappings and metadata
- **No Content Caching**: Direct connections to registered servers
- **Access Logging**: Optional last-accessed timestamps only

---

## 📈 Performance

- **Startup Time**: <2 seconds on modern hardware
- **Memory Usage**: ~150MB at idle
- **Network Efficiency**: Direct connections, no proxy overhead
- **Search Speed**: Sub-second MongoDB text search responses

---

## 🐛 Troubleshooting

### Common Issues

**"Domain not found" errors:**
- Verify the domain is registered: `GET /api/check/domain?tld=tld`
- Check backend connectivity and MongoDB status

**Connection timeouts:**
- Ensure target servers are accessible and responding
- Verify HTTPS certificates are valid
- Check firewall settings for custom ports

**Build failures:**
- Clear node_modules: `rm -rf node_modules && npm install`
- Update Electron: `npm update electron`
- Check system compatibility

### Debug Mode
```bash
# Enable verbose logging
DEBUG=v12:* npm start

# Open DevTools automatically
NODE_ENV=development npm start
```

---

## 📚 Documentation

- **[Technical Architecture](technical-details.md)** - Deep dive into the VIRT protocol
- **[Backend Analysis](tech-analysis-2.md)** - Complete API and database documentation
- **[Extended Analysis](extended-technical-analysis.md)** - Advanced technical concepts

---

## 🏆 Roadmap

### Phase 1 (Current) ✅
- Basic VIRT protocol implementation
- Domain registration and resolution
- Electron desktop application
- GitHub repository integration

### Phase 2 (Next) 🚧
- Enhanced search and discovery
- Content indexing and caching
- Mobile application support
- Advanced security features

### Phase 3 (Future) 🔮
- P2P domain resolution
- Decentralized registry
- Custom SSL certificate management
- Enterprise features

---

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/Vince489/vrt-browser-2/issues)
- **Discussions**: [GitHub Discussions](https://github.com/Vince489/vrt-browser-2/discussions)
- **Documentation**: See files in `/docs` directory

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

**Copyright © 2026 VIRT Browser Team**

Built with ❤️ by developers, for developers.

---

<div align="center">

**Ready to revolutionize web browsing?**

[Download VIRT Browser](#download) • [Register Your Domain](virt://register.at) • [Explore Sites](virt://lookin.at)

*Join the decentralized web revolution!*

</div>
