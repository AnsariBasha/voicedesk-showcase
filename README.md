# VoiceDesk — LiveKit Voice Helpdesk

A personal voice-AI helpdesk prototype built with **Python, LiveKit Agents, WebRTC, hosted speech/language models and SQLite**.

Users can discuss a fictional VPN, Wi-Fi or microphone issue, retrieve troubleshooting guidance, create a reviewed support ticket and check saved records in another conversation.

## Implemented capabilities

- Browser voice through the official LiveKit client, including microphone controls, agent audio and transcripts.
- A frontline AI and a ticket-specialist AI with shared conversation context.
- Permission to collect ticket details, followed by a separate final review and approval before saving.
- Fictional employee IDs for grouping tickets and checking existing open issues.
- A choice between discussing an existing issue and creating a separately reviewed new ticket.
- Ticket-ID lookup and employee open-ticket lookup across conversations using stored receipts.
- Scoped corrections that preserve unrelated draft fields and require a fresh review.
- Transactional SQLite writes with application-request idempotency.
- Turn/state and model-usage logs, plus configured provider fallback adapters.

## Architecture

```text
Browser microphone
    -> LiveKit room / WebRTC
    -> Python AgentSession
    -> speech recognition -> language model -> speech synthesis
    -> browser speaker

Model-selected tools
    -> fictional knowledge articles
    -> reviewed ticket workflow
    -> SQLite ticket creation and lookup
```

The language model interprets the request. Python validates data, controls draft state, checks message ordering and commits approved records. A caller's permission to collect details is separate from approval to save the final ticket.

The default models use LiveKit Inference. VAD and turn detection coordinate speech activity and response timing. The ticket specialist is another AI role, not a transfer to a human engineer.

## Verification and boundaries

The current offline suite passes **28 tests**, covering ticket contracts, read-only lookup, employee grouping, reviewed writes, corrections, retry behaviour, SDK workflow and browser-token handling. Existing development evidence also records real-model tests and a synthetic-audio creation journey for their respective revisions.

The author has reported a successful local browser conversation. This is separate from automated microphone or browser verification.

The current storage is SQLite. A LiveKit Cloud deployment can be used as a disposable demo; container-local ticket files are not durable or shared across instances. A persistent external database is a separate enhancement. Fictional employee IDs group records and do not authenticate callers.

There is no real password reset, ITSM integration, engineer assignment or ticket-resolution workflow. Provider fallback is configured; independent outage-recovery testing is not claimed.

## Repository scope

This public repository contains only the project overview. The runnable application, setup documentation and tests are maintained in a separate private repository. Credentials and runtime ticket databases are excluded from Git.

Technology: [LiveKit Agents](https://docs.livekit.io/agents/), [LiveKit browser client](https://docs.livekit.io/frontends/), [Python SQLite](https://docs.python.org/3/library/sqlite3.html).
