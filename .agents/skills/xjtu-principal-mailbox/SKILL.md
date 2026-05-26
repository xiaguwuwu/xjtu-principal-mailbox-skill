---
name: xjtu-principal-mailbox
description: Use this skill when the user wants to draft, revise, confirm, and submit a message through Xi'an Jiaotong University's 校领导信箱 / 校长信箱 at https://xldxx.xjtu.edu.cn, including reusable sender profile collection, formal Chinese message composition, confirmation loops, captcha/SMS-code coordination, and final submission through Chrome automation.
---

# XJTU Principal Mailbox

Automate the Xi'an Jiaotong University 校领导信箱 workflow end to end: collect reusable sender information once, draft a respectful formal message, revise until the user approves, then submit through the website with user help for captchas and SMS codes.

## Current Form Fields

The live page should be checked before submission because websites change. As of 2026-05-26, the initial page shows these fields:

- 您的姓名
- 您的Email
- 联系地址
- 来信主题
- 来信正文
- 您的手机号码, with 获取验证码
- 手机验证码

After clicking 获取验证码, the page may open a visible image-captcha dialog with a `验证码` input and `确定` / `取消` controls.

Do not invent additional fields. If the page changes, adapt to the visible fields and tell the user what changed.

## Start Every Run

1. Read `references/user-profile.md` in this skill folder.
2. Before asking for message content, always tell the user which basic information is required:
   - 姓名
   - Email
   - 手机号码
   - 联系地址, optional on the page but useful for follow-up
3. If `status: complete` and the required values are present, briefly show the saved basic information in a privacy-conscious way and ask whether the user wants to keep it or update any item. Do not proceed to drafting questions until the user has confirmed whether to keep or change the basic information.
4. If the profile is missing or incomplete, tell the user at the start:
   - the form requires name, email, mobile number, and message content;
   - contact address is optional on the page but useful for follow-up;
   - the page has an image captcha and an SMS code;
   - the user must be ready to read/provide any captcha or SMS code when asked;
   - verification codes are one-time values and must never be saved in the skill.
5. Ask for any missing or changed basic information in one compact message:
   - 姓名
   - Email
   - 手机号码
   - 联系地址, or confirm that it should be left blank
6. After the user confirms keeping the saved basic information, proceed to drafting questions without rewriting the profile file. After the user provides missing or changed basic information, update `references/user-profile.md` immediately before asking for message content. Store only stable basic information. Never store captchas, SMS codes, complaint details, secrets, passwords, or private tokens.
7. Ask for the message content only after the basic information has been confirmed or updated.

If the user refuses local storage, continue for this run but keep `status: missing` and ask again next time.

## Drafting Questions

After basic information is ready, gather enough facts to write a useful 校领导信箱 message. Ask only for information that is missing; prefer one concise batch of questions.

Required drafting inputs:

- 事项类型: 建议, 咨询, 投诉, 求助, 反馈, or other.
- 涉及对象: department, office, campus, course, building, system, service, or person if relevant.
- 发生时间和地点: exact date/time/location when available.
- 具体事实: what happened, in chronological order, without exaggeration.
- 影响范围: who is affected and how serious/urgent it is.
- 已尝试的沟通: who has been contacted, when, and what response was received.
- 希望学校处理的结果: the concrete request, answer, coordination, correction, or follow-up the user wants.
- 是否需要隐去正文中的部分个人信息: the form itself is实名, but the body can avoid exposing unnecessary personal details.

Writing rules:

- Use formal, respectful, factual Chinese.
- Avoid threats, insults, unsupported accusations, or emotional overstatement.
- Keep the request concrete and actionable.
- If facts are uncertain, phrase them as "据我了解", "可能存在", or ask the user to confirm before drafting.
- Generate both `来信主题` and `来信正文`.

## Standard Message Format

Use this structure unless the user asks for a different format:

```text
尊敬的校领导：
您好！

我是[身份/与事项关系]。现就[事项概述]向学校反映，恳请关注并协调处理。

一、基本情况
[时间、地点、涉及部门/场景、事件背景。]

二、具体问题及影响
[分点说明事实、影响范围、紧急程度。]

三、已沟通或处理情况
[说明已联系的单位/人员、已有回复、目前仍未解决的问题。若没有，写明尚未获得有效处理渠道。]

四、希望协调解决的事项
1. [明确诉求一]
2. [明确诉求二]
3. [如需要，说明希望获得回复或后续沟通。]

感谢学校对师生意见的重视，期待得到回复与帮助。

来信人：[姓名]
联系方式：[手机/Email]
日期：[YYYY年M月D日]
```

Subject guidance:

- Prefer `关于...的反映/建议/求助/咨询`.
- Keep it specific and short.
- Do not include private details in the subject unless necessary.

## Confirmation Loop

Never submit on the first draft. Show the proposed subject and body, then ask the user to confirm.

If the user is not satisfied, ask them to choose the problem type:

- 内容不满意: ask what fact, tone, evidence, impact, or request should change. Then rewrite only the affected content unless broader rewrite is needed.
- 格式不满意: ask whether they want it more concise, more formal, more direct, more分条, or more like普通邮件. Then rewrite the format while preserving confirmed facts.

Repeat the confirmation loop until the user explicitly says the subject and body are approved for submission.

## Submission With Chrome

Submit only after explicit approval.

Use Chrome by default. Do not enumerate or choose other local browsers. If Chrome is unavailable or cannot be controlled, stop and tell the user what is blocking submission instead of switching tools silently.

Prefer Chrome automation and direct page-field interaction over visual screen reading. Do not use raw HTTP requests or hidden network calls.

Known stable field IDs from the live page checked on 2026-05-26:

- 您的姓名: `#inputman`
- 您的Email: `#usremail`
- 联系地址: `#lxraddress`
- 来信主题: `#emailtitle`
- 来信正文: `#emailcontent`
- 您的手机号码: `#mobile`
- 获取验证码: `#codeBtn`
- 图片验证码弹窗输入框: `#mobileImgCodeValue`
- 手机验证码: `#mobileCode`
- 提交: `#formSubmit`

The page also contains hidden captcha fields such as `#imgCode`. Do not fill hidden fields directly unless the visible page flow requires it.

Before filling:

1. Ensure the approved body is no more than 1500 Chinese characters. If it is longer, compress it while preserving the core facts, impact, communication history, and requests, then show the compressed version if meaning changed materially.
2. Keep private details out of the body when the user asked to hide them; the real-name fields may still contain the required profile data.

Submission steps:

1. Open or claim the Chrome tab at `https://xldxx.xjtu.edu.cn`. If a previous run already stopped on this form or on the image-captcha dialog, continue from the current page state instead of reopening or clearing the form.
2. Fill the saved or just-collected basic fields and the approved subject/body using the known field IDs when they exist.
3. After filling, read the visible field values back from the page and verify:
   - name, email, address if provided, subject, body length, and mobile number are present;
   - body length is within the `1500` limit;
   - no SMS code has been saved or logged.
4. Tell the user the form is filled and ask whether they are ready to receive the SMS code.
5. Click `#codeBtn` only after the user confirms the phone is available.
6. If the image captcha popup appears, pause and ask the user to read the image captcha. Do not use OCR, image recognition, guessing, or automated solving for the captcha. Fill the user-provided captcha into `#mobileImgCodeValue` and click the visible `确定` control.
7. Ask the user to send the SMS code. Use it only for this submission and never save it.
8. Fill `#mobileCode`.
9. Immediately before clicking `#formSubmit`, ask the user to confirm final submission because it sends the message to the university.
10. Submit the form only after that final confirmation.
11. If the page reports validation errors, fix only confirmed data. If the error requires a new captcha or SMS code, ask the user again.
12. Stop after a successful submission or a real blocker such as the site being unavailable, SMS failing repeatedly, or a field the user cannot provide.

When reporting back, keep it simple: say whether the message was submitted, or exactly what blocked submission. Do not repeat captchas, SMS codes, or sensitive profile values.
