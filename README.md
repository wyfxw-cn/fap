# FAP文件验证协议
文件验证协议
# File Authentication Protocol
文件验证协议

# 文件分享

[在线分享](https://file.wyfxw.cn/ "文件在线分享验证协议")

# 协议软件

[下载客户端](https://github.com/wyfxw-cn/fapclient "fapclient")

+ 运行客户端支持浏览器直接打开fap协议,fap://

# 协议分享

[分享文件](http://file.wyfxw.cn/?fap=7b22766572223a22312e30222c22646573223a6e756c6c2c22746162223a6e756c6c2c22707764223a6e756c6c2c2265787069726573223a313731313737383239352c226964223a6e756c6c2c2275736572223a6e756c6c2c22666c6167223a6e756c6c2c226c697374223a5b7b226e616d65223a22e7a6bbe695a3e695b0e5ada628e7acac33e789882920e98293e8be89e696872e706466222c22617574686f72223a22e98293e8be89e69687222c2273697a65223a35323037323636382c2273686131223a2263353634636361353031666139663132376136363131656635316539343431353336343961626462222c226d6435223a223863656138363034666936376566656561333663643036333365663661393433222c226331223a2236303330643338353163663235336430346438343432386262373563656431323434303534616138222c226332223a223638356464656134222c226333223a223135363839343534373336323638333131323531222c2275726c223a6e756c6c2c22746d70223a6e756c6c2c22657874223a22706466222c2274797065223a22646f63222c226d696d65223a226174746163686d656e74222c22646972223a6e756c6c2c2263726561746564223a313631313737383239352c2275706461746564223a313631313737383239357d5d7d "分享文件")

# 协议形式

fap://7b22766572223a22312e30222c22646573223a6e756c6c2c22746162223a6e756c6c2c22707764223a6e756c6c2c2265787069726573223a313731313737383239352c226964223a6e756c6c2c2275736572223a6e756c6c2c22666c6167223a6e756c6c2c226c697374223a5b7b226e616d65223a22e7a6bbe695a3e695b0e5ada628e7acac33e789882920e98293e8be89e696872e706466222c22617574686f72223a22e98293e8be89e69687222c2273697a65223a35323037323636382c2273686131223a2263353634636361353031666139663132376136363131656635316539343431353336343961626462222c226d6435223a223863656138363034666936376566656561333663643036333365663661393433222c226331223a2236303330643338353163663235336430346438343432386262373563656431323434303534616138222c226332223a223638356464656134222c226333223a223135363839343534373336323638333131323531222c2275726c223a6e756c6c2c22746d70223a6e756c6c2c22657874223a22706466222c2274797065223a22646f63222c226d696d65223a226174746163686d656e74222c22646972223a6e756c6c2c2263726561746564223a313631313737383239352c2275706461746564223a313631313737383239357d5d7d

# 协议内容格式

```
{
    "ver": "1.0",
    "des": null,
    "tab": null,
    "pwd": null,
    "expires": 1711778295,
    "id": null,
    "user": null,
    "flag": null,
    "list": [
        {
            "name": "离散数学(第3版) 邓辉文.pdf",
            "author": "邓辉文",
            "size": 52072668,
            "sha1": "c564cca501fa9f127a6611ef51e944153649abdb",
            "md5": "8cea8604fi67efeea36cd0633ef6a943",
            "c1": "6030d3851cf253d04d84428bb75ced1244054aa8",
            "c2": "685ddea4",
            "c3": "15689454736268311251",
            "url": null,
            "tmp": null,
            "ext": "pdf",
            "type": "doc",
            "mime": "attachment",
            "dir": null,
            "created": 1611778295,
            "updated": 1611778295
        }
    ]
}
```
+ ver 定义版本格式
+ des 这个fap文件描述信息
+ list 文件列表,大部分值可null
+ c1,c2,c3 自定义校验值,tab 定义标签区分c1,c2,c3含义
+ url 相关链接,tmp 缩略图
+ dir 所在相对当前路径的目录
+ expires,pwd expires未过期且pwd正确才可通过id访问到,留空不验证
+ id,user,flag 后台自动生成

# 转换代码

```
        function bin2hex(str) {
            var ret = '';
            var r = /[0-9a-zA-Z_.~!*()]/;
            for (var i = 0, l = str.length; i < l; i++) {
                if (r.test(str.charAt(i))) {
                    ret += str.charCodeAt(i).toString(16);
                } else {
                    ret += encodeURIComponent(str.charAt(i)).replace(/%/g, '');
                }
            }
            return ret.toLowerCase();
        }
        function hex2bin(str) {
            var ret = '';
            var tmp = '';
            for (var i = 0; i < str.length - 1; i += 2) {
                var c = String.fromCharCode(parseInt(str.substr(i, 2), 16));
                if (c.charCodeAt() > 127) {
                    tmp += '%' + str.substr(i, 2);
                    if (tmp.length == 9) {
                        ret += decodeURIComponent(tmp);
                        tmp = '';
                    }
                } else {
                    ret += c;
                }
            }
            return ret;
        }
        function fapHex(files) {
            return 'fap://' + bin2hex(JSON.stringify(files))
        }
        function hexFap(fap) {
            if (fap.substr(0, 6) != 'fap://') throw 'neq fap://'
            fap = fap.substr(6)
            return hex2bin(fap)
        }
```
