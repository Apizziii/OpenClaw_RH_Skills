# Video Model Selection

**Whenever** the user wants ANY video (text-to-video OR image-to-video), you MUST show this menu and WAIT:

> 好的！先帮你选个最合适的视频模型～
>
> 1. 🚀 **全能视频V3.1 Fast** — 我最推荐的！又快效果又好，性价比之王
> 2. 🔥 **全能视频X** — Grok 驱动，画面想象力超强，创意天花板
> 3. 🎯 **可灵 v3.0 Pro** — 运动特别自然，拍人物选它准没错
> 4. 🎬 **全能视频V3.1 Pro** — 电影感拉满，适合风景大片
> 5. ✨ **Vidu Q3 Pro** — 风格化独特，适合创意类短片
> 6. ⭐ **全能视频S** — Sora 同款引擎效果好，但最近模型负载比较高，可能要多等一会儿
> 7. 🌊 **海螺 Hailuo** — 速度快画面细腻，适合创意类内容
> 8. 🌱 **超能视频SD2.0** — 效果超赞！最长15秒+自动配音，适合动画/风景，不适合真人，价格偏高
>
> 说个数字就行～ 不选的话我默认用 🚀全能视频V3.1 Fast 哦！

**Do NOT invent your own model list. Do NOT skip this menu. Use EXACTLY this 8-model list.**

After user replies, map choice → endpoint:

**Text-to-video** (no image):
| # | Endpoint |
|---|----------|
| 1 (default) | `rhart-video-v3.1-fast/text-to-video` |
| 2 | `rhart-video-g/text-to-video` |
| 3 | `kling-v3.0-pro/text-to-video` |
| 4 | `rhart-video-v3.1-pro/text-to-video` |
| 5 | `vidu/text-to-video-q3-pro` |
| 6 | `rhart-video-s/text-to-video` |
| 7 | `minimax/hailuo-02/t2v-pro` |
| 8 | `rhart-video/sparkvideo-2.0/text-to-video` |

**Image-to-video** (user has image):
| # | Endpoint |
|---|----------|
| 1 (default) | `rhart-video-v3.1-fast/image-to-video` |
| 2 | `rhart-video-g/image-to-video` |
| 3 | `kling-v3.0-pro/image-to-video` |
| 4 | `rhart-video-v3.1-pro/image-to-video` |
| 5 | `vidu/image-to-video-q3-pro` |
| 6 | `rhart-video-s/image-to-video` |
| 7 | `minimax/hailuo-2.3-fast/image-to-video` |
| 8 | `rhart-video/sparkvideo-2.0/image-to-video` |

## Matching Rules

- Number 1-8 → use that model
- Partial name ("可灵", "海螺", "全能", "万相", "Grok", "Seedance", "超能", "种子") → match
- "随便" / "你选" / "默认" → choice 1
- "最快的" / "便宜的" → choice 1
- "万相" → use `alibaba/wan-2.6/text-to-video` or `alibaba/wan-2.6/image-to-video-flash`
- "效果最好的" / "创意最好的" → choice 2 (全能X) or 3 (可灵)
- "最长的" / "15秒" / "长视频" / "自动配音" → recommend choice 8 (超能视频SD2.0)
- "多模态" / "图片+视频" → use multimodal endpoint: `rhart-video/sparkvideo-2.0/multimodal-video`
- Real people in image → recommend choice 3 (可灵). **NEVER recommend 8 (超能视频SD2.0) for real people.**

Skip menu ONLY if: user named a specific model, or said "跟上次一样" / "再来一个".

## After Model Is Chosen

**Before running the script**, ALWAYS send a progress notification via `message` tool:
> "好嘞，开始用 XX模型 生成视频啦！一般需要几分钟，请稍等～ 🎬"

This is critical — video generation takes 1-5 minutes and users need to know the task has started. Send the notification FIRST, then execute the script.

Confirm the choice warmly, then ask for missing info if needed:
> "好嘞，用可灵 v3.0 Pro！视频时长要多久？默认 5 秒，也可以选 10 秒～"

Smart defaults (use these if user doesn't specify):
- Duration: 5s for text-to-video, 5s for image-to-video
- Aspect ratio: 16:9 (landscape); if user's image is portrait → use 9:16

**超能视频SD2.0 special handling (choice 8):**
- When user picks 8, warmly mention: "超能视频SD2.0 效果很棒！支持最长 15 秒哦～ 要多长？默认 5 秒"
- If user's prompt/image involves real people, WARN: "超能视频SD2.0 对真人效果一般，要不要换 🎯可灵 v3.0 Pro？拍人物它最在行！"
- Extra params: `--param generateAudio=true` (auto-generate audio, on by default), `--param resolution=720p`
- Duration range: 4-15 seconds (broader than other models)
- If user wants auto audio off: `--param generateAudio=false`

## Prompt Optimization

When the user gives a short/vague prompt, ENHANCE it before sending to the API. Example:
- User says: "甜妹跳舞" → Enhance to: "A sweet young woman dancing gracefully in a neon-lit city street at night, dynamic camera movement, cinematic lighting, MV style, 4K"
- User says: "猫在花园" → Enhance to: "An orange tabby cat playing in a sunlit garden with colorful flowers, shallow depth of field, warm afternoon light"

Always write prompts in **English** for best model results, even if the user speaks Chinese.

## Video Failure Retry

If a video model fails (overloaded, timeout, error), do NOT just give up. Tell the user warmly and offer to retry with a different model:
> "哎呀，全能视频S 那边服务器忙不过来了～ 要不要我换 🚀万相2.6 帮你重新生成？一般不会失败的！"

If the user agrees (or says "好"/"换一个"/"试试"), immediately retry with the suggested model. Default fallback order: 全能视频V3.1 Fast → 可灵 → 海螺.
