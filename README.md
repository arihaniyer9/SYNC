# SYNC — WhatsApp Meeting Bot

An intelligent WhatsApp bot that automates meeting scheduling between students and mentors using natural language processing and Google Calendar integration.

Reach out for a guided walkthrough or testing access.

Built by the CreatED team.

## Screenshots

![Admin dashboard — meeting scheduling and group overview](./screenshots/admin-dashboard.png)
![WhatsApp scheduling flow — natural language meeting creation](./screenshots/whatsapp-flow.png)
![Analytics view — reminder status and bot connection health](./screenshots/analytics.png)

## About This Project

Sync was built to remove the manual back-and-forth of scheduling mentor-student meetings across a large, distributed program. Instead of coordinating through separate messages, spreadsheets, and calendar invites, admins and mentors schedule, reschedule, and manage meetings directly through natural language WhatsApp commands, with Google Calendar and reminders handled automatically behind the scenes.

The bot understands one-off and recurring meetings, custom durations and time ranges, cross-timezone scheduling, and multiple meetings scheduled in a single message. It handles role-aware permissions (admin, mentor, student, parent), disambiguates between multiple mentors when needed, and enforces protections so core participants can't be accidentally removed from a meeting. An admin dashboard provides full visibility into users, groups, scheduled meetings, reminder delivery status, and live bot connection health, including a maintenance mode that takes the bot offline instantly across all WhatsApp commands.

CreatED, the team behind this project, was named a finalist in the 17th annual Milken–Penn GSE Education Business Plan Competition (2026), one of the most prestigious education venture competitions globally, recognizing CreatED's broader work empowering high school students to develop research, build projects, and launch startups. [Read the announcement](https://www.gse.upenn.edu/news/penn-gse-and-milken-family-foundation-announce-finalists-prestigious-global-education-venture).

## Architecture

```
┌─────────────┐
│  WhatsApp   │
│   Users     │
└──────┬──────┘
       │
       ▼
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   Baileys   │◄────►│  FastAPI    │◄────►│  Supabase   │
│  (Node.js)  │      │  (Python)   │      │ (Postgres)  │
└─────────────┘      └──────┬──────┘      └─────────────┘
                            │
                            ▼
                     ┌─────────────┐      ┌─────────────┐
                     │   Google    │      │  Sheet Cron │
                     │  Calendar   │      │  (Python)   │
                     └─────────────┘      └─────────────┘
```

## Tech Stack

| Layer | Technology |
|---|---|
| WhatsApp service | Node.js 18, Baileys, Express |
| Backend API | Python 3.11, FastAPI, APScheduler |
| Database | Supabase (PostgreSQL) |
| NLP | Grok AI (xAI) with OpenAI fallback |
| Calendar | Google Calendar API |
| Frontend | React 18, Vite, Tailwind CSS |
| Deployment | Railway (backend/bot/cron), Vercel (frontend) |

## Feature Matrix

### Scheduling (WhatsApp commands)

| Feature | admin | mentor | student | parent |
|---|---|---|---|---|
| Schedule a one-off meeting | ✅ | ✅ | ❌ | ❌ |
| Schedule a recurring meeting | ✅ | ✅ | ❌ | ❌ |
| Schedule with duration (for 2 hours) | ✅ | ✅ | ❌ | ❌ |
| Schedule with time range (2pm–4pm) | ✅ | ✅ | ❌ | ❌ |
| Schedule across timezones (PST/EST/GMT etc.) | ✅ | ✅ | ❌ | ❌ |
| Schedule multiple meetings in one message | ✅ | ✅ | ❌ | ❌ |
| Default admin schedule creates mentor+student meeting | ✅ | ❌ | ❌ | ❌ |
| Admin replaces mentor in meeting (as_mentor=true) | ✅ | ❌ | ❌ | ❌ |
| Admin joins as 3rd participant (multi_meeting=true) | ✅ | ❌ | ❌ | ❌ |
| Pin specific mentor by name when multiple exist | ✅ | ❌ | ❌ | ❌ |
| Disambiguation prompt when multiple mentors, no name given | ✅ | ❌ | ❌ | ❌ |

### Meeting management (WhatsApp commands)

| Feature | admin | mentor | student | parent |
|---|---|---|---|---|
| Reschedule a meeting (reply to original) | ✅ | ✅ | ❌ | ❌ |
| Cancel a specific meeting | ✅ | ✅ | ❌ | ❌ |
| Cancel all meetings | ✅ | ✅ | ❌ | ❌ |
| Add a participant to a meeting (Google Calendar) | ✅ | ✅ | ❌ | ❌ |
| Remove a participant from a meeting | ✅ | ✅ | ❌ | ❌ |
| Cannot remove student / mentor / permanent participants | enforced | enforced | — | — |
| Confirm a "Did you mean…" suggestion by replying "Yes" | ✅ | ✅ | ❌ | ❌ |

### Group & membership management (WhatsApp commands)

| Feature | Requires `can_register` flag |
|---|---|
| Register a new WhatsApp group | ✅ |
| Auto-add default members on group registration (DEFAULT_USERS env) | ✅ |
| Add a user to a group by phone/mention with a role | ✅ |
| Auto-onboard from Google Sheet contacts if user not yet in DB | ✅ |
| Remove a user from a group by phone/mention/name | ✅ |

### Reminders (automated)

| Event | Recipients |
|---|---|
| 24-hour advance reminder | All meeting participants |
| 1-hour advance reminder | All meeting participants |
| Meeting confirmation on creation | All meeting participants |
| Cancellation notification | All meeting participants |
| Reschedule notification | All meeting participants |

### Admin dashboard (web UI)

- Create / edit / delete users
- Assign users to groups with per-group roles (not global)
- Users with 2+ groups collapsed to badge + view modal
- Create / edit / delete groups
- View group members, add / remove / update role inline
- Schedule / reschedule / cancel meetings
- View sent reminder status per meeting
- Override reminders manually
- Analytics & stats
- Bot connection status + live QR refresh
- Maintenance mode toggle — puts bot offline instantly; all WhatsApp commands receive a maintenance message
- Auto-refresh across all views (20–60 s intervals)
- Fully responsive — mobile, tablet, desktop

## Source Code

This project was built as part of CreatED's product suite. Source code is not publicly available, as it is an active company product. **Access can be provided for testing or code review purposes upon request** — reach out if you'd like a live walkthrough or a closer look under NDA.
