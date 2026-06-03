# Playbook 使用手册

playbook 是操作规程的集合。每次会话启动时读取。

## 结构

```
playbook/
├── README.md          # 本文件
├── process/           # 通用底线规则，每次会话必读
│   ├── approach.md    #   收到任务先分类，按重量配深度
│   ├── information.md #   开口之前，确认每条信息可靠
│   └── role.md        #   知道什么能做、什么不能替
└── contracts/         # 合同审查（contract-review skill 使用）
    ├── README.md      #   合同工作流程：背景→类型→情境→审查→输出
    ├── review/        #   审查规则，按合同类型逐步积累
    └── templates/     #   公司合同范本，起草时取用
```

## 使用约定

- process/ 下三个文件每次会话全量加载，不挑任务。
- 其余路径由对应 skill 按需加载。加载前先读该路径下的 README.md。
- 每个 skill 可独立扩展自己的子目录，其他 skill 不感知。
- 用户档案在 `.claude/profiles/`，由冷启动生成。说 "update my profile" 更新。
- 工具列表和状态在 `.claude/profiles/` 中，不在此文件维护。
