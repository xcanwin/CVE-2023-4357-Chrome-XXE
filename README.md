## 工具简介

- 全球首款单文件利用 CVE-2023-4357 Chrome XXE 漏洞 EXP，实现对访客者本地文件窃取。
- The world's first single file exploits the CVE-2023-4357 Chrome XXE vulnerability EXP, allowing attackers to obtain local files of visitors.
- 单文件漏洞利用的思路：自我包含。第一次实体声明引用外部实体是被拦截的，于是想到通过将自身作为外部XML文档进行自我包含后，再进行第二次实体声明引用外部实体，并且要求两次的引用的格式能相互兼容不报错，即可绕过拦截，读取本地文件。

## 漏洞简介

| 信息 | 内容 |
| --- | --- |
| 漏洞名称 | Chromium XXE |
| 漏洞编号 | CVE-2023-4357 |
| 风险等级 | 高危 |
| 漏洞类型 | XXE |
| 利用难度 | 低 |

## 漏洞描述

- 漏洞根源在于libxslt。默认情况下，Chromium 会严格校验XML文档的实体声明所引用的外部实体URL是否跨域，但是，如果 Chromium 是先解析为XSL样式表，再调用document()包含外部XML文档，则此时的 Chromium 不对这个外部XML文档URL进行跨域校验，造成访问者本地文件泄露。

## 影响版本

- [Chrome](https://www.google.com/chrome/) 版本 < 116.0.5845.96
- [Chromium](https://www.chromium.org/) 版本 < 116.0.5845.96
- [Electron](https://www.electronjs.org/) 版本 < 26.1.0
- [微信Mac版](https://mac.weixin.qq.com/) 版本 < 3.8.5.17

## 复现截图

| 访问者环境 | 截图 |
| -- | -- |
| Linux + Chromium | <img src="/assets/poc-lc.png" width="660"></img> |
| Windows + Chromium | <img src="/assets/poc-wc.png" width="660"></img> |
| MacOS + 微信Mac版 | <img src="/assets/poc-mw.png" width="660"></img> |

## 复现1

1. 访问者环境：

```
Linux + Chromium
```

2. 下载并运行 Chrome

```
wget https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/114.0.5735.90/linux64/chrome-linux64.zip
unzip chrome-linux64.zip
./chrome-linux64/chrome --no-sandbox
```

3. 启动 Web 服务

```
wget https://codeload.github.com/xcanwin/CVE-2023-4357-Chrome-XXE/zip/refs/heads/main -O CVE-2023-4357-Chrome-XXE.zip
unzip CVE-2023-4357-Chrome-XXE.zip
python3 -m http.server 8888 -d CVE-2023-4357-Chrome-XXE-main
```

4. 浏览器访问

```
http://127.0.0.1:8888/d.svg
```

## 复现2

1. 访问者环境：

```
Windows + Chromium
```

2. 下载并运行 Chrome

```
https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/114.0.5735.90/win64/chrome-win64.zip

./chrome-win64/chrome --no-sandbox
```

3. 启动 Web 服务

```
https://codeload.github.com/xcanwin/CVE-2023-4357-Chrome-XXE/zip/refs/heads/main

python3 -m http.server 8888 -d CVE-2023-4357-Chrome-XXE-main
```

4. 浏览器访问

```
http://127.0.0.1:8888/d.svg
```

## 复现3

1. 访问者环境：

```
MacOS + 微信Mac版
```

2. 下载并运行 微信Mac版

3. 启动 Web 服务

```
wget https://codeload.github.com/xcanwin/CVE-2023-4357-Chrome-XXE/zip/refs/heads/main -O CVE-2023-4357-Chrome-XXE.zip
unzip CVE-2023-4357-Chrome-XXE.zip
python3 -m http.server 8888 -d CVE-2023-4357-Chrome-XXE-main
```

4. 向文件传输助手发送并访问

```
http://127.0.0.1:8888/d.svg
```
