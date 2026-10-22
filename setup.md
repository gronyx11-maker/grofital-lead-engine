# Grofital Lead Engine: Setup Reference

Company: Grofital Consulting LLP (grofital.com)
Goal: An automated cloud routine that finds qualified leads (US, Europe, UAE, Australia) for CFO/fractional CFO and data analysis services and prepares outreach drafts, even when the PC is off.
Written: 22 Oct 2026. Claude Routines are a research preview, so details may change. Check code.claude.com/docs for current behaviour.

---

## 1. How it works

- A **Routine** is a saved prompt + GitHub repo + environment + triggers that runs on Anthropic's cloud on a schedule, even when your computer is off.
- Create/manage at claude.ai/code/routines (or `/schedule` in the CLI, or Desktop app: Code tab → Routines → New routine → **Cloud**).
- Available on Pro, Max, Team, Enterprise. Needs a claude.ai subscription login (not an API key).
- Desktop "Local" scheduled tasks only run while the PC is on and awake. Do not use Local.
- Each run clones the repo's default branch, works, and pushes to `claude/`-prefixed branches.
- Runs are autonomous: no permission prompts, so the prompt must be self-contained.

---

## 2. Repo structure

```
grofital-lead-engine/
├── CLAUDE.md
├── config/targets.md
├── prompts/lead_finder.md
├── leads/leads.csv
└── outreach/drafts/   (.gitkeep inside)
```

### CLAUDE.md

```
# Grofital Lead Engine
You are the lead-research agent for Grofital Consulting LLP (grofital.com).
Every run is unattended. Never ask questions; use config/targets.md.
Follow prompts/lead_finder.md exactly.

Rules:
- Use public business information only. Cite a source URL for every lead.
- Never invent companies, people, emails, or phone numbers. Write "Unverified" if unsure.
- Before adding a lead, check leads/leads.csv and skip duplicates (match on website domain).
- Append new leads to leads/leads.csv.
- Save outreach as text files in outreach/drafts/ (YYYY-MM-DD_company.md). Never send anything.
- Finish by writing a short run summary to leads/RUNLOG.md (date, region, leads added, issues).
```

### config/targets.md

```
Rotation by weekday (use the current day):
- Monday: United States
- Tuesday: Europe (UK, Germany, Netherlands, France, Ireland, Nordics, Spain, Italy)
- Wednesday: UAE (Dubai, Abu Dhabi)
- Thursday: Australia
- Friday: Best-performing region from leads.csv, or the US if none
Industries: SaaS/tech, e-commerce/D2C, healthcare, logistics, manufacturing, real estate, fintech, professional services, startups
Leads per run: 15
Pitch: both (fractional CFO and data analysis)
```

### leads/leads.csv (header row only)

```
date_added,region,company,website,industry,size,trigger,source_url,contact_name,contact_title,profile_url,contact_route,score,pain_points,service_to_pitch,status
```

### prompts/lead_finder.md

```
ROLE
You are a senior B2B lead-research and market-intelligence analyst working for Grofital Consulting LLP (grofital.com), a data analysis and fractional/outsourced CFO advisory firm. Your job is to find high-intent, qualified prospects and give me the intelligence needed to approach them.

ABOUT GROFITAL
- Services: data analysis and BI (dashboards, KPI frameworks, forecasting), financial analytics, outsourced/fractional CFO services (budgeting, cash-flow management, financial modelling, fundraising support, board reporting, M&A due diligence support).
- Target regions: United States, Europe (UK, Germany, Netherlands, France, Ireland, Nordics, Spain, Italy), UAE (Dubai, Abu Dhabi), and Australia.

IDEAL CLIENT PROFILE
- Company size: 10-500 employees, or revenue USD 1M-100M
- Industries: SaaS/tech, e-commerce/D2C, healthcare, logistics, manufacturing, real estate, fintech, professional services, startups (seed to Series B), PE-backed SMEs
- Buying triggers (high intent):
  1. Public job posts for CFO, Finance Director, VP Finance, Head of FP&A, Financial Controller, Data Analyst, BI Analyst, or Analytics Manager (these companies may prefer outsourcing)
  2. Founders or CEOs publicly asking for a fractional CFO, financial modelling help, or data/BI support
  3. Recent funding rounds (last 12 months) with no visible finance leader
  4. Recent CFO departure or leadership change
  5. Rapid growth, expansion, M&A, or IPO preparation
  6. Posts in startup, finance, and founder communities asking for recommendations for CFO or analytics services

TASK
For the region specified, find the configured number of qualified leads using only publicly available, legitimate sources: company websites, public job boards, LinkedIn public posts, Crunchbase or news funding announcements, Clutch/GoodFirms-style directories, public startup/founder communities, and government business registries.

For EACH lead provide:
1. Company name, website, industry, location, approximate size
2. Why they are a fit (the specific trigger and a source link)
3. Decision-maker name and title, with the public profile link
4. Business contact route (public business email format or contact page, never guessed personal data)
5. Lead score (1-10) with a one-line justification
6. Likely pain points (based on the trigger and industry)
7. Recommended Grofital service to pitch
8. A personalised opening line referencing something specific and public about them
9. Suggested outreach channel (LinkedIn, email, or contact form) and best timing in their time zone

OUTPUT FORMAT
- A table sorted by lead score, highest first
- Then an outreach pack for the top 5 leads: a cold email (under 120 words), a LinkedIn connection note (under 300 characters), and two follow-ups spaced 3 and 7 days apart
- Finish with a region summary: market size signals, common pain points, and recommended messaging angle

RULES AND QUALITY CONTROL
- Use only verifiable, public information. Cite a source link for every claim. If you cannot verify something, write "Unverified" instead of guessing.
- Never fabricate companies, people, emails, or phone numbers.
- Never scrape or use private, hacked, leaked, or purchased personal databases.
- Do not collect personal/sensitive data beyond what is needed for business contact (name, title, business contact route).
- Respect regional rules: GDPR/ePrivacy (Europe, UK), CAN-SPAM (US), the Spam Act 2003 (Australia), and UAE PDPL. Note the compliant outreach approach for each region (for example, legitimate-interest basis and opt-out line for Europe).
- Exclude competitors, companies already served by large audit/consulting firms, and companies with fewer than 5 employees.
- Remove duplicates and flag any lead you are less than 70% confident about.
- Do not request or use any credentials, passwords, API keys, or confidential data.

SCHEDULED-RUN MODE
- Do not ask questions. Use the region, industry, lead count, and pitch from config/targets.md.
- Find only NEW leads not already listed in leads/leads.csv. Append them.
- Save outreach as DRAFT files only. Never send any email or message.
- Use public business information only.
- End with a short run summary: leads added, sources used, anything unverified.
```

---

## 3. Commit to GitHub (Codespaces)

Terminal (Ctrl + `):

```bash
touch outreach/drafts/.gitkeep
git status
git add .
git commit -m "Add Grofital lead engine setup"
git push origin main
```

Or use the Source Control panel (Ctrl+Shift+G): stage with **+**, type a message, **Commit**, then **Sync Changes**.

Verify on github.com that all files are on the default branch (Settings → General → Default branch). If your branch is `master`, push to `master`.

---

## 4. Create the routine

1. Authorise GitHub access for Claude (from the routine form, or `/web-setup` in the CLI). If the connection lapses, runs are skipped for up to 72 hours, then the routine turns off.
2. Go to claude.ai/code/routines → **New routine**.
3. **Name:** Grofital Daily Lead Finder
4. **Prompt:**

```
Read CLAUDE.md, config/targets.md, and prompts/lead_finder.md in this repo and follow them exactly.
Determine today's target region from the weekday rotation in config/targets.md.
Research and add up to the configured number of NEW, verified leads to leads/leads.csv,
write outreach drafts to outreach/drafts/, update leads/RUNLOG.md, then commit and push.
Do not send any email or message. Do not ask questions.
```

5. **Repository:** grofital-lead-engine
6. **Environment:** set Network access to **Full** (see section 5)
7. **Trigger:** Schedule → Weekdays, at a time such as 6:00 AM (local time is converted automatically; runs may start a few minutes late; minimum interval is 1 hour)
8. **Connectors:** remove all of them. Included connectors can perform write actions without asking permission during a run.
9. Click **Create**.

Desktop app alternative: Code tab → Routines → New routine → choose **Cloud** (not Local).

---

## 5. What "Network access: Full" means

Each run happens in a temporary cloud environment. Network access controls which websites it can reach.

| Level | Allows |
|---|---|
| Trusted (default) | Only a short approved list (package registries, cloud APIs, dev tools) |
| Custom | Only the domains you list (optionally plus the default list) |
| Full | Any website |

Lead research needs company sites, job boards, and news pages, which are not on the default list. On Trusted, requests fail with a 403 (`host_not_allowed`).

To change it: routine form → click the cloud icon below the Instructions box (says "Default") → hover over Default → click the gear icon → set **Network access** to **Full** → **Save changes**. It applies from the next run.

---

## 6. Test and verify

1. Open the routine → **Run now**.
2. Open the run and read the full transcript. A green status only means the session ran without an infrastructure error, not that the task worked.
3. On GitHub, check for a new `claude/...` branch with updated `leads.csv`, `outreach/drafts/`, and `leads/RUNLOG.md`.
4. Improve results by editing `CLAUDE.md` or the prompt files, pushing, and running again.

### Make duplicate-checking work across days

Each run starts from the default branch, so yesterday's leads are only visible if they are on `main`. Either:
- merge each `claude/` branch/PR into `main` daily, or
- tell the prompt to push directly to `main` (works only if `main` is not protected; test with one run).

---

## 7. Managing the routine

- **Run now:** start immediately, optionally with extra text.
- **On/off switch:** pause or resume the schedule.
- **Edit menu:** change the prompt, repo, environment, connectors, or triggers.
- **CLI:** `/schedule list`, `/schedule update`, `/schedule run`.
- **Limits:** routines count toward your subscription usage and a daily run cap. Check claude.ai/code/routines or claude.ai/settings/usage. One routine with weekday rotation uses one run per day. Four separate routines use four.

---

## 8. Troubleshooting

| Problem | Fix |
|---|---|
| Repo not listed | Reconnect GitHub from the routine form, or run `/web-setup` |
| 403 / blocked requests in the transcript | Set Network access to Full |
| `/schedule` says "Unknown command" | You may be logged in with an API key or inside a cloud session. Use the web UI at claude.ai/code/routines |
| Routines disabled by organisation policy | A Team/Enterprise Owner must enable Routines in admin settings |
| Push rejected | Protected branch, or branch has someone else's PR or commits. Use a `claude/` branch |
| Same leads repeat | Merge previous runs' branches into `main` |

---

## 9. Working practices

- **Drafts only:** the routine writes outreach drafts as files. You review and send from your own email. AI can produce wrong contacts or invented details, so verify the source links before contacting anyone.
- **Compliance:** follow GDPR/ePrivacy (Europe/UK), CAN-SPAM (US), the Spam Act 2003 (Australia), and UAE PDPL. Include an opt-out line and a legitimate business reason.
- **Refresh targets:** hiring and funding triggers go stale quickly, so keep the schedule running weekly or daily.
