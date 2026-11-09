# Typecho Passkey 插件

为 Typecho 提供基于 WebAuthn 的 Passkey 登录功能。

本仓库基于 [little-gt/Passkey](https://github.com/little-gt/Passkey) 修改维护。插件的完整功能说明、配置方法和安全设计请参考上游仓库。

当前版本：`1.2.3`

## 本仓库修改

### 自动注入修复

- 使用 Typecho 正式的前台和后台模板钩子加载资源。
- 修复 VOID 等主题首页登录表单无法自动显示 Passkey 按钮的问题。
- 根据实际登录表单注入按钮，不再依赖 `login.php` 或 `/admin/` URL 判断。
- 支持通过 `__TYPECHO_ADMIN_DIR__` 自定义后台目录。
- 管理面板未登录时使用 Typecho 提供的 `$options->loginUrl` 跳转。

### 认证器兼容性修复

“允许所有验证器”模式不再指定：

- `authenticatorAttachment`
- `client-device`、`security-key` hints
- 显式 `mediation`

认证器选择交由浏览器处理，不主动偏向设备验证器、物理安全密钥或特定密码管理器。

WebAuthn 无法指定或主动唤起 1Password。使用前需要在浏览器中启用 1Password Passkey 集成，并在 1Password 中注册对应 RP ID 的凭证。

“仅允许平台验证器”和严格模式仍会设置：

```text
authenticatorAttachment=platform
```

平台认证失败后不会降级到其他验证器。

### 登录按钮样式

- Passkey 按钮放置在原登录按钮下方。
- 删除“或”分隔文字和独立背景区域。
- 自动继承原登录按钮的颜色、边框、字体及高度。
- 按钮宽度占满登录表单。
- 适配 VOID 窄侧栏和 Typecho 后台登录页面。

### 同步上游 v1.2.1

- 设置页内联 `onclick` 改为 `data-action` 事件委托，移除 `@deprecated` 全局函数。
- 一并移除参数输入框上残留的 `onchange`/`oninput`（上游遗漏，会在控制台报 `ReferenceError`）。
- 未同步上游重新生成的 `panel.css`（依赖 BooAdmin 变量与 `color-mix()`，对本插件无实际收益）。

### 插件信息

- 插件 `@link` 指向本仓库。

### 仓库结构

- 插件文件铺平至仓库根目录。
- 将 `assist/` 更名为 `assets/`。
- 删除未使用的 Tailwind 构建配置和旧截图。
- 保留原项目 MIT 许可证及作者署名。

## 安装

插件运行目录必须命名为 `Passkey`：

```bash
cd /var/www/typecho/usr/plugins/
git clone https://github.com/mikusaa/Typecho-Plugin-Passkey.git Passkey
```

然后在 Typecho 后台启用并配置 Passkey 插件。

## 上游项目

- 项目地址：<https://github.com/little-gt/Passkey>
- 完整文档：<https://github.com/little-gt/Passkey#readme>

## 许可证

本项目继续使用 [MIT License](LICENSE)。

原项目作者：GARFIELDTOM & little-AI
