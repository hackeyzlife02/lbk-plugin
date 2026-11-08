---
description: Summarize your Lutz Bath & Kitchen quotes, with what's open, what needs your approval and what expires soon
argument-hint: "[job name, optional]"
---

Using the `lbk` connector, summarize my quotes. If I named a job ("$ARGUMENTS"), cover just that job.

Call `get_my_account` first if you haven't in this conversation, and follow its `guidance`. Then
use `list_jobs` and `list_quotes`, and group the quotes by job: number, title, status, total and
valid-until date. Flag anything that expires in the next 14 days. For open quotes, use `get_quote`
and list the items that still need my approval.
