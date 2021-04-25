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
