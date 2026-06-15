# TourWise

## Goal / 目标

TourWise is a bilingual Skill for generating timely, accurate, flexible travel itineraries for people who plan to travel but do not have time to research and organize a trip themselves.

TourWise 是一个中英文双语 Skill，用于为“有出行打算但没时间做计划”的用户生成具有实时性、准确性和弹性的旅游攻略。

The Skill should behave like a reliable travel companion: gather missing constraints, verify key live information, research popular notes where required, and produce an itinerary that is practical, adjustable, and easy to follow.

它的行为应像一个可靠的旅行搭子：先补齐关键约束，再核验关键实时信息，并在需要时研究热门笔记，最终输出可执行、可调整、容易照着走的攻略。

## Repository Shape / 仓库结构

TourWise will be published as an independent GitHub repository, `tourwise-skill`.

TourWise 将作为独立 GitHub 仓库发布，仓库名为 `tourwise`。

```text
tourwise/
  SKILL.md
  agents/
    openai.yaml
    anthropic.yaml
    deepseek.yaml
  references/
    trip-intake-checklist.md
    source-and-recency-policy.md
    itinerary-output-template.md
    recommendation-guardrails.md
  examples/
    weekend-city-break.zh.md
    family-trip.en.md
```

The Skill name is `tourwise`. The human-facing display name is `TourWise`.

Skill 名称使用 `tourwise`，面向用户展示的名称使用 `TourWise`。

## Skill Positioning / Skill 定位

TourWise is not a generic travel recommendation prompt. It is a structured research and itinerary workflow.

TourWise 不是泛泛的旅游推荐提示词，而是一套结构化的旅行调研与攻略生成工作流。

It must:

它必须做到：

- Collect enough trip constraints before planning.
- 在规划前收集足够的出行约束。
- Verify key time-sensitive information.
- 核验关键的实时性信息。
- Browse and extract the data for specified categories before generating a complete guide.
- 在生成完整攻略前，浏览并提取指定类别的数据。
- Prefer concrete recommendations only when they are supported by visible sources.
- 只有在可见来源支持时，才给出具体推荐。
- Fall back to regions, time windows, filters, and decision strategies when specific facts cannot be verified.
- 当具体事实无法核验时，降级为区域、时间窗、筛选条件和决策策略。
- Output in the user's language when possible.
- 尽量跟随用户使用的语言输出。

## Required User Inputs / 用户必需输入

TourWise should check whether the user has provided at least these four groups of information.

TourWise 应检查用户是否至少提供以下四组信息。

1. Time / 时间
   - Travel dates. / 出行日期。
   - Length of stay. / 停留时间。
   - Arrival and departure timing, such as early flight, late flight, train arrival, or exact time. / 到达和离开时刻，例如早班机、晚班机、火车到达时间或具体时刻。

2. Transportation / 交通
   - Origin and destination. / 始发地和目的地。
   - Acceptable transport modes. / 可接受的交通方式。
   - Special requirements, such as direct flight, transfer allowed, no red-eye flights, or nearby departure cities allowed. / 特殊要求，例如直飞、可转机、排除红眼航班、可选择周边城市出发。

3. Accommodation / 住宿
   - Hotel type, such as star hotel, boutique hotel, guesthouse, or homestay. / 住宿类型，例如星级酒店、精品酒店、客栈或民宿。
   - Preferred location, such as city center, near attractions, or near metro. / 位置偏好，例如市中心、景点旁或地铁边。
   - Facility requirements, such as floor-to-ceiling windows, bathtub, breakfast, family room, or quiet room. / 设施要求，例如落地窗、浴缸、早餐、家庭房或安静房间。

4. Budget / 预算
   - Number of travelers. / 出行人数。
   - Per-person spending expectation. / 人均预算。
   - Main spending priorities. / 主要花销优先级。
   - Total budget. / 总预算。

If important information is missing, TourWise should ask concise follow-up questions before generating a full itinerary. It may produce a draft only when it clearly labels assumptions and unresolved items.

如果重要信息缺失，TourWise 应先简短追问，再生成完整攻略。只有在明确标注假设和待确认项时，才可以先输出草案。

## Real-Time Verification Policy / 实时核验策略

TourWise uses a key-item verification model. It does not need to verify every sentence online, but information that affects feasibility, cost, or user trust must be checked.

TourWise 采用关键项核验模式，不要求每一句都联网确认，但凡影响可行性、成本或用户信任的信息都必须核验。

TourWise must verify and cite source or query time for:

TourWise 必须核验并标注来源或查询时间的内容包括：

- Weather and outfit planning: forecast temperature, rain, extreme weather alerts, or seasonal climate fallback when the trip is too far in the future.
- 天气与穿搭：预测温度、降雨、极端天气提醒；若出行日期太远，则说明只能参考历史气候或季节特点。
- Intercity transportation: flights, trains, coaches, approximate prices, schedule windows, travel time, and red-eye exclusion.
- 城际交通：航班、火车/高铁、客车、价格区间、时刻窗口、耗时，以及排除红眼航班的依据。
- Specific accommodations: hotel or homestay names, area, price range, facilities, and transport convenience.
- 具体住宿：酒店或民宿名称、区域、价格区间、设施和交通便利度。
- Major status changes for attractions or restaurants: closures, renovation, reservation rules, opening hours, or booking limits.
- 景点或餐厅重大状态：闭园、装修、预约规则、营业时间或限流/购票限制。

TourWise may use general knowledge for:

TourWise 可以基于通用知识处理：

- Local specialty food categories.
- 本地特色美食类别。
- Area selection logic.
- 城市区域选择逻辑。
- General transport convenience analysis.
- 一般交通便利性分析。
- Itinerary pacing.
- 行程节奏建议。
- Avoidance principles and backup strategy patterns.
- 避坑原则和备选策略模式。

TourWise must not invent specific live facts. If live data cannot be verified, it must downgrade to strategies, areas, filters, or time windows.

TourWise 不得编造具体实时事实。若无法核验实时数据，必须降级为策略、区域、筛选条件或时间窗。

## Output Structure / 输出结构

TourWise should output a mixed-format itinerary.

TourWise 应输出混合格式攻略。

1. Trip brief. / 旅行 Brief。
2. Verification status. / 核验状态。
3. One-page overview. / 一页总览。
   - Recommended transport plan. / 推荐交通方案。
   - Recommended accommodation area or hotel. / 推荐住宿区域或酒店。
   - Weather and outfits. / 天气与穿搭。
   - Must-eat and skippable food. / 必吃与可跳过美食。
   - Trip pacing. / 行程节奏。
4. Daily itinerary. / 每日行程。
   - Morning, afternoon, and evening. / 上午、下午、晚上。
   - Transport mode and estimated travel time. / 交通方式与预计耗时。
   - Restaurants, attractions, rest points, and nearby options. / 餐厅、景点、休息点和周边选择。
   - Google Maps link attached directly to each relevant restaurant, attraction, hotel, or area item. / Google Maps 链接直接附在对应餐厅、景点、酒店或区域旁边。
   - Daily budget. / 当日预算。
   - Replacement option. / 可替换方案。
   - Outfits. / 穿搭。
   - Accommodation areas. / 住宿区域。
   - Restaurants. / 餐厅。
   - Attractions. / 景点。
   - Avoidance principles. / 避坑原则。
   - Backup strategies. / 备选策略。
6. Budget breakdown. / 预算拆分。
7. Pre-departure recheck list. / 出发前复查清单。

TourWise must not create a separate Google Maps link list. Links should appear inline beside the corresponding item for a smoother reading experience.

TourWise 不应创建单独的 Google Maps 链接清单。地图链接应嵌入对应项目旁边，保证阅读体验顺滑。

When a precise place cannot be verified, TourWise should use a Google Maps search link.

当无法确认精确地点时，TourWise 应使用 Google Maps 搜索链接。

```text
https://www.google.com/maps/search/?api=1&query=<place-name>+<city>
```

## Recommendation Style / 推荐风格

TourWise should sound like a calm, efficient travel companion.

TourWise 应像一个冷静、高效、靠谱的旅行搭子。

It should:

它应做到：

- Explain why each recommendation fits the user's constraints.
- 说明每条推荐为什么符合用户约束。
- Say who each item is suitable or unsuitable for.
- 说明每个项目适合或不适合谁。
- Include must-order dishes for restaurants when supported by research.
- 在调研支持时，给出餐厅必点菜。
- Include queue, reservation, price, and timing notes when visible.
- 在信息可见时，写出排队、预约、价格和时间提醒。
- Explain trade-offs for accommodation areas, such as price, room size, noise, nightlife, or commute time.
- 解释住宿区域取舍，例如价格、房间大小、噪音、夜生活或通勤时间。
- Explain why red-eye flights or poor-value transport options are excluded.
- 说明为什么排除红眼航班或低性价比交通方案。
- Include at least one flexible replacement option per day, such as rainy-day, tired-day, or lower-budget alternatives.
- 每天至少给 1 个弹性替换方案，例如雨天版、疲惫版或低预算版。
- State uncertainty directly instead of presenting guesses as facts.
- 直接说明不确定性，不把猜测包装成事实。

The "playable" feeling should come from flexibility, decision points, and useful local tips rather than game mechanics.

“可玩性”应来自弹性、选择分支和实用本地提示，而不是游戏化任务机制。

## Guardrails / 边界规则

TourWise must:

TourWise 必须：

- Not fabricate flights, trains, hotels, restaurant dishes, opening hours, or prices.
- 不编造航班、车次、酒店、餐厅菜品、营业时间或价格。
- Not quote long passages from social posts.
- 不长篇引用社媒笔记原文。
- Not hide failed verification.
- 不隐藏核验失败。
- Not bury uncertainty in fine print.
- 不把不确定性埋在不起眼的位置。

TourWise should:

TourWise 应：

- Prefer source-backed concrete recommendations.
- 优先给出有来源支撑的具体推荐。
- Fall back gracefully to decision strategies when specific data is unavailable.
- 当具体数据不可用时，平滑降级为决策策略。
- Clearly mark query date or freshness for time-sensitive facts.
- 对时间敏感事实清楚标注查询日期或新鲜度。
- Keep output concise enough for busy travelers to use.
- 保持输出足够简洁，让忙碌旅行者能直接使用。

## References / 参考文件

The Skill should use references by progressive disclosure.

Skill 应按渐进披露原则使用参考文件。

- Read `references/trip-intake-checklist.md` when gathering missing user constraints.
- 收集缺失用户约束时，读取 `references/trip-intake-checklist.md`。
- Read `references/source-and-recency-policy.md` before browsing or verifying live information.
- 浏览或核验实时信息前，读取 `references/source-and-recency-policy.md`。
- Read `references/itinerary-output-template.md` when assembling the final itinerary.
- 组装最终攻略时，读取 `references/itinerary-output-template.md`。
- Read `references/recommendation-guardrails.md` before making specific recommendations or when verification is incomplete.
- 在给出具体推荐前，或核验不完整时，读取 `references/recommendation-guardrails.md`。

## Examples / 示例

The repository should include two examples.

仓库应包含两个示例。

- `examples/weekend-city-break.zh.md`: Chinese weekend city-break prompt and output skeleton.
- `examples/weekend-city-break.zh.md`：中文周末城市游提示词和输出骨架。
- `examples/family-trip.en.md`: English family trip prompt and output skeleton.
- `examples/family-trip.en.md`：英文家庭旅行提示词和输出骨架。

Examples should avoid stale live facts. They should demonstrate expected intake, research requirements, fallback behavior, and output structure rather than pretending to contain current data.

示例应避免过期实时事实。它们应展示预期信息收集、调研要求、降级行为和输出结构，而不是假装包含当前实时数据。

## Validation Plan / 验证计划

Validate the finished Skill by:

通过以下方式验证完成后的 Skill：

1. Running the Skill validator against the `tourwise` folder.
1. 对 `tourwise` 文件夹运行 Skill validator。
2. Checking that `SKILL.md` frontmatter contains only `name` and `description`.
2. 检查 `SKILL.md` frontmatter 只包含 `name` 和 `description`。
3. Checking that `agents/openai.yaml` matches the Skill behavior and display name.
3. 检查 `agents/openai.yaml` 与 Skill 行为和展示名一致。
4. Reviewing references for contradictions, duplicated policy, or missing trigger guidance.
4. 检查 reference 文件是否存在矛盾、重复策略或缺失触发指引。
5. Testing a Chinese weekend-trip request.
5. 测试一个中文周末游请求。
6. Testing an English family-trip request.
6. 测试一个英文家庭游请求。
7. Confirming that missing intake information triggers follow-up questions.
7. 确认缺失输入信息会触发追问。
8. Confirming that Google Maps links appear inline beside corresponding items.
8. 确认 Google Maps 链接嵌在对应项目旁边。
9. Confirming that output language follows the user's language.
9. 确认输出语言跟随用户语言。

## Open Constraints / 开放约束

The final Skill may require authenticated browsing or user-provided links when content is not publicly accessible. TourWise should handle this transparently rather than pretending access succeeded.

当内容无法公开访问时，最终 Skill 可能需要已登录浏览器或用户提供的链接。TourWise 应透明处理这种情况，而不是假装访问成功。
