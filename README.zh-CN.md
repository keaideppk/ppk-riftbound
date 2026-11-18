# 界隙行者 · Riftbound

[English](README.md) · [在线试玩](https://keaideppk.github.io/riftbound/)

《界隙行者》是一款横版 2D 类银河恶魔城游戏，可以在浏览器中直接玩。界门失联的第七夜，你收到了一条来自自己的求救信号。你从界门站出发，穿过钢铁矿区、荆棘古堡和菌丝废墟，夺回三枚界印，追踪最后一段信号。

## 开始游戏

- **在线玩**。打开 [GitHub Pages 试玩版](https://keaideppk.github.io/riftbound/)，选择「踏入界隙」。
- **离线玩**。在 [index.html 文件页](index.html) 点击「Download raw file」，下载后用浏览器打开。场景、角色、特效和合成音频都包含在这一个 HTML 文件里。

## 探索与战斗

- **18 个相连房间，4 场 Boss 战**。在长房间里沿地面推进，或登上高台寻找补给、残留记录和生命残片。
- **3 项移动能力**。用裂相冲刺跨过断桥，用荆棘钩索登上高台，用回声跃抵达远处平台。在钟冠阶廊连续勾两个锚环，就能到达荆冠守爵的门前。战斗中也能勾向普通敌人，拉近距离后接近战攻击。
- **走进普通通道即可换房**。标有 `↔` 的通道自动通行，跨世界界门用 `E`。入场落点和界标周围有安全区。
- **界标与存档**。在界标按 `E` 恢复生命并设置复活点。靠近界标时打开地图，可以传送到已点亮的界标。进度保存在浏览器本地，在线继续游戏时请使用同一浏览器和网址。

## 操作

| 动作 | 键位 |
| --- | --- |
| 移动 | `A` / `D` 或 `←` / `→` |
| 跳跃 | `Space`、`Z` 或 `W` |
| 攻击 | `J` 或 `X`，行者刃攻击期间再按一次可衔接第二击 |
| 切换武器 | `1`–`4` 直选，`F` 循环，也可点击武器栏 |
| 冲刺 | `Shift` 或 `K`，需先解锁 |
| 钩索 | `V`，需先解锁。触屏按钮为 `↗` |
| 交互 | `E` 或 `↑` |
| 修复 | `Q` 或 `C`，消耗 3 格能量 |
| 地图 | `M` 或 `Tab` |
| 暂停 | `Esc` |

手机和平板有屏幕触控按钮，也可在「操作与设置」中手动开启。离线存档取决于浏览器对本地文件的存储支持，继续游戏时请保留同一文件位置。

## 武器

| 武器 | 取得地点 | 用法 |
| --- | --- | --- |
| 行者刃 | 初始装备 | 两段连击，命中回复能量 |
| 线圈弩 | 泄压泵房 | 每发消耗 1 格能量，辅助瞄准，弹体会被地形挡住 |
| 荆棘枪 | 覆根礼拜堂 | 长距离突刺，击退普通敌人，命中回复能量 |
| 回声锤 | 回声培养巢 | 慢速横扫身前身后，可打散范围内的敌方弹体 |

当前装备和已收集武器也会存档。旧档里已解锁的荆棘攀跃会自动换成荆棘钩索，荆棘枪的解锁记录会保留。

<details>
<summary>开发工作区的构建与验证</summary>

公开仓库用于分发已构建的 [index.html](index.html)。以下命令在带有 `src/`、`build.mjs` 和 `test.mjs` 的本地开发工作区运行。

运行 `node build.mjs`，将 `src` 中的页面、样式与脚本合并为 `index.html`。房间和武器配置位于 `src/world.js`，场景与特效位于 `src/art.js`，角色和武器绘制位于 `src/characters.js`，模拟、音频和存档位于 `src/game.js`，菜单、地图和输入位于 `src/ui.js`。构建只需要 Node.js。

`test.mjs` 通过 Playwright Core 驱动 Chromium。可以用 `RIFTBOUND_PLAYWRIGHT` 指定模块路径，用 `CHROME_PATH` 指定已安装的 Chromium 可执行文件。测试结束会输出截图所在的临时目录。

```powershell
node build.mjs
$env:RIFTBOUND_PLAYWRIGHT = 'C:/path/to/node_modules/playwright-core'
$env:CHROME_PATH = 'C:/Program Files/Google/Chrome/Application/chrome.exe'
node test.mjs
```

回归覆盖 18 个房间、3 项能力、3 枚生命残片、4 场 Boss 战、结局、读档、复活、界标传送和离线启动。通道测试覆盖走入和掉头返回、进门后的停留保护、普通弹体隔离、能力锁和补给箱存档。主线寻路测试隔离了普通敌人，Boss 战保留完整 AI、伤害与能量结算。武器测试覆盖拾取和切换、消耗、射程、墙体遮挡、弹体清除和旧档迁移。另测了 1440×900 桌面、844×390 横屏触控、390×844 与 320×568 竖屏触控，以及正常帧循环和浏览器音频启动。

可以给 HTML 地址添加 `?seed=7201` 固定美术种子、`?q=low` 降低粒子预算、`?noaudio=1` 静音或 `?autostart=1` 跳过标题。测试使用 `?test=1` 与 `window.__G.step()` 推进固定帧。

### 设计资料

- 开发工作区的《单文件 HTML 游戏开发经验手册》和《程序化素材制作手册》记录了单文件交付、数据驱动房间、定步长模拟、场景分层、角色关节动画与合成音效的做法。
- [Hollow Knight 官方网站](https://www.hollowknight.com/)与[Metroid Dread 官方介绍](https://www.nintendo.com/us/store/products/metroid-dread-switch/)。参考相连区域、能力解锁和返回旧区域；房间、剧情与素材为本项目制作。
- [Dead Cells 官方网站](https://deadcells.com/)与 [Canvas 离屏预渲染文档](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)。分别参考动作与平台穿越的节奏组合、离屏缓存的实现方式。

</details>
