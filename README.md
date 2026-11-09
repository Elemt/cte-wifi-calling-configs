# CTE Wi‑Fi Calling 激活配置模板

这是一套供 iPhone 用户参考的 Shadowrocket 与 Surge 配置模板，用于让 CMLink／CTExcel 相关的 Wi‑Fi Calling 注册流量经英国 WireGuard 出口转发，并为美国 AT&T Wi‑Fi Calling 保留必要的直连域名。

仓库只包含脱敏模板，不包含任何可用的 VPN 私钥、预共享密钥、账号、订阅地址或个人静态 IP。每位使用者必须从自己的 VPN 服务商获取独立的 WireGuard 配置。

## 文件

- `shadowrocket/CTE-Shadowrocket-Mobile.conf`：可直接分享和导入的 CTE 分流配置，不含节点密钥。
- `shadowrocket/VPNUK-WireGuard.example.conf`：标准 WireGuard 节点字段示例，必须替换占位符。
- `surge/CTE-VPNUK-WireGuard.example.conf`：Surge WireGuard 与 CTE 路由示例，必须合并到个人配置并替换占位符。

## iPhone + Shadowrocket（无需软路由）

建议使用 iOS 16.4 或以上版本，并使用支持 WireGuard 的新版 Shadowrocket。

1. 导入 `shadowrocket/CTE-Shadowrocket-Mobile.conf`，并将其设为当前配置。
2. 从自己的 VPN 服务商下载一份独立 WireGuard `.conf`，导入 Shadowrocket 并选择该节点。
3. 在“设置 → 隧道”中启用：
   - 包括所有网络
   - 包括蜂窝服务
   - 强制路由
4. 保持“包括本地网络”关闭、“睡眠时断开”关闭。
5. 初次激活时将“全局路由”设为“代理”。
6. 如运营商校验定位，先将设备定位修改到英国。
7. 开启飞行模式，再单独开启 Wi‑Fi，连接 Shadowrocket 后启用 SIM/eSIM 与 Wi‑Fi Calling。
8. 激活完成后可把“全局路由”切回“配置”，仅让配置中的 CTE 流量走英国节点。

国内 Wi‑Fi 只是 WireGuard 的底层入口网络。只要该网络允许 UDP，代理后的出口仍可以是英国 IP。

## Surge

`surge/CTE-VPNUK-WireGuard.example.conf` 是结构示例，不应直接覆盖现有 Surge 配置。请把其中的 `[Proxy]`、`[Proxy Group]`、`[WireGuard cte-uk]` 与 CTE 规则合并到个人配置，并将占位符替换为自己的 WireGuard 参数。

修改后先检查语法：

```bash
/Applications/Surge.app/Contents/Applications/surge-cli --check /path/to/profile.conf
```

## 必须注意

- 不要上传或分享真实的 `PrivateKey`、`PresharedKey`、订阅链接或账号信息。
- 不要让两台设备同时使用同一个 WireGuard 私钥和内部地址；应为每台设备生成独立配置。
- WireGuard 依赖 UDP。公司、校园、酒店或公共 Wi‑Fi 可能会阻断 UDP。
- AT&T Wi‑Fi Calling 使用 `epdg.epc.att.net` 建立运营商的 Wi‑Fi Calling 隧道；配置中将该域名及相关服务域名设为直连，以兼容 UDP 500/4500 的 IPsec 建连。
- 该仓库只提供网络配置示例，不保证运营商一定允许或完成注册。
- 请遵守运营商服务条款和所在地法律法规。
