# AIPhoneAgent

**Let an AI model use your iPhone, from the iPhone itself.**

一个 iPhone App，让大模型直接操作你手里的 iPhone：看截图、点屏幕、打字、打开 App。不用电脑、不用越狱、不用开发者账号，用 TrollStore 安装就能跑。

[中文](#中文) | [English](#english)

---

## 中文

### 能干什么

- 你说一句话（语音或打字），它就去手机上帮你办：打开淘宝搜东西、打开设置改开关、查东西……
- 每一步都是：截屏 → 发给大模型看 → 模型说点哪 → App 执行 → 再截屏，循环到做完。
- 付款、输密码、下单、发送这类步骤会自动停住，等你自己手动处理。

### 需要什么

| 东西 | 说明 |
| --- | --- |
| iPhone | 能装 TrollStore 的系统（iOS 14.0–16.6.1、17.0），比如 iPhone 13 / iOS 16.1 |
| TrollStore | 用来安装这个 App（.tipa） |
| 大模型 Key | 在 App 设置里填自己的 Key。默认 DeepSeek（`https://api.deepseek.com`），也支持任何 OpenAI 兼容、能看图的接口，比如智谱 GLM-4V |

### 安装

1. 直接下载本仓库里的 [PhoneAgent-v0.5.16.tipa](PhoneAgent-v0.5.16.tipa)（最新版也在 Actions 的 `PhoneAgent-tipa` 里）。
2. 把 `.tipa` 文件传到 iPhone，用 TrollStore 打开安装。
3. 打开 App，在设置里填好 **Key**、**接口地址**、**模型名**，点 **开始任务**，再打开屏幕录制广播（Broadcast）。
4. 给它一句话任务就行。

### 语音控制

不用打开 App 也能用：做一个快捷指令——听写文本 → URL 编码 → 打开 URL `phoneagent://task?token=你的口令&text=编码后的文本`。详细步骤见 [语音控制设置.md](语音控制设置.md)。口令在 App 的日志里看，6 位数字，只保存在你自己手机上。

### 远程控制（可选）

App 本身只在手机上跑。如果你想在外面用微信发任务让它控制手机，需要自己搭一个中转站（一个很小的 Python 服务），中转站代码不在这个仓库里。App 里填好中转站地址和口令就能用。

### 自己编译

推到 GitHub 就行，Actions 会自动编译出 `.tipa`（见 [.github/workflows/build.yml](.github/workflows/build.yml)，跑在 `macos-15-intel` 上，`bash ./build.sh`）。

### 原理（简单说）

- 看屏幕：用系统录屏扩展（ReplayKit Broadcast）抓图。
- 动手：App 自己点屏幕、打字。
- 脑子：不在手机里，在云端的大模型（DeepSeek 等），App 只负责传图和执行。
- 所以 App 本身很小（几十 KB），重的活都在云端。

### 注意

- 只在你自己的手机、自己的账号上用。
- 付款、密码、订单这类操作它会停住等你，不要让它自己去点。
- 这个项目用到了 iOS 的私有能力和完整权限，只能通过 TrollStore 安装，不能上架 App Store。

---

## English

**Let an AI model use your iPhone, from the iPhone itself.**

An iPhone app that lets a vision model operate your iPhone: it looks at screenshots, taps, types, and opens apps. No computer at run time, no jailbreak, no developer account — install the `.tipa` with TrollStore and go.

### What it does

Give it a task in plain words (typed or by voice), and it carries it out on the phone, one step at a time: screenshot → vision model → action (tap / swipe / type / open app) → screenshot again, until done. Payment, password, order and send steps stop and wait for you.

### Requirements

- An iPhone that can run TrollStore (iOS 14.0–16.6.1, 17.0), e.g. iPhone 13 / iOS 16.1.
- TrollStore to install the `.tipa`.
- Your own API key for a vision model, entered in the app's settings. DeepSeek (`https://api.deepseek.com`) by default; any OpenAI-compatible vision endpoint works (e.g. Zhipu GLM-4V).

### Install

1. Download [PhoneAgent-v0.5.16.tipa](PhoneAgent-v0.5.16.tipa) directly from this repo (the latest build is also in **Actions** as `PhoneAgent-tipa`).
2. Send the `.tipa` to your iPhone and install it with TrollStore.
3. Open the app, fill in your **Key**, **base URL** and **model** in Settings, tap **开始任务** (Start), and turn on the screen-recording broadcast.
4. Give it a task.

### Voice control

Use it without opening the app: build a Shortcut — Dictate Text → URL Encode → Open URL `phoneagent://task?token=YOUR_TOKEN&text=ENCODED_TEXT`. Details in [语音控制设置.md](语音控制设置.md) (Chinese). The 6-digit token is shown in the app's log and stays on your phone.

### Remote control (optional)

The app runs on the phone only. To drive it from WeChat while away, you need your own tiny relay server (a small Python service, not in this repo). Fill in the relay address and token in Settings and it will poll for tasks.

### Build it yourself

Just push to GitHub — Actions builds the `.tipa` automatically (see [.github/workflows/build.yml](.github/workflows/build.yml), `macos-15-intel`, `bash ./build.sh`).

### How it works

- **Eyes:** a ReplayKit broadcast extension captures the screen.
- **Hands:** the app taps and types on the phone itself.
- **Brain:** not on the phone — a cloud vision model (DeepSeek, etc.). The app only passes screenshots and executes actions, which is why the whole app is only tens of KB.

### Notes

- Only automate devices and accounts you own.
- It stops at payment / password / order steps — don't let it tap through those on its own.
- Uses private iOS capabilities and full entitlements; install via TrollStore only, it cannot go on the App Store.

## License

MIT. See [LICENSE](LICENSE).
