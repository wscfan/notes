# JS监测网络状态

[toc]
## 1. navigator.onLine

机器未连接到局域网或者路由器返回false，其它返回true。缺点是存在机器连接上没联通网络的路由器返回true的情况。

## 2. ajax请求

采用get请求方式，根据是否返回数据判断当前网络状态。缺点是无法区分是服务器故障还是用户网络问题。

## 3. 获取网络资源

在页面放一张隐藏的图片，通过图片的onerror事件判断图片资源是否请求成功来判断用户网络。缺点是如果图片服务器故障导致图片不能访问也会被onerror事件监听到。

```html
  <body>
    <img
      id="hiddenImg"
      onerror="handleError()"
      style="display: none"
      src="http://skin.wscfan.cn/img_1px.png"
      alt=""
    />
    <button id="checkBtn">检查网络状态</button>

    <script>
      document.getElementById("checkBtn").addEventListener("click", () => {
        document.getElementById(
          "hiddenImg"
        ).src = `http://skin.wscfan.cn/img_1px.png?timestamp=${Date.parse(
          new Date()
        )}`;
      });

      function handleError() {
        alert("网络连接失败");
      }
    </script>
  </body>
```

## 4. 监测online和offline事件

通过 `window.addEventListener("online", () => {})` 和 `window.addEventListener("offline", () => {})` 来监听网络状态。
