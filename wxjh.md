# 一、项目开始建设流程

1.public文件夹——>新建static文件夹——>新建config.js(接口ip存放位置)

```js
let apiUrl = 'http://192.168.120.156:9006'
```

2.public文件夹——>inde.html

```
<script src="./static/config.js"></script>
```

3.api引入(TS)

```js
//可以直接使用路径变量
declare var apiUrl: any;
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
        a.download = `${date}.xls`;
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

