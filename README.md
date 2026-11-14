# cc-hongz-ztools

基于 [Hutool](https://hutool.cn/) 重构的 Java 工具集，按能力拆分为独立模块，统一坐标与版本管理。

## 特性

- **按能力分模块**：可按需引入单个模块，也可通过 `cc-hongz-ztools-all` 全量聚合
- **版本集中管理**：三方库版本统一在根 `pom.xml` 的 `<properties>` 中定义，子模块引用 `${xxx.version}`，不散落硬编码
- **BOM 托管**：`cc-hongz-ztools-bom` 统一管理各子模块版本，并支持通过 `exclude` 排除不需要的模块
- **依赖隔离**：三方集成依赖在模块 pom 中声明为 `optional`，不会传递到使用方
- **代码规范**：Checkstyle 校验（4 空格缩进、public 方法 javadoc、方法参数 `final`、工具类私有构造器等）

## 模块清单

### 基础工具

| 模块 | 说明 |
| --- | --- |
| `cc-hongz-ztools-core` | 核心工具模块 |
| `cc-hongz-ztools-setting` | 配置文件增强模块（Properties/Setting/YAML） |
| `cc-hongz-ztools-system` | 系统级工具模块（系统属性、环境信息、OS 检测等） |
| `cc-hongz-ztools-log` | 日志封装模块 |
| `cc-hongz-ztools-extra` | 扩展工具类模块 |

### 数据结构与算法

| 模块 | 说明 |
| --- | --- |
| `cc-hongz-ztools-cache` | 缓存模块（FIFO/LFU/LRU/Timed/Weak 缓存） |
| `cc-hongz-ztools-bloomFilter` | 布隆过滤器模块（位图 + 多哈希函数） |
| `cc-hongz-ztools-dfa` | 基于 DFA 的关键词查找模块 |

### 加密与安全

| 模块 | 说明 |
| --- | --- |
| `cc-hongz-ztools-crypto` | 加密工具模块（对称/非对称/摘要，含 X.509 证书生成） |
| `cc-hongz-ztools-jwt` | JWT 生成、解析和验证模块 |
| `cc-hongz-ztools-captcha` | 验证码模块（线段/圆圈/扭曲/GIF 验证码 + 内容生成器） |
| `cc-hongz-ztools-protector` | 代码混淆与加密保护工具包 |
| `cc-hongz-ztools-protector-demo` | 代码混淆加密保护端到端演示（引用 aop 动态代理） |

### 数据与网络

| 模块 | 说明 |
| --- | --- |
| `cc-hongz-ztools-db` | 数据库模块（JDBC ORM 封装 + 多连接池 + 多数据库方言） |
| `cc-hongz-ztools-json` | JSON 序列化模块 |
| `cc-hongz-ztools-http` | HTTP 客户端封装 |
| `cc-hongz-ztools-socket` | Socket 套接字封装模块（BIO/NIO/AIO） |

### 文档处理

| 模块 | 说明 |
| --- | --- |
| `cc-hongz-ztools-poi` | POI 工具类模块（Office 文档 / OFD 操作） |
| `cc-hongz-ztools-pdf` | PDF 工具类模块（HTML / Word / Office / Excel 转 PDF） |

### 任务与增强

| 模块 | 说明 |
| --- | --- |
| `cc-hongz-ztools-cron` | 定时任务模块（Cron 表达式解析 + 调度 + 时间轮） |
| `cc-hongz-ztools-script` | 脚本执行封装 |
| `cc-hongz-ztools-aop` | AOP 动态代理模块（JDK/Cglib/SpringCglib 代理） |

### 业务支撑

| 模块 | 说明 |
| --- | --- |
| `cc-hongz-ztools-status` | 统一状态码抽象（业务状态码 + HTTP 状态码），可插拔 |
| `cc-hongz-ztools-i18n` | 国际化消息解析（业务状态码 → 本地化消息） |
| `cc-hongz-ztools-response` | 响应封套目录与归一化（多来源 API/HTTP 载荷） |
| `cc-hongz-ztools-ai` | AI 模块（LLM 调用封装：OpenAI/DeepSeek/Gemini/Grok/Ollama/豆包） |

### 聚合

| 模块 | 说明 |
| --- | --- |
| `cc-hongz-ztools-all` | 全量聚合模块（聚合全部子模块工具类） |
| `cc-hongz-ztools-bom` | BOM（统一管理各子模块版本，可通过 exclude 排除不需要的模块） |

## 使用

引入单个模块：

```xml
<dependency>
    <groupId>cc.hongz</groupId>
    <artifactId>cc-hongz-ztools-json</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

或用 BOM 统一托管版本（推荐）：

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>cc.hongz</groupId>
            <artifactId>cc-hongz-ztools-bom</artifactId>
            <version>1.0.0-SNAPSHOT</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

全量引入：

```xml
<dependency>
    <groupId>cc.hongz</groupId>
    <artifactId>cc-hongz-ztools-all</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

> 注意：各模块中的三方集成依赖（如 POI、iText、BouncyCastle）声明为 `optional`，不会自动传递。使用相关能力时需在使用方 pom 中显式引入这些依赖。

## 仓库镜像

- Gitee：`https://gitee.com/deligense/cc-hongz-ztools`（`origin`）
- GitHub：`https://github.com/hongzhe757/cc-hongz-ztools`（`github`）
- 各子模块均为**独立仓库**，已同步到 GitHub 同名仓；`.gitmodules` 使用相对路径 `../<repo>.git`，从 Gitee 或 GitHub 克隆聚合仓均可解析到同平台兄弟仓。

```bash
git clone --recurse-submodules https://github.com/hongzhe757/cc-hongz-ztools.git
# 或
git clone --recurse-submodules https://gitee.com/deligense/cc-hongz-ztools.git
```

## 构建

```bash
mvn clean install
```

跳过测试：

```bash
mvn clean install -DskipTests
```

## 工程约定

- **版本管理**：新增三方依赖时，版本定义在根 `pom.xml` 的 `<properties>`，模块中引用属性
- **依赖声明**：三方依赖标 `<scope>compile</scope>` + `<optional>true</optional>`；测试依赖（JUnit5、Lombok）只写 `<scope>test</scope>`，版本由根 pom 的 `dependencyManagement` 托管
- **代码规范**：utils 类为 `final` + 私有构造器，public 方法必须写 javadoc（中文、句末带句号），方法参数 `final`
- **异常**：统一抛非受检异常（`IORuntimeException`、`UtilException`、模块专属异常如 `CryptoException`）
- **提交顺序**：本仓库各子模块均为独立 git 仓库，改动须自底向上提交——先子模块、再本仓库、最后父仓库

## 相关

- 与 Hutool 的关系与差异，以及各模块的移植/重构进度，见父仓库 `cc-hongz-parent/README.md`
