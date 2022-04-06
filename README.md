## 日常总结


年初面试找工作的时候,投了一家公司,公司的负责人是百度出来的,想找几个人做项目.由于我的种种表现他觉得我还可以，想要带我,由于我只有高中学历,他说他不介意学历,另外由于个人一些不好的人生经历.就没有答应去,去了一家目前的我觉得还可以的公司上班.对于我这样一个从富士康流水线出来的只有高中学历的人来说已经很好了.别的不说，至少不用上夜班.相处的人的素质也还可以。
<br/>


趁着周末总结一点知识点,当初面试这家公司被笔试的.个人觉得也挺重要的.
<br/>

给定一个时间戳,例如4764,用一个函数转换成00:00:00这样的时分秒的形式.个人答案:
```
let fomartTime = (time) =>{
            let hours = String(Number.parseInt(time / 3600)).padStart(2,"0"),
                minutes = String(Number.parseInt(time / 60) % 60).padStart(2,"0"),
                seconds = String(time % 60).padStart(2,"0");

                return `${hours}:${minutes}:${seconds}`;

        }
         console.log(fomartTime(4764)); // 01:19:24
```
 倒计时就可以这样做(通常后端传的都是一个时间戳).

***
今天(2021-04-25)做项目的时候遇到后端传来一个时间戳,要求转换为年月日时分秒的形式,为此来总结一波:
```
/**
 * 
 * @param {*} time  时间戳
 */
export const formatDateTime = (time, seperator = "-") => {
    let date = new Date(time),
        fmt = `yyyy${seperator}MM${seperator}dd hh:mm:ss`;
    const o = {
        "M+": date.getMonth() + 1,
        "d+": date.getDate(),
        "h+": date.getHours(),
        "m+": date.getMinutes(),
        "s+": date.getSeconds(),
    };
    if (/(y+)/.test(fmt)) {
        fmt.replace(RegExp.$1, date.getFullYear());

    }
    for (let k in o) {
        if (new RegExp('(' + o[k] + ')').test(fmt)) {
            fmt = fmt.replace(RegExp.$1, String(o[k]).padStart(2,"0"));

        }

    }
    return fmt;

};

console.log(formatDateTime(1572728572986)) // 2019-11-03 05:02:52
console.log(formatDateTime("2019-10-11T05:04:02.506Z")) // 2019-10-11 13:04:02
console.log(formatDateTime("Fri Oct 11 2019 13:04:02 GMT+0800")) // 这个是东八区时间格式,2019-10-11 13:04:02

```
***
今天(2021.08.24) 来总结一波
有时说话啊，不要太过于早下结论了，搞到最后容易打自己脸(过去的事就让它过去吧,人生嘛，总是不断的经历，不断在经历中去成长)。

最近公司要做微信名片分享，决定提前做技术储备，免得到时手慌脚乱。
```
// canvas 画矩形圆角边框
drawSquare(context,x,y,w,h,radius){
	context.beginPath();
	context.moveTo(x + radius,y);
	context.lineTo(x + w - radius,y),
	// 右上角圆
	context.arcTo(x + w,y,x + w,y + radius,radius);
	context.lineTo(x + w,y + h - radius);
	// 右下角圆
	context.arcTo(x + w,y + h,x + y - radius,y+h,radius);
	context.lineTo(x + radius,y + h);
	// 左下角圆
	context.arcTo(x,y+h,x,y+h-radius,radius)
	context.lineTo(x,y+radius);
	// 左上角
	context.arcTo(x,y,x+radius,y,radius)
	context.closePath();
	context.setStrokeStyle("#333333");
	context.stroke();
	context.draw();
		          
                       }
```
如图所示:
![示意图](https://github.com/xiaolan92/notes/blob/master/images/WechatIMG30.jpeg)

***
 * 复制：将文本写入剪贴板(注意:只在localhost和https下生效)
```
  async copy (text){
      try {
         await navigator.clipboard.writeText(text);
        
      } catch (error) {
        console.log(error);
      }
    }
```
***
* JSbase64编码、解码
```
window.btoa(args) // 编码

window.atob(args) // 解码

```

***

* 粘性定位

  ` position: sticky  `

  > ​     基本上，可以看出是`position:relative`和`position:fixed`的结合体——当元素在屏幕内，表现为relative，就要滚出显示器屏幕的时候，表现为fixed。
  >
  > **注意:**
  >
  > > **1.position:sticky要想生效，top｜bottom| left|right 属性（看滚动方向）是必须要有明确的计算值的，否则fixed的表现不会出现。**
  > >
  > > **2. 父级及祖先元素不能有任何`overflow:visible`以外的overflow设置，否则没有粘滞效果。**

***

* nginx 响应头解决跨域

  ```
  server {
    listen       80;
    server_name  be.sherlocked93.club;
    
  	add_header 'Access-Control-Allow-Origin' $http_origin;   # 全局变量获得当前请求origin，带cookie的请求不支持*
  	add_header 'Access-Control-Allow-Credentials' 'true';    # 为 true 可带上 cookie
  	add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';  # 允许请求方法
  	add_header 'Access-Control-Allow-Headers' $http_access_control_request_headers;  # 允许请求的 header，可以为 *
  	add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
  	
    if ($request_method = 'OPTIONS') {
  		add_header 'Access-Control-Max-Age' 1728000;   # OPTIONS 请求的有效期，在有效期内不用发出另一条预检请求
  		add_header 'Content-Type' 'text/plain; charset=utf-8';
  		add_header 'Content-Length' 0;
      
  		return 204;                  # 200 也可以
  	}
    
  	location / {
  		root  /usr/share/nginx/html/be;
  		index index.html;
  	}
  }
  ```

  
