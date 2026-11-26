---
status: backlog
---
## Description

A web app that helps people keep track of their online subscriptions so they stop getting charged for ones they forgot. It is for a busy person first, then a budget student and a lazy user, so it must feel quick and calm: the first subscription added within a minute, one main action per screen, plain language, mobile-friendly, and a few well-timed alerts.

The MVP covers four features. **Capture:** add a subscription in under 30 seconds, with a popular-service picker that prefills the name and typical price, then the amount, billing cycle and next debit date. **Display:** a list sorted by next debit date, the monthly total on top, and due-soon subscriptions highlighted. **Alerts:** email reminders 7, 3 and 1 day before a debit, each toggleable, with snooze. **Monthly report:** an email with total spend, upcoming renewals and new subscriptions.

The app is React and TypeScript, built with Vite. Data lives in the Claude artifact database rather than Postgres, in three collections: subscriptions, alert preferences and an alert log. A scheduled daily job sends alerts and a monthly job sends the report, both by email through a connected Gmail account. Alert times respect the user's time zone, and a next debit date rolls forward by its billing cycle once it passes.

That platform is an unverified assumption, so the first commit is a short spike to confirm it.

Out of scope: success metrics, bank or email auto-import, cancellation help, price-rise detection and shared plans.

## Agent instructions

1. Read `AGENTS.md`, this ticket, and `TKT-05` for an example of a finished ticket's shape.
2. **Spike first.** Confirm that a React artifact can read and write the artifact database, that a scheduled job can run daily and monthly and read that same data, and that the job can send an email through the connected Gmail account. Record what works, what doesn't, and any limits in a `## Spike findings` section of this ticket. Propose go or no-go to the person, and wait for their sign-off. On no-go, stop and replan this ticket with them before writing any app code.
3. Set up `subscription-tracker/` with Vite, React, TypeScript, Vitest with React Testing Library, ESLint and Prettier, as commit 2.
4. Propose the data shape for the three collections, and how weekly and yearly cycles convert to the monthly total. Wait for the person's sign-off before writing it.
5. Propose the popular-service list with typical prices. Wait for the person's sign-off before writing it.
6. Propose the time of day alerts go out in the user's time zone, and how long a snooze lasts. Wait for the person's sign-off before writing it.
7. Propose the wording of the alert email and the monthly report email. Wait for the person's sign-off before writing each.
8. Emails only ever go to the user's own address. Use the alert log so that no alert is sent twice for the same subscription, debit date and reminder day.
9. Test the date roll-forward and alert timing with fixed dates and several time zones, including a month-end date such as 31 January, so they're checked without waiting for real emails.
10. Work the plan one row at a time.

## Commit plan

| # | Commit message | What it covers | Status |
| --- | --- | --- | --- |
| 1 | `docs(work): record the platform spike findings (TKT-06)` | The spike's findings and go/no-go decision, written into this ticket | |
| 2 | `chore(tracker): set up vite, vitest and eslint (TKT-06)` | The `subscription-tracker/` skeleton, one passing test, and lint and format scripts | |
| 3 | `feat(tracker): add the subscription model and roll-forward (TKT-06)` | Subscription types, monthly-total maths, and next-debit roll-forward by cycle, with unit tests | |
| 4 | `feat(tracker): store subscriptions in the artifact db (TKT-06)` | Reading and writing the three collections, behind a small interface that tests can fake | |
| 5 | `feat(tracker): add a subscription with the service picker (TKT-06)` | The add screen with the popular-service picker that prefills name and price | |
| 6 | `feat(tracker): list subscriptions with the monthly total (TKT-06)` | The list sorted by next debit, the monthly total on top, and the due-soon highlight | |
| 7 | `feat(tracker): add alert settings with snooze (TKT-06)` | Toggles for the 7, 3 and 1 day reminders, and snooze, saved to alert preferences | |
| 8 | `feat(tracker): send daily alert emails (TKT-06)` | The daily job that picks due reminders in each user's time zone, emails them, and writes the alert log | |
| 9 | `feat(tracker): send the monthly report email (TKT-06)` | The monthly job that emails total spend, upcoming renewals and new subscriptions | |

## Post-commit testing

| # | Check | How | Status |
| --- | --- | --- | --- |
| 1 | The checks pass | Run the tests and the linter in `subscription-tracker/`, and see no failures or warnings | |
| 2 | A first subscription takes under a minute | Open the app on a phone-sized screen, pick a popular service, set the date and save, timing it from first load | |
| 3 | The list and total are right | Add a monthly, a yearly and a weekly subscription, and check the order, the monthly total and the due-soon highlight | |
| 4 | Reminders arrive once, on time | Add a subscription due in 3 days, run the daily job twice, and see one email arrive, at the agreed local time | |
| 5 | Toggles and snooze work | Turn off the 3-day reminder, or snooze it, run the daily job, and see no email | |
| 6 | The monthly report arrives | Run the monthly job, and see one email with the total, upcoming renewals and new subscriptions | |
| 7 | Dates roll forward | Set a next debit date in the past, open the app, and see it moved on by one billing cycle | |

## Acceptance criteria

- [ ] The person signed off the spike's go/no-go
- [ ] The person signed off the data shape and monthly-total conversion
- [ ] The person signed off the popular-service list
- [ ] The person signed off the alert time and snooze length
- [ ] The person signed off the alert and report email wording
- [ ] A subscription can be added in under 30 seconds with the service picker
- [ ] The list is sorted by next debit date, with the monthly total on top and due-soon subscriptions highlighted
- [ ] Reminders go out 7, 3 and 1 day before a debit, each toggleable and snoozable, and never twice
- [ ] A monthly report email shows total spend, upcoming renewals and new subscriptions
- [ ] Alert times follow the user's time zone, and debit dates roll forward by billing cycle
- [ ] The app works on a phone-sized screen
