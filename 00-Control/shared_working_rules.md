<!-- Copy of 00-Control/shared_working_rules.md from MASKonsult/repository-index, commit 958f011, version 20261024a. Refreshed from the source; do not edit here. -->

## 13. Shared working rules (from the shared bootstrap) - version 20261024a

Appended to the global instructions in `/etc/claude-code/CLAUDE.md` for every Claude Code account on Obelix. The full shared bootstrap is `shared_bootstrap.md` at the Google Drive root; it governs accounts, Jira assignment, Calendar and Gmail handling, and is read through the Drive connector before using those services. Agents change it only when Matt says to.

**Verification.** A claim about external state needs tool evidence. A write is finished only when it has been written and read back. Never say "done" before that.

**Retries.** A failed connected-service operation is retried: the same operation, recipient, content, arguments and route. The delays between attempts are 10, 10, 20, 30, 50, 80, 130, 210, 340, 550 seconds and so on (Fibonacci times ten), with no fixed attempt count. Every wait is a real blocking wait. After a failed write, read the destination to see whether it landed before resending. After success, read the result back.

**Reporting.** Report what was done and found. No lists of what was not done, no authorisation or deployment disclaimers, no manufactured decisions, blockers or numbered "things to resolve".

**Dictation and capture.** Matt's words are verbatim. Remove only speech fillers and obvious transcription noise. Keep analysis separate from his words. Authorship unknown is labelled unknown. Misheard names and words from voice-to-text are fixed silently (canonical spellings: Sheumack, MASKonsult). When Matt says save, record, park or keep something, write it to a durable location and read it back.

**What may be touched.** Connected services (mail, Drive, Calendar and the rest) are used when Matt asks, not unprompted. Live filesystems, rclone processes and services are not mounted, unmounted, restarted or reconfigured without that exact instruction. Drive is handover material, not canonical. Source material is retired by dated rename or by a move into `99-Archive`, never deleted. No secrets or client data go in Drive.

**Formats and names.** Markdown for prose, JSON for structured data, CSV for tables, YAML only when it clearly beats those. Prose is never hard-wrapped: one paragraph per line, paragraphs separated by one blank line. Dates in names are compact `YYYYMMDD`. Directory numbering follows the repository standard in each repository's `00-Control/standard.md`.

**Geography.** Matt's location, country and time zone are not fixed. Work, services and schedules are not designed on a single-country basis; the relevant jurisdiction and time zone are established per task.
