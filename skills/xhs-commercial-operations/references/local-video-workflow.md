# 本机视频实践

## 2026-09-30 实际环境

用户明确授权使用电脑上的 ComfyUI，自行搭建约 10–15 秒视频并验收。实查 RTX 4060 Ti 8GB，约 32GB 系统内存；ComfyUI 0.37.0 已安装 MiniMax H3 图生视频模型、8 步 Turbo LoRA、Qwen3VL 编码器和对应 VAE，以及 SeedVR2 3B Int8 修复模型。未安装新节点或下载新模型。

使用已有 Python 启动本机服务，仅监听 `127.0.0.1`，关闭云 API 节点与额外自定义节点。利用本机 `/object_info` 确认节点接口，再向本机 `/prompt` 提交自建图。不能用网页的隐藏状态或接口替代小红书发布操作。

## 已运行的图生视频

验收过的整托装盘图 → LoadImage → MiniMaxH3ImageToVideo；UNETLoader + 8 步 LoRA；BasicGuider + res_multistep + simple scheduler → SamplerCustomAdvanced → VAEDecodeTiled → CreateVideo → SaveVideo。

- 首帧：白色矮款密胺外壳，钢内胆嵌入，实物窄口沿，四菜与米饭组成完整套餐。
- 尺寸 480×640、length 294、24fps、8 步；实测文件时长 12.248 秒、294 帧，约 434 秒执行完成。
- 提示词强调缓慢推近、轻微侧移、单镜头，碗形、数量、颜色、嵌入关系、菜品不变；不生成递碗动作、夸张环绕、排队、品牌、水印或字幕。
- 不将生成内容称作手机实拍、真实门店经营现场或实际客流证据。
- 抽查开头、每约两秒及末帧共七帧；同时核对时长、帧数和分辨率。记录「抽查」，不能写成逐帧保证完全一致。

## 清晰度提升

已搭建 LoadVideo → GetVideoComponents → ImageScale 720×960 → SeedVR2Preprocess → VAEEncodeTiled → SeedVR2TemporalChunk → SeedVR2Conditioning + 单步 KSampler → SeedVR2TemporalMerge → VAEDecodeTiled → SeedVR2PostProcessing → CreateVideo → SaveVideo。

分块处理降低显存占用，但必须核对输出时长和帧数，避免只处理了一段。提升清晰度可能改变口沿或菜品细节，需重新抽帧检查，不能只看文件分辨率。以实际运行和最终验收记录为准。

本次实测清晰度版：720×960、12.246 秒、294 帧，约 523 秒执行完成。复查同七个帧位置，未观察到碗口明显加厚、钢内胆凸出或数量跳变。输出为额外视频素材，没有另发新笔记。保持无配音、无音乐的静音成片，未将未解码的模型音轨称作现场音频。

可复用的实际 API 图：[图生视频](../assets/workflows/restaurant-12s-api.json)、[清晰度提升](../assets/workflows/restaurant-upscale-api.json)。需把自己的合格输入图或视频放入本机 ComfyUI input，确认模型名、节点、文件名后使用；JSON 是本机 API 图，不是外部平台发布指令。

## 保存与复用

工作区保存 API 图 JSON、提示词、提交结果、执行历史、视频文件、抽帧和验收元数据。流程可复用，下一商品必须替换为自身合格参考图和提示词，并重新核对本机模型与节点版本。若本机缺模型，先报告实际缺项，不能伪造输出或悄悄改用付费云服务。
