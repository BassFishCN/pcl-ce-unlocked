# PCL-CE 解锁构建（学习与连通性测试用途 · 非官方）

> **重要声明**
>
> - 本仓库**仅用于学习与技术研究**：目的是学习、测试 Minecraft 启动器在不同系统区域设置下与官方及镜像下载服务之间的**连通性行为**（区域策略如何判定、下载链路如何工作）。
> - 本仓库**不是** Plain Craft Launcher（PCL）或 PCL 社区版（PCL-CE）的官方发布，与原作者龙腾猫跃及 PCL-Community 社区**没有任何隶属、授权或合作关系**。请优先支持并使用官方渠道发布的版本。
> - 本构建基于开源项目 [PCL-Community/PCL-CE](https://github.com/PCL-Community/PCL-CE) 的 v2.15.1 源码自行编译，遵循其开源协议 **Apache License 2.0**（见仓库根目录 `LICENSE`）。PCL-CE 的上游为龙腾猫跃的 PCL2，在此向原作者与社区维护者致谢。

## 与上游的唯一差异

PCL-CE 自 2026-10-20 起新增了区域策略：综合设备区域、系统语言与时区判定为中国大陆时，禁用 Minecraft 下载、版本安装、整合包安装与文件补全功能。

本构建只修改了一个文件，解除该区域限制，完整差异见 [`unlock.patch`](unlock.patch)：

- `Plain Craft Launcher 2/Modules/Minecraft/RestrictedFeaturePolicy.cs`
  —— 所有受限功能都经过这个策略类，本构建令其校验恒定放行（`IsRestricted => false`、`Check => true`、`ThrowIfRestricted` 不再拦截）。除此之外的代码与 v2.15.1 上游源码逐字节一致。

## 构建与校验

- 构建命令（与上游官方 CI 相同路线）：`dotnet publish "Plain Craft Launcher 2/Plain Craft Launcher 2.csproj" -p:Configuration=Release -p:Platform=x64 --no-self-contained`（.NET SDK 10.0.402）。
- 产物 `PCL2_CE_Unlocked_x64.exe` 见本仓库 Releases。体积与官方 Release 资产仅相差约 1KB（官方构建含签名块与内置密钥，本构建不含）。
- 已对编译产物做反编译抽查，确认解锁逻辑已编译进成品。
- 注意：本构建未内置官方 CI 的 CurseForge API 密钥，游戏本体/加载器下载与 Modrinth 资源不受影响，CurseForge 站内搜索可能受限。

## 运行要求

- Windows 10/11 x64；首次运行若提示缺少运行时，请安装 **.NET 10 桌面运行时（x64）** 后再打开。
- 免安装，单文件，双击即用。下载源建议在启动器设置中选择镜像源以提升连通性与速度。

## 协议与署名

- 本仓库代码与构建遵循 **Apache License 2.0**，与上游 PCL-CE 保持一致。
- “PCL”“Plain Craft Launcher”等名称与相关权利归原作者所有；本仓库仅作学习测试用途，请勿用于商业分发。
- 如原作者或社区认为本仓库不妥，请通过 Issue 联系，将第一时间配合处理（包括下架）。
