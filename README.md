# TourWise

## Goal / 目标

TourWise is a bilingual Skill designed to generate timely, accurate, and practical travel itineraries for people who want to travel but do not have the time to research and organize trips themselves.

TourWise 是一个中英文双语 Skill，用于为“有出行打算但没有时间做攻略”的用户生成具有实时性、准确性与实用性的旅行方案。

It behaves like a reliable travel companion: gathering missing constraints, verifying important live information, researching when necessary, and producing itineraries that are flexible, actionable, and easy to follow.

它的行为应像一个可靠的旅行搭子：先补齐关键约束，再核验重要实时信息，并在必要时调研，最终输出可执行、可调整、容易照着走的旅行攻略。

---

## Repository Structure / 仓库结构

```text
tourwise/
│
├── SKILL.md
├── README.md
├── LICENSE
├── .gitignore
│
├── agents/
│   ├── openai.yaml
│   ├── deepseek.yaml
│   └── anthropic.yaml
│
├── examples/
│   ├── tokyo_5d.md
│   ├── osaka_budget.md
│   └── paris_luxury.md
│
└── references/
    ├── output_template.md
    ├── verification_policy.md
    └── constraint_checklist.md
```

The internal Skill identifier is `tourwise`.

The user-facing display name is **TourWise**.

Skill 内部标识符使用 `tourwise`，对外展示名称使用 **TourWise**。

---

## Core Workflow / 核心工作流

TourWise follows a structured travel-planning workflow rather than acting as a generic recommendation prompt.

TourWise 并非泛泛的旅游推荐提示词，而是一套结构化旅行规划流程。

The workflow is:

1. Collect missing trip constraints.
2. Verify important time-sensitive information.
3. Research and extract useful travel data when necessary.
4. Generate practical itineraries.
5. Clearly distinguish verified facts, assumptions, and fallback strategies.

工作流程如下：

1. 收集缺失的出行约束；
2. 核验关键实时信息；
3. 必要时调研并提取旅行数据；
4. 生成可执行攻略；
5. 清晰区分已核验事实、假设与降级策略。

---

## Required User Inputs / 用户输入要求

Before producing a complete itinerary, TourWise checks whether the following information has been provided.

在生成完整攻略前，TourWise 会检查以下信息是否齐全：

### Time / 时间

* Travel dates.

* Length of stay.

* Arrival and departure timing.

* 出行日期；

* 停留时长；

* 到达与离开时间。

### Transportation / 交通

* Origin and destination.

* Acceptable transport modes.

* Special transportation preferences.

* 出发地与目的地；

* 可接受交通方式；

* 特殊交通偏好。

### Accommodation / 住宿

* Accommodation type.

* Preferred area.

* Facility requirements.

* 住宿类型；

* 区域偏好；

* 设施要求。

### Budget / 预算

* Number of travelers.

* Total budget.

* Spending priorities.

* 出行人数；

* 总预算；

* 花费优先级。

If important information is missing, TourWise asks concise follow-up questions before proceeding.

若关键信息缺失，TourWise 会先简短追问，再继续规划。

---

## Verification Policy / 实时核验策略

TourWise verifies information that affects feasibility, cost, or user trust.

TourWise 会核验影响可行性、成本或用户信任的信息。

Examples include:

* Weather forecasts.
* Flights and intercity transportation.
* Hotel availability and pricing ranges.
* Attraction closures or reservation requirements.
* Restaurant opening status.

例如：

* 天气情况；
* 航班及跨城交通；
* 酒店与住宿价格区间；
* 景点开放状态与预约要求；
* 餐厅营业状态。

If real-time information cannot be verified, TourWise falls back to general strategies, areas, time windows, or decision frameworks instead of fabricating facts.

若实时信息无法核验，TourWise 会降级为区域建议、时间窗、筛选条件或决策策略，而不会编造事实。

---

## Output Structure / 输出结构

TourWise produces itineraries using the following structure:

TourWise 输出采用以下结构：

1. Trip Brief
2. Verification Status
3. One-Page Overview
4. Daily Itinerary
5. Budget Breakdown
6. Pre-Departure Checklist

其中每日行程包含：

* Morning / Afternoon / Evening plans；
* Transport suggestions；
* Restaurants and attractions；
* Estimated travel times；
* Daily budgets；
* Replacement options.

每日行程包含：

* 上午／下午／晚间安排；
* 交通建议；
* 餐厅与景点推荐；
* 预计耗时；
* 每日预算；
* 替代方案。

Google Maps links should appear inline beside the relevant item whenever applicable.

Google Maps 链接应直接附在对应项目旁边，而不是单独列出。

---

## Multi-Provider Agent Support / 多模型支持

TourWise supports multiple LLM providers through interchangeable agent configurations.

TourWise 支持多模型配置，可根据需要切换不同提供商。

Current providers include:

* OpenAI
* DeepSeek
* Anthropic

对应配置文件：

```text
agents/openai.yaml
agents/deepseek.yaml
agents/anthropic.yaml
```

Each configuration should maintain consistent TourWise behavior and output standards.

不同模型配置应保持一致的 TourWise 行为规范与输出质量。

---

## Examples / 示例

The repository includes example itineraries demonstrating expected behavior.

仓库包含多个示例，用于展示预期输出效果。

* `examples/tokyo_5d.md`
* `examples/osaka_budget.md`
* `examples/paris_luxury.md`

Examples focus on workflow and structure rather than preserving real-time facts.

示例重点展示流程与结构，而非保证其中实时信息长期有效。

---

## References / 参考文件

TourWise relies on reference documents to maintain consistent behavior.

TourWise 使用参考文件保持行为一致性。

* `references/constraint_checklist.md`
* `references/verification_policy.md`
* `references/output_template.md`

These references define intake requirements, verification standards, and output formatting expectations.

这些参考文件分别定义输入检查、实时核验标准以及最终输出模板。

---

## Guardrails / 边界规则

TourWise must not:

TourWise 不得：

* Fabricate flights, hotels, prices, schedules, or opening hours.

* Hide failed verification.

* Present assumptions as verified facts.

* 编造航班、酒店、价格、营业时间等信息；

* 隐瞒核验失败；

* 将假设包装成事实。

TourWise should:

TourWise 应：

* Prefer source-backed recommendations.

* State uncertainty clearly.

* Remain concise and practical.

* 优先采用有依据的推荐；

* 清晰说明不确定性；

* 保持简洁实用。

---

## License

This repository is released under the MIT License.

本仓库采用 MIT License 开源。
