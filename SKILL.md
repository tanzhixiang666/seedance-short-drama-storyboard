---
name: seedance-short-drama-storyboard
description: 世界顶级导演水准的 Seedance（即梦）短剧分镜视频提示词生成技能。两步流程：先把剧本改编为 camera-content JSON 分镜剧本，再按专业电影运镜规范生成 video_prompt。内置 Seedance 2.0 镜头与运镜使用手册，覆盖主观POV/双人/多人/斜拍/推拉摇移跟拍等全套导演级运镜语法。当用户需要生成 Seedance 短剧分镜脚本、把小说原文改成 13-15 秒分镜组的 video_prompt、生成竖屏短剧分镜、为即梦/Seedance 2.0/2.5 写分镜提示词时使用。
---

# Seedance 短剧分镜提示词生成

## Overview

以**世界顶级导演**的专业水准，将输入的小说原文 / 剧本，经过**两步**转化为可直接喂给 Seedance（即梦）模型的 `video_prompt`：

1. **第一步：剧本改编** — 把原文切分为 15 秒分镜组，输出 camera-content JSON 格式剧本
2. **第二步：导演级分镜提示词生成** — 加载运镜手册，按电影摄影规范把每段 camera-content 改写为 video_prompt

## 核心工作流

### Step 0: 接收输入

确认用户提供了以下信息：
- **原文/剧本**（唯一信息来源，不可超范围创作）
- **前后分镜衔接信息**（可选）

---

### Step 1: 剧本改编 → camera-content JSON

**必须先读取** `references/camera-content-format.md`，按规范把原文改编为 camera-content JSON。

改编要点：
1. 把原文切分为每段 15 秒的分镜组，每组 2-5 个镜头
2. 每个镜头用 `▲时间范围:景别，运镜，画面描述` 格式
3. 对话保留原文不改，语气放在对话后括号里
4. 每段结尾加音频描述行
5. 输出单行 JSON，含 `subtitle` 数组，每个元素有 `id`、`camera-content`、`originalText`

---

### Step 2: 导演级分镜提示词生成 → video_prompt

**必须同时读取以下两份参考文件：**
- `references/prompt-template.md` — Seedance 分镜输出格式与基本规则
- `references/cinematography-manual.md` — **Seedance 2.0 镜头与运镜使用手册（必读，决定导演水准）**

生成时必须以世界顶级导演思维设计每个镜头：
- 每个运镜都要回答：**为什么动？从谁的感受出发？动到哪里停？停下来让观众看什么？**
- 每个镜头写全**七要素**：镜头类型 / 景别 / 机位角度 / 焦距倾向 / 运动方式 / 路径速度 / 叙事目的
- 按场景情绪选择对应运镜策略（压迫→低仰推近；疏离→广角拉开；凝视→长焦推近停住；失控→轻斜手持）
- 双人镜头必须写人物走位与身体朝向关系，禁止站桩对话
- 运动镜头必须写清起点→路径→终点→停住秒数，禁止无目的漂移
- 多镜头 15 秒遵循三段式：建置(3-5s) → 冲突推进(4-6s) → 信息落点(3-5s)

转化基础要点：
1. **内容忠实**：所有画面、动作、对白必须来自 camera-content，禁止虚构或加心理活动。心理描写一律外化为表情/动作/道具互动。
2. **分镜组结构**：每组 13–15 秒，3–5 个片段，单镜头 1–4 秒，最后一镜必须是大特写卡点。
3. **对白节制**：每组对白 ≤3 句，单句 ≤10 字；信息多用道具特写/动作承担。
4. **角色声线**：每个角色固定一个 VoiceID，配音指令中只改语气旋钮。
5. **连续性**：与前后分镜无缝衔接，人物外貌、服装、场景全部复述，禁止"同上/同前"。

### Step 3: 输出

严格按 `references/prompt-template.md` 中的输出格式模板输出 video_prompt。

## 关键硬规则速查

| 维度 | 规则 |
|------|------|
| 总流程 | 先改编 camera-content JSON → 再导演级转 video_prompt |
| 运镜水准 | 必须读取 cinematography-manual.md，按导演七要素写每个镜头 |
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
- `references/prompt-template.md` — Seedance 分镜提示词输出格式模板与基础规则（**Step 2 必读**）
- `references/cinematography-manual.md` — **Seedance 2.0 镜头与运镜使用手册，世界顶级导演运镜语法（Step 2 必读，决定镜头质量）**
