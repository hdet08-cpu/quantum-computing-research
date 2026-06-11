# EdenCode · AI 量子纠错解码器分析

> EdenCode Inc. · edencode.ai · "Decoding the Quantum Future"
> Seed Round Investor Memo · 2026
> 信息来源：DocSend Deck（7页）+ 讯飞听见会议录音纪要（6月4日）

## 一、公司定位

量子计算机运行时不断出错。**纠错解码器（QEC Decoder）** 实时检测"哪里出了错"并告诉系统"怎么修"。

EdenCode 的核心主张：**用 AI（Graph Transformer）替代传统人工设计算法**来做解码，做成独立硬件产品（GPU → FPGA → ASIC），卖给量子计算机厂商。

**在量子计算栈中的位置：**
```
量子硬件（QuEra等）→ 控制电子（激光/微波）→ ⭐纠错解码层（EdenCode）→ 编译器 → 应用层
```

自称"第一家独立的 AI-native 量子解码器公司"。

## 二、核心团队（5人全职）

### Dr. Wanda Hou 侯万达 — Co-Founder & CEO · 持股 ~40%
- Ph.D. Physics, UC San Diego
- 曾与 Google 合作 AI for quantum systems 研究

### Prof. Everett (Yi-Zhuang) You 尤一庄 — Co-Founder & CTO · 持股 ~53%
- UCSD 量子理论教授
- **⚠ 计划 2026 年秋 take leave**（Deck 原文），但录音中 CEO 称其"全职出来做"——两个表述有矛盾
- 团队声称 prior work 累计 10万+ 引用

### Dr. Hong-Ye Hu — Co-Founder
- Ph.D. Physics, UCSD，曾在 Harvard Quantum Initiative
- 2026年5月 leave 加入

### Dr. Anil Bilgin — Researcher
- Ph.D. Quantum Engineering, U Chicago。负责 AI 训练

### Pranav Srinivas Murali — FPGA Engineer
- M.S. EE, U Washington。**Ex-Meta, Ex-CERN ATLAS**。负责 FPGA 实现

### 顾问团（很强）
- **Prof. Ehud Altman** — 量子理论, UC Berkeley
- **Prof. Xiao-Liang Qi 戚晓亮** — 量子理论, Stanford
- **Prof. Wenchao Xu** — 中性原子实验, ETH Zurich
- **Dr. Ethan Lake** — 量子理论, UC Berkeley
- **Prof. Yuhei Chen** — AI, UC Davis

### 股权结构（来自录音）
- CEO + CTO 合计持有 95%+（不含期权池）
- 期权池 ~15%
- CTO 持股（~53%）远超 CEO（~40%）——权力结构偏技术

## 三、技术方案

### 核心产品：Graph Transformer Decoder (GTD)

基于注意力机制的图神经网络，直接在纠错码的 **Tanner 图**上做消息传递。扩展了 NVIDIA 的长上下文技术。

### Deck 公布的性能数据

| 指标 | 数据 |
|---|---|
| Surface code 阈值 | ~99% + 0.1%（接近 MWPM 的 97% 理论上限） |
| 逻辑错误率降低 | 57%（d=12, p=5%，对比标准算法） |
| 基础模型 | 单一权重解码 d=3 到 d=21，**无需重新训练** |
| 解码延迟 | 0.64 + 1.58 ms/syndrome（NVIDIA H200） |
| NVIDIA 合作 | 被列为 12 家早期合作伙伴之一 |

### 路线图：GPU → FPGA → ASIC

| 阶段 | 时间 | 延迟 | 状态 |
|---|---|---|---|
| GPU (H200) | 现在 | ~2.2ms | ✅ 已完成 |
| FPGA 原型 | 8-12 个月 | μs 级目标 | 开发中 |
| ASIC | 长期 | ns-μs 级 | 规划中 |

### ⚠ 关键延迟 Gap
当前 GPU 上 **2.2ms** → 中性原子需要 **μs 级** → **1000 倍延迟压缩**。这不是简单换芯片能解决的，需要大幅简化模型或重新设计 FPGA 架构。这是最大的技术风险。

## 四、对中性原子的专属价值

### 核心叙事：Atom Loss 是传统算法解决不了的

CEO 原话（录音）："你丢掉两个原子剩 98 个，不是补充两个就 ok。丢了之后剩下 98 个都变了，就像俄罗斯方块从 random 的地方抽掉两块，整个堆叠都不一样了。"

- 传统 MWPM 等算法把错误建模为"成对翻转"，完全不考虑原子丢失
- 确实没有人工设计的算法能很好处理 atom loss（学术界公认难题）
- AI 可以学习量子计算机本身的特性，发现"这里总丢原子"的模式

### ⚠ 但需要注意
- QuEra 2025年已演示**30万原子/秒实时重新加载**，atom loss 的严重性正在被硬件端缓解
- Atom Computing 2026年6月用 toric code 演示了包含原子丢失纠正的完整 QEC 循环——用传统方法非 AI
- 中性原子的"擦除错误"优势：你**知道**哪个原子丢了，可能不需要 AI 来"猜"

## 五、Deck vs 会议录音 · 信息差异

| 维度 | Deck（对外） | 录音（对内） | 差异 |
|---|---|---|---|
| 适用范围 | Modality-agnostic（通用） | 首款产品聚焦中性原子 | Deck 讲更大，实际先做中性原子 |
| 融资金额 | 未写 | 种子轮 $100万+，本轮目标 $1-2M | 非常早期 |
| CTO 状态 | "expected to take leave fall 2026" | "全职出来做" | **矛盾**——CTO 还没正式离开 UCSD |
| 投资人偏好 | 未提 | **只要美国本土美元基金**，规避 CFIUS | 对人民币基金可能排斥 |
| 竞品 | 无 dominant 独立 AI decoder | 明确提到 Riverlane 做超导 QEC 但不做 atom loss | 一致 |

## 六、商业模式

### 三阶段 GTM
1. **软件许可**：benchmarking/评估工具 → 低门槛进入
2. **客户集成**：硬件适配 + FPGA 部署 → 付费增长
3. **嵌入式**：FPGA/ASIC 进控制回路 → 长期收入

### 目标客户
QuEra、D-Wave、研究实验室。录音提到已有 anchor customer 在接触。

### 竞品格局

| 竞品 | 路线 | 差异 |
|---|---|---|
| **Riverlane** | 超导专用 ASIC 解码器 | 不做中性原子；用传统算法非 AI |
| **IBM/Google 自研** | 内部预设算法 | 不对外售卖 |
| **Q-CTRL / Qruise** | 量子控制软件 | 纯软件，不做 FPGA 硬件 |
| **QuEra 自建** | 可能自研 | 不一定需要外部供应商 |

## 七、里程碑与融资

### 计划时间线
| 时间 | 里程碑 |
|---|---|
| Q1 2026 | Production decoder v1.0 for neutral atoms ✅ |
| Q1 2026 | First commercial pilot |
| Q1 2027 | FPGA prototype + 3 个硬件集成项目 |
| Q3 2027 | 可重复部署流程 |
| Q1 2027 | 首笔 FPGA 产品收入 + Series A readiness |

### 资金
- 已融：种子轮 $100万+（硅谷朋友 + YC 关联人士）
- 本轮目标：$1-2M（1-2年 runway）
- 用途：45% R&D+招人 | 25% 产品化 | 20% 硬件合作 | 10% 运营

## 八、综合判断

### ✅ 看好的理由
- **方向正确**：QEC 解码是公认瓶颈，AI decoder 是学术前沿共识
- **学术信号好**：NVIDIA 合作、Google/IBM 共同发表、一线学者顾问
- **竞争空白**：独立 AI-native QEC decoder 确实无人做
- **Atom Loss 叙事有力**：中性原子的真实痛点
- **资金效率高**：$1-2M 支撑 FPGA 原型，里程碑清晰

### ⚠ 核心风险
- **延迟 1000x gap**：GPU 2.2ms → 需要 μs 级，硬工程问题
- **CTO 未全职**：核心技术人物 2026 秋才可能 leave UCSD
- **独立生态位存疑**：QuEra/Pasqal 可能自建解码层（核心能力不外包）
- **极早期**：5人团队，$100万+ 累计融资
- **只要美元基金**：明确规避 CFIUS，对人民币基金不友好
- **Atom Loss 痛点被硬件缓解**：实时原子重新加载技术进步中

### 结论
学术叙事很强、技术方向正确，但极早期且商业化路径存疑。$1-2M 的票面本质上是对"AI QEC decoder 能否成为独立产品类别"的期权赌注。关键验证点：CTO 全职时间、FPGA 原型、anchor customer 签约。

### ⚠ 投资限制
他们明确表示优先美国本土美元基金、规避 CFIUS。如果是人民币基金，可能直接被排除。
