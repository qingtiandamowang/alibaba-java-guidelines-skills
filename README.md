# 阿里巴巴Java开发手册 · Agent Skills

把《阿里巴巴Java开发手册（黄山版）》拆成 8 个可被 AI 编码助手自动触发的 skill，覆盖 324 条规约。

支持 Claude Code、Cursor、Codex 等读取 `SKILL.md` 的 AI 编码工具。

## 为什么重做一遍

市面上已有的手册 skill 化版本，大多是把 PDF 直接机械提取后按章切分。这种做法会留下一批系统性缺陷，我们逐一修掉了：

| 缺陷 | 表现 | 后果 |
|---|---|---|
| 跨章节串档 | 每个文件都包含"自己那章 + 后面所有章节" | 编程规约一个文件塞了整本手册；设计规约重复 7 遍 |
| 等级标记破损 | `【强 制】`、`【强制 】` | **【强制】是全书最关键的语义单元**，断裂后等于把强制规约降级成普通文字 |
| 页码粘连行首 | ` 33/51 15.【推荐】 单表行数超过…` | 条文编号断裂，扫描时整条被漏掉 |
| 列分隔符丢失 | `A0001 用户端错误一级宏观错误码` | 错误码表三列粘成一团，无法解析 |
| 附录混入正文 | 版本变更历史、错误码表塞在设计规约里 | 设计规约文件里 69% 不是设计规约 |

修复量：**削减 3993 行重复内容**，修复 **324 个等级标记**，剥离 **41 处粘连页码**，清理 **2902 处 PDF 空格伪影**。最终 1926 行全部是有效规约。

此外做了两处结构调整：错误码表（189 行）从设计规约中**拆为独立 skill**，因为它体量大、用途独立；专有名词解释**并入工程结构**，因为 DO/DTO/BO/VO、一方库/二方库/三方库这些术语正是该章规约在用的。

## 技能列表

跨语言标记说明：手册虽为 Java 而写，但其中数据库设计、安全、测试纪律等章节的约束**与实现语言无关**。我们据此重写了触发描述——该跨语言的放宽，该锁 Java 的锁死。

| Skill | 内容 | 适用语言 |
|---|---|---|
| `java-mysql-database` | 建表规约、索引规约、SQL 语句、ORM 映射 | 🌍 全语言 |
| `java-security-standards` | 越权校验、脱敏、SQL 注入、XSS/CSRF、防重放、上传校验 | 🌍 全语言 |
| `java-unit-testing` | AIR 原则、assert 而非打印、用例独立性、边界值 | 🌍 全语言 |
| `java-exception-logging` | 异常捕获与抛出、错误码设计、日志分级与脱敏 | 🌍 全语言 |
| `java-design-standards` | 需求分析、状态图、领域模型、可扩展性设计 | 🌍 全语言 |
| `error-code-catalog` | A/B/C 三段式错误码体系与完整错误码列表 | 🌍 全语言 |
| `java-coding-standards` | 命名、常量、格式、OOP、集合、并发、控制语句、注释 | 🔒 仅 Java |
| `java-project-structure` | 应用分层、领域模型、二方库依赖、服务器配置 | 🔒 仅 Java/Maven |

为什么这样分：`java-mysql-database` 里"表名小写、索引前缀 `pk_`/`uk_`/`idx_`、金额用 decimal 不用 float、禁止物理删除"这类约束，你用 Go 的 sqlx 还是 Rust 的 sea-orm 写，成立性完全一样——schema 一旦定错，改动代价极大，所以在敲第一条 DDL 时就该受管。反过来，编程规约里大量涉及 POJO、`equals`/`hashCode`、Java 集合 API，塞进 Rust 项目就是纯噪音，因此明确锁死。

## 安装

### Claude Code（推荐）

```
/plugin marketplace add qingtiandamowang/alibaba-java-guidelines-skills
/plugin install alibaba-java-guidelines@alibaba-java-guidelines-skills
```

### 手动安装（Cursor / Codex / 其他）

```bash
git clone https://github.com/qingtiandamowang/alibaba-java-guidelines-skills.git
cp -r alibaba-java-guidelines-skills/skills/* ~/.claude/skills/
```

只装其中某一个：

```bash
cp -r alibaba-java-guidelines-skills/skills/java-mysql-database ~/.claude/skills/
```

## 使用

装完重启会话即可，无需手动调用——描述里写明了触发条件，AI 会在你设计表结构、写 SQL、做鉴权、写单测、设计错误码时自动介入。

想强制调用某个技能，直接说技能名即可，例如"按 java-mysql-database 检查这个建表语句"。

## 规约等级

沿用手册原有的三级标记：

- **【强制】** 必须遵守，违反需有明确理由
- **【推荐】** 默认遵守
- **【参考】** 供权衡参考

## 来源与许可

规约正文全部来自 **《阿里巴巴Java开发手册（黄山版）》**，由阿里巴巴集团技术团队编写，版权归阿里巴巴所有。本仓库仅做格式转换、缺陷修复与结构重组，未修改任何规约内容。

手册官方项目：[alibaba/p3c](https://github.com/alibaba/p3c)

本仓库的转换与修复工作以 [Apache-2.0](./LICENSE) 授权，与 p3c 保持一致。
