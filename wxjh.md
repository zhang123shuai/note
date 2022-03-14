# 一、项目开始建设流程

## 1.步骤

①.public文件夹——>新建static文件夹——>新建config.js(接口ip存放位置)

```js
let apiUrl = 'http://192.168.120.156:9006'
```

②.public文件夹——>inde.html

```html
<script src="./static/config.js"></script>
```

③.api引入(TS)

```js
//可以直接使用路径变量
declare var apiUrl: any;
```

④.api引入(JS)

src文件夹——>api文件夹——>config.js文件

```js
const apiUrl = apiUrlConfig;

export { apiUrl };
```

使用

```js
import { apiUrl } from './config.js'
```

# 二、blob流文件下载

## 1.axios接口

```js
// 导出Excel
export function downReceive() {
    return axios({
        method: `POST`,
        responseType: 'blob',
        url: `${apiUrl}/receive/downReceive`,
    })
}
```

## 2.页面使用

```js
import common from "@/lib/common";  

// 导出excel
  dcExcel() {
    downReceive()
      .then((res) => {
        const blob = res.data;
        const a = document.createElement("a");
        const date = common.formatDate(new Date().getTime());//下载时间为文件名字
        // a.download = `${date}.xls`;    //excel文件
        // a.download = `维修凭证${date}.doc`;   //word文件
        a.download = `${date}.rar`;  //压缩包
        let s = URL.createObjectURL(blob);
        a.href = s;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
      })
      .catch((err: any) => {
        this.$message.error("导出失败！");
      });
  }
//时间戳转日期
  formatDate(timeStamp: any) {
    timeStamp = Number(timeStamp);
    let year = new Date(timeStamp).getFullYear();
    let month =
      new Date(timeStamp).getMonth() + 1 < 10 ?
        "0" + (new Date(timeStamp).getMonth() + 1) :
        new Date(timeStamp).getMonth() + 1;
    let date =
      new Date(timeStamp).getDate() < 10 ?
        "0" + new Date(timeStamp).getDate() :
        new Date(timeStamp).getDate();
    let hh =
      new Date(timeStamp).getHours() < 10 ?
        "0" + new Date(timeStamp).getHours() :
        new Date(timeStamp).getHours();
    let mm =
      new Date(timeStamp).getMinutes() < 10 ?
        "0" + new Date(timeStamp).getMinutes() :
        new Date(timeStamp).getMinutes();
    let ss =
      new Date(timeStamp).getSeconds() < 10 ?
        "0" + new Date(timeStamp).getSeconds() :
        new Date(timeStamp).getSeconds();
    let nowTime =
      year + "-" + month + "-" + date + " " + hh + ":" + mm + ":" + ss;
    return nowTime;
  },
```

# 三、vue.config.js

## 1.普通打包配置

```js

module.exports = {
  publicPath: '/',
  // 全局配置css预编译器全局变量
 // css: {
     // loaderOptions: {
         // sass: {
             // prependData: `@import "~@/style/_variable.scss";`,
         // },
     // },
 // },
  devServer: {
      open: true, // 设置项目打开后浏览器自动打开
  },
}
```

## 2.当有配置代理接口时

### ①.vue.config.js配置

```js

module.exports = {
  publicPath: './',
  // 全局配置css预编译器全局变量
  // css: {
  //   loaderOptions: {
  //     sass: {
  //       prependData: `@import "~@/styles/_variable.scss";`,
  //     },
  //   },
  // },
  devServer: {
    open: true,
    host: 'localhost',
    port: 8080,
    https: false,
    //以上的ip和端口是我们本机的;下面为需要跨域的
    proxy: {  //配置跨域
      '/api': {
        target: 'http://192.168.130.233:9091/status/get',  //这里后台的地址模拟的;应该填写你们真实的后台接口
        ws: true,
        changOrigin: true,  //允许跨域
        pathRewrite: {
          '^/api': ''  //请求的时候使用这个api就可以
        }
      }
    }
  }
}
```

### ②.接口使用

```js
// 获取算法json
export function statusGet() {
  return axios({
    method: 'get',
    url:"/api",
  })
}
```

