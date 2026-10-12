---
name: seedance-short-drama-storyboard
description: 把小说/剧本转化为 Seedance（即梦）短剧分镜视频提示词的专家技能。两步流程：先把剧本改编为 camera-content JSON 分镜剧本，再按 Seedance 分镜规范生成 video_prompt。当用户需要生成 Seedance 短剧分镜脚本、把小说原文改成 13-15 秒分镜组的 video_prompt、生成竖屏短剧分镜、为即梦/Seedance 2.0/2.5 写分镜提示词时使用。覆盖剧本改编、分镜组切分、镜头语法（景别/机位/运镜/焦段）、蒙太奇衔接、角色声线与配音指令、商业短剧节奏参数等完整规范。
---

# Seedance 短剧分镜提示词生成

## Overview

将输入的小说原文 / 剧本，经过**两步**转化为可直接喂给 Seedance（即梦）模型的 `video_prompt`：

1. **第一步：剧本改编** — 把原文切分为 15 秒分镜组，输出 camera-content JSON 格式剧本
2. **第二步：分镜提示词生成** — 把每段 camera-content 按 Seedance 规范改写为 video_prompt

## 核心工作流

### Step 0: 接收输入

确认用户提供了以下信息：
- **原文/剧本**（唯一信息来源，不可超范围创作）
- **前后分镜衔接信息**（可选）

如果用户只给了一段长文案，需要主动按故事节奏切分。

---

### Step 1: 剧本改编 → camera-content JSON

**必须先读取** `references/camera-content-format.md`，按规范把原文改编为 camera-content JSON。

改编要点：
1. 把原文切分为每段 15 秒的分镜组，每组 2-5 个镜头
2. 每个镜头用 `▲时间范围:景别，运镜，画面描述` 格式
3. 对话保留原文不改，语气放在对话后括号里
4. 每段结尾加音频描述行
5. 输出单行 JSON，含 `subtitle` 数组，每个元素有 `id`、`camera-content`、`originalText`

**此步输出完成后，再进入 Step 2。**

---

### Step 2: 分镜提示词生成 → video_prompt

**必须读取** `references/prompt-template.md`，把每段 camera-content 转化为 Seedance video_prompt。

转化要点：
1. **内容忠实**：所有画面、动作、对白必须来自 camera-content，禁止虚构或加心理活动。心理描写一律外化为表情/动作/道具互动。
2. **分镜组结构**：每组 13–15 秒，3–5 个片段，单镜头 1–4 秒，最后一镜必须是大特写卡点。
3. **对白节制**：每组对白 ≤3 句，单句 ≤10 字；信息多用道具特写/动作承担。
4. **运镜规范**：每个片段必须写清景别、机位高度、运动方式、速度节奏、跟焦对象、落点。
5. **角色声线**：每个角色固定一个 VoiceID（性别/年龄段/音域/音色/咬字/气息），配音指令中只改语气旋钮（音量/语速/气息/尾音/情绪）。
6. **连续性**：与前后分镜无缝衔接，人物外貌、服装、场景、时间天气全部复述，禁止"同上/同前"。

### Step 3: 输出

严格按 `references/prompt-template.md` 中的输出格式模板输出 video_prompt，包括：
- 开头音效/音乐/字幕声明行
- 场景 + 场景设计
- 目的标签（推进信息/制造冲突/情绪加压/反转钩子/交代行动）
- 片段数 + 片段说明
- 每个 `|切片段N-` 画面描述 + `运镜补充：`
- 对白块 + 配音指令行

## 关键硬规则速查

| 维度 | 规则 |
|------|------|
| 总流程 | 先改编 camera-content JSON → 再转 video_prompt |
| camera-content 时长 | 每段 15 秒，2-5 个镜头 |
| video_prompt 字数 | ≤900 字 |
| video_prompt 片段数 | 每组 3–5 个，默认 4 个 |
| 单片段动作 | ≤3 个动作 |
| 对白 | 每组 ≤3 句，单句 ≤10 字 |
| 结尾镜头 | 大特写（眼睛/道具关键字）卡点 |
| 心理描写 | 禁止"他想/她觉得"，全部外化 |
| 省略词 | 禁止"同上/同前"，重复元素全部写出 |
| 台词 | 不允许改写原词，超长则分到不同片段 |

## References

- `references/camera-content-format.md` — 剧本改编为 camera-content JSON 的格式规范与示例（**Step 1 必读**）
- `references/prompt-template.md` — Seedance 分镜提示词完整规则、输出格式模板与示例（**Step 2 必读**）
