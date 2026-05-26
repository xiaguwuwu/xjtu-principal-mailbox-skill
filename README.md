

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
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
├── assets/
│   └── workflow-infographic.png
└── references/
    └── user-profile.example.md
```

真实使用时，本地还会有一个 `references/user-profile.md`，用于保存稳定的基础联系信息。这个文件包含个人信息，已被 `.gitignore` 排除，不应该提交到 GitHub。

## 使用方法

1. 把 `.agents/skills/xjtu-principal-mailbox` 放到可被 Codex 读取的 skills 目录中。

2. 复制基础信息模板，创建本地 profile 文件：

```bash
cp .agents/skills/xjtu-principal-mailbox/references/user-profile.example.md \
  .agents/skills/xjtu-principal-mailbox/references/user-profile.md
```

3. 在 Codex 中提出请求，例如：

```text
使用校长信箱 skill 发送一封建议信
```

4. 按提示确认基础信息。第一次使用时需要提供姓名、邮箱、手机号和联系地址；之后再次使用时，skill 会先让你确认是否沿用或更换这些信息。

5. 提供来信内容。建议说明事项类型、涉及对象、具体事实、影响范围、已沟通情况、希望学校处理的结果，以及正文中是否需要隐去部分个人信息。

6. 审阅草稿。skill 会先生成来信主题和正文，必须等你明确确认后，才会进入网页填写流程。

7. 进入网页提交。skill 默认使用 Chrome 打开或接管校领导信箱页面，填写表单后会核对姓名、邮箱、联系地址、主题、正文长度和手机号。

8. 完成验证码步骤。图片验证码和短信验证码都需要你人工提供；skill 只会临时填入页面，不会保存。

9. 最终提交前再次确认。正式点击提交前，skill 会再次询问你是否确认提交。

## 安全说明

这个 skill 的设计目标是让流程更稳、更少重复输入，但正式提交前仍需要用户确认。涉及验证码、短信码和向学校提交信息的步骤，都会暂停等待用户人工提供或确认。
