## 简介

针对这计划的 GitHub Releases、Archives 以及项目文件的加速项目，支持 `git clone`，有Cloudflare Workers 无服务器版本以及 Python 版本。由原作者 hunshcn 的版本修改而来。

## 主站

[https://gh.mirrors.zheproject.us.kg/](https://gh.mirrors.zheproject.us.kg/)（规划中）

如果主站不可用，欢迎朋友们自行部署镜像站点。

## Python 版本和 CF Workers 版本差异

- Python 版本支持进行文件大小限制，超过设定返回上游地址（[源仓库 Issue #8](https://github.com/hunshcn/gh-proxy/issues/8)）。

- Python 版本支持特定 user/repo 封禁/白名单以及 passby（[源仓库 Issue #41](https://github.com/hunshcn/gh-proxy/issues/41)）。

## 使用

直接在复制出来的资源链接前加`https://gh.mirrors.zheproject.us.kg/`即可。***只可加速计划持有的仓库，其它仓库会被 403。***

也可以直接访问，在输入框中粘贴资源链接，点击“加速”按钮即可开始下载。

访问计划的私有仓库可以通过

`git clone https://user:TOKEN@gh.mirrors.zheproject.us.kg/https://github.com/xxxx/xxxx`（[源仓库 Issue #71](https://github.com/hunshcn/gh-proxy/issues/71)）

以下都是合法输入（仅示例，文件不存在）：

- 分支源码：https://github.com/thezheproject/project/archive/main.zip

- Releases 源码：https://github.com/thezheproject/project/archive/v0.1.0.tar.gz

- Releases 文件：https://github.com/thezheproject/project/releases/download/v0.1.0/example.zip

- 分支文件：https://github.com/thezheproject/project/blob/main/filename

- Git Commit 文件：https://github.com/thezheproject/project/blob/1111111111111111111111111111/filename

- Gist：https://gist.githubusercontent.com/cielpy/351557e6e465c12986419ac5a4dd2568/raw/cmd.py

## CF Workers 版本部署

首先通过 Cloudflare 仪表板[注册一个账号](https://dash.cloudflare.com/sign-up)（如果已经注册，登录即可），然后访问 [Workers 的首页](https://workers.cloudflare.com)，划到下面并点击“Start building”按钮，在跳出的仪表板界面中设置一个子域名，然后点击“Create a Worker”。

复制 [index.js](https://cdn.jsdelivr.net/gh/thezheproject/zheproject-ghproxy@master/index.js) 的内容到左侧代码框，点击“Save and deploy”。如果正常，右侧应显示 Workers 仪表板首页，并伴有提示。

`ASSET_URL`是静态资源的链接（实际上就是现在显示出来的那个输入框单页面）

`PREFIX`是前缀，默认（根路径情况为"/"），如果自定义路由为 example.com/gh/*，请将PREFIX改为 '/gh/'。

*注意：*代码对路径格式的要求严苛，**少一个杠都会错！**

## Python 版本部署

<!--
### Docker 部署

```
docker run -d --name="gh-proxy-py" \
  -p 0.0.0.0:80:80 \
  --restart=always \
  hunsh/gh-proxy-py:latest
```

第一个80是你要暴露出去的端口
-->

### 直接部署

安装依赖（请使用 Python 3，并且尽量使用新版本）

```pip install flask requests```

按需求修改 `app/main.py` 的前几项配置

*注意：*可能需要在 `return Response` 前加以下两行代码：
```python
if 'Transfer-Encoding' in headers:
    headers.pop('Transfer-Encoding')
```

### 注意

针对 Python 版本，如果机器无法正常访问（或当地防火墙屏蔽）`github.io`，程序会自动报错。遇此情况请自行修改静态文件链接。

Python 版本默认走服务器（2021.3.27 更新）

## Cloudflare Workers 计费

到仪表板的 `Overview` 页面可参看使用情况。免费版每天有 10 万次免费请求，并且有每分钟 1000 次请求的限制。

如果不够用，可升级到 $5 的高级版本，每月可用 1000 万次请求（超出部分额外支付 $0.5/百万次请求）。

## 更新日志

### 计划更改版本

### 原始版本
* 2020.04.10 增加对`raw.githubusercontent.com`文件的支持
* 2020.04.09 增加Python版本（使用Flask）
* 2020.03.23 新增了clone的支持
* 2020.03.22 初始版本

## 链接

[原作者 hunshcn 的博客](https://hunsh.net)

## 参考

[jsproxy](https://github.com/EtherDream/jsproxy/)

## 向原作者 hunshcn 捐赠

![wx.png](https://img.maocdn.cn/img/2021/04/24/image.md.png)
![ali.png](https://www.helloimg.com/images/2021/04/24/BK9vmb.md.png)
