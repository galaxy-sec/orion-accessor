# Orion Accessor

[![CI](https://github.com/galaxy-sec/orion-accessor/workflows/CI/badge.svg)](https://github.com/galaxy-sec/orion-accessor/actions)
[![Coverage Status](https://codecov.io/gh/galaxy-sec/orion-accessor/branch/main/graph/badge.svg)](https://codecov.io/gh/galaxy-sec/orion-accessor)
[![crates.io](https://img.shields.io/crates/v/orion-accessor.svg)](https://crates.io/crates/orion-accessor)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

一个Rust库，提供地址重定向、模板处理和变量扩展功能，专为现代开发工作流设计。

## 🚀 功能特性

### 地址重定向服务 (RedirectService)
- **智能重定向**: 基于通配符和精确匹配的重定向规则
- **环境变量支持**: 在配置中使用 `${VAR}` 和 `${VAR:-default}` 语法
- **多环境配置**: 支持不同环境的灵活配置
- **认证集成**: 内置HTTP基本认证支持

### 地址处理
- **统一接口**: 统一的地址访问抽象 (AddrAccessor)
- **多协议支持**: HTTP(S)、Git、本地文件系统
- **代理支持**: 内置HTTP代理和Git代理配置

## 📦 安装

在您的 `Cargo.toml` 中添加：

```toml
[dependencies]
orion-accessor = "0.6.0"
```

## 🚦 快速开始

### 重定向服务配置

创建 `redirect-rules.yml`:

```yaml
enable: true
units:
  - name: "github-mirror"
    rules:
      - pattern: "https://github.com/*"
        target: "https://ghproxy.com/https://github.com/"
      - pattern: "https://raw.githubusercontent.com/*"
        target: "https://ghproxy.com/https://raw.githubusercontent.com/"
    auth:
      username: "${GITHUB_USER}"
      password: "${GITHUB_TOKEN}"
```

### 代码使用示例

```rust
use orion_accessor::addr::redirect::RedirectService;

// 从配置文件加载
let service = RedirectService::from_file("redirect-rules.yml")?;

// 重定向地址
let original = "https://github.com/user/repo";
if let Some(redirected) = service.redirect(original) {
    println!("重定向到: {}", redirected);
}

// 从字符串加载配置
let config = r#"
enable: true
units:
  - rules:
      - pattern: "https://example.com/*"
        target: "https://mirror.example.com/"
"#;
let service = RedirectService::from_str(config)?;
```

### 环境变量使用

```yaml
# 使用环境变量的高级配置
enable: true
units:
  - name: "enterprise-proxy"
    rules:
      - pattern: "https://${INTERNAL_DOMAIN}/*"
        target: "https://${PROXY_HOST}/${INTERNAL_PATH}/"
    auth:
      username: "${PROXY_USER:-admin}"
      password: "${PROXY_PASS:-default123}"
```

## 📖 文档

- [重定向规则配置文档](docs/redirect-rules.md) - 完整的配置指南和示例
- [API文档](https://docs.rs/orion-accessor) - 详细的API参考

## 🧪 测试

```bash
# 运行所有测试
cargo test

# 运行特定模块测试
cargo test addr::redirect
```


## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件
