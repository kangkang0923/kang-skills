# kang-skills

自研 Agent skills 的产地与成品。三个 skill 共用一套项目文档约定，但职责不同：

- `kang-grill`：把目标、约束、取舍和隐含假设问清楚。
- `kang-decision`：在需求明确后比较可行方案，给出唯一推荐或明确的证据缺口。
- `kang-docs`：建立并维护 8 文件文档体系，负责初始化、日常整改和存量迁移。

它们不是必须顺序执行的固定管道。新项目可以先用 `kang-docs` 建立文档骨架，再追问和选型；已有项目也可以只使用其中一个。

> 开发工作区的背景与历史取舍见 `../docs/decisions-doc-standard.md`。安装后的 skill 不依赖该文件。

## 1. 快速选择

| 用户意图 | 使用 | 典型说法 |
|---|---|---|
| 想法模糊，想发现遗漏或挑战前提 | `kang-grill` | 「帮我想清楚」「拷打我」「有什么没想到的」 |
| 技术栈、架构或服务需要拍板 | `kang-decision` | 「用什么技术做」「A 还是 B」「怎么部署」 |
| 新建、接手、整理或迁移项目文档 | `kang-docs` | 「新建项目」「整理文档」「接手项目」「规范整改」 |

不要这样用：

- 用 `kang-grill` 代替技术调查或直接改实现。
- 用 `kang-decision` 处理按钮位置、命名等纯偏好问题。
- 在需求未明时让 `kang-decision` 强行推荐。
- 让 `kang-docs` 发明需求、设计或业务规则。

## 2. 协作流程

```text
kang-docs（可选：先建 8 文件骨架）
        ↓
kang-grill：事实由 AI 查，决策由用户定
        ↓ 经确认写 REQUIREMENTS / DECISIONS / PLAN
kang-decision：核实事实 → 比较方案 → 唯一推荐或暂不推荐
        ↓ 用户确认后写 DECISIONS / DESIGN / PLAN
kang-docs：维护文档权威关系、生命周期和存量迁移
```

关键约定：

1. **事实与决策分开**：目录、代码、依赖、环境和官方资料由 AI 查；目标、偏好、成本和取舍由用户决定。
2. **每轮最多 5 问**：同轮问题不得有先后依赖；父节点未定时，不提前问子节点。
3. **需求经确认才落盘**：AI 可以代写，但不能替用户决定或发明需求。
4. **选型允许 blocked**：缺少决定性证据时明确「暂不推荐」，不凑方案、不假装确定。
5. **实现动作另行确认**：追问结束不等于获得修改代码、删除文件或提交 git 的授权。

## 3. 文档读写权限

三个 skill 都可以读取全部项目文档、代码和环境事实，但读取不等于可以把历史内容当成新需求。写入遵循最小权限：

| Skill | 可以写入 | 不能写入 |
|---|---|---|
| `kang-grill` | `REQUIREMENTS.md`（需求经明确确认）、`DECISIONS.md`、`PLAN.md` | `README.md`、`AGENTS.md`、`DESIGN.md`、`MEMORY.md`、`CHANGELOG.md`、实现文件 |
| `kang-decision` | 选型确认后写 `DECISIONS.md`、`DESIGN.md`、`PLAN.md`；证据不足时只在 `PLAN.md` 记录待验证项 | `REQUIREMENTS.md`、`README.md`、`AGENTS.md`、`MEMORY.md`、`CHANGELOG.md`、实现文件 |
| `kang-docs` | 全部 8 个文档；其中 `REQUIREMENTS.md` 必须经用户明确确认 | 实现文件；未经确认不得改需求或删除旧文档 |

补充边界：

- `kang-decision` 可以读历史 `DECISIONS.md` 来避免重提已否方案，但不能从中猜需求。
- `kang-docs` 负责文档体系，不代表可以任意改写内容；存量迁移只搬运，不借整理改变事实。
- 所有 skill 都不得自动 commit、stash、批量删除或覆盖用户改动；这些动作需要单独授权。
- 无法写文件时必须输出可复制内容，不能声称已经落盘。

## 4. 8 文件文档体系

程序类、内容类和混合项目都强制建立完整 8 个文件。暂时没有内容的文件也要写「暂无内容的原因 + 最后确认日期」，并在 `AGENTS.md` 导航中标为待填。

| 文件 | 唯一职责 | 不装什么 |
|---|---|---|
| `README.md` | 项目是什么、怎么使用、文档入口 | 当前状态、设计细节、决策理由 |
| `AGENTS.md` | 命令、硬约束、长期有效的坑、文档导航 | 需求副本、任务清单、进度流水账 |
| `docs/REQUIREMENTS.md` | 目标、不做什么、约束、验收标准 | 实现方案、未确认猜测 |
| `docs/DESIGN.md` | 当前实现契约、结构、算法、不变量 | 当前进度、方案取舍理由 |
| `docs/PLAN.md` | 待办、下一步、近期完成 | 当前环境状态、完整历史 |
| `docs/MEMORY.md` | 当前阶段、交接提示、临时环境问题 | 下一步任务副本、长期硬约束 |
| `docs/DECISIONS.md` | 结论、理由、被否方案、取代关系 | 当前设计副本、操作流水账 |
| `CHANGELOG.md` | 根目录下的版本级或月度变化摘要 | 逐条任务和完整操作细节 |

核心原则：**同一事实只有一个权威位置，其他地方只放链接。**

### 生命周期

- `PLAN.md`：打勾保留，近期完成最多 20 条；总长不超过 150 行，旧内容按主题压缩归档。
- `MEMORY.md`：覆盖式更新，只反映现在，不追加成交接流水账。
- `DECISIONS.md`：只追加，不改旧条目；新决策用 `取代：D-xxx` 指向被替代条目。
- `CHANGELOG.md`：追加；程序类按版本，内容类按月份。

完整规范只看 `kang-docs/references/doc-standard.md`，不要在其他文件复制一份。

## 5. 目录结构

```text
skill-production/
  README.md
  kang-grill/
    SKILL.md
  kang-decision/
    SKILL.md
    references/
      questionnaire.md
  kang-docs/
    SKILL.md
    references/
      doc-standard.md
      migration.md
    assets/
      program-templates.md
      content-templates.md
```

- `SKILL.md`：触发描述与执行流程。
- `references/`：执行时按需加载的规范和清单。
- `assets/`：供输出复制使用的模板，不是需要通读的参考文档。

## 6. 安装与更新

把三个 `kang-*` 目录完整复制到 Agent 的用户级 skills 目录，**包括 `references/` 和 `assets/`**。

| 工具 | 用户级目标路径 |
|---|---|
| CodeBuddy | `%USERPROFILE%/.codebuddy/skills/` |
| 其他 Agent | 查对应工具文档；确保每个 skill 目录下直接存在 `SKILL.md` |

安装后检查：

1. 目录名与 frontmatter 中的 `name` 完全一致。
2. 新会话能看到三个 skill 的触发描述。
3. 旧 `architect`、`grill` 与本套同时启用时可能重复触发；选择一套启用，避免指令竞争。
4. 更新时同步替换整个对应 skill 目录，避免只更新 `SKILL.md` 而留下旧 references/assets。

不安装也可以让 Agent 直接读取相应 `SKILL.md` 及其引用文件照做，但不会获得自动触发能力。

## 7. 验收方式

修改 skill 后，至少做以下真实会话测试，而不只检查 Markdown 是否好看：

1. **新项目追问**：需求结论经确认后进入 `REQUIREMENTS.md`，决策和未决项各归其位。
2. **跨会话选型**：只凭需求文件和可验证事实继续，不重复询问已定需求，不从历史决策猜需求。
3. **证据不足**：无法核实决定性事实时输出「暂不推荐」，不编造版本、价格或方案。
4. **内容类项目**：仍建立 8 文件，但使用内容组织模板，不生造构建命令。
5. **存量迁移**：先盘点和确认；逐个验证待删原文可恢复；不误删忽略文件或未跟踪文件。
6. **单一事实来源**：当前状态、下一步、决策理由和长期坑没有在两处重复维护。
7. **新会话接手**：只读 `AGENTS.md`、`MEMORY.md`、`PLAN.md`，能定位现状、下一步和其他权威文档。

## 8. 维护约定

- 改文档规范：只改 `kang-docs/references/doc-standard.md`，同步两套模板和迁移流程。
- 改提问规则：同步 `kang-grill`、`kang-decision` 和 `questionnaire.md`，保持「最多 5 个互不依赖的问题」一致。
- 改读写权限：同步三个 `SKILL.md` 与本 README 的权限矩阵，避免一处放开、另一处仍禁止。
- Markdown 中的路径统一使用 `/`，不要写入本机绝对路径。
- 三个 skill 都是纯 Markdown，无脚本依赖；可移植性仍取决于目标 Agent 的 skill 加载规则。
- 重要设计变化应记录到开发工作区的决策文件；记录理由，不复制规范正文。
