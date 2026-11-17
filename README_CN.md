# 电商用户转化漏斗优化与 A/B 测试分析
### From Customer Behavior Analytics to Conversion Optimization

**SQL · Python · Pandas · NumPy · SciPy · StatsModels · Power BI**

[English](./README.md) | [中文项目介绍](./README_CN.md)

---

## 一、项目概述（Executive Summary）

本项目基于 Kaggle 电商用户行为数据集，分析用户从浏览商品到加入购物车、最终完成购买的行为路径，识别电商转化漏斗中的关键流失环节，并通过优惠券提醒策略的 A/B Testing 分析评估转化优化方案。

项目使用超过 **300万条电商用户行为记录**，结合 SQL、Python 和 Power BI，构建从数据处理、用户行为分析、转化漏斗分析到实验评估及商业影响估算的完整分析流程。

**核心分析成果：**

| 分析指标 | 项目结果 |
|---|---:|
| 用户行为记录 | 3,000,000 |
| 商品浏览事件（View） | 2,904,210 |
| 加入购物车事件（Cart） | 44,988 |
| 购买事件（Purchase） | 50,802 |
| Control Group 购买转化率 | 1.70% |
| Treatment Group 购买转化率 | 2.44% |
| 绝对转化率提升 | 0.74 个百分点 |
| 相对转化率提升 | 43.5% |
| 统计检验结果 | p < 0.001 |
| 估算新增购买人数 | 3,041 |
| 估算增量 GMV | $973,248 |

以上实验结果及商业影响数字为原项目记录的分析结果和模拟推广估算，不代表已经在真实电商平台完成全量部署。

---

## 二、业务背景与分析目标（Business Context & Objectives）

电商平台通常拥有大量商品浏览流量，但浏览用户并不一定会完成购买。

用户可能在商品浏览、加入购物车或购买决策等不同阶段离开平台，导致潜在交易机会流失。

因此，本项目关注的不只是平台整体销售额，而是如何通过用户行为数据识别转化瓶颈，并提出可以进一步验证的优化策略。

### 核心业务问题

1. 用户主要在哪个购买阶段流失？
2. 哪些用户群体贡献了更多平台收入？
3. 用户首次访问之后的留存表现如何？
4. 优惠券提醒策略能否改善购买转化？
5. 如果将优化策略推广到更大规模的用户群体，可能产生怎样的 GMV 增量？

### 分析目标

- 建立电商用户购买行为分析体系。
- 通过 SQL 构建购买转化漏斗。
- 基于消费行为及消费金额进行用户分群。
- 使用 Cohort Analysis 分析用户留存。
- 构建优惠券提醒策略的 A/B Testing 分析框架。
- 通过统计检验和置信区间评估实验结果。
- 使用 Power BI 构建交互式商业分析 Dashboard。

---

## 三、数据集与数据处理（Dataset & Data Preparation）

### 3.1 数据来源

**Dataset:** Kaggle E-Commerce Behavior Dataset

**分析样本：**
- 3,000,000 条用户交互记录。
- October 用户活动数据。

### 3.2 主要数据字段

| 字段 | 说明 |
|---|---|
| event_time | 用户行为发生时间 |
| event_type | 用户行为类型：View / Cart / Purchase |
| product_id | 商品唯一标识 |
| category_code | 商品类别 |
| brand | 商品品牌 |
| price | 商品价格 |
| user_id | 用户标识 |
| user_session | 用户会话标识 |

### 3.3 数据清洗

在分析之前，完成以下数据准备工作：

- 将事件时间转换为标准 Datetime 格式。
- 检查数据缺失情况。
- 验证不同事件类型的分布。
- 统一用户行为事件类型。

### 3.4 特征工程

基于原始用户行为记录构建：

- Event Date
- Purchase Frequency
- User Total Spend
- User Segments
- Retention Cohorts
- Experimental Groups

通过以上处理，为漏斗分析、用户分群、留存分析及实验评估提供统一的数据基础。

---

## 四、购买转化漏斗分析（E-Commerce Funnel Analysis）

### 4.1 分析框架

项目采用以下购买行为路径：

**View → Cart → Purchase**

通过用户行为事件分析电商平台从商品浏览到最终购买的转化情况。

### 4.2 事件分布

| 行为阶段 | 事件数量 |
|---|---:|
| View | 2,904,210 |
| Cart | 44,988 |
| Purchase | 50,802 |

以上为不同事件类型的记录数量，并非经过严格顺序约束后的唯一用户漏斗人数。

### 4.3 核心发现

- 大量用户行为集中在商品浏览阶段。
- 从商品浏览到完成购买之间存在明显的行为数量差异。
- 用户购买路径中存在值得进一步分析的转化机会。

### 4.4 业务洞察

仅依赖流量增长并不能保证销售额同步增长。

平台可以进一步分析商品浏览、购物车行为与购买行为之间的关系，识别用户购买过程中的潜在阻碍。

**优化方向：**

- 减少商品浏览到购买环节的转化阻力。
- 针对具有购买意向但尚未完成购买的用户设计干预策略。
- 结合用户分群制定差异化营销方案。

---

## 五、用户分群分析（Customer Segmentation）

### 5.1 分群方法

根据用户购买行为和消费贡献，将用户划分为三个主要群体：

| 用户群体 | 用户特征 |
|---|---|
| High Value | 购买频率较高、消费金额较高 |
| Medium Value | 购买频率和消费水平处于中间区间 |
| Low Value | 用户参与度和消费贡献相对较低 |

### 5.2 核心发现

- 高价值用户贡献了不成比例的较高平台收入。
- 大部分用户属于低价值和中等价值群体。
- 高价值用户具有更高的留存维护优先级。

### 5.3 业务建议

**High Value Customers**
- 优先维护高价值客户。
- 探索会员及忠诚度计划。
- 提高重复购买机会。

**Medium Value Customers**
- 通过个性化推荐及营销提醒提升购买频率。
- 识别具有向高价值群体转化潜力的用户。

**Low Value Customers**
- 分析低参与度的可能原因。
- 通过有针对性的营销触达改善用户活跃度。

---

## 六、用户留存分析（Customer Retention Analysis）

### 6.1 Cohort Analysis

项目基于用户首次活动日期建立 Cohort Retention 分析框架，观察用户首次交互之后的回访情况。

### 6.2 Day-1 Retention Rate

**Day-1 Retention Rate = Day 1 Returning Users ÷ Day 0 Acquired Users**

### 6.3 核心发现

- 用户留存率随着时间推移呈下降趋势。
- Repeat User Ratio 低于 3%。
- 大部分用户在首次访问之后没有再次返回。

### 6.4 业务洞察

单纯依赖新用户获取并不足以支撑长期业务增长。

平台需要同时关注用户获取、首次购买和后续重复购买，通过用户留存策略提升长期客户价值。

**建议关注的指标：**

- Day-1 Retention
- Cohort Retention
- Repeat User Ratio
- Purchase Frequency
- User Total Spend

---

## 七、转化优化策略设计（Optimization Strategy）

### 7.1 问题识别

根据前面的漏斗和用户行为分析，项目关注具有购买意向但没有完成购买的用户。

这部分用户可能存在购买犹豫或购买流程中的阻碍，因此具有进一步营销干预的潜力。

### 7.2 提出优化假设

**Coupon Reminder Strategy — 优惠券提醒策略**

向符合预设条件的用户发送优惠券提醒，以鼓励其完成购买。

### 7.3 预期效果

- 提高购买转化率。
- 减少潜在购买放弃。
- 增加平台 GMV。
- 改善营销触达的效果。

本策略作为后续实验评估的干预方案，具体效果需要通过实验数据进行验证。

---

## 八、A/B Testing 实验设计（Experimental Design）

### 8.1 实验分组

项目建立了 Control Group 与 Treatment Group 两组对比框架。

| 实验组 | 干预方式 |
|---|---|
| Control Group | 不实施优惠券提醒 |
| Treatment Group | 实施优惠券提醒 |

### 8.2 核心评价指标

**Purchase Conversion Rate**

购买转化率是评估优惠券提醒策略效果的主要指标。

### 8.3 实验评估目标

通过比较两个实验组的购买转化率，评估优惠券提醒策略与购买转化表现之间的差异。

---

## 九、A/B Testing 结果分析（Experiment Results）

### 9.1 转化率对比

| 指标 | Control Group | Treatment Group |
|---|---:|---:|
| Purchase Conversion Rate | 1.70% | 2.44% |

### 9.2 绝对转化率提升（Absolute Uplift）

**2.44% − 1.70% = 0.74 个百分点**

Treatment Group 的购买转化率比 Control Group 高出 0.74 个百分点。

### 9.3 相对转化率提升（Relative Uplift）

**(2.44% − 1.70%) ÷ 1.70% × 100% = 43.5%**

项目记录的相对购买转化率提升为43.5%。

### 9.4 结果解读

在项目所采用的实验分析框架下，Treatment Group 的购买转化率高于 Control Group。

这一结果为进一步评估优惠券提醒策略提供了数据依据。

---

## 十、统计验证与置信区间分析（Statistical Validation）

### 10.1 假设检验

项目使用独立样本 T-Test 进行统计检验。

**Null Hypothesis (H₀)**

The intervention does not affect conversion rate.

优惠券提醒策略不会影响购买转化率。

**Alternative Hypothesis (H₁)**

The intervention improves conversion rate.

优惠券提醒策略能够提高购买转化率。

### 10.2 检验结果

**p-value < 0.001**

根据项目记录的检验结果，差异达到统计显著性水平。

### 10.3 95% Confidence Interval

项目使用 Wilson Method 计算两组购买转化率的95%置信区间。

| 实验组 | 转化率 | 95% Confidence Interval |
|---|---:|---:|
| Control | 1.70% | 1.64% – 1.76% |
| Treatment | 2.44% | 2.37% – 2.51% |

项目报告的两组置信区间没有重叠，为观察到的转化率差异提供了进一步的统计参考。

### 10.4 统计结论

项目记录的统计结果支持 Treatment Group 与 Control Group 之间存在显著的购买转化率差异。

需要注意，统计显著性本身并不自动证明因果关系。实验分组方式、样本独立性、检验方法及数据生成过程仍然是解释实验结果的重要前提。

---

## 十一、商业影响估算（Business Impact Estimation）

为了评估优惠券提醒策略在更大规模用户群体中的潜在商业价值，项目进一步进行了平台范围的推广情景模拟。

### 11.1 估算结果

| 商业指标 | 项目估算 |
|---|---:|
| Estimated Additional Buyers | 3,041 |
| Estimated Incremental GMV | $973,248 |

### 11.2 业务解读

即使购买转化率只提高一定幅度，当这一变化应用于较大的用户规模时，也可能形成可观的增量 GMV。

这一结果用于展示优化策略的潜在商业价值，而不是已经实现的实际收入增长。

### 11.3 业务应用方向

- 根据用户购买意向识别营销触达对象。
- 评估优惠券提醒的潜在增量价值。
- 对不同用户群体设计差异化营销策略。
- 继续通过实验验证其他转化优化方案。

---

## 十二、Power BI Dashboard（商业分析可视化）

本项目使用 Power BI 构建交互式商业分析 Dashboard，将用户行为、产品表现、用户分群、留存及实验结果整合到统一的分析界面中。

Dashboard 包含五个主要页面。

### Page 1 — Executive Overview

整体业务表现分析：

- GMV
- Orders
- Active Users
- Repeat Users
- Daily Trends

### Page 2 — Product Analysis

商品及品牌表现分析：

- Category GMV
- Brand Performance
- Revenue Distribution
- Average Selling Price

### Page 3 — Customer Behavior Analysis

用户购买行为分析：

- Top Buyers
- Purchase Distribution
- User Segmentation

### Page 4 — Retention & Funnel Analysis

用户转化及留存分析：

- Retention Trend
- Funnel Conversion
- User Journey Analysis

### Page 5 — Experiment Analysis

实验效果分析：

- Control vs Treatment
- Conversion Uplift
- Confidence Intervals
- P-value
- Estimated GMV Impact

---

## 十三、Dashboard 展示（Dashboard Showcase）

### 1. Executive Overview

![Executive Overview](./assets/page1_executive_overview.png)

整体展示平台 GMV、活跃用户、订单量、重复购买率及业务趋势。

### 2. Product Analysis

![Product Analysis](./assets/page2_product_analysis.png)

展示商品类别收入贡献、品牌 GMV 分布及平均销售价格。

### 3. Customer Behavior Analysis

![Customer Behavior Analysis](./assets/page3_user_analysis.png)

展示高价值用户、购买分布及用户分群情况。

### 4. Retention & Funnel Analysis

![Retention and Funnel Analysis](./assets/page4_retention_funnel.png)

展示用户留存趋势、购买转化漏斗及用户行为路径。

### 5. Experiment Analysis

![Experiment Analysis](./assets/page5_experiment_analysis.png)

展示 A/B Testing 实验结果、转化率提升、置信区间、统计显著性及预估 GMV 影响。

---

## 十四、业务建议（Business Recommendations）

根据项目分析结果，提出以下五项业务建议。

### Recommendation 1 — 优化优惠券提醒策略

对具有较强购买意向的用户开展优惠券提醒实验，持续评估购买转化表现。

### Recommendation 2 — 优化购买转化路径

重点分析 View 到 Purchase 之间的行为差异，识别用户放弃购买的潜在环节。

### Recommendation 3 — 建立用户留存运营机制

针对首次访问及首次购买用户设计持续触达策略，提高重复购买机会。

### Recommendation 4 — 加强高价值客户维护

根据用户消费贡献及购买频率识别高价值客户，探索会员体系和忠诚度计划。

### Recommendation 5 — 持续开展实验验证

对后续营销策略进行持续实验评估，结合转化率、统计检验和商业影响估算判断策略效果。

---

## 十五、项目技术栈（Technology Stack）

| 技术类别 | 使用工具 | 主要用途 |
|---|---|---|
| 数据查询 | SQL | 用户行为数据分析、购买漏斗构建 |
| 数据处理 | Python / Pandas / NumPy | 数据清洗、特征工程及用户分群 |
| 统计分析 | SciPy / StatsModels | 假设检验及统计分析 |
| 数据可视化 | Matplotlib | 探索性数据分析 |
| 商业智能 | Power BI | 交互式业务分析 Dashboard |
| 数据来源 | Kaggle | 电商用户行为数据集 |

---

## 十六、项目目录结构（Repository Structure）

```text
E-Commerce-Funnel-Optimization-Experimentation/
│
├── assets/
│   ├── page1_executive_overview.png
│   ├── page2_product_analysis.png
│   ├── page3_user_analysis.png
│   ├── page4_retention_funnel.png
│   └── page5_experiment_analysis.png
│
├── result_data/
│   ├── experiment_summary.csv
│   ├── experiment_funnel.csv
│   └── experiment_ci.csv
│
├── ecommerce_analysis.sql
├── ecommerce_analysis.ipynb
├── ab_test_real_users.ipynb
│
├── README.md
└── README_CN.md
```

---

## 十七、项目能力展示（Skills Demonstrated）

通过本项目展示以下数据分析及商业分析能力：

**SQL & Data Analytics**
- SQL Analytics
- Data Cleaning & Transformation
- Funnel Analysis
- User Behavior Analysis

**Business Analytics**
- Customer Segmentation
- Cohort Retention Analysis
- Conversion Optimization
- Business Impact Estimation

**Statistics & Experimentation**
- A/B Testing
- Hypothesis Testing
- Confidence Interval Estimation
- Statistical Significance Analysis

**Business Intelligence**
- Power BI Dashboard Development
- Business KPI Monitoring
- Data Visualization
- Analytical Reporting

---

## 十八、项目范围与说明（Scope & Limitations）

- **数据来源：** Kaggle E-Commerce Behavior Dataset，项目使用的分析样本为3,000,000条用户交互记录。
- **分析范围：** 基于项目所使用的 October 用户活动数据。
- **漏斗统计：** View、Cart、Purchase 的事件数量是事件记录数量，不应直接解释为严格按顺序完成各阶段的唯一用户数量。
- **用户分群：** 根据项目中使用的购买行为和消费贡献进行分组。
- **实验性质：** 项目建立优惠券提醒策略的 A/B Testing 分析框架，实验结果应结合实际分组和数据生成过程解释。
- **统计验证：** 检验结果及置信区间以项目现有分析记录为准。
- **商业估算：** Additional Buyers 和 Incremental GMV 属于推广情景估算，不代表真实部署后的已实现收益。
- **因果解释：** 统计显著性不应脱离实验设计、样本分配及数据质量单独解释。

本项目为独立的数据分析作品集项目，旨在展示电商用户行为分析、转化优化、实验评估及商业决策支持能力。

---

**Built with SQL, Python, Pandas, SciPy, StatsModels and Power BI.**
