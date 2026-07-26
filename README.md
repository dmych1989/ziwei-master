# 天纪紫薇 (Ziwei Master)

> 输入生辰信息，生成一张精美的紫微斗数可视化命盘解读报告，并可打包为安卓 App 在手机上使用。

融合紫微三合派、中州派方法论，用 Python / JavaScript 精确排盘，AI 生成有温度的解读，暗色星空主题 HTML 可视化输出，并支持 Capacitor 打包为安卓 APK。

---
![](https://github.com/dmych1989/ziwei-master/blob/main/p.png)
## 特性

- **精确排盘** —— 基于 [iztro](https://github.com/SylarLong/iztro)（JS）/ [iztro-py](https://github.com/spyfree/iztro-py)（Python）做数学级排盘，避免 LLM 算错。
- **格局自动判定** —— 内置府相朝垣、英星入庙、阳梁昌禄、机月同梁等数十种格局引擎（`scripts/patterns.py` 与 `app/src/engine/patterns.js` 双轨同步）。
- **AI 解读** —— OpenAI 兼容接口（OpenAI / DeepSeek / 硅基流动 / 智谱 / 通义等 16+ 渠道），系统提示词内嵌方法论与风格指南，输出结构化卡片 JSON。
- **多命盘隔离** —— 每个命盘独立会话、后台并行生成、切换不串台。
- **移动端** —— Vue 3 + Capacitor 6，可打包为安卓 App，API Key 本地保存。
- **命例库互操作** —— 开放的 `.mlk` 中性明文格式导入/导出（兼容文墨天机命名，但用开放 JSON，需用户手动提供生辰明文）。

---

## 仓库结构

```
ziwei-master/
├── app/                          # 前端 + 移动端（Vue 3 + Vite + Capacitor）
│   ├── src/
│   │   ├── api/llm.js            # LLM 调用（OpenAI 兼容，多渠道）
│   │   ├── engine/               # 排盘/格局/HTML 渲染引擎
│   │   │   ├── ziwei.js          # 基于 iztro 的排盘
│   │   │   ├── patterns.js       # 格局判定
│   │   │   ├── generateHtml.js   # 命盘 HTML 生成
│   │   │   ├── caseExchange.js   # .mlk 命例库导入导出
│   │   │   ├── brightness.js / qrcodeShare.js / driveBackup.js
│   │   ├── pages/                # CaseDetail / Settings / NewPage / QrPage
│   │   ├── store/                # 命例状态管理
│   │   ├── App.vue / main.js / style.css
│   ├── android/                  # Capacitor 生成的安卓原生壳（不含 build/）
│   ├── index.html / vite.config.js / capacitor.config.json / package.json
├── scripts/                      # Python 管线（与 JS 双轨，需保持同步）
│   ├── calculate_chart.py        # 排盘计算
│   ├── generate_html.py          # HTML 命盘生成
│   ├── patterns.py               # 格局判定
│   ├── stream_preview.py
├── references/                   # 解读方法论参考
│   ├── interpretation_guide.md / stars_reference.md / four_hua_reference.md
├── templates/                    # HTML 模板
│   └── chart_template.html
├── assets/                       # 示例命盘截图
├── yijing/                       # 易经推命模块（年/月/日卦算法与数据）
├── SKILL.md                      # Agent Skills 协议描述文件
└── README.md
```

---

## 快速开始（Web）

```bash
cd app
npm install
npm run dev        # 本地开发预览
# 或
npm run build && npm run preview
```

打开后进入「新建命盘」输入生辰，App 会本地排盘并调用你配置的 LLM 接口生成解读。

---

## 配置 API Key

进入 App 内「设置」页：

1. 选择渠道（OpenAI / DeepSeek / 硅基流动 / 智谱 / 通义 等），或选「自定义」填任意 OpenAI 兼容接口。
2. 粘贴你的 `API Key` 与模型名。
3. 点「测试模型」验证连通性。

所有配置仅保存在本机（Capacitor Preferences / localStorage），不会上传。

> **安卓联网说明**：App 已内置 `network_security_config` 允许明文流量并信任系统/用户证书；如遇连接失败，请先用「测试模型」按钮确认错误（401=Key 错误，404=地址/模型错误，timeout=网络/代理问题）。

---

## 打包安卓 APK

需要本地安装 Android SDK 与 Gradle（或 Android Studio 自带的 Gradle）。

```bash
cd app
npm install
npm run build
npx cap sync android
cd android
# 方式一：用项目 gradlew
./gradlew assembleDebug
# 方式二：用本地已安装的 gradle
gradle assembleDebug
```

产物：`app/android/app/build/outputs/apk/debug/app-debug.apk`

安装到手机：

```bash
adb install -r app/android/app/build/outputs/apk/debug/app-debug.apk
```

> 首次打包需配置 `app/android/local.properties`（**不提交**）：
> ```
> sdk.dir=/你的/Android/Sdk/绝对路径
> ```

---

## Python 管线（独立使用）

```bash
pip install iztro-py
python scripts/calculate_chart.py    # 排盘
python scripts/generate_html.py      # 生成 HTML 命盘
```
## 1.1
![](https://github.com/dmych1989/ziwei-master/blob/main/ScreenShot_2026-07-22_151201_972.png)
---

## 许可证

[MIT](LICENSE)

---

*「算命是对话，不是表演。准确度随沟通趋近。」*
