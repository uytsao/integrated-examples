nginx SNI 分流共用443端口

利用 nginx 支持 SNI 分流特性，对 vless+tcp、trojan+tcp、naiveproxy(caddy2) 进行端口分流（四层转发），实现共用443端口。vless+tcp 以 h2 或 http/1.1 自适应协商连接，分流出 ws（WebSocket）连接，回落给 nginx。trojan+tcp 以 h2 协商连接，也回落给 nginx。另 caddy2 同时为 vless/vmess+h2c 提供反向代理，为 naiveproxy 提供正向代理。v2ray 包括应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它ws类应用，参考反向代理ws类的单一示例。）

3、SS+v2ray-plugin+tls（tls由vless+tcp+tls提供及处理，不需配置。）

4、vless+h2c+tls（tls由caddy2提供及处理，不需配置；另可改成或添加vmess+h2c+tls应用，参考反向代理h2类的单一示例。）

5、trojan+tcp+tls（回落配置。）

6、vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 协议。 

2、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）；故此 vless+tcp 应用中的回落端口或进程必须分开，分别对应。

3、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用），故回落端口或进程必须分开。而 trojan+tcp 目前不支持回落端口或进程分离 ，故 trojan+tcp 连接及回落只能二选一。本次示例 trojan+tcp 采用 h2 连接及回落，毕竟 h2 连接自带链路复用，且延迟小一点。

4、nginx 预编译程序包可能不带支持 PROXY protocol 协议的模块。如要使用此项协议应用，需加 http_realip_module 及 stream_realip_module 两模块构建自定义模板，再进行源代码编译和安装。另编译时选取源代码版本建议不要低于1.13.11。

5、nginx 预编译程序包一般不带支持 SNI 分流协议的模块。如要使用此项协议应用，需加 stream_ssl_preread_module 模块构建自定义模板，再进行源代码编译和安装。

6、caddy2 等于或大于 v2.2.0-rc.1 版才支持 h2c proxy，即支持 v2ray 的 h2（http/2）反向代理。

7、使用本人 github 中编译好的 caddy2 文件，才可同时支持 naiveproxy、h2（http/2）反向代理的应用。

8、配置1：端口转发、端口回落及 nginx SNI 的端口分流，没有启用 PROXY protocol。配置2：进程转发、进程回落及 nginx SNI 的进程分流，没有启用 PROXY protocol。配置3：进程转发、进程回落及 nginx SNI 的进程分流，启用了 PROXY protocol。
