---
name: task-router
description: Identifies the task type and directs the orchestrator to the correct decompose skill.
---

## How to Use
1. Read the user's query carefully.
2. Determine if it's a **short-answer research question** or a **table/list generation task**.
3. Match it against the task types below (check short-answer types FIRST).
4. Call `read_orchestrator_skill("decompose-<matched_type>")` to load the specialized strategy.

## Short-Answer Research Questions
Check these first for queries expecting a concise answer (number, name, date, phrase):

### linear-multi-hop-dependency
**Match when:** Finding the answer requires a CHAIN: find A → use A to find B → use B to find answer. Each step depends on the previous.
**Load skill:** `decompose-linear-multi-hop-dependency`
**Key signals:** Nested possessives ("X的Y的Z"), "在某事件发生的那天/那年", entity defined through another entity.
**Example:** "全球最大连锁酒店第一家酒店成立当年中国公演的歌剧原著作者安葬地所在区面积"

### constrained-set-search
**Match when:** Finding ONE entity that satisfies 3+ independent conditions. The clues are parallel, not sequential.
**Load skill:** `decompose-constrained-set-search`
**Key signals:** Multiple "且/并且" conditions, riddle format ("有一个X...同时...还..."), bullet-listed clues.
**Example:** "一位艺人从韩国练习生归来，2024年9月新剧开播，收入占公司1/5"

### comparative-data-extraction
**Match when:** Look up 2+ specific data points and compute a result (difference, ratio, CAGR).
**Load skill:** `decompose-comparative-data-extraction`
**Key signals:** "差值/差额", "增长率", "是...的几倍", "比...多/少多少", specific dates/sources.
**Example:** "计算美的集团2016-2019年归母净利润CAGR"

### multimedia-source-verification
**Match when:** The answer is in a specific social media post, video, or platform content.
**Load skill:** `decompose-multimedia-source-verification`
**Key signals:** Platform names (抖音/B站/小红书/微博), video/post references, "某期视频", uploader names.
**Example:** "小红书博主X置顶的关于挪威视频中part2的旅行地点"

## Table/List Generation Tasks
For queries expecting structured tabular output with multiple rows:

### split-by-rank-segment
**Match when:** The query asks for a specific "Top N" list or a numbered ranking (e.g., "Top 50 movies," "100 best-selling albums"). The request relies on a pre-existing ordinal sequence.
**Load skill:** `decompose-split-by-rank-segment`
**Key signal:** Presence of ordinal numbers, "Top [X]," or "Ranked" phrasing.

### split-by-time-period
**Match when:** The query specifies a continuous chronological range or a multi-year history (e.g., "from 2010 to 2024," "all releases in the 1990s").
**Load skill:** `decompose-split-by-time-period`
**Key signal:** Date ranges, decades, or "year-by-year" requirements.

### split-by-entity
**Match when:** The query lists specific, discrete subjects like brand names, individual people, or specific product models that require deep attribute extraction (e.g., "Nikon Z6, Sony A7IV, and Canon R6").
**Load skill:** `decompose-split-by-entity`
**Key signal:** Proper nouns of specific products, companies, or individuals.

### split-by-category
**Match when:** The query is organized by broad domain classifications, geographic regions, or institutional departments (e.g., "by country," "academic subjects," or "sports leagues").
**Load skill:** `decompose-split-by-category`
**Key signal:** Use of "by [Category Name]" or lists of distinct sectors/regions.

### annual-rank-stats
**Match when:** The query asks for annual statistics, yearly rankings, or season-by-season performance data (e.g., "annual GDP rankings," "yearly box office leaders," "season stats for each year").
**Load skill:** `decompose-annual-rank-stats`
**Key signal:** "annual," "yearly," "per season," combined with rankings or statistics.

### entity-benchmarking
**Match when:** The query requires collecting multi-attribute specifications or benchmark data across a defined set of entities (e.g., "compare specs of these 10 laptops," "benchmark all models in this product line").
**Load skill:** `decompose-entity-benchmarking`
**Key signal:** Spec sheets, benchmark comparisons, multi-attribute tables for known entities.

### geographic-registries
**Match when:** The query involves location-based registries, inventories, or catalogs organized by geographic boundaries (e.g., "all UNESCO sites by country," "hospitals in each province," "national parks by state").
**Load skill:** `decompose-geographic-registries`
**Key signal:** Geographic partitioning, "by country/state/region," registry or inventory language.

### temporal-event-logs
**Match when:** The query asks for a chronological log of discrete events, incidents, or occurrences (e.g., "all earthquakes above magnitude 6 since 2000," "product recall history," "timeline of policy changes").
**Load skill:** `decompose-temporal-event-logs`
**Key signal:** Event logs, incident histories, timelines of discrete occurrences (not continuous product releases — use split-by-time-period for those).

## Default Fallback
If no type matches clearly:
- For **short-answer questions**: use `decompose-comparative-data-extraction`
- For **table/list tasks**: split by entity or category, keep each worker under 30 rows