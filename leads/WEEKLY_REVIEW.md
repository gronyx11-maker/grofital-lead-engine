# Weekly Pipeline Review (about 20 minutes, every Friday)

## 1. Numbers (fill in each week)
| Metric | This week | Last week |
|---|---|---|
| Leads added | | |
| Leads scoring 8+ | | |
| Drafts sent | | |
| Replies | | |
| Calls booked | | |
| Proposals sent | | |
| Won | | |

## 2. Lead statuses (use these in leads.csv)
New → Check manually → Contacted → Follow-up 1 → Follow-up 2 → Replied → Meeting → Proposal → Won / Lost / Dropped

## 3. Review steps
1. Open leads.csv and sort by date_added. Update the status of every lead you contacted.
2. Anyone Contacted 3+ days ago and not replied: send Follow-up 1.
3. Anyone Follow-up 1 sent 4+ days ago and not replied: send Follow-up 2, then stop.
4. Mark leads that opted out as Dropped, and never contact them again.
5. Check three source links from the week. If more than one was wrong, tighten the rules in prompts/lead_finder.md.

## 4. Which region and trigger perform best?
| Region | Leads | Replies | Reply rate |
|---|---|---|---|
| US | | | |
| Europe | | | |
| UAE | | | |
| Australia | | | |

| Trigger type | Leads | Replies |
|---|---|---|
| Hiring post | | |
| Funding | | |
| Public request | | |
| Leadership change | | |

If one region or trigger clearly wins for 3-4 weeks, update the Friday rotation in config/targets.md to favour it.

## 5. Prompt tuning notes
- What was wrong or weak in this week's leads (wrong size, stale triggers, bad contacts)?
- What change will you make to prompts/lead_finder.md or CLAUDE.md?
- Date the change was made:

## 6. Next week's focus
- Priority region/industry:
- Messaging angle to test:
- Target calls booked:
