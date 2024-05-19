<div class="Box-sc-g0xbh4-0 bJMeLZ js-snippet-clipboard-copy-unpositioned" data-hpc="true"><article class="markdown-body entry-content container-lg" itemprop="text"><div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">缪斯谈话</font></font></h1><a id="user-content-musetalk" class="anchor" aria-label="永久链接：MuseTalk" href="#musetalk"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto">MuseTalk: Real-Time High Quality Lip Synchronization with Latent Space Inpainting
<br>
Yue Zhang <sup>*</sup>,
Minhao Liu<sup>*</sup>,
Zhaokang Chen,
Bin Wu<sup>†</sup>,
Yingjie He,
Chao Zhan,
Wenjiang Zhou
(<sup>*</sup>Equal Contribution, <sup>†</sup>Corresponding Author, <a href="mailto:benbinwu@tencent.com">benbinwu@tencent.com</a>)</p>
<p dir="auto"><strong><a href="https://github.com/TMElyralab/MuseTalk"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">github </font></font></a></strong>    <strong><a href="https://huggingface.co/TMElyralab/MuseTalk" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Huggingface </font></font></a></strong>    <strong><a href="https://huggingface.co/spaces/TMElyralab/MuseTalk" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">space</font></font></a></strong>    <strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">项目（即将推出）</font></font></strong>    <strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">技术报告（即将推出）</font></font></strong></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">我们推出了</font></font><code>MuseTalk</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">一种</font></font><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">实时高质量</font></font></strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">口型同步模型（在 NVIDIA Tesla V100 上为 30fps+）。 MuseTalk 可以与输入视频一起应用，例如由</font></font><a href="https://github.com/TMElyralab/MuseV"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">MuseV</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">生成的视频，作为完整的虚拟人解决方案。</font></font></p>
<div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">概述</font></font></h1><a id="user-content-overview" class="anchor" aria-label="永久链接：概述" href="#overview"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><code>MuseTalk</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是一个实时高质量音频驱动的口型同步模型，在 的潜在空间中进行训练</font></font><code>ft-mse-vae</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，其中</font></font></p>
<ol dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">根据输入音频修改未见过的脸部，脸部区域的大小为</font></font><code>256 x 256</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">支持中文、英文、日文等多种语言的音频。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">支持 NVIDIA Tesla V100 上 30fps+ 的实时推理。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">支持修改面部区域中心点建议，这</font></font><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">显</font></font></strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">着影响生成结果。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">检查点可用在 HDTF 数据集上进行训练。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">培训代码（即将推出）。</font></font></li>
</ol>
<div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">消息</font></font></h1><a id="user-content-news" class="anchor" aria-label="永久链接：新闻" href="#news"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">[04/02/2024] 发布 MuseTalk 项目和预训练模型。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">[04/16/2024] 在 HuggingFace Spaces 上发布 Gradio</font></font><a href="https://huggingface.co/spaces/TMElyralab/MuseTalk" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">演示</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">（感谢 HF 团队的社区资助）</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">[04/17/2024] 📣 我们发布了一个利用 MuseTalk 进行实时推理的管道。</font></font></li>
</ul>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">模型</font></font></h2><a id="user-content-model" class="anchor" aria-label="永久链接： 模型" href="#model"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/figs/musetalk_arc.jpg"><img src="/TMElyralab/MuseTalk/raw/main/assets/figs/musetalk_arc.jpg" alt="模型结构" style="max-width: 100%;"></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">
MuseTalk 在潜在空间中进行训练，其中图像由冻结的 VAE 进行编码。音频由冻结</font></font><code>whisper-tiny</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">模型编码。生成网络的架构借鉴了UNet </font></font><code>stable-diffusion-v1-4</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，其中音频嵌入通过交叉注意力融合到图像嵌入。</font></font></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">请注意，虽然我们使用与稳定扩散非常相似的架构，但 MuseTalk 的不同之处在于它</font></font><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">不是</font></font></strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">扩散模型。相反，MuseTalk 通过一步修复潜在空间来进行操作。</font></font></p>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">案例</font></font></h2><a id="user-content-cases" class="anchor" aria-label="永久链接：案例" href="#cases"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">MuseV + MuseTalk 让人类照片活起来！</font></font></h3><a id="user-content-musev--musetalk-make-human-photos-alive" class="anchor" aria-label="永久链接：MuseV + MuseTalk 让人类照片栩栩如生！" href="#musev--musetalk-make-human-photos-alive"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<table>
  <tbody><tr>
        <td width="33%"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">图像</font></font></td>
        <td width="33%"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">缪斯V</font></font></td>
        <td width="33%"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">+MuseTalk</font></font></td>
  </tr>
  <tr>
    <td>
      <a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/demo/musk/musk.png"><img src="/TMElyralab/MuseTalk/raw/main/assets/demo/musk/musk.png" width="95%" style="max-width: 100%;"></a>
    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 musk_musev.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">musk_musev.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318818257-4a4bb2d1-9d14-4ca9-85c8-7f19c39f712e.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4ODE4MjU3LTRhNGJiMmQxLTlkMTQtNGNhOS04NWM4LTdmMTljMzlmNzEyZS5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yYzExNmY5ZDE0NmZkZmQwMGJmOWYyYWI0NThmZGZhYWFlZTcyZTc3ZjEzMmFlOThjYTIwOGRlNDExNGU2ZmQxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.oKPEeCb40_gYj8ySUNqzPcX90ao0H0_2fbzPGjnk88U" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318818257-4a4bb2d1-9d14-4ca9-85c8-7f19c39f712e.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4ODE4MjU3LTRhNGJiMmQxLTlkMTQtNGNhOS04NWM4LTdmMTljMzlmNzEyZS5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yYzExNmY5ZDE0NmZkZmQwMGJmOWYyYWI0NThmZGZhYWFlZTcyZTc3ZjEzMmFlOThjYTIwOGRlNDExNGU2ZmQxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.oKPEeCb40_gYj8ySUNqzPcX90ao0H0_2fbzPGjnk88U" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 musk_musetalk.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">musk_musetalk.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318818436-b2a879c2-e23a-4d39-911d-51f0343218e4.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4ODE4NDM2LWIyYTg3OWMyLWUyM2EtNGQzOS05MTFkLTUxZjAzNDMyMThlNC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02NzlmZTNkNmE1OGI5MjczYmUwZDE5MWRhNDdhN2FhOTUwYmM5YmEzNmMzNGE1NGEwYWFkMzRiMjA2NTIwY2NlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9._Nye4Lq-FRpmcjqFPVrW031O6M68iYf9e-01yB7gOWI" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318818436-b2a879c2-e23a-4d39-911d-51f0343218e4.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4ODE4NDM2LWIyYTg3OWMyLWUyM2EtNGQzOS05MTFkLTUxZjAzNDMyMThlNC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02NzlmZTNkNmE1OGI5MjczYmUwZDE5MWRhNDdhN2FhOTUwYmM5YmEzNmMzNGE1NGEwYWFkMzRiMjA2NTIwY2NlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9._Nye4Lq-FRpmcjqFPVrW031O6M68iYf9e-01yB7gOWI" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
  </tr>
  <tr>
    <td>
      <a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/demo/yongen/yongen.jpeg"><img src="/TMElyralab/MuseTalk/raw/main/assets/demo/yongen/yongen.jpeg" width="95%" style="max-width: 100%;"></a>
    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 yongen_musev.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">yongen_musev.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318745600-57ef9dee-a9fd-4dc8-839b-3fbbbf0ff3f4.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQ1NjAwLTU3ZWY5ZGVlLWE5ZmQtNGRjOC04MzliLTNmYmJiZjBmZjNmNC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kZWQ3ZTJmNzNiYmEyMWZiYjI3NWY1MjkzYTUxMzhmNzI3MDYwODU0YzkwMTY3ZDI2ZGYyNTNlZjM1N2M2ZjJhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.bxuHvObAbHgWYqvSFsslaOk5P32vacHZXH5CBPP972A" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318745600-57ef9dee-a9fd-4dc8-839b-3fbbbf0ff3f4.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQ1NjAwLTU3ZWY5ZGVlLWE5ZmQtNGRjOC04MzliLTNmYmJiZjBmZjNmNC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kZWQ3ZTJmNzNiYmEyMWZiYjI3NWY1MjkzYTUxMzhmNzI3MDYwODU0YzkwMTY3ZDI2ZGYyNTNlZjM1N2M2ZjJhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.bxuHvObAbHgWYqvSFsslaOk5P32vacHZXH5CBPP972A" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频说明 yongen_musetalk.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">yongen_musetalk.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318745040-94d8dcba-1bcd-4b54-9d1d-8b6fc53228f0.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQ1MDQwLTk0ZDhkY2JhLTFiY2QtNGI1NC05ZDFkLThiNmZjNTMyMjhmMC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iYzNhZDI0ZjI4MzhlMzZiM2MzZjQyNGM5ZThjZWE3YjAzM2NhYzM5MzdjNzAwOGZhY2RjMWZkMGQ4YmMyMGNlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.FlwhfsRw3mEpDBOBSlnlW5P8O--aN_5akihaqxFRAt0" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318745040-94d8dcba-1bcd-4b54-9d1d-8b6fc53228f0.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQ1MDQwLTk0ZDhkY2JhLTFiY2QtNGI1NC05ZDFkLThiNmZjNTMyMjhmMC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iYzNhZDI0ZjI4MzhlMzZiM2MzZjQyNGM5ZThjZWE3YjAzM2NhYzM5MzdjNzAwOGZhY2RjMWZkMGQ4YmMyMGNlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.FlwhfsRw3mEpDBOBSlnlW5P8O--aN_5akihaqxFRAt0" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
  </tr>
  <tr>
    <td>
      <a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/demo/sit/sit.jpeg"><img src="/TMElyralab/MuseTalk/raw/main/assets/demo/sit/sit.jpeg" width="95%" style="max-width: 100%;"></a>
    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频说明 sat_musev.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">坐_musev.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/319032476-5fbab81b-d3f2-4c75-abb5-14c76e51769e.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDMyNDc2LTVmYmFiODFiLWQzZjItNGM3NS1hYmI1LTE0Yzc2ZTUxNzY5ZS5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03ZTIxNThlZjcyYWQ4ZmMwOTA1NmNhMjdkNDJmZmJjYmI1ODE2OWI2OGFiOTY3M2M1NDJkOWU3YTdhYzE0ZjJhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.wspdrvSHBKVfAkon_Yw6_H1BvCzWT4oNrXsXYkmPxEk" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/319032476-5fbab81b-d3f2-4c75-abb5-14c76e51769e.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDMyNDc2LTVmYmFiODFiLWQzZjItNGM3NS1hYmI1LTE0Yzc2ZTUxNzY5ZS5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03ZTIxNThlZjcyYWQ4ZmMwOTA1NmNhMjdkNDJmZmJjYmI1ODE2OWI2OGFiOTY3M2M1NDJkOWU3YTdhYzE0ZjJhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.wspdrvSHBKVfAkon_Yw6_H1BvCzWT4oNrXsXYkmPxEk" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频说明 sat_musetalkmp4.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">坐_musetalkmp4.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/319032511-f8100f4a-3df8-4151-8de2-291b09269f66.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDMyNTExLWY4MTAwZjRhLTNkZjgtNDE1MS04ZGUyLTI5MWIwOTI2OWY2Ni5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03YTZmMzYzZjI0ZDdmZWQ5MzcxOTJjNjIwZDJhZjQ5MWRlZjkzODg1ODFlMjBiODJkNzU0NzI0MzVhOGYzYWFkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.BIV-7__NTCrsh7x0gZvwCQrlxXo_sLW4NuulQl9UWmY" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/319032511-f8100f4a-3df8-4151-8de2-291b09269f66.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDMyNTExLWY4MTAwZjRhLTNkZjgtNDE1MS04ZGUyLTI5MWIwOTI2OWY2Ni5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03YTZmMzYzZjI0ZDdmZWQ5MzcxOTJjNjIwZDJhZjQ5MWRlZjkzODg1ODFlMjBiODJkNzU0NzI0MzVhOGYzYWFkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.BIV-7__NTCrsh7x0gZvwCQrlxXo_sLW4NuulQl9UWmY" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
  </tr>
   <tr>
    <td>
      <a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/demo/man/man.png"><img src="/TMElyralab/MuseTalk/raw/main/assets/demo/man/man.png" width="95%" style="max-width: 100%;"></a>
    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频说明 man_musev.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">man_musev.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/319017878-a6e7d431-5643-4745-9868-8b423a454153.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDE3ODc4LWE2ZTdkNDMxLTU2NDMtNDc0NS05ODY4LThiNDIzYTQ1NDE1My5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03MGUwMDA5MmMzZDFhMzk1ZmI1NzVjNGRiNjgyMjFkNDU5MDVkYzMxZWUwMjRhOWIzZWYzZDlhYmY0M2RjMThkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.KaIEyJgauDuMT7SxP3B6qcN-8-9wkO_AkQTxz3H0Eao" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/319017878-a6e7d431-5643-4745-9868-8b423a454153.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDE3ODc4LWE2ZTdkNDMxLTU2NDMtNDc0NS05ODY4LThiNDIzYTQ1NDE1My5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03MGUwMDA5MmMzZDFhMzk1ZmI1NzVjNGRiNjgyMjFkNDU5MDVkYzMxZWUwMjRhOWIzZWYzZDlhYmY0M2RjMThkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.KaIEyJgauDuMT7SxP3B6qcN-8-9wkO_AkQTxz3H0Eao" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频说明 man_musetalk.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">man_musetalk.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/319016842-6ccf7bc7-cb48-42de-85bd-076d5ee8a623.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDE2ODQyLTZjY2Y3YmM3LWNiNDgtNDJkZS04NWJkLTA3NmQ1ZWU4YTYyMy5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hYjlhOWY4ODI3ZDQ0N2IxMjEyOTQ2ZmRhOWQ3YWQwZTI5MjZlNDExNGFhNTZmN2VmZTA3MjNiNzdkNzJkZWMxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.SGcpX6RlDYBA581g5wtVB5UmTBi9li72ztVnL5ni2ks" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/319016842-6ccf7bc7-cb48-42de-85bd-076d5ee8a623.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDE2ODQyLTZjY2Y3YmM3LWNiNDgtNDJkZS04NWJkLTA3NmQ1ZWU4YTYyMy5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hYjlhOWY4ODI3ZDQ0N2IxMjEyOTQ2ZmRhOWQ3YWQwZTI5MjZlNDExNGFhNTZmN2VmZTA3MjNiNzdkNzJkZWMxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.SGcpX6RlDYBA581g5wtVB5UmTBi9li72ztVnL5ni2ks" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
  </tr>
  <tr>
    <td>
      <a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/demo/monalisa/monalisa.png"><img src="/TMElyralab/MuseTalk/raw/main/assets/demo/monalisa/monalisa.png" width="95%" style="max-width: 100%;"></a>
    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 monalisa_musev.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">蒙娜丽莎_musev.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318740002-1568f604-a34f-4526-a13a-7d282aa2e773.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwMDAyLTE1NjhmNjA0LWEzNGYtNDUyNi1hMTNhLTdkMjgyYWEyZTc3My5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yOTFiYTU1MWU2YWVkNzhjOGE1NmE0NGVlZDBiMmZmMzMxNjI5NTIzMmVhN2ZiYTFlYzRjYjY5OTY5ZDhjMjBlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.E5U1Mm8Dqt3X40RUNFmaY0QiDTs4blJe5LsPaewszZM" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318740002-1568f604-a34f-4526-a13a-7d282aa2e773.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwMDAyLTE1NjhmNjA0LWEzNGYtNDUyNi1hMTNhLTdkMjgyYWEyZTc3My5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yOTFiYTU1MWU2YWVkNzhjOGE1NmE0NGVlZDBiMmZmMzMxNjI5NTIzMmVhN2ZiYTFlYzRjYjY5OTY5ZDhjMjBlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.E5U1Mm8Dqt3X40RUNFmaY0QiDTs4blJe5LsPaewszZM" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频说明 monalisa_musetalk.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">蒙娜丽莎_musetalk.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318740099-a40784fc-a885-4c1f-9b7e-8f87b7caf4e0.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwMDk5LWE0MDc4NGZjLWE4ODUtNGMxZi05YjdlLThmODdiN2NhZjRlMC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hOWY1YWNkOTc3ZTkzZjFhYTM1ZThlNjIwNjQzYzJjNTUyZDQ1ODU1NTA3Mjk0ZjVhNWRmYmRmNTNmMGViOTZhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.B5HtjYxEw7FNuFTyN46q55VW-KkE7-YwwCEpdq2chTk" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318740099-a40784fc-a885-4c1f-9b7e-8f87b7caf4e0.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwMDk5LWE0MDc4NGZjLWE4ODUtNGMxZi05YjdlLThmODdiN2NhZjRlMC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hOWY1YWNkOTc3ZTkzZjFhYTM1ZThlNjIwNjQzYzJjNTUyZDQ1ODU1NTA3Mjk0ZjVhNWRmYmRmNTNmMGViOTZhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.B5HtjYxEw7FNuFTyN46q55VW-KkE7-YwwCEpdq2chTk" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
  </tr>
  <tr>
    <td>
      <a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/demo/sun1/sun.png"><img src="/TMElyralab/MuseTalk/raw/main/assets/demo/sun1/sun.png" width="95%" style="max-width: 100%;"></a>
    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 sun_musev.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">太阳博物馆.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318740276-37a3a666-7b90-4244-8d3a-058cb0e44107.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwMjc2LTM3YTNhNjY2LTdiOTAtNDI0NC04ZDNhLTA1OGNiMGU0NDEwNy5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zMTg3Zjc4NmEwMGEwYTNmZTgxMmFlMWQyZDNkNDQ1M2U2NGVlZTdlZmMzYWU2ZjY4MzJlZDBhYjZhOGE3ZmQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.-_qZgV_aeh9hGC8SrXfn993ZvgR8oKxj0y4rAfsQ-x0" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318740276-37a3a666-7b90-4244-8d3a-058cb0e44107.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwMjc2LTM3YTNhNjY2LTdiOTAtNDI0NC04ZDNhLTA1OGNiMGU0NDEwNy5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zMTg3Zjc4NmEwMGEwYTNmZTgxMmFlMWQyZDNkNDQ1M2U2NGVlZTdlZmMzYWU2ZjY4MzJlZDBhYjZhOGE3ZmQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.-_qZgV_aeh9hGC8SrXfn993ZvgR8oKxj0y4rAfsQ-x0" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 sun_musetalk.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">sun_musetalk.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318740459-172f4ff1-d432-45bd-a5a7-a07dec33a26b.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwNDU5LTE3MmY0ZmYxLWQ0MzItNDViZC1hNWE3LWEwN2RlYzMzYTI2Yi5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zMWM3Mzk3ODNkODIwZDYxZWI5ZTU3MjczOGY1YWE3MGZmZjQ2ZmY5MWI3NjIwNjMzOGY1NzYxOGU2MGE2YWUzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.gI2WOx-3i7YpZdMfvW1wgtMPsZg_ppNwqBI9MRxwERA" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318740459-172f4ff1-d432-45bd-a5a7-a07dec33a26b.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwNDU5LTE3MmY0ZmYxLWQ0MzItNDViZC1hNWE3LWEwN2RlYzMzYTI2Yi5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zMWM3Mzk3ODNkODIwZDYxZWI5ZTU3MjczOGY1YWE3MGZmZjQ2ZmY5MWI3NjIwNjMzOGY1NzYxOGU2MGE2YWUzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.gI2WOx-3i7YpZdMfvW1wgtMPsZg_ppNwqBI9MRxwERA" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
  </tr>
  <tr>
    <td>
      <a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/demo/sun2/sun.png"><img src="/TMElyralab/MuseTalk/raw/main/assets/demo/sun2/sun.png" width="95%" style="max-width: 100%;"></a>
    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 sun_musev.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">太阳博物馆.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318740276-37a3a666-7b90-4244-8d3a-058cb0e44107.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwMjc2LTM3YTNhNjY2LTdiOTAtNDI0NC04ZDNhLTA1OGNiMGU0NDEwNy5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zMTg3Zjc4NmEwMGEwYTNmZTgxMmFlMWQyZDNkNDQ1M2U2NGVlZTdlZmMzYWU2ZjY4MzJlZDBhYjZhOGE3ZmQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.-_qZgV_aeh9hGC8SrXfn993ZvgR8oKxj0y4rAfsQ-x0" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318740276-37a3a666-7b90-4244-8d3a-058cb0e44107.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwMjc2LTM3YTNhNjY2LTdiOTAtNDI0NC04ZDNhLTA1OGNiMGU0NDEwNy5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zMTg3Zjc4NmEwMGEwYTNmZTgxMmFlMWQyZDNkNDQ1M2U2NGVlZTdlZmMzYWU2ZjY4MzJlZDBhYjZhOGE3ZmQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.-_qZgV_aeh9hGC8SrXfn993ZvgR8oKxj0y4rAfsQ-x0" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 sun_musetalk.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">sun_musetalk.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318740560-85a6873d-a028-4cce-af2b-6c59a1f2971d.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwNTYwLTg1YTY4NzNkLWEwMjgtNGNjZS1hZjJiLTZjNTlhMWYyOTcxZC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05ZTM4ZGRmMzJiZjk3OWM1YzRjOTc2MWE0NmM2NmY2ZGQ1MGYxMDVhYjk4MzFkYjc1ZjViMWNlNjE3YzMyYmJkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.I3T1uJJztXhAZo9LWXJHkFkTwIojFXg-fjZ1TS18bt4" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318740560-85a6873d-a028-4cce-af2b-6c59a1f2971d.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzQwNTYwLTg1YTY4NzNkLWEwMjgtNGNjZS1hZjJiLTZjNTlhMWYyOTcxZC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05ZTM4ZGRmMzJiZjk3OWM1YzRjOTc2MWE0NmM2NmY2ZGQ1MGYxMDVhYjk4MzFkYjc1ZjViMWNlNjE3YzMyYmJkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.I3T1uJJztXhAZo9LWXJHkFkTwIojFXg-fjZ1TS18bt4" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
  </tr>
</tbody></table>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">最后两行的人物</font></font><code>Xinying Sun</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是一位超模KOL。你可以在</font></font><a href="https://www.douyin.com/user/MS4wLjABAAAAWDThbMPN_6Xmm_JgXexbOii1K-httbu2APdG8DvDyM8" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">抖音</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">上关注她</font><font style="vertical-align: inherit;">。</font></font></li>
</ul>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">视频配音</font></font></h2><a id="user-content-video-dubbing" class="anchor" aria-label="永久链接：视频配音" href="#video-dubbing"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<table>
  <tbody><tr>
        <td width="70%"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">缪斯谈话</font></font></td>
        <td width="30%"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">原创视频</font></font></td>
  </tr>
  <tr>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频描述 Let_the_Bullets_Fly.mp4" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">让子弹飞.mp4</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/318737056-4d7c5fa1-3550-4d52-8ed2-52f158150f24.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzM3MDU2LTRkN2M1ZmExLTM1NTAtNGQ1Mi04ZWQyLTUyZjE1ODE1MGYyNC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hMDBmODhlZDFmMGMyYWJjZDcyNWVlNjk3OTFhNTRkOGM2NWQ1ZWE1NDY1Y2YxMGQzYjdmYjVjYjRiZmRlY2U5JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.9nrXf8PsdXCJ19__ssGMjBMOaG1aDhpFi-tfQ_6Ov10" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/318737056-4d7c5fa1-3550-4d52-8ed2-52f158150f24.mp4?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE4NzM3MDU2LTRkN2M1ZmExLTM1NTAtNGQ1Mi04ZWQyLTUyZjE1ODE1MGYyNC5tcDQ_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hMDBmODhlZDFmMGMyYWJjZDcyNWVlNjk3OTFhNTRkOGM2NWQ1ZWE1NDY1Y2YxMGQzYjdmYjVjYjRiZmRlY2U5JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.9nrXf8PsdXCJ19__ssGMjBMOaG1aDhpFi-tfQ_6Ov10" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
    <td>
      <a href="//www.bilibili.com/video/BV1wT411b7HU" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">关联</font></font></a>
      
    </td>
  </tr>
</tbody></table>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">对于视频配音，我们应用了自主开发的工具，可以识别说话的人。</font></font></li>
</ul>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">一些有趣的视频！</font></font></h2><a id="user-content-some-interesting-videos" class="anchor" aria-label="永久链接：一些有趣的视频！" href="#some-interesting-videos"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<table>
  <tbody><tr>
        <td width="50%"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">图像</font></font></td>
        <td width="50%"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">MuseV + MuseTalk</font></font></td>
  </tr>
  <tr>
    <td>
      <a target="_blank" rel="noopener noreferrer" href="/TMElyralab/MuseTalk/blob/main/assets/demo/video1/video1.png"><img src="/TMElyralab/MuseTalk/raw/main/assets/demo/video1/video1.png" width="95%" style="max-width: 100%;"></a>
    </td>
    <td>
      <details open="" class="details-reset border rounded-2">
  <summary class="px-3 py-2">
    <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-device-camera-video">
    <path d="M16 3.75v8.5a.75.75 0 0 1-1.136.643L11 10.575v.675A1.75 1.75 0 0 1 9.25 13h-7.5A1.75 1.75 0 0 1 0 11.25v-6.5C0 3.784.784 3 1.75 3h7.5c.966 0 1.75.784 1.75 1.75v.675l3.864-2.318A.75.75 0 0 1 16 3.75Zm-6.5 1a.25.25 0 0 0-.25-.25h-7.5a.25.25 0 0 0-.25.25v6.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-6.5ZM11 8.825l3.5 2.1v-5.85l-3.5 2.1Z"></path>
</svg>
    <span aria-label="视频说明 video1.mov" class="m-1"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">视频1.mov</font></font></span>
    <span class="dropdown-caret"></span>
  </summary>

  <video src="https://private-user-images.githubusercontent.com/163980830/319033040-1f02f9c6-8b98-475e-86b8-82ebee82fe0d.mov?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDMzMDQwLTFmMDJmOWM2LThiOTgtNDc1ZS04NmI4LTgyZWJlZTgyZmUwZC5tb3Y_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kN2Y1YmNiODEwZmFmYzY0NDFhOGJhZWFiNTQwN2U3YWQ0MzU5OTNlMDJmNWNlYjhlNzcxZmJhYzhjYmVhMDQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.x6qG6h6rvSTeVLyCA4cFeUMZFz96HGJ5-aj4z1TsB04" data-canonical-src="https://private-user-images.githubusercontent.com/163980830/319033040-1f02f9c6-8b98-475e-86b8-82ebee82fe0d.mov?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTYwODQzNTMsIm5iZiI6MTcxNjA4NDA1MywicGF0aCI6Ii8xNjM5ODA4MzAvMzE5MDMzMDQwLTFmMDJmOWM2LThiOTgtNDc1ZS04NmI4LTgyZWJlZTgyZmUwZC5tb3Y_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDUxOVQwMjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kN2Y1YmNiODEwZmFmYzY0NDFhOGJhZWFiNTQwN2U3YWQ0MzU5OTNlMDJmNWNlYjhlNzcxZmJhYzhjYmVhMDQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.x6qG6h6rvSTeVLyCA4cFeUMZFz96HGJ5-aj4z1TsB04" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px">

  </video>
</details>

    </td>
  </tr>
</tbody></table>
<div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">全部：</font></font></h1><a id="user-content-todo" class="anchor" aria-label="永久链接： 一切：" href="#todo"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<ul class="contains-task-list">
<li class="task-list-item"><input type="checkbox" id="" disabled="" class="task-list-item-checkbox" checked=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">训练有素的模型和推理代码。</font></font></li>
<li class="task-list-item"><input type="checkbox" id="" disabled="" class="task-list-item-checkbox" checked=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Huggingface 渐变</font></font><a href="https://huggingface.co/spaces/TMElyralab/MuseTalk" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">演示</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></li>
<li class="task-list-item"><input type="checkbox" id="" disabled="" class="task-list-item-checkbox" checked=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">实时推理的代码。</font></font></li>
<li class="task-list-item"><input type="checkbox" id="" disabled="" class="task-list-item-checkbox"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">技术报告。</font></font></li>
<li class="task-list-item"><input type="checkbox" id="" disabled="" class="task-list-item-checkbox"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">培训代码。</font></font></li>
<li class="task-list-item"><input type="checkbox" id="" disabled="" class="task-list-item-checkbox"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">更好的模型（可能需要更长的时间）。</font></font></li>
</ul>
<div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">入门</font></font></h1><a id="user-content-getting-started" class="anchor" aria-label="永久链接：开始使用" href="#getting-started"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">我们为新用户提供了关于MuseTalk的安装和基本使用的详细教程：</font></font></p>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">第三方集成</font></font></h2><a id="user-content-third-party-integration" class="anchor" aria-label="永久链接：第三方集成" href="#third-party-integration"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">感谢第三方集成，让大家安装和使用更加方便。我们还希望您注意，我们尚未验证、维护或更新第三方。具体结果请参考本项目。</font></font></p>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><a href="https://github.com/chaojie/ComfyUI-MuseTalk"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">舒适用户界面</font></font></a></h3><a id="user-content-comfyui" class="anchor" aria-label="永久链接：ComfyUI" href="#comfyui"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">安装</font></font></h2><a id="user-content-installation" class="anchor" aria-label="永久链接：安装" href="#installation"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">要准备Python环境并安装其他软件包，例如opencv、diffusers、mmcv等，请按照以下步骤操作：</font></font></p>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">搭建环境</font></font></h3><a id="user-content-build-environment" class="anchor" aria-label="永久链接：构建环境" href="#build-environment"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">我们推荐 python 版本 &gt;=3.10 和 cuda 版本 =11.7。然后搭建环境如下：</font></font></p>
<div class="highlight highlight-source-shell notranslate position-relative overflow-auto" dir="auto"><pre>pip install -r requirements.txt</pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="pip install -r requirements.txt" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">mmlab 包</font></font></h3><a id="user-content-mmlab-packages" class="anchor" aria-label="永久链接：mmlab 包" href="#mmlab-packages"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<div class="highlight highlight-source-shell notranslate position-relative overflow-auto" dir="auto"><pre>pip install --no-cache-dir -U openmim 
mim install mmengine 
mim install <span class="pl-s"><span class="pl-pds">"</span>mmcv&gt;=2.0.1<span class="pl-pds">"</span></span> 
mim install <span class="pl-s"><span class="pl-pds">"</span>mmdet&gt;=3.1.0<span class="pl-pds">"</span></span> 
mim install <span class="pl-s"><span class="pl-pds">"</span>mmpose&gt;=1.1.0<span class="pl-pds">"</span></span> </pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="pip install --no-cache-dir -U openmim 
mim install mmengine 
mim install &quot;mmcv>=2.0.1&quot; 
mim install &quot;mmdet>=3.1.0&quot; 
mim install &quot;mmpose>=1.1.0&quot; " tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下载 ffmpeg-static</font></font></h3><a id="user-content-download-ffmpeg-static" class="anchor" aria-label="永久链接：下载 ffmpeg-static" href="#download-ffmpeg-static"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下载 ffmpeg-static 和</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>export FFMPEG_PATH=/path/to/ffmpeg
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="export FFMPEG_PATH=/path/to/ffmpeg" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">例如：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>export FFMPEG_PATH=/musetalk/ffmpeg-4.4-amd64-static
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="export FFMPEG_PATH=/musetalk/ffmpeg-4.4-amd64-static" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下载权重</font></font></h3><a id="user-content-download-weights" class="anchor" aria-label="永久链接：下载权重" href="#download-weights"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">您可以手动下载权重，如下所示：</font></font></p>
<ol dir="auto">
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下载我们训练过的</font></font><a href="https://huggingface.co/TMElyralab/MuseTalk" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">权重</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
</li>
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下载其他组件的权重：</font></font></p>
<ul dir="auto">
<li><a href="https://huggingface.co/stabilityai/sd-vae-ft-mse" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">SD-VAE-FT-MSE</font></font></a></li>
<li><a href="https://openaipublic.azureedge.net/main/whisper/models/65147644a518d12f04e32d6f3b26facc3f8dd46e5390956a9424a650c0ce22b9/tiny.pt" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">耳语</font></font></a></li>
<li><a href="https://huggingface.co/yzd-v/DWPose/tree/main" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">dwpose</font></font></a></li>
<li><a href="https://github.com/zllrunning/face-parsing.PyTorch"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">面解析二分</font></font></a></li>
<li><a href="https://download.pytorch.org/models/resnet18-5c106cde.pth" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">资源网18</font></font></a></li>
</ul>
</li>
</ol>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">最后，这些权重应组织</font></font><code>models</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如下：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>./models/
├── musetalk
│   └── musetalk.json
│   └── pytorch_model.bin
├── dwpose
│   └── dw-ll_ucoco_384.pth
├── face-parse-bisent
│   ├── 79999_iter.pth
│   └── resnet18-5c106cde.pth
├── sd-vae-ft-mse
│   ├── config.json
│   └── diffusion_pytorch_model.bin
└── whisper
    └── tiny.pt
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="./models/
├── musetalk
│   └── musetalk.json
│   └── pytorch_model.bin
├── dwpose
│   └── dw-ll_ucoco_384.pth
├── face-parse-bisent
│   ├── 79999_iter.pth
│   └── resnet18-5c106cde.pth
├── sd-vae-ft-mse
│   ├── config.json
│   └── diffusion_pytorch_model.bin
└── whisper
    └── tiny.pt" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<div class="markdown-heading" dir="auto"><h2 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">快速开始</font></font></h2><a id="user-content-quickstart" class="anchor" aria-label="永久链接：快速入门" href="#quickstart"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<div class="markdown-heading" dir="auto"><h3 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">推理</font></font></h3><a id="user-content-inference" class="anchor" aria-label="永久链接： 推理" href="#inference"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在这里，我们提供推理脚本。</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>python -m scripts.inference --inference_config configs/inference/test.yaml 
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="python -m scripts.inference --inference_config configs/inference/test.yaml " tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">configs/inference/test.yaml 为推理配置文件路径，包括video_path和audio_path。 video_path 应该是视频文件、图像文件或图像目录。</font></font></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">建议您输入视频为</font></font><code>25fps</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，与训练模型时使用的 fps 相同。如果您的视频远低于25fps，建议您应用帧插值或使用ffmpeg直接将视频转换为25fps。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用 bbox_shift 获得可调整的结果</font></font></h4><a id="user-content-use-of-bbox_shift-to-have-adjustable-results" class="anchor" aria-label="永久链接：使用 bbox_shift 获得可调整的结果" href="#use-of-bbox_shift-to-have-adjustable-results"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">🔎我们发现面罩上界对张口度有重要影响。因此，为了控制掩模区域，我们建议使用该</font></font><code>bbox_shift</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">参数。正值（朝下半部分移动）会增加嘴巴张开度，而负值（朝上半部分移动）会减少嘴巴张开度。</font></font></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">您可以先使用默认配置运行以获得可调整的值范围，然后在此范围内重新运行脚本。</font></font></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">例如，在 的情况下</font></font><code>Xinying Sun</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，运行默认配置后，显示可调值范围为 [-9, 9]。那么，为了减小嘴巴张开度，我们将 该值设置为</font></font><code>-7</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>python -m scripts.inference --inference_config configs/inference/test.yaml --bbox_shift -7 
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="python -m scripts.inference --inference_config configs/inference/test.yaml --bbox_shift -7 " tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">📌 更多技术细节可以在</font></font><a href="/TMElyralab/MuseTalk/blob/main/assets/BBOX_SHIFT.md"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">bbox_shift</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">中找到。</font></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">结合 MuseV 和 MuseTalk</font></font></h4><a id="user-content-combining-musev-and-musetalk" class="anchor" aria-label="永久链接：结合 MuseV 和 MuseTalk" href="#combining-musev-and-musetalk"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">作为虚拟人生成的完整解决方案，建议您首先参考</font><a href="https://github.com/TMElyralab/MuseV?tab=readme-ov-file#text2video"><font style="vertical-align: inherit;">此应用</font></a></font><a href="https://github.com/TMElyralab/MuseV"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">MuseV</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">生成视频（文本转视频、图像转视频或姿势转视频）</font><font style="vertical-align: inherit;">。建议进行帧插值以提高帧速率。然后，您可以通过引用</font><a href="https://github.com/TMElyralab/MuseTalk?tab=readme-ov-file#inference"><font style="vertical-align: inherit;">此</font></a><font style="vertical-align: inherit;">来生成口型同步视频</font><font style="vertical-align: inherit;">。</font></font><a href="https://github.com/TMElyralab/MuseV?tab=readme-ov-file#text2video"><font style="vertical-align: inherit;"></font></a><font style="vertical-align: inherit;"></font><code>MuseTalk</code><font style="vertical-align: inherit;"></font><a href="https://github.com/TMElyralab/MuseTalk?tab=readme-ov-file#inference"><font style="vertical-align: inherit;"></font></a><font style="vertical-align: inherit;"></font></p>
<div class="markdown-heading" dir="auto"><h4 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">🆕 实时推理</font></font></h4><a id="user-content-new-real-time-inference" class="anchor" aria-label="永久链接：：new：实时推理" href="#new-real-time-inference"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在这里，我们提供推理脚本。该脚本首先预先应用必要的预处理，例如人脸检测、人脸解析和 VAE 编码。在推理过程中，只涉及UNet和VAE解码器，这使得MuseTalk具有实时性。</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>python -m scripts.realtime_inference --inference_config configs/inference/realtime.yaml --batch_size 4
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="python -m scripts.realtime_inference --inference_config configs/inference/realtime.yaml --batch_size 4" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">configs/inference / </font><font style="vertical-align: inherit;">realtime.yaml 为实时推理配置文件路径，包括</font></font><code>preparation</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><code>video_path</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><code>bbox_shift</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font><code>audio_clips</code><font style="vertical-align: inherit;"></font></p>
<ol dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">开始</font><font style="vertical-align: inherit;">准备新</font><font style="vertical-align: inherit;">的</font><font style="vertical-align: inherit;">材料</font></font><code>preparation</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">（如</font><font style="vertical-align: inherit;">发生变化，还需重新准备材料。）</font></font><code>True</code><font style="vertical-align: inherit;"></font><code>realtime.yaml</code><font style="vertical-align: inherit;"></font><code>avatar</code><font style="vertical-align: inherit;"></font><code>bbox_shift</code><font style="vertical-align: inherit;"></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">之后，</font></font><code>avatar</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将使用从中选择的音频剪辑来</font></font><code>audio_clips</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">生成视频。
</font></font><div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>Inferring using: data/audio/yongen.wav
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="Inferring using: data/audio/yongen.wav" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
</li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">当 MuseTalk 进行推理时，子线程可以同时将结果传输给用户。生成过程在 NVIDIA Tesla V100 上可以达到 30fps+。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您想使用相同的头像生成更多视频，请设置并运行此</font></font><code>preparation</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">脚本</font><font style="vertical-align: inherit;">。</font></font><code>False</code><font style="vertical-align: inherit;"></font></li>
</ol>
<div class="markdown-heading" dir="auto"><h5 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">实时推理注意事项</font></font></h5><a id="user-content-note-for-real-time-inference" class="anchor" aria-label="永久链接：实时推理注意事项" href="#note-for-real-time-inference"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<ol dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您想使用相同的头像/视频生成多个视频，您还可以使用此脚本来</font></font><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">显</font></font></strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">着加快生成过程。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在前面的脚本中，生成时间也受到I/O（例如保存图像）的限制。如果你只是想测试生成速度而不保存图像，你可以运行</font></font></li>
</ol>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>python -m scripts.realtime_inference --inference_config configs/inference/realtime.yaml --skip_save_images
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="python -m scripts.realtime_inference --inference_config configs/inference/realtime.yaml --skip_save_images" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">致谢</font></font></h1><a id="user-content-acknowledgement" class="anchor" aria-label="永久链接：致谢" href="#acknowledgement"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<ol dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">我们感谢开源组件，如</font></font><a href="https://github.com/openai/whisper"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Whisper</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><a href="https://github.com/IDEA-Research/DWPose"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">dwpose</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><a href="https://github.com/1adrianb/face-alignment"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Face-Alignment</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><a href="https://github.com/zllrunning/face-parsing.PyTorch"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Face-Parsing</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><a href="https://github.com/yxlijun/S3FD.pytorch"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">S3FD</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">MuseTalk 多次提到了</font></font><a href="https://github.com/huggingface/diffusers"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">扩散器</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和</font></font><a href="https://github.com/isaacOnline/whisper/tree/extract-embeddings"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">isaacOnline/whisper</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">MuseTalk 是基于</font></font><a href="https://github.com/MRzzm/HDTF"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">HDTF</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">数据集构建的。</font></font></li>
</ol>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">感谢开源！</font></font></p>
<div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">局限性</font></font></h1><a id="user-content-limitations" class="anchor" aria-label="永久链接：限制" href="#limitations"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<ul dir="auto">
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">分辨率：虽然MuseTalk使用256 x 256的面部区域大小，这使得它比其他开源方法更好，但它尚未达到理论分辨率界限。我们将继续处理这个问题。如果您需要更高分辨率，可以将</font><a href="https://github.com/TencentARC/GFPGAN"><font style="vertical-align: inherit;">GFPGAN</font></a></font><br><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">
等超分辨率模型</font><font style="vertical-align: inherit;">与 MuseTalk 结合使用。</font></font><a href="https://github.com/TencentARC/GFPGAN"><font style="vertical-align: inherit;"></font></a><font style="vertical-align: inherit;"></font></p>
</li>
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">身份保存：原始脸部的一些细节没有得到很好的保存，例如胡须、唇形和颜色。</font></font></p>
</li>
<li>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">抖动：由于目前的管线采用单帧生成，存在一定的抖动。</font></font></p>
</li>
</ul>
<div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">引文</font></font></h1><a id="user-content-citation" class="anchor" aria-label="永久链接：引文" href="#citation"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<div class="highlight highlight-text-bibtex notranslate position-relative overflow-auto" dir="auto"><pre><span class="pl-k">@article</span>{<span class="pl-en">musetalk</span>,
  <span class="pl-s">title</span>=<span class="pl-s"><span class="pl-pds">{</span>MuseTalk: Real-Time High Quality Lip Synchorization with Latent Space Inpainting<span class="pl-pds">}</span></span>,
  <span class="pl-s">author</span>=<span class="pl-s"><span class="pl-pds">{</span>Zhang, Yue and Liu, Minhao and Chen, Zhaokang and Wu, Bin and He, Yingjie and Zhan, Chao and Zhou, Wenjiang<span class="pl-pds">}</span></span>,
  <span class="pl-s">journal</span>=<span class="pl-s"><span class="pl-pds">{</span>arxiv<span class="pl-pds">}</span></span>,
  <span class="pl-s">year</span>=<span class="pl-s"><span class="pl-pds">{</span>2024<span class="pl-pds">}</span></span>
}</pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="@article{musetalk,
  title={MuseTalk: Real-Time High Quality Lip Synchorization with Latent Space Inpainting},
  author={Zhang, Yue and Liu, Minhao and Chen, Zhaokang and Wu, Bin and He, Yingjie and Zhan, Chao and Zhou, Wenjiang},
  journal={arxiv},
  year={2024}
}" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<div class="markdown-heading" dir="auto"><h1 tabindex="-1" class="heading-element" dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">免责声明/许可</font></font></h1><a id="user-content-disclaimerlicense" class="anchor" aria-label="永久链接：免责声明/许可" href="#disclaimerlicense"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a></div>
<ol dir="auto">
<li><code>code</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：MuseTalk 的代码是在 MIT 许可证下发布的。学术和商业用途均没有限制。</font></font></li>
<li><code>model</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：经过训练的模型可用于任何目的，甚至商业用途。</font></font></li>
<li><code>other opensource model</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：使用的其他开源模型必须遵守其许可证，例如</font></font><code>whisper</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><code>ft-mse-vae</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><code>dwpose</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">、</font></font><code>S3FD</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">等。</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">测试数据是从互联网收集的，仅可用于非商业研究目的。</font></font></li>
<li><code>AIGC</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：该项目致力于对人工智能驱动的视频生成领域产生积极影响。用户可以自由地使用此工具创建视频，但他们应该遵守当地法律并负责任地使用它。开发者对用户潜在的误用不承担任何责任。</font></font></li>
</ol>
</article></div>
