**这里是主流科学上网的优化配置及最优组合示例。如是不太了解科学上网，建议先依次从简单到复杂参考及部署。**  
1. 示例实现了端口转发到进程转发及PROXY protocol的从低到高（效能）应用支持。
2. 示例实现了端口回落分流到进程回落分流及PROXY protocol的从低到高（效能）应用支持。
3. 综合应用示例实现了nginx SNI/haproxy SNI/v2ray SNI的端口分流到进程分流及PROXY protocol的从低到高（效能）应用支持。
4. nginx SNI分流实现了tcp/udp分流，为fullcone应用提供了基础。
5. naiveproxy除进程监听（server进程）外，实现了支持http/3代理应用，即quic协议传输。
6. 除v2ray(vless\vmess+kcp+seed)示例外，所有示例实现了回落或反代网站都支持http自动跳转到https，且SSL/TLS安全评估报告为A+。
* **注：** 端口转发、端口回落分流、SNI的端口分流指基于local loopback应用，实现的不同应用功能；进程转发、进程回落分流、SNI的进程分流指基于Unix Domain Socket应用，实现的不同应用功能。

### 单一应用服务器端配置示例
1. [v2ray(vless\vmess+kcp+seed)](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%5Cvmess%2Bkcp%2Bseed)) （vless或vmess的kcp应用。若网络极差，推荐部署。）  
2. [v2ray(vless\vmess+ws)+caddy2\nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%5Cvmess%2Bws)%2Bcaddy2%5Cnginx) （caddy2或nginx反向代理vless或vmess的WS应用。）  
3. [v2ray(socks\shadowsocks+ws)+caddy2\nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(socks%5Cshadowsocks%2Bws)%2Bcaddy2%5Cnginx) （caddy2或nginx反向代理socks或shadowsocks的WS应用。）  
4. [v2ray(vless\vmess+h2c)+caddy2](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%5Cvmess%2Bh2c)%2Bcaddy2) （caddy2反向代理vless或vmess的H2应用。）  
5. [v2ray(SS+v2ray-plugin)+caddy2\nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(SS%2Bv2ray-plugin)%2Bcaddy2%5Cnginx) （caddy2或nginx反向代理shadowsocks加v2ray-plugin插件的WS应用。）  
6. [v2ray(vless+tcp+tls)+caddy2](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%2Btcp%2Btls)%2Bcaddy2) （vless的tcp应用，WEB回落给caddy2。）  
7. [v2ray(vless+tcp+tls)+nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%2Btcp%2Btls)%2Bnginx) （vless的tcp应用，WEB回落给nginx。）  
8. [v2ray(trojan+tcp+tls)+caddy2](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(trojan%2Btcp%2Btls)%2Bcaddy2) （兼容trojan应用，WEB回落给caddy2。）  
9. [v2ray(trojan+tcp+tls)+nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(trojan%2Btcp%2Btls)%2Bnginx) （兼容trojan应用，WEB回落给nginx。）  
10. [v2ray(trojan+ws)+caddy2\nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(trojan%2Bws)%2Bcaddy2%5Cnginx) （caddy2或nginx反向代理兼容trojan-go的WS应用。） 
---
1. [trojan\trojan-go+caddy2\naiveproxy](https://github.com/lxhao61/integrated-examples/tree/master/trojan%5Ctrojan-go%2Bcaddy2%5Cnaiveproxy) （trojan或trojan-go应用，WEB回落给caddy2\ +naiveproxy应用。）  
2. [trojan\trojan-go+nginx](https://github.com/lxhao61/integrated-examples/tree/master/trojan%5Ctrojan-go%2Bnginx) （trojan或trojan-go应用，WEB回落给nginx。）  
---
1. [naiveproxy(caddy2+forwardproxy)](https://github.com/lxhao61/integrated-examples/tree/master/naiveproxy(caddy2%2Bforwardproxy)) （naiveproxy应用，以H2或H3代理。）  

### 综合应用服务器端配置示例
#### &emsp;v2ray或Xray为主、caddy2为辅及其它应用。
1. [v2ray(complete+h2c-tcp)+caddy2\naiveproxy](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete%2Bh2c-tcp)%2Bcaddy2%5Cnaiveproxy) （caddy2反向代理WS与H2的综合应用\ +naiveproxy应用。）  
---
1. [v2ray(vless+tcp&ws+tls)+caddy2\naiveproxy](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%2Btcp%26ws%2Btls)%2Bcaddy2%5Cnaiveproxy) （vless+tcp与WS类应用，WEB回落给caddy2\ +naiveproxy应用。）  
2. [v2ray(complete+h2c)+caddy2\naiveproxy](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete%2Bh2c)%2Bcaddy2%5Cnaiveproxy) （v2ray或Xray综合应用+反向代理H2应用\ +naiveproxy应用。）  
3. [v2ray(complete+h2c)+naiveproxy+trojan](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete%2Bh2c)%2Bnaiveproxy%2Btrojan) （上一项应用+trojan应用及共用端口。）  
4. [v2ray(complete+h2c)+naiveproxy+trojan+haproxy](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete%2Bh2c)%2Bnaiveproxy%2Btrojan%2Bhaproxy) （用haproxy对上一项应用进行SNI分流，共用端口。）  
---
1. [v2ray(vless&trojan+tcp&ws+tls)+caddy2](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%26trojan%2Btcp%26ws%2Btls)%2Bcaddy2) （回落终极部署/套娃方式，或共用端口。）  
2. [v2ray(complete+trojan+h2c)+naiveproxy](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete%2Btrojan%2Bh2c)%2Bnaiveproxy) （v2ray或Xray全部应用+naiveproxy应用及共用端口。）  
3. [v2ray(complete+trojan+h2c)+naiveproxy+haproxy](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete%2Btrojan%2Bh2c)%2Bnaiveproxy%2Bhaproxy) （用haproxy对上一项应用进行SNI分流，共用端口。）  
#### &emsp;v2ray或Xray为主、nginx为辅及其它应用。  
1. [v2ray(complete-tcp)+nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete-tcp)%2Bnginx) （nginx反向代理WS的综合应用。）  
---
1. [v2ray(vless+tcp&ws+tls)+nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%2Btcp%26ws%2Btls)%2Bnginx) （vless+tcp与WS类应用，WEB回落给nginx。）  
2. [v2ray(complete)+nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete)%2Bnginx) （v2ray或Xray综合应用。）  
3. [v2ray(complete)+nginx+trojan](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete)%2Bnginx%2Btrojan) （上一项应用+trojan应用及共用端口。）  
4. [v2ray(complete+h2c)+nginx+trojan+naiveproxy](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete%2Bh2c)%2Bnginx%2Btrojan%2Bnaiveproxy) （上一项应用+naiveproxy+反向代理H2应用及共用端口。）  
---
1. [v2ray(vless&trojan+tcp&ws+tls)+nginx](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(vless%26trojan%2Btcp%26ws%2Btls)%2Bnginx) （回落终极部署/套娃方式，或共用端口。）  
2. [v2ray(complete+trojan+h2c)+nginx+naiveproxy](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(complete%2Btrojan%2Bh2c)%2Bnginx%2Bnaiveproxy) （v2ray或Xray全部应用+naiveproxy应用及共用端口。）  

### 以上所有实例（含单一与综合示例）注意:
1. 所有v2ray或Xray配置文件都配置了禁用BT。如不需要，可以删除相关配置（参考v2ray(other configuration)中BT_config.json文件）。  
2. v2ray从版本v4.33.0删除了xtls应用，故若还想用xtls应用，请选Xray。Xray是v2ray分支，也是因为这个应用分家。另外注意：配置示例中log部分的路径名称，需修改为对应的xray或v2ray。   
3. complete表示包含v2ray或Xray的vless+tcp+tls、vless+ws+tls、SS+v2ray-plugin+tls、vless+kcp+seed的综合应用。  
4. naiveproxy=caddy2+forwardproxy。此程序文件已编译好，本人github下载即可。  
5. 目前naiveproxy的Unix Domain Socket应用（即进程回落与进程转发给它），不支持http3，如开启caddy无法启动。  
6. 所有ws（WebSocket）类应用支持CDN加速，本人github示例中全部启用了tls，保证非CDN加速应用也可以安全的科学上网。

### 特殊应用服务器端配置示例
1. [v2ray(other configuration)](https://github.com/lxhao61/integrated-examples/tree/master/v2ray(other%20configuration)) （v2ray或Xray其它多种特殊应用配置方法。）  
2. [caddy2(other configuration)](https://github.com/lxhao61/integrated-examples/tree/master/caddy2(other%20configuration)) （分别回落caddy2不同网站的配置方法。）  
3. [nginx(other configuration)](https://github.com/lxhao61/integrated-examples/tree/master/nginx(other%20configuration)) （nginx SNI分流v2ray或Xray回落应用与网站应用的配置方法。） 

### client configuration  
&emsp;[官方客户端文本配置示例](https://github.com/lxhao61/integrated-examples/tree/master/client%20configuration)（使用图形界面客户端仅参考即可。）

### service configuration  
&emsp;[程序service配置示例](https://github.com/lxhao61/integrated-examples/tree/master/service%20configuration)（手工配置程序由操作系统管理及自动运行可参考。）

### 使用/贡献指南  
1. 若程序增加新功能，开始在单一应用服务器端配置示例中添加；过一段时间稳定后才会综合应用服务器端配置示例中添加。如除trojan+tcp套娃外，vless+tcp及trojan+tcp的xtls已全部加上。  
2. 欢迎你提交 PR ,如对现行配置示例优化修订，或将自己使用的配置制作模板提交等。
