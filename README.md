# project-management — AI 项目管理代理 Skill

| [中文](README.md) | [English](README.en.md) |
| :---: | :---: |

> 把一个"项目管理会话"蒸馏成可复用方法论：会话开场审查 → 决策先落文档 → 每日收尾自动写日志 → Bug 登记跟踪 → 路径变更同步索引 → Git 提交规范。面向**单人独立开发者**（游戏/应用/内容皆可），方法论通用，通过项目根 `PROJECT_INDEX.md` 绑定具体项目。

## 许可证

**CC0 1.0 Universal**（公共领域奉献）——可自由使用、修改、再分发、商用，无需署名。详见 [LICENSE](LICENSE)。

## 安装

```bash
# 方式一：复制到用户级 skill 目录（本机已装）
cp -r project-management ~/.dsh/skills/

# 方式二：git clone 到 skill 目录
cd ~/.dsh/skills && git clone <repo-url> project-management
```

安装后在新会话中应看到 `project-management` 出现在可用 skill 列表（如未出现，重启会话）。

## 绑定项目（首次使用）

1. 会话中说"审查进度"，并提供**项目根目录**（Git 仓库根）。
2. skill 会读取（或按模板生成）`PROJECT_INDEX.md`——它是项目的唯一配置入口。
3. 之后每次会话先读它，路径与约定全部从索引读取。

## 触发词速查

| 说 | 做什么 |
|---|---|
| "审查进度" / "开始" | 会话开场：git 审查 + 文档一致性核对 + 今日建议 |
| "今天任务已结束" / "更新日志" | 读模板 → 生成当日 devlog → 同步 ROADMAP → 提交 |
| "记录决策" + 内容 | 决策写 devlog + ROADMAP/GDD 同步 |
| "发现bug" / "修复了bug" | bug 登记跟踪表 + devlog + 修复后状态更新 |
| "同步索引" | 路径/结构变更后更新 PROJECT_INDEX.md |
| "提交代码" | 分拣 → 规范提交 → 推送 |

## 目录结构

```
project-management/
├── SKILL.md                        # 总协议 + 路由表 + 三条原则
├── skills/
│   ├── session-start/SKILL.md      # 会话开场审查
│   ├── daily-wrap/SKILL.md         # 每日收尾日志
│   ├── decision-log/SKILL.md       # 决策记录与同步
│   ├── bug-track/SKILL.md          # Bug 登记与跟踪
│   ├── index-sync/SKILL.md         # 索引同步
│   └── commit/SKILL.md             # Git 提交规范
├── templates/
│   ├── devlog.template.md          # 开发日志模板
│   ├── project-index.template.md   # 项目索引模板（= 项目配置）
│   └── decision.template.md        # 重大决策模板
├── shared-references/
│   ├── doc-roles.md                # 文档职责划分（+ 6 条铁律）
│   ├── git-workflow.md             # Git 工作流约定
│   ├── 协作与交付.md                # 多会话协作 / 提交历史 / 对外交付验收（16 条判据）
│   ├── 事实核实.md                  # 下结论之前的核实清单（11 条判据）
│   └── 需求与范围.md                # 需求澄清与范围取舍（7 条判据）
├── LICENSE                         # CC0 1.0
└── README.md                       # 本文件
```

## 适配新项目（3 步）

1. 复制 `templates/project-index.template.md` 到项目根为 `PROJECT_INDEX.md`，替换 `{{PLACEHOLDER}}`。
2. 复制 `templates/devlog.template.md` 到 `docs/devlog/TEMPLATE.md`。
3. 用 `{{DOCS_DIR}}`/`{{ENGINE_DIR}}` 约定的默认值（`docs/`、引擎目录名）或按项目实际在索引中声明。

## 设计来源

本 skill 是从 **EndlessRegret**（Godot 2D 平台跳跃游戏）项目的真实项目管理会话中蒸馏而来，在真实开发中迭代成型。核心思想：**文档是记忆，代码是结果；小步提交防丢失；一次只聚焦一个里程碑。**

`shared-references/` 的判据库（协作与交付 / 事实核实 / 需求与范围，共 34 条）来自 **Jam（FirstProject）** 项目的 88 条实战教训——那是为"隔离 + 积攒经验反哺 project-manager"而专设的练手项目，结束后按主题筛选、逐条反哺（每条标注 `Jam L<编号>` 可回溯原始出处）。另有 26 条引擎/技术知识与记忆系统、环境专属经验按来源分流，不进本 skill。
