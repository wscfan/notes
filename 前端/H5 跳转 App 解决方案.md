# H5 跳转 App 解决方案

## 一、跳转方案概述

### 1. 主要实现方式
- URL Scheme 跳转
- Universal Links (iOS)
- App Links (Android)
- Intent 协议 (Android)
- 应用市场下载

### 2. 兼容性考虑
- iOS vs Android 差异处理
- 不同浏览器环境
- 微信等应用内打开
- App 是否安装判断

## 二、基础实现方案

### 1. URL Scheme
```javascript
class AppRouter {
    constructor(options) {
        this.config = {
            scheme: options.scheme, // 应用协议，如 myapp://
            iosStore: options.iosStore, // iOS App Store 链接
            androidStore: options.androidStore, // 安卓应用市场链接
            timeout: options.timeout || 2000 // 超时时间
        };
    }

    // 基础跳转方法
    open(path) {
        const url = `${this.config.scheme}${path}`;
        const hidden = this.getVisibilityProperty();
        
        // 记录页面隐藏前的时间
        const startTime = Date.now();
        
        // 监听页面隐藏事件
        const visibilityChange = () => {
            const endTime = Date.now();
            if (document[hidden]) {
                // 页面隐藏，说明唤起成功
                document.removeEventListener(this.getVisibilityEvent(), visibilityChange);
            }
        };
        
        document.addEventListener(this.getVisibilityEvent(), visibilityChange);
        
        // 尝试打开应用
        location.href = url;
        
        // 超时检查
        setTimeout(() => {
            const now = Date.now();
            if (now - startTime < this.config.timeout + 200) {
                // 未成功唤起应用，跳转到应用商店
                this.goStore();
            }
        }, this.config.timeout);
    }

    // 获取页面可见性属性
    getVisibilityProperty() {
        if (typeof document.hidden !== "undefined") {
            return "hidden";
        }
        if (typeof document.msHidden !== "undefined") {
            return "msHidden";
        }
        if (typeof document.webkitHidden !== "undefined") {
            return "webkitHidden";
        }
        return null;
    }

    // 获取可见性改变事件名
    getVisibilityEvent() {
        if (typeof document.hidden !== "undefined") {
            return "visibilitychange";
        }
        if (typeof document.msHidden !== "undefined") {
            return "msvisibilitychange";
        }
        if (typeof document.webkitHidden !== "undefined") {
            return "webkitvisibilitychange";
        }
        return null;
    }

    // 跳转应用商店
    goStore() {
        if (this.isIOS()) {
            location.href = this.config.iosStore;
        } else {
            location.href = this.config.androidStore;
        }
    }

    // 判断是否 iOS 设备
    isIOS() {
        return /iPhone|iPad|iPod/i.test(navigator.userAgent);
    }
}
```

### 2. Universal Links (iOS)
```html
<!-- 在 head 中添加关联文件 -->
<head>
    <meta name="apple-itunes-app" content="app-id=YOUR_APP_ID">
    <link rel="apple-app-site-association" href="/apple-app-site-association">
</head>
```

```javascript
// apple-app-site-association 文件内容
{
    "applinks": {
        "apps": [],
        "details": [{
            "appID": "TeamID.BundleID",
            "paths": ["*"]
        }]
    }
}
```

### 3. App Links (Android)
```html
<!-- Digital Asset Links JSON -->
<head>
    <link rel="digital-asset-links" href="/.well-known/assetlinks.json">
</head>
```

```javascript
// assetlinks.json 文件内容
[{
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
        "namespace": "android_app",
        "package_name": "com.example.app",
        "sha256_cert_fingerprints": ["SHA256 证书指纹"]
    }
}]
```

## 三、增强型实现方案

### 1. 综合路由类
```javascript
class EnhancedAppRouter {
    constructor(options) {
        this.options = {
            scheme: options.scheme,
            universalLink: options.universalLink,
            appLink: options.appLink,
            iosStore: options.iosStore,
            androidStore: options.androidStore,
            timeout: options.timeout || 2000,
            intentUrl: options.intentUrl,
            fallback: options.fallback
        };
        
        this.init();
    }

    init() {
        // 初始化环境检测
        this.env = this.detectEnvironment();
        // 初始化跳转策略
        this.strategy = this.getStrategy();
    }

    detectEnvironment() {
        const ua = navigator.userAgent;
        return {
            isIOS: /iPhone|iPad|iPod/i.test(ua),
            isAndroid: /Android/i.test(ua),
            isWeChat: /MicroMessenger/i.test(ua),
            isChrome: /Chrome/i.test(ua),
            isSafari: /Safari/i.test(ua),
            isQQ: /QQ/i.test(ua)
        };
    }

    getStrategy() {
        if (this.env.isWeChat) {
            return 'wechat';
        }
        if (this.env.isIOS) {
            return this.env.isSafari ? 'universal' : 'scheme';
        }
        if (this.env.isAndroid) {
            return this.env.isChrome ? 'intent' : 'scheme';
        }
        return 'scheme';
    }

    open(path, params = {}) {
        const url = this.buildUrl(path, params);
        switch (this.strategy) {
            case 'universal':
                this.openUniversal(url);
                break;
            case 'intent':
                this.openIntent(url);
                break;
            case 'wechat':
                this.openWechat(url);
                break;
            default:
                this.openScheme(url);
        }
    }

    buildUrl(path, params) {
        const query = Object.keys(params)
            .map(key => `${key}=${encodeURIComponent(params[key])}`)
            .join('&');
        return query ? `${path}?${query}` : path;
    }

    openUniversal(path) {
        const url = `${this.options.universalLink}/${path}`;
        this.tryOpen(url, () => this.openScheme(path));
    }

    openIntent(path) {
        const intent = this.buildIntent(path);
        this.tryOpen(intent, () => this.goStore());
    }

    buildIntent(path) {
        const schemeUrl = `${this.options.scheme}${path}`;
        return `intent://${path}#Intent;` +
               `scheme=${this.options.scheme.replace('://', '')};` +
               `package=${this.options.package};` +
               `S.browser_fallback_url=${encodeURIComponent(this.options.androidStore)};` +
               'end';
    }

    openWechat(path) {
        // 微信中显示引导提示
        this.showWeChatGuide();
    }

    tryOpen(url, fallback) {
        const startTime = Date.now();
        const hidden = this.getVisibilityProperty();
        
        const visibilityChange = () => {
            if (document[hidden]) {
                document.removeEventListener(
                    this.getVisibilityEvent(), 
                    visibilityChange
                );
            }
        };
        
        document.addEventListener(
            this.getVisibilityEvent(), 
            visibilityChange
        );
        
        location.href = url;
        
        setTimeout(() => {
            if (Date.now() - startTime < this.options.timeout + 200) {
                fallback && fallback();
            }
        }, this.options.timeout);
    }
}
```

### 2. 使用示例
```javascript
const router = new EnhancedAppRouter({
    scheme: 'myapp://',
    universalLink: 'https://example.com',
    appLink: 'android-app://com.example.app',
    iosStore: 'https://apps.apple.com/app/id123456',
    androidStore: 'market://details?id=com.example.app',
    package: 'com.example.app',
    timeout: 2000
});

// 基础跳转
router.open('home');

// 带参数跳转
router.open('product/detail', {
    id: '123',
    source: 'h5'
});
```

## 四、进阶优化方案

### 1. 预加载优化
```javascript
class PreloadAppRouter extends EnhancedAppRouter {
    constructor(options) {
        super(options);
        this.preloadStatus = false;
    }

    preload() {
        if (this.preloadStatus) return;
        
        const link = document.createElement('link');
        link.rel = 'preconnect';
        link.href = this.options.scheme;
        document.head.appendChild(link);
        
        this.preloadStatus = true;
    }

    open(path, params) {
        this.preload();
        super.open(path, params);
    }
}
```

### 2. 智能降级处理
```javascript
class SmartAppRouter extends PreloadAppRouter {
    constructor(options) {
        super(options);
        this.failedAttempts = 0;
        this.maxAttempts = 3;
    }

    shouldDegrade() {
        return this.failedAttempts >= this.maxAttempts;
    }

    open(path, params) {
        if (this.shouldDegrade()) {
            this.goStore();
            return;
        }

        super.open(path, params);
        this.failedAttempts++;
    }

    resetAttempts() {
        this.failedAttempts = 0;
    }
}
```

## 五、最佳实践建议

1. **跳转策略**
   - 优先使用 Universal Links 和 App Links
   - 根据环境智能选择跳转方式
   - 实现可靠的失败降级机制

2. **用户体验**
   - 添加加载提示
   - 优化跳转超时时间
   - 提供清晰的引导说明

3. **性能优化**
   - 实现预加载机制
   - 缓存检测结果
   - 优化重试策略

4. **安全考虑**
   - 参数传递加密
   - 防止跳转劫持
   - URL 编码处理

## 六、注意事项

1. **开发配置**
   - iOS 需配置 Universal Links
   - Android 需配置 App Links
   - 需要在应用内实现相应协议处理

2. **环境限制**
   - 微信内无法直接跳转
   - iOS 9+ 推荐使用 Universal Links
   - Android Chrome 25+ 推荐使用 Intent

3. **调试方法**
   - 使用 Safari 调试 iOS
   - 使用 Chrome 调试 Android
   - 抓包分析跳转流程

## 总结

实现可靠的 H5 跳转 App 方案需要：
- 全面的兼容性处理
- 可靠的失败降级机制
- 良好的用户体验设计
- 完善的安全性考虑

通过以上方案，可以实现稳定可靠的 H5 跳转 App 功能，同时保证良好的用户体验。