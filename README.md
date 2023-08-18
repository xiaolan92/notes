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
const addZreo  = (number) =>{
        return string(number).padStart(2,"0");
      }
      const formatTime = (time,seperator = "-") =>{
        const _time = new Date(time);
        const year =_time.getFullYear();
        const month = _time.getMonth() + 1;
        const day = _time.getDate();
        const hours = _time.getHours();
        const minutes = _time.getMinutes();
        const seconds  = _time.getSeconds();

        return  `${year}${seperator}${addZreo(month)}${seperator}${addZreo(day)} ${addZreo(hours)}:${addZreo(minutes)}:${addZreo(seconds)}`;

      }

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

  

***

* nginx静态资源服务器

  ```
  location ~* \.(png|gif|jpg|jpeg)$ {
      root    /root/static/;  
      autoindex on;
      access_log  off;
      expires     10h;# 设置过期时间为10小时          
  }
  ```
  


***

* ##  配置https

  具体配置过程网上挺多的了，也可以使用你购买的某某云，一般都会有[免费申请](https://link.juejin.cn?target=https%3A%2F%2Fcloud.tencent.com%2Fdocument%2Fproduct%2F400%2F6814)的服务器证书，安装直接看所在云的操作指南即可。

  我购买的腾讯云提供的亚洲诚信机构颁发的免费证书只能一个域名使用，二级域名什么的需要另外申请，但是申请审批比较快，一般几分钟就能成功，然后下载证书的压缩文件，里面有个 nginx 文件夹，把 `xxx.crt` 和 `xxx.key` 文件拷贝到服务器目录，再配置下：

  ```nginx
  server {
    listen 443 ssl http2 default_server;   # SSL 访问端口号为 443
    server_name sherlocked93.club;         # 填写绑定证书的域名
  
    ssl_certificate /etc/nginx/https/1_sherlocked93.club_bundle.crt;   # 证书文件地址
    ssl_certificate_key /etc/nginx/https/2_sherlocked93.club.key;      # 私钥文件地址
    ssl_session_timeout 10m;
  
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;      #请按照以下协议配置
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
    ssl_prefer_server_ciphers on;
    
    location / {
      root         /usr/share/nginx/html;
      index        index.html index.htm;
    }
  }
  ```

  写完 `nginx -t -q` 校验一下，没问题就 `nginx -s reload`，现在去访问 `https://sherlocked93.club/` 就能访问 HTTPS 版的网站了。

  一般还可以加上几个增强安全性的命令：

  ```nginx
  add_header X-Frame-Options DENY;           # 减少点击劫持
  add_header X-Content-Type-Options nosniff; # 禁止服务器自动解析资源类型
  add_header X-Xss-Protection 1;             # 防XSS攻击
  ```

***

* css文字换行

      ```
      white-space:normal;
      word-break:break-all;
      word-wrap:break-word;
      ```



*  css多行省略

  ```
  width:200px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;（行数）
  -webkit-box-orient: vertical;
  ```

* css单行省略

  ```
  width:200px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space:nowrap;
  word-break:break-all;
  word-wrap:break-word;
  ```
***
神奇软件网站:
```
https://appstorrent.ru/

```
***
```
// 函数防抖
export const debounce = (fn, delay = 3000) => {
  let timer = null;
  return function (...agrs) {
    if (timer) {
      clearTimeout(timer);
    }

    timer = setTimeout(() => {
      fn.apply(this, agrs);
    }, delay);
  };
};
```
```
// 函数节流
export const throttle = (fn, delay = 3000) => {
  let startTime = 0;
  return function (...agrs) {
    let endTime = Date.now();
    if (endTime - startTime > delay) {
      startTime = endTime;
      fn.apply(this, agrs);
    }
  };
};
```
