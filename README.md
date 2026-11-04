# 爱斯宾浩 · Ebbinghaus Language Learner

![HTML5](https://img.shields.io/badge/HTML5-zero%20build-orange)
![License](https://img.shields.io/badge/license-MIT-blue)
![Pages](https://img.shields.io/badge/GitHub%20Pages-ready-2088ff)

基于**艾宾浩斯遗忘曲线（Ebbinghaus Forgetting Curve）**的间隔重复（Spaced Repetition）多语种词汇学习工具，附带**意大利语发音**与**英 / 中 / 西 / 意 四语互译**。

单文件静态应用：无构建步骤、无后端、无依赖，数据保存在浏览器本地。

---

## 功能

| 模块 | 说明 |
|---|---|
| 📚 学习 | 按到期队列推送卡片，三态反馈（记住 / 模糊 / 忘了）自动调整下次复习时间 |
| 🗂 词库 | 一张卡片挂载四语释义，含状态标签、下次复习时间、朗读与删除 |
| 🌐 翻译 | 英 / 中 / 西 / 意 任意双向互译，添加卡片时自动回填其余三种语言 |
| 🔊 发音 | Web Speech API；优先匹配系统 `it-IT` 语音，顶部提供意大利语发音测试 |
| 📈 统计 | 理论遗忘曲线与实测留存率对照、复习次数与掌握度 |
| 📐 设计说明 | 应用内置的架构、算法与迭代路线说明 |

内置 10 张意大利语 / 西班牙语种子卡片，打开即可开始复习。

---

## 快速开始

```bash
cd 2026-github
python3 -m http.server 8000
# 打开 http://localhost:8000
```

也可以直接双击 `index.html`。但部分浏览器会限制 `file://` 页面发起的跨域翻译请求，因此**建议使用本地 HTTP 服务**。

无需 `npm install`，无需环境变量，无需 API 密钥。

---

## 上传到 GitHub

本地仓库已完成初始化与首次提交，只需绑定远端并推送：

```bash
cd 2026-github
git remote add origin https://github.com/<USERNAME>/<REPO>.git
git push -u origin main
```

### 开启 GitHub Pages（自动部署）

1. 仓库 → **Settings** → **Pages**
2. **Build and deployment → Source** 选择 **GitHub Actions**
3. 之后每次 push 到 `main`，`.github/workflows/deploy-pages.yml` 会自动发布

站点地址：`https://<USERNAME>.github.io/<REPO>/`

应用全部资源内联在 `index.html` 中，无相对路径依赖，因此可工作于任意子路径。

---

## 复习调度算法

固定间隔序列（艾宾浩斯式，10 档）：

```
5 分 → 30 分 → 12 时 → 1 天 → 2 天 → 4 天 → 7 天 → 15 天 → 30 天 → 90 天
```

| 反馈 | 等级变化 | 下次复习 | 留存统计 |
|---|---|---|---|
| 😎 记住 | `level + 1`（上限 L9） | 序列下一档 | 记为记住 |
| 🤔 模糊 | `level − 1`（下限 L0） | 退回上一档 | 记为未记住 |
| 😵 忘了 | 归零 L0 | 5 分钟后 | 记为未记住 |

> 局限：固定序列未建模个体记忆强度。生产版本建议迁移到 SM-2 / FSRS 动态间隔算法。

---

## 技术实现

| 能力 | 方案 | 约束 |
|---|---|---|
| 间隔重复 | 固定间隔序列 + 等级状态机 | 未建模个体差异 |
| 语音合成 | `SpeechSynthesis` + `it-IT` / `en-US` / `zh-CN` / `es-ES` voice 匹配 | 依赖操作系统内置语音包 |
| 机器翻译 | MyMemory 公共 REST（无需密钥）+ 结果缓存 | 需联网、有配额限制；释义可手动填写兜底 |
| 持久化 | `localStorage` 单键 JSON | 仅本机单浏览器，无云同步 |
| 可视化 | Canvas 2D 绘制遗忘曲线 | — |
| 部署 | GitHub Actions → GitHub Pages | — |

隐私：所有学习数据仅存于本机浏览器，不会上传；翻译请求仅发送用户主动输入的文本至第三方翻译服务。

---

## 目录结构

```
2026-github/
├── index.html                        # 完整应用（单文件）
├── README.md
├── LICENSE                           # MIT
├── .gitignore
└── .github/
    └── workflows/
        └── deploy-pages.yml          # Pages 自动部署
```

---

## 已知限制与后续

1. 意大利语发音依赖本机语音包；建议接入云端 / 离线 TTS 保证一致性。
2. MyMemory 免费接口有速率与配额限制；正式使用应替换为商业翻译 API。
3. `localStorage` 无云同步、无多设备支持，需账号体系与导入导出（CSV / Anki）。
4. 固定间隔序列可升级为 FSRS，并增加听写模式、例句与反向卡片。

---

## License

[MIT](LICENSE) © 2026 Ebbinghaus Language Learner Contributors
