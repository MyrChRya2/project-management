---
name: commit
description: Git 提交规范。小步提交、提交信息格式、避免误提交无关文件、推送前检查。触发词："提交代码"/"commit"/"推送"。
allowed-tools: Bash(*), Read, Glob, Grep
---

# /commit — Git 提交规范

目标：**历史可读、可回退、可 blame；每次提交是一个原子变更**。

## 提交信息格式

```
<type>: <subject>

type:
  feat    新功能
  fix     修复
  refactor 重构（行为不变）
  docs    文档
  chore   杂项（gitignore、配置、脚本）
  WIP     进行中（慎用，避免长期滞留）
```

示例：`feat: add ledge grab state with area2d sensors` / `docs: log raycast v1 failure & plan-A preference`

## 流程

```
[用户：提交代码 / 推送]
   ↓
[Phase 1: 审查] git status + git diff --stat，确认改动范围
   ↓
[Phase 2: 分拣] 只 add 本次意图相关的文件；无关改动（如另一工作流的文件）**不混入**
   ↓
[Phase 3: 提交] 按格式写信息；提交后 git log --oneline -3 确认
   ↓
[Phase 4: 推送] 先做**网络预检**（见下「推送前网络预检」）；push 前 git status --short 确认工作区干净（或仅剩有意保留的未跟踪项）
```

## 推送前网络预检（WSL / 代理环境）

推送失败先分清是**认证**还是**网络**，别盲目重试。四步：

1. **探针**：`ssh -T git@github.com` → 期望 `Hi <账号>! You've successfully authenticated…`
   - `Permission denied (publickey)` ＝ **认证**问题（密钥缺失 / 权限过开 / 未加载），**不是**网络问题；
   - 卡住 / `Connection timed out` ＝ **网络**问题。
2. **本机固定姿势（2026-10-11 起，WSL 侧）**：`~/.ssh/config` 里 github.com 走 `~/.ssh/proxy-fallback.sh` —— **直连优先，直连不通自动经宿主机 FlClash 的 socks5 跳板**（先试默认网关＝NAT 模式，再试 `127.0.0.1`＝mirrored 模式）。
   - ⚠️ 直连判据是**读到 SSH banner**，不是 TCP 连通：本机环境下 `nc -z` / bash `/dev/tcp` 对**任意远端 IP 都报成功**（假通），只有 banner 能区分真假。
   - 代理可用的前提：FlClash 打开 **Allow LAN**（GUI 开关），或 WSL 用 mirrored 模式（`C:\Users\<用户>\.wslconfig` 的 `[wsl2]` + `networkingMode=mirrored`；**改后必须 `wsl --shutdown` 才生效、会打断所有会话**——配置可先备好，重启由用户挑时间）。
   - 直连可用时**不要长期挂代理**（代理只做回退）。
3. **新环境先自检两件事**：`git config --global user.name` / `user.email` 缺失 → commit 报 `Author identity unknown`（临时可用 `git -c user.name=… -c user.email=…`）；`safe.directory` 缺失 → /mnt 下报 `dubious ownership`。
4. **引用新鲜度**：`git status -sb` 的 ahead/behind 基于**上次 fetch**、可能过期 → **先 `git fetch origin`**，再下"领先/落后 N 笔"的结论。
5. **失败处置**：本地提交保留、**不强推**；把预检输出（`ssh -T` 结果 + fetch 后真实差距）报给用户，由他决定推送时机。

## 规则

- **小步**：一个原子变更一个提交；大改动拆多个提交。
- **不加 -A 盲提交**：`git add -A` 前先看 status——防止误提交无关/敏感文件（教训：曾误提交另一个工作流的整个目录）。
- 多会话共享同一工作区时：**只提交自己负责的文件**，其他会话的未提交工作保持原样。
- **用户/他人正在同一工作区开着编辑器时**，提交前必须 `git status` 看清范围、只 add 自己改过的文件；**已推送的历史不强改**（禁 force push），当场说明 + 提议后续修正比偷偷回滚诚实（`协作与交付.md` §2，Jam L31）。
- **提交信息不等于事实**：审查关键改动（工程配置、单行字段）用 `git show <commit> -- <path>` 核对真实 diff，别信 commit message（`协作与交付.md` §8，Jam L36）。
- **审查他人提交要审计新增的二进制素材来源**：`git diff --stat` 里 `Bin` 只有一行、极易划过去，看到新增 `*.png / *.wav / *.ogg / *.ttf` 就当场问来源（`事实核实.md` §3，Jam L25）。
- **`git diff --name-status` 的 `R`（改名）条目必须核对两端路径**：语义不搭（场景文件跑到素材目录）＝先怀疑误操作（`协作与交付.md` §9，Jam L67）。
- 提交前顺手确认：**文档勾选/状态是否与本次代码实际一致**（"标完成但未接线"是最常见的文档失真）。
- force push 需用户明确确认（单人仓库可用 `--force-with-lease`，仅用于修正刚推送的错误提交）。
- 推送失败（SSH 凭据 / 网络）时：本地提交保留、**不要强推**；按「推送前网络预检」定位原因，把结论与真实差距报给用户，由他决定推送时机。
