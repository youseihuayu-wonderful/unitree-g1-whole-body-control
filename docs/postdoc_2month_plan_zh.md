# 博士后级 2 个月执行方案：Unitree G1 Whole-Body Control

**项目：** Whole-Body Control for Unitree G1  
**实验室：** **INSI**（Institute for Intelligent Networked Systems Laboratory），Professor Yanzhi Wang，Northeastern University  
**团队：** 博士后/主责 1 人 + 物理合作者 1 人 + 导师  
**硬约束：** G1 真机；官方 sim + SDK；偏控制算法、可发 paper；约 8 周  
**成功定义（Must-hit）：** 1 篇可投稿的技术报告/论文初稿 + 可复现实验（sim 完整 + 真机子集）  
**Stretch：** 接上 Pi0.5 或 LingBot-VA 之一作为高层接口 demo（非必须）

---

## 0. 项目定位（博士后标准）

### 0.1 科学问题（写进 paper 的那一句）

> Under realistic DDS/control-loop latency, can a whole-body tracking/balance controller on Unitree G1 improve stability and tracking metrics versus a clear baseline, with transfer from official simulation to the physical robot and an explicit failure taxonomy?

### 0.2 非目标（防止 scope creep）

- 不从零训练大规模人形 foundation model / 不复现 SONIC 级算力  
- 不把 Pi0.5 / LingBot-VA 当作第 1–4 周关键路径  
- 不以多相机感知为主贡献（左相机 fragile）  
- 不做开放世界 loco-manipulation 全覆盖  

### 0.3 交付物（Definition of Done）

| # | 交付物 | 验收 |
| --- | --- | --- |
| D1 | 可运行管线（官方 sim ↔ 同一控制代码路径） | 新人按文档 1 天内复现 baseline |
| D2 | 指标与日志协议 | 每 trial 有 config hash、latency、outcome |
| D3 | Baseline + 提出方法（二选一对比） | sim 上统计显著或至少一致优势趋势 |
| D4 | 真机实验子集 | ≥1 类任务、安全限幅、失败记录完整 |
| D5 | Paper draft | Intro/Method/Experiments/Limitations 齐全 + 主图 4–6 张 |
| D6（可选） | VLA/VA 接入 demo | 高层指令 → 你们的低层接口 → 短 demo |

### 0.4 角色与 RACI（每周一更新）

| 工作包 | 主责 | 协作 | 导师 |
| --- | --- | --- | --- |
| G1 bring-up / SDK / 安全 | 你 | 物理 | 审批真机窗口 |
| 动力学直觉 / 指标 / 失败物理分析 | 物理 | 你 | 讨论 |
| 控制算法设计与实现 | 你 | 物理 | 定 contribution |
| 实验设计与统计 | 物理 + 你 | — | review |
| Paper 叙事与投稿目标 venue | 导师 + 你 | 物理 | 最终拍板 |
| VLA 接入（仅 W6–W8） | 你 | — | 是否保留 |

---

## 1. 总体时间线（8 周甘特）

```text
W1  Bring-up + 安全 + 延迟标定
W2  日志/指标协议 + 冻结“方法候选”
W3–W4  Baseline 全身控制（sim）
W5  提出方法实现（唯一主贡献）
W6  Sim 消融 + 扰动/延迟扫描
W7  真机子集 + 失败 taxonomy
W8  Paper 初稿 + 视频/开源整理
     └─ Stretch: VLA 插件（仅当 W6 绿灯）
```

**决策门（Go/No-Go）**

- **Gate A（W2 末）：** 选定唯一控制贡献点；否则项目失败风险高  
- **Gate B（W4 末）：** Baseline 可复现且指标稳定；否则砍真机范围  
- **Gate C（W6 末）：** 提出方法在 sim 相对 baseline 有清晰增益；否则改写为“系统+评测+失败分析”论文，砍 VLA  
- **Gate D（W7 中）：** 真机安全与数据够用；否则真机只做定性验证  

---

## 2. Step-by-step 结构（按周）

### Phase I — 底座（Week 1–2）

#### Step 1｜组内流程与安全（Day 1–2）
- 读 Google Doc **Starting Point**  
- 输出：`docs/safety_sop.md`（急停、围栏、人员、左相机限制）  
- **Done：** 三人签字确认安全 SOP  

#### Step 2｜官方栈 bring-up（Day 2–5）
- 安装/对接：`unitree_sdk2_python` + `unitree_mujoco`（G1，unitree_hg）  
- 跑通官方 example；确认 DDS domain 与真机隔离策略  
- **Done：** sim 中 10s 稳定站立/示例运动 + raw log  

#### Step 3｜延迟与信号标定（Day 5–7）
- 测量：控制环周期、命令→状态往返近似延迟、抖动  
- 列出观测：关节 q/dq、IMU、接触（若可得）  
- **Done：** `docs/latency_baseline.md` + 一张延迟图  

#### Step 4｜评测协议冻结（Week 2）
- 定义任务套件 T（建议 3–5 条轨迹/行为，宁少勿滥）  
- 指标：跟踪误差、姿态/CoM 晃动、摔倒率、超调、延迟分位（p50/p95）  
- Trial 模板：配置 YAML + 随机种子 + 结果 JSON  
- **Gate A：** 从候选中锁定 **一个** 方法  
  - M1 QP/优先级全身跟踪  
  - M2 延迟补偿/调度  
  - M3 鲁棒/自适应跟踪（sim→real mismatch）  
  - （推荐博士后：优先 **M2 或 M1**，故事清晰）  

---

### Phase II — Baseline 与方法（Week 3–5）

#### Step 5｜Baseline 实现（Week 3）
- 实现 B0：PD/官方 loco 或最简 WBC  
- 同一任务套件 T 上跑 N≥20（sim）  
- **Done：** baseline 表 + 失败录像索引  

#### Step 6｜Baseline 加固（Week 4）
- 修数值/限幅/接触切换 bug  
- 加入标准扰动：推力、命令延迟注入、模型参数扰动（选 2）  
- **Gate B：** 独立复现脚本 `scripts/run_baseline.sh` 一键出表  

#### Step 7｜提出方法（Week 5）
- 只实现 Gate A 锁定的那一个 M*  
- 写清：假设、方程/伪代码、与 B0 差在哪、复杂度/实时性  
- **Done：** 方法在 sim 上至少跑通任务套件 T  

---

### Phase III — 证据链（Week 6–7）

#### Step 8｜Sim 消融（Week 6）
- B0 vs M* 主对比  
- 消融：去掉关键模块 / 不同延迟水平 / 不同扰动强度  
- 统计：均值±方差或置信区间；预注册假设写进笔记  
- **Gate C：**  
  - 绿灯：准备真机 + 可选 VLA  
  - 黄灯：缩小真机；paper 偏系统评测  
  - 红灯：停止加功能，全力整理已有证据  

#### Step 9｜真机子集（Week 7）
- 安全限幅下迁移 **同一代码路径**  
- 任务数可少于 sim，但指标同构  
- 强制记录：左相机限制导致的设定变更  
- 建立 **Failure Taxonomy**：摔、振荡、跟踪发散、通信超时、人工急停  
- **Gate D：** 够不够支撑 paper 的 Real-Robot 小节  

---

### Phase IV — 成文与开源（Week 8）

#### Step 10｜Paper 初稿（Week 8 前半）
结构建议：
1. Intro：人形全身控制缺口 + 延迟/真机  
2. Related：WBC、motion tracking（SONIC）、Unitree 系统、VLA 分层（一笔）  
3. Method：接口 + B0 + M*  
4. Experiments：sim / 消融 / real / failures  
5. Limitations：相机、数据量、未做大规模 VLA  
6. Conclusion  

目标 venue（与导师定 1 个主投 + 1 个保底）：workshop / RA-L short / IROS late / 预印本  

#### Step 11｜可复现打包（Week 8 后半）
- README 复现步骤、配置、种子  
- 主图：系统图、延迟、对比曲线、真机快照、失败案例  
- 开源策略：控制代码开源；真机原始大数据按需  

#### Step 12｜Stretch（仅 Gate C 绿灯）
- 选 **Pi0.5 或 LingBot-VA 之一**  
- 做最小闭环：高层输出 → 你们的全身接口 → 短时任务  
- Paper 中单列 “Interface to VLA” 作为 discussion/附录，不抢主贡献  

---

## 3. 每周仪式（博士后管理节奏）

| 节奏 | 内容 |
| --- | --- |
| 每日 | 15 min stand-up：昨日/今日/阻塞 |
| 每周一 | Gate 检查 + 更新 `docs/weekly/WXX.md` |
| 每周三 | 技术深潜（控制/物理）45–60 min |
| 每周五 | Demo day：视频 1 段 + 指标表更新 |
| 双周 | 与导师 30–45 min：contribution 是否仍可发 |

**文档规范（强制）**
- `docs/weekly/W01.md` … `W08.md`  
- `results/sim/...`、`results/real/...`  
- 任何真机试验：事前 checklist + 事后 postmortem  

---

## 4. 风险登记与对策

| 风险 | 可能性 | 影响 | 对策 |
| --- | --- | --- | --- |
| 真机损坏/相机再损 | 中 | 高 | 限幅、少头部动作、sim 为主证据 |
| Baseline 四周仍不稳 | 中 | 高 | Gate B 砍真机；paper 改系统评测 |
| 方法无增益 | 中 | 高 | Gate C 改叙事；强调延迟表征+失败学 |
| VLA 吞掉时间 | 高 | 高 | W6 前禁止 VLA 主线 |
| 三人沟通成本 | 中 | 中 | 单一任务看板；一人 merge 权 |

---

## 5. Paper 贡献点模板（Week 2 必须填空）

**Claim：** We propose _____ on Unitree G1 that improves _____ vs baseline under _____.  

**Evidence：** sim N=__, real N=__, metrics=____, ablations=____.  

**Novelty vs SONIC/GR00T：** We do not scale foundation training; we contribute _____ on a deployable G1 stack with measured latency and real-robot validation.  

---

## 6. 第一个 72 小时行动清单（立刻执行）

1. 三人一起过 Google Doc Starting Point + 安全 SOP  
2. 你：sim bring-up PR 到本仓库 `feature/w1-bringup`  
3. 物理：起草指标定义 v0（1 页）  
4. 导师：确认目标 venue 类型 + 真机可用时段  
5. 冻结：左相机使用规则写进 SOP  

---

## 7. 成功概率（管理预期）

| 目标 | 估计 |
| --- | --- |
| 完成 D1–D5（控制 paper 初稿 + 证据） | **高**（按本方案执行） |
| 再加 D6（VLA demo） | **中** |
| 2 个月内完整自研人形 VLA | **低（不作为承诺）** |

---

**原则一句话：**  
博士后在 2 个月里卖的是 **可复现的 G1 全身控制贡献 + 严格评测**；VLA 是可选接口证明，不是主菜。
