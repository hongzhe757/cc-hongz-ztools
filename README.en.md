# cc-hongz-ztools

A Java utility library refactored from [Hutool](https://hutool.cn/), split into independent modules by capability with unified coordinates and version management.

## Features

- **Modular by capability**: pull in a single module as needed, or aggregate everything via `cc-hongz-ztools-all`
- **Centralized version management**: third-party versions are defined once in the root `pom.xml` under `<properties>`; modules reference `${xxx.version}` rather than hardcoding values
- **BOM support**: `cc-hongz-ztools-bom` manages all module versions and allows excluding modules you do not need
- **Dependency isolation**: third-party integration dependencies are declared `optional`, so they are never passed on to consumers
- **Code style**: enforced by Checkstyle (4-space indentation, javadoc on public methods, `final` method parameters, private constructors for utility classes)

## Modules

### Core Utilities

| Module | Description |
| --- | --- |
| `cc-hongz-ztools-core` | Core utilities |
| `cc-hongz-ztools-setting` | Configuration enhancement (Properties / Setting / YAML) |
| `cc-hongz-ztools-system` | System-level utilities (system properties, environment info, OS detection) |
| `cc-hongz-ztools-log` | Logging facade |
| `cc-hongz-ztools-extra` | Extra utilities |

### Data Structures and Algorithms

| Module | Description |
| --- | --- |
| `cc-hongz-ztools-cache` | Caches (FIFO / LFU / LRU / Timed / Weak) |
| `cc-hongz-ztools-bloomFilter` | Bloom filter (bitmap with multiple hash functions) |
| `cc-hongz-ztools-dfa` | DFA-based keyword lookup |

### Cryptography and Security

| Module | Description |
| --- | --- |
| `cc-hongz-ztools-crypto` | Cryptography (symmetric / asymmetric / digest, including X.509 certificate generation) |
| `cc-hongz-ztools-jwt` | JWT generation, parsing and verification |
| `cc-hongz-ztools-captcha` | CAPTCHA (line / circle / shear / GIF styles plus content generators) |
| `cc-hongz-ztools-protector` | Code obfuscation and encryption protection toolkit |
| `cc-hongz-ztools-protector-demo` | End-to-end demo of code obfuscation protection (uses the aop module) |

### Data and Networking

| Module | Description |
| --- | --- |
| `cc-hongz-ztools-db` | Database (JDBC ORM wrapper, multiple connection pools, multiple dialects) |
| `cc-hongz-ztools-json` | JSON serialization |
| `cc-hongz-ztools-http` | HTTP client wrapper |
| `cc-hongz-ztools-socket` | Socket wrapper (BIO / NIO / AIO) |

### Document Processing

| Module | Description |
| --- | --- |
| `cc-hongz-ztools-poi` | POI utilities (Office documents / OFD) |
| `cc-hongz-ztools-pdf` | PDF utilities (HTML / Word / Office / Excel to PDF) |

### Scheduling and Enhancement

| Module | Description |
| --- | --- |
| `cc-hongz-ztools-cron` | Scheduling (cron expression parsing, scheduling, timing wheel) |
| `cc-hongz-ztools-script` | Script execution wrapper |
| `cc-hongz-ztools-aop` | AOP dynamic proxies (JDK / Cglib / SpringCglib) |

### Business Support

| Module | Description |
| --- | --- |
| `cc-hongz-ztools-status` | Unified status code abstraction (business status code + HTTP status code), pluggable |
| `cc-hongz-ztools-i18n` | i18n message resolution (business status code to localized message) |
| `cc-hongz-ztools-response` | Response envelope catalog and normalization (multi-source API/HTTP payloads) |
| `cc-hongz-ztools-ai` | AI module (LLM invocation wrapper: OpenAI / DeepSeek / Gemini / Grok / Ollama / Doubao) |

### Aggregation

| Module | Description |
| --- | --- |
| `cc-hongz-ztools-all` | Aggregates every submodule |
| `cc-hongz-ztools-bom` | BOM (manages all submodule versions; modules can be excluded) |

## Usage

Add a single module:

```xml
<dependency>
    <groupId>cc.hongz</groupId>
    <artifactId>cc-hongz-ztools-json</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

Or manage versions with the BOM (recommended):

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

Or aggregate everything:

```xml
<dependency>
    <groupId>cc.hongz</groupId>
    <artifactId>cc-hongz-ztools-all</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

> Note: third-party integration dependencies (POI, iText, BouncyCastle, etc.) are declared `optional` and are not transitively resolved. Add them explicitly in your own pom when using the corresponding capabilities.

## Build

```bash
mvn clean install
```

Skip tests:

```bash
mvn clean install -DskipTests
```

## Conventions

- **Versions**: define third-party versions in the root `pom.xml` under `<properties>`, and reference them as properties from modules
- **Dependencies**: third-party dependencies use `<scope>compile</scope>` plus `<optional>true</optional>`; test dependencies (JUnit 5, Lombok) only declare `<scope>test</scope>` with versions managed by the root `dependencyManagement`
- **Code style**: utility classes are `final` with a private constructor; public methods require javadoc; method parameters are `final`
- **Exceptions**: throw unchecked exceptions only (`IORuntimeException`, `UtilException`, or module-specific ones such as `CryptoException`)
- **Commit order**: every submodule is an independent git repository, so changes must be committed bottom-up — submodule first, then this repository, then the parent repository

## See Also

- The relationship with Hutool and the porting/refactoring status of each module are documented in the parent repository's `cc-hongz-parent/README.md`
