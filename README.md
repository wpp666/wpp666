# 王平 · wpp666

本地 AI 视频与 Agent 工具链的维护者。做的事情大多在**离线、单机、不依赖云服务**这一侧:
把图像生成、视频生成、TTS 这些能力串成能跑完的管线,并写成可复用的 Agent skill。

> Maintainer of local-first AI video and agent tooling. My work sits on the
> offline, single-machine side: wiring image/video generation into pipelines
> that actually finish, and packaging them as reusable agent skills.

---

## 主要项目 / Featured

### [mv-local-pipeline](https://github.com/wpp666/mv-local-pipeline) — 本地 MV 五段式管线

从分镜到成片全在本机 ComfyUI 上跑,不向任何托管服务提交素材:

```
锚点图 (Krea 2) → 分镜 (Qwen-Image 2.1 + Krea 2) → 视频 (MiniMax H3)
      → 按母带精确拼接 → 中英双语歌词字幕
```

关键在于**每段的时长对齐母带音频**,而不是先出片再硬凑;字幕走 ASS 排版,
不用逐帧贴字。整条链路离线可复现。

*Local, five-stage MV pipeline: anchors → storyboards → video → master-audio-aligned
assembly → bilingual ASS subtitles. Nothing leaves the machine.*

### [cloud-comfyui-ops](https://github.com/wpp666/cloud-comfyui-ops) — 把 ComfyUI 搬到租来的 GPU 上

总结了一个容易踩错的判断:**搬管线到云 GPU,难点几乎从不是算力**,
而是那台机器上的镜像到底装了什么、配置怎么改、文件怎么进出。
仓库里是完整流程加各家平台档案,重点是提交前**逐节点、逐模型校验依赖是否存在**,
避免排两小时队才发现缺一个自定义节点。

*A procedure plus per-provider profiles for moving a local ComfyUI pipeline to a
rented GPU box: probe the image, adapt config, verify every node and model exists
before you submit a job.*

### [VideoSubtitleRemover140mix](https://github.com/wpp666/VideoSubtitleRemover140mix) — 给上游补回 GPU 支持

上游 [video-subtitle-remover](https://github.com/YaoFANGUK/video-subtitle-remover)
v1.4.0 只发了 CPU 安装包,但源码里的 CUDA 检测是完整的 —— 换 CUDA 版 torch 即可。
本仓库用嵌入式 Python 把这个组合打包成绿色便携版,适配 Windows + NVIDIA。

*Restores GPU acceleration for upstream v1.4.0, which shipped CPU-only despite
having intact CUDA code paths. Embedded Python, portable, no registry writes.*

### 其它 / Also

- **[dsh-lazy-studio](https://github.com/wpp666/dsh-lazy-studio)** — DSH 侧栏「导演台」:
  四级串行评审(文本 → 锚点 → 分镜 → 视频),计数在客户端现算,不读宿主的过期快照。
- **[clio-style-krea2](https://github.com/wpp666/clio-style-krea2)** — Krea 2 提示词成稿工具,
  内置 398 个视觉风格,遵守官方的自然语言写法规范。
- **[trae-skills](https://github.com/wpp666/trae-skills)** — 自研 Agent skill 合集备份。

---

## 环境 / Environment

Windows 10/11 · RTX 3070 Ti (8 GB) · CUDA 13.0 · ComfyUI (portable) ·
Python via pymanager · 主要语言 Python / JavaScript

8 GB 显存是这些项目共同的约束条件:上面多数设计取舍(分块加载、模型分档、
本地试片再上云)都是为了让管线在这张卡上跑得完。

---

## 联系 / Contact

Issues 和 PR 都欢迎。提问题时带上显存、CUDA 版本和完整的报错信息,能省一轮往返。

*Issues and PRs welcome. Please include your VRAM, CUDA version, and the full
error output — it saves a round trip.*

<!-- 徽章示意,可删 -->
![GitHub](https://img.shields.io/badge/GitHub-wpp666-181717?logo=github)
