# sap_interface

一个基于 .NET Framework 的 SAP 集成工具集合与示例项目。该仓库提供了一个用于与 SAP 系统通信的核心库（RFC/BAPI/ReadTable/Transaction 等封装）、若干桌面 GUI 工具（WinForms）用于生成 ABAP / 表结构代码，以及一个基于 ASP.NET MVC + WCF 的 Web/服务端示例。适用于在 Windows + Visual Studio 环境下开发或调试 SAP 集成场景。

## 主要功能
- 与 SAP 建立连接并调用 RFC/BAPI（封装在 SAPINT 库中）
- ReadTable / 事务 / 类型转换 等常见 SAP 操作的封装
- WCF 服务（示例）以远程方式暴露部分功能（在 SAPINTWEB/WcfServices）
- ASP.NET MVC 管理/查看界面（在 SAPINTWEB/MvcApplication1）
- 多个桌面工具（SAPINTCODE）用于生成 ABAP 代码、查看表结构与快速测试

## 仓库结构（概要）
- SAPINT/                核心库：连接、RFC/Function、ReadTable、工具类（SAPConnection.cs、SAPFunction.cs、ReadTable.cs 等）
- SAPINTCODE/            WinForms 工具，包含多个 GUI 窗口与代码生成器（FormAbapCode, FormGenerateTableClass 等）
- SAPINTWEB/             Web 层：ASP.NET MVC 应用与 WCF 服务（MvcApplication1、WcfServices）
- SAPINTDB/、SAPINTDBtest/  数据库相关项目（持久化或测试）
- SAPINTSERVER/、SAPINTGUI/  额外服务/GUI 项目
- MiniSqlQuery*          辅助 SQL 查询/解析工具
- sapint.config          全局或部署时的 SAP 配置文件示例/占位

## 关键文件
- SAPINT/Connect/SAPConnection.cs — 管理与 SAP 的连接
- SAPINT/Function/SAPFunction.cs — RFC/BAPI 调用封装
- SAPINT/Utils/ReadTable.cs — Read Table 操作
- SAPINTWEB/WcfServices/*.svc, ICodeManager.cs — WCF 服务接口与实现
- SAPINTCODE/*.cs / Designer.cs — WinForms 窗口与代码生成器实现

## 运行 / 开发（开发者快速上手）
先决条件：
- Windows（推荐） + Visual Studio（支持 .NET Framework 的版本）
- 有效的 SAP 访问凭据/目的地（RFC 登录信息）
- 安装 NuGet 包（仓库使用 packages.config）

快速步骤（在开发机上）：
1. 克隆仓库并打开解决方案
   - 在 Visual Studio 中打开 `SAPINTGUI.sln`（或需要的具体 .csproj）
2. 恢复 NuGet 包（Visual Studio 会基于 packages.config 恢复）
3. 配置 SAP 连接信息
   - 编辑根目录或项目下的配置文件（`sapint.config`、各项目的 `app.config` 或 `Web.config`），将 SAP 登录/目的地配置填入
4. 构建并运行
   - 在 Visual Studio 中选择启动项目并按 F5
   - 或使用命令行构建：
     ```
     msbuild SAPINTGUI.sln /p:Configuration=Debug
     ```

运行说明（按项目）：
- Web 应用（MVC）：将 SAPINTWEB/MvcApplication1 设为启动项目，或在 IIS 中部署该项目（检查 Web.config）
- WCF 服务：部署或在 Visual Studio 中启动 SAPINTWEB/WcfServices（检查 .svc 文件与 Web.config）
- 桌面工具：将 SAPINTCODE 或 SAPINTCODE 下具体 WinForms 项目设为启动项目并运行

## 配置示例（sapint.config / app.config）
仓库中未包含实际可用的 SAP 凭证。示例（请替换为你们环境的真实值）：
```xml
<!-- 示例片段（注意安全性：不要把真实密码放公共仓库） -->
<configuration>
  <appSettings>
    <add key="SAP_Server" value="sap.example.com"/>
    <add key="SAP_SystemNumber" value="00"/>
    <add key="SAP_Client" value="100"/>
    <add key="SAP_User" value="YOUR_USER"/>
    <add key="SAP_Password" value="YOUR_PASSWORD"/>
    <add key="SAP_Language" value="EN"/>
  </appSettings>
</configuration>
