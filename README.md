# Legal Workstation

## 这玩意儿解决什么问题

Claude Code 每次新对话是全新上下文——它不记得上一轮聊了什么，更不记得你是谁。你在每一次对话开头都要手动说明："我是公司法务，主要审工程合同，用中国大陆法律，输出偏实务，风险用🔴🟠🟡来标……"

**这个项目做的事：第一次花3-5分钟跟 Claude 聊完，它把你的身份、领域、法域、内部政策、输出偏好写到一个文件里。此后每次在这个目录打开 Claude Code，它自动读取这份档案——不用你再重复。**

不是让你"配一个 AI"，是让你**不用每次重复告诉 AI 你是谁**。

---

## 安装

```bash
cd ~/my-legal-work
git clone https://github.com/lukeruc/legal-workstation.git /tmp/legal-workstation
cp -r /tmp/legal-workstation/coldstart/. ./
rm -rf /tmp/legal-workstation
```

目录内容：

```
CLAUDE.md              # Agent 指令
scripts/               # 冷启动脚本（临时）
playbook/              # 操作规程
.claude/
├── profiles/           # 空，等待配置
└── settings.local.json
```

## 冷启动

在这个目录下打开 Claude Code。CLAUDE.md 检测到 profiles/ 为空，自动启动冷启动对话：

1. **种子文件提取（可选）。** 你可以给合同模板、制度文件范本。agent 自动提取公司业务、法域、风格。
2. **对话补充。** 种子文件看不出来的——公司政策红线、审批链、输出偏好——聊天补充。
3. **工具测试。** 你提到的每个工具，agent 实际调用验证是否连通。
4. **写入档案。** 对话内容写入 `.claude/profiles/`。
5. **创建目录。** `sessions/` `records/` `archive/` 就位。
6. **自清理。** `scripts/` 目录删除。

冷启动了解什么：公司业务、法域、交易角色、内部政策红线、审批链、行业标准。**只记能改变模型输出行为的信息。**

想改档案？说 "update my profile"，或直接编辑 `.claude/profiles/` 下的文件。

---

## 冷启动完成后

```
你的法律工作目录/
├── CLAUDE.md                    ← 启动逻辑 + 共享规则
├── .claude/
│   └── profiles/                ← 你的档案（纯文本，可手改）
├── playbook/                     ← 操作规程
│   ├── process/                  ←   通用操作标准（每次必读）
│   └── contracts/                ←   合同审查清单（按需加载）
├── sessions/                     ← 审查任务工作区
├── records/                      ← 过往工作记录
└── archive/                      ← 审查归档
```

---

## 功能模块

功能模块（合同审核、文书起草等）由用户手动安装。将模块仓库克隆到 `.claude/skills/` 下即可。

```bash
# 以 contract-review 为例
git clone https://github.com/lukeruc/contract-review.git .claude/skills/contract-review
```

模块只依赖路径契约——不需要修改本工作区的任何文件。

---

## 依赖

- Claude Code。别无其他。

## License

MIT
