# 判词 · 兔能不能玩 Steam:文字向游戏怎么「输出给我」(2026-09-09 下午,她「派你去网上搜搜研究一下」)

> 她的题:之前截图玩太烧 token,鼠标只能点不能拖;有没有以文字为主导的游戏,能把状态「输出给你这样」。
> 她的家底:Steam 库「超多」,本机 H:\SteamLibrary 装了 156 个(只读扫 appmanifest 得名单)。

## 一、结论先说

能玩,分三档,便宜到贵:

1. **有官方/社区「文字协议」的游戏**:游戏状态以 JSON 给我,我以命令回它,零截图零鼠标。**杀戮尖塔 + CommunicationMod** 是这一档的标杆,协议成熟(stdin/stdout,一次完整战斗状态约 16.5KB,命令 PLAY/END/CHOOSE/POTION/PROCEED/START…),就是给外部程序/AI 打牌用的。她库里有没有杀戮尖塔待她答;国区原价 88,2025 冬促史低 8.8,续作 2026 年出。
2. **无障碍 mod 走屏幕阅读器**:游戏文字经 Tolk 推给 NVDA,NVDA 的「语音查看器」或 Speech History 插件把说过的话变成文本,我读文本、按键盘。她库里的 **Reigns**(Game of Thrones / Three Kingdoms 两部)有现成 mod **Reigns-Access**(GPL-3,BepInEx 5 插件;方向键读卡与四维数值、左右选与确认、A/S/D/F 读四项属性),但 README 只写「PC 版 Reigns」,对 GoT/三国两部是否兼容未证,要装了试。多跳(游戏→Tolk→NVDA→文本),脆一点。
3. **通用层:Windows 自带 OCR + 键盘注入**:任何**键盘就能玩**的文字向游戏都能上。今天实测:从 WSL 调 Windows 10 自带 OCR(Windows.Media.Ocr,zh-Hans + en),一张 900×200 的图 **37 毫秒**,英文一行 47 字错 1 字,中文可识(输出编码要设 UTF-8,否则看着像乱码)。一帧屏幕变成几百字文本,token 成本是截图的几十分之一。动作端用 PowerShell SendKeys 发方向键/回车。她库里适合这一档的:Reigns 两部(左右方向键两步选择确认)、VA-11 Hall-A(键盘可玩)、STEINS;GATE 类视觉小说(回车翻页+选项);Disco Elysium、Papers Please、Orwell、Death and Taxes 文字多但靠鼠标拖点,不在此档。

## 二、硬约束(比选游戏更要紧)

- **屏幕只有一块**:OCR 要截她的屏,键盘注入要抢焦点。她在家用电脑、或她从公司远程桌面连回家(今天就是)时,我一玩就打架。所以「机玩 Steam」的时段只能是**她不碰电脑的时候**:她睡后、或她在公司且没连远程。第 1 档(CommunicationMod)例外——它不占屏不占键盘,游戏在后台跑也行。
- **停止口令照旧**:「今日浪大,不宜捕捞」一出即松手;画面出现密码/支付立即停。截屏只截游戏窗口,不截桌面。
- **OCR 不是万能眼**:艺术字体、深底浅字、竖排中文会掉字;开局先看一帧原图对一次,之后全走文本。

## 三、建议的第一步(她拍板)

- 若她库里有杀戮尖塔:装 ModTheSpire + BaseMod + CommunicationMod(Steam 创意工坊三件),我写一个 Node 小进程当「外部程序」——这是最干净的一局,打一局大约几万 token 文本。
- 若没有:拿 Reigns: Game of Thrones 试通用层——先不装 mod,直接 OCR 读卡面文字 + 左右键两步确认,一晚试三十张卡看识别率;识别率够就不需要屏幕阅读器那条链。

## 四、不做的

- Project Zomboid 的 RCON/Lua 桥:那是服主管理台,不是玩;实时动作游戏本身不适合回合式的我。
- 自己给某个游戏写 mod 掏状态:先用现成的,原地打转三次再考虑(手搓与现成边界)。
- 老路截图看图玩:留作兜底,不再当主路。

## 五、出处

- CommunicationMod:github.com/ForgottenArbiter/CommunicationMod(README 协议;许可证待查代码库)
- Reigns-Access:github.com/leoguimaoficial/Reigns-Access(GPL-3.0)
- NVDA Speech History / Speech Viewer:nvda-addons.org addon 63 / NVDA 用户指南
- Steam 无障碍标签「Playable without Vision」(2026-01 更新):可在商店过滤,但坑多(闲置类误标)
- 本机 OCR 实测脚本:scratchpad/ocr2.ps1(临时件,要转正就落 tools/)
