# Tech Signal - News Aggregator (Week 06 In-Class Activity)

**Workflow name:** Tech Signal - News Aggregator
**Source:** TechCrunch AI RSS
**Feed URL:** https://techcrunch.com/category/artificial-intelligence/feed/
**Topic:** AI

## Source note
TechCrunch AI RSS was retained. I did not swap the feed.

## Nodes in run order
1. When clicking 'Execute workflow' (Manual Trigger): starts the run by hand.
2. RSS Read: pulls the TechCrunch AI feed (19 items on the last run).
3. Limit: keeps the first 10 items.
4. Filter: keeps items whose title or content mentions "AI" or whose title mentions "agent" (case-insensitive, OR logic); 8 items kept.
5. Edit Fields (Manual Mapping): outputs exactly seven fields: item_url, title, published_at, source, topic, summary, collected_at.
6. Upsert row(s) (Data Table): writes to the tech_signal_items table, matching on item_url so repeat runs update existing rows instead of adding duplicates.

## Evidence
- `screenshot_1_successful_execution.jpg`: a successful full workflow execution, all six nodes green.
- `screenshot_2_tech_signal_items_table.jpg`: the completed tech_signal_items table (9 rows, 9 unique item_url values).
- `news_aggregator.json`: workflow exported after the final successful run.

Duplicate check: after the first run the table held 8 rows. On the second run TechCrunch had published one new article, so the table grew to 9 rows (the 8 existing rows were updated, not repeated). A third run left it at 9 rows.

## Reflection
n8n made the plumbing easy. Reading an RSS feed, limiting and filtering items, reshaping fields, and saving rows to a data table took six visual nodes and no custom code. The Upsert node keyed on item_url let me run the workflow three times without creating a single duplicate, and the per-node output view made debugging quick. What I still had to decide myself was the feed, the keyword filter, the field names, and which column identifies a unique story. If this ran every day for a semester, I would change how I store data: the table would grow without limit, so I would add a retention rule that archives or deletes rows older than a few weeks, and I would review the broad AI keyword match, which lets in loosely related stories.
