# XJTU Principal Mailbox Skill

这个仓库保存了一个 Codex skill，用于协助起草、确认并通过西安交通大学校领导信箱提交建议信、咨询、反馈或求助类来信。

## 能做什么

- 先确认姓名、邮箱、手机号、联系地址等基础信息。
- 根据用户提供的事实，整理正式、克制、可执行的中文来信。
- 在用户确认主题和正文后，默认使用 Chrome 打开校领导信箱页面并填表。
- 填写后自动核对页面内容，包括正文 1500 字限制。
- 在图片验证码、短信验证码和最终提交前停下来，等待用户人工确认。

## 不会做什么

- 不保存图片验证码或短信验证码。
- 不自动识别或破解验证码。
- 不绕过网页正常流程。
- 不把真实个人联系方式提交到这个仓库。

## 文件结构

```text
.agents/skills/xjtu-principal-mailbox/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── user-profile.example.md
```

真实使用时，本地还会有一个 `references/user-profile.md`，用于保存稳定的基础联系信息。这个文件包含个人信息，已被 `.gitignore` 排除，不应该提交到 GitHub。

## 使用方式

1. 把 `.agents/skills/xjtu-principal-mailbox` 放到可被 Codex 读取的 skills 目录中。
2. 复制示例资料文件：

```bash
cp .agents/skills/xjtu-principal-mailbox/references/user-profile.example.md \
  .agents/skills/xjtu-principal-mailbox/references/user-profile.md
```

3. 在 Codex 中提出类似请求：

```text
使用校长信箱 skill 发送一封建议信
```

4. 按提示确认基础信息、来信内容、验证码和最终提交。

## 安全说明

这个 skill 的设计目标是让流程更稳、更少重复输入，但正式提交前仍需要用户确认。涉及验证码、短信码和向学校提交信息的步骤，都会暂停等待用户人工提供或确认。
