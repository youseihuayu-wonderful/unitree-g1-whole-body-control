# 必知信息 + 学习路线（资料总览）

面向：Unitree G1 whole-body control 三人小组（你 / 物理合作者 / 导师）  
时间框：约 2 个月，**控制主线优先**，VLA 后接。

---

## 1. 一句话看清所有资料在干什么

```text
[高层大脑]  Pi0.5 / LingBot-VA     → 语言/视频 → 动作意图
      ↓ 需要你们做的“接口”
[全身控制]  GR00T-WBC / SONIC 思路 → 全身协调、跟踪、平衡
      ↓ 必须跑在你们机器上
[真机底座]  Unitree G1 官方文档+SDK+sim → DDS / 关节 / 传感器
      ↓
[组内流程]  Google Doc Starting Point → 怎么开机、安全、日常
```

**你们 2 个月最该打穿的是中间两层（G1 底座 + 可测的全身控制）。**  
最上的 VLA 是“以后接上的大脑”，不是第一周主线。

---

## 2. 每份资料：是什么 / 必知 / 必学

### A. 组内主文档（最先看）

| | |
| --- | --- |
| 链接 | [Google Doc](https://docs.google.com/document/d/1QoZlcU7rkTfpKey8VM_59dIZGsulCQKsC3P8WNycYpw/edit?tab=t.i74e3rpuj2kc) |
| 是什么 | 实验室内部流程与说明 |
| **必知** | 新人从左侧 **Starting Point** tab 开始；这是日常操作的权威来源 |
| **必学** | 安全规范、开机/急停、网络与 DDS 域、谁负责真机、实验怎么记日志 |

### B. Unitree G1 官方开发文档（真机圣经）

| | |
| --- | --- |
| 链接 | https://support.unitree.com/home/zh/G1_developer/about_G1 （站内有 AI 可问） |
| 是什么 | G1 硬件/开发者文档 |
| **必知** | 你们用的是 **G1 真机**；控制走官方 **sdk2 / sdk2_python**；仿真优先 **unitree_mujoco**（与真机同 DDS，易迁移） |
| **必知（硬件）** | **左相机 demo day 坏过、胶粘修复 → 极脆弱**；少磕头/左目；实验要记录左右视觉不对称 |
| **必学** | G1 自由度与坐标系；`LowCmd` / `LowState`（G1 用 **unitree_hg**）；IMU；运控/底层模式；急停与限幅；官方 example 怎么跑 |

### C. NVIDIA GR00T Whole-Body Control

| | |
| --- | --- |
| 链接 | https://nvlabs.github.io/GR00T-WholeBodyControl/ |
| 是什么 | NVIDIA 人形全身控制相关文档/栈 |
| **必知** | 这是 **方法与架构参考**，不是 Unitree 一键部署包 |
| **必学** | 他们如何拆高层任务 vs 低层全身控制；观测/动作接口长什么样；评测怎么做——再映射到 G1 |

### D. GEAR-SONIC（全身运动跟踪放大）

| | |
| --- | --- |
| 链接 | https://nvlabs.github.io/GEAR-SONIC/ |
| 是什么 | 大规模 **motion tracking → 自然全身控制**；统一 token 可接 VR / VLA（文中接 GR00T N1.5） |
| **必知** | 核心洞见：**把 motion tracking 当可扩展的人形控制任务**；上层（VLA/遥操作）和下层（跟踪策略）可共用接口 |
| **必知** | 原论文 scale 极大（大数据/多 GPU）；**你们 2 个月不要复现训练规模**，学接口思想与评测 |
| **必学** | motion tracking 指标；扰动鲁棒；“高层命令 → 统一控制接口 → 全身执行”的分层；失败模式（摔、跟丢、抖） |

### E. Pi0.5（π₀.₅）

| | |
| --- | --- |
| 链接 | [blog](https://www.pi.website/blog/pi05) · [openpi](https://github.com/Physical-Intelligence/openpi) · [arxiv](https://arxiv.org/abs/2504.16054) |
| 是什么 | Physical Intelligence 的 VLA，强调 open-world 泛化 |
| **必知** | 输入多是图像+语言+本体感觉 → 输出动作 chunk；**不是**专门为 G1 全身开箱即用 |
| **必学（后期）** | 推理输入输出格式；如何 fine-tune / 适配到你们的 action 空间；延迟有多大 |

### F. LingBot-VA（及 LingBot-VLA）

| | |
| --- | --- |
| 链接 | [LingBot-VA](https://technology.robbyant.com/lingbot-va) · [github](https://github.com/Robbyant/lingbot-va) · [VLA-v2](https://github.com/robbyant/lingbot-vla-v2) |
| 是什么 | Robbyant：**视频-动作世界模型**（VA）以及更广的 VLA 线 |
| **必知** | 偏“预测未来画面 + 反推动作 / 闭环重接地”；和 Pi0.5 同属**高层策略候选**，不是 Week 1 |
| **必学（后期）** | 与 Pi0.5 对比：接口、延迟、是否好接人形；选一个做集成实验即可 |

---

## 3. 你们三人各自最该学什么

| 角色 | 优先学 | 可稍后 |
| --- | --- | --- |
| **你（工程/主开发）** | G1 SDK、DDS、sim↔真机、日志与延迟测量、baseline 控制器 | VLA 训练细节 |
| **物理合作者** | 平衡/接触/稳定性直觉、实验设计、失败物理原因、指标定义 | 大规模深度学习训练 |
| **导师** | 贡献叙事（控制 paper 点）、算力/真机资源、是否碰效率/部署 | 日常 debug |

---

## 4. 两周内“必须会”的清单（过关标准）

1. 能按 Google Doc Starting Point 安全启动流程讲清楚。  
2. 在 `unitree_mujoco` 跑通官方 G1 例子，并知道和真机共用什么通信。  
3. 能量测并记录：**控制环周期 / 端到端延迟**。  
4. 能列出 G1 上你们关心的关节/IMU 信号。  
5. 知道左相机脆弱，实验设计避开依赖左目的危险动作。  
6. 读完 SONIC/GR00T-WBC 后，能用 5 句话说清：“我们低层要跟踪什么、上层以后怎么接 VLA”。  
7. **还不必会**：从零训 Pi0.5 / LingBot-VA。

---

## 5. 建议学习顺序（不要并行摊大饼）

**第 1 周**

1. Google Doc → Starting Point  
2. Unitree G1 文档 + 官方 example（sim）  
3. 写一周笔记：延迟、安全、左相机限制  

**第 2–4 周**

4. Baseline 全身/跟踪控制（PD / 官方 loco / 简化 WBC）  
5. 指标：跟踪误差、姿态晃动、摔倒率、延迟  
6. 精读 SONIC / GR00T-WBC：**只偷接口与评测思想**  

**第 5–8 周**

7. 一个控制改进 + sim 消融 + 有限真机  
8. 若主线稳了，再选 **Pi0.5 或 LingBot-VA 之一** 做高层接入小实验  

---

## 6. 常见误解（提前避开）

| 误解 | 更正 |
| --- | --- |
| “有 SONIC/GR00T 就能直接控 G1” | 要映射到 Unitree SDK；通信与安全仍是你们的活 |
| “先把 VLA 训起来项目才算开始” | 2 个月应先控稳；VLA 是插件 |
| “π5” | 一般指 **π₀.₅（Pi0.5）**，没有单独的 π5 型号 |
| “左相机胶好了就当新的” | 当 fragile；写进实验限制 |

---

## 7. 和发 paper 的关系（记住这句）

- **可发的点**：G1 上可复现的全身控制改进 + 延迟/稳定/失败分析（sim→真机）。  
- **加分但不依赖**：接上 Pi0.5 / LingBot-VA 证明分层接口。  
- **不要押宝**：复现 SONIC 级大规模训练，或 2 个月内自研完整人形 VLA。
