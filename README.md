# One

## 简介
~~前段时间(大概是暑假的时候)整理博客的时候发现博客静态资源加载速度太慢了，而且也加载了各种来源的资源，于是就萌发了写一个整合静态资源的小程序，因为只会 PHP，所以自然也就用 PHP 来写了 (x~~

因为 PHP 同步执行的特性，貌似不是很适合来写这种请求密集型应用，正好最近爱上了 Node，所以就决定用 Node.js 重写一个 (

## Feature

- 简易的配置文件
- 轻量级的代码
- 高效的缓存机制
- Node.js 异步快速响应
- 各种各样奇怪的Bug (x

## 食用方法

- ```git clone https://github.com/LoliLin/One.git```
- ```cd One && npm i```
- 修改 `Cache` 文件中的储存库信息及静态文件地址
- Enjoy it!

根目录下的 `replace.js` 为替换页面资源地址的脚本，未完整测试，不保证无错误 (

## 配置说明

在 `Cache` 目录下，每个子目录即为一个命名空间，在每个子目录中应存在一个 `info.json`，该文件格式如下:
```json
{
    "host": "https://xiaolin.in/",
    "expireTime": 0
}
```
`host` 为要代理的主机，结尾必须加 `/`，`expireTime` 为过期时间，以毫秒为单位

缓存的数据会存在命名空间的 `data` 目录下

程序默认监听 `3000` 端口，需要更改可直接在程序中更改

## Docker

咕了一百年终于有空把凄惨 `Dockerfile` 给写了

运行方法很简单，`git clone` 之后在根目录输入 `sudo docker build -t one:v0.1 .` 就完成了镜像的部署

向外暴露的端口是 `3000`，`docker run` 的时候记得开放端口和目录

示例代码: `sudo docker run -d -p 3000:3000 -v /path/to/One/Cache:/home/One/Cache one:v0.1`

## 性能

```
# isXiaoLin/One
> wrk -t12 -c400 -d10s http://127.0.0.1:3000/avatar/fddaa643e0f9173104dbcb711b363b5f
Running 10s test @ http://127.0.0.1:3000/avatar/fddaa643e0f9173104dbcb711b363b5f
  12 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     8.70ms   15.47ms 140.26ms   90.98%
    Req/Sec     5.18k     2.05k    9.08k    70.97%
  466580 requests in 10.10s, 1.08GB read
  Socket errors: connect 155, read 87, write 0, timeout 0
Requests/sec:  46194.75
Transfer/sec:    109.21MB

# idawnlight/one-rust debug
> wrk -t12 -c400 -d10s http://127.0.0.1:8088/test/fddaa643e0f9173104dbcb711b363b5f
Running 10s test @ http://127.0.0.1:8088/test/fddaa643e0f9173104dbcb711b363b5f
  12 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    14.29ms   41.30ms   1.60s    94.17%
    Req/Sec     2.29k     0.89k    9.15k    68.29%
  205918 requests in 10.10s, 61.86MB read
  Socket errors: connect 155, read 109, write 0, timeout 4
Requests/sec:  20387.33
Transfer/sec:      6.12MB

# idawnlight/one-rust release
> wrk -t12 -c400 -d10s http://127.0.0.1:8088/test/fddaa643e0f9173104dbcb711b363b5f
Running 10s test @ http://127.0.0.1:8088/test/fddaa643e0f9173104dbcb711b363b5f
  12 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     7.28ms   22.27ms   1.02s    92.76%
    Req/Sec    12.19k     5.28k   29.02k    66.04%
  1217540 requests in 10.10s, 365.76MB read
  Socket errors: connect 155, read 104, write 0, timeout 7
Requests/sec: 120554.33
Transfer/sec:     36.22MB
```

疯狂优化才勉强追上了隔壁 Rust 版的屁股, 服气 (

## DEMO

~~static.xiaolin.in~~

~~getxiaol.in~~

~~国内使用腾讯云 CDN，国外使用Azure CDN 因为嫖了个域名没备案所以就裸机扔在了 rixCloud 的 HK 服务器上~~ rixCloud 的服务器又双叒叕炸了，只好搞了下扔到了腾讯云上

访问 小霖的个人页 并单击 F12 查看 Source 选项卡，静态资源全部都是 static.xiaolin.in 是吧 (x

## Tips

- ~~建议套 CDN ，不然可能起不到啥效果~~
- 小心你的流量 (
- 当遇到很奇葩的 CDN 供应商(就是那种缓存内容不看 max-age 的，比如腾讯云)的时候，你需要到 CDN 供应商那里通过目录设置缓存
- 小心你的服务器磁盘
- 不要用部署在国内的服务器本程序去缓存 404 网站的内容 (
- 小心 Bug (((
