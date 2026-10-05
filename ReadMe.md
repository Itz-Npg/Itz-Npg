# Itz-Npg

**I build tools that report what actually happened.**

I don't have a lot here yet. One real project, and a short list of opinions I've
had to learn the expensive way.

---

## 🔐 [Cryptoric Agent](https://github.com/Itz-Npg/Cryptoric-Agent)

**An AI-native desktop development environment. Bring your own key. Open source, MIT.**

[![Release](https://img.shields.io/github/v/release/Itz-Npg/Cryptoric-Agent?label=release&style=flat-square)](https://github.com/Itz-Npg/Cryptoric-Agent/releases)
[![License](https://img.shields.io/github/license/Itz-Npg/Cryptoric-Agent?style=flat-square)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178C6?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)

<p align="center">
  <img src="https://github.com/Itz-Npg/Cryptoric-Agent/raw/main/docs/screenshots/02-agent.png" width="640" alt="Cryptoric Agent — the agent surface">
</p>

A desktop agent that runs a real loop against **your** model provider —
OpenRouter, APINEX, or anything OpenAI-compatible on your machine. No account, no
backend, no bundled key. Electron + React + TypeScript, MIT licensed.

What it's actually about is the boring part: **it refuses to lie about what it
did.**

> A request to build a landing page once returned
> **"Task complete — no files were changed"** — with all five stages ticked
> green at `0 ms`. The engine treated a stage returning normally as proof the
> task was done, and had `continue: true` on a no-op. It now hashes the
> filesystem before and after implementation and reports `BLOCKED`, with a named
> reason, when nothing changed.

It also shipped a "verification" stage that ran no verification at all — it
counted running processes and the UI called it *"Ran tests and verified"*. Both
bugs are written up in [`audit.md`](https://github.com/Itz-Npg/Cryptoric-Agent/blob/main/audit.md)
rather than quietly patched, because the recurrence is the lesson.

**Verified, not asserted:** 500 unit tests, plus live runs against a real
provider — 13/13 on the agent loop, 8/8 on the full stage pipeline, with the
model really writing files to disk.

👉 **[Cryptoric Agent](https://github.com/Itz-Npg/Cryptoric-Agent)** — grab a
release or build it yourself with your own key.

---

## Things I believe, learned the hard way

**A step that completes is not a task that completed.** A stage returning
normally proves the stage ran. It says nothing about whether the work happened.
Measure the thing, don't infer it.

**A UI label is a promise.** Rendering "Ran tests and verified" above a process
count isn't a display bug, it's the product making a claim on the agent's behalf
that nothing backs up.

**Make the important line reachable by a test.** The line deciding whether a task
is finished shipped broken twice, both times because it lived somewhere a test
couldn't load. That's why `execution.ts` and `evidence.ts` import nothing at all.

**Zero is not a duration.** `Math.max(0, end - start)` will cheerfully print
`0 ms` for a phase that never ran. If it didn't execute, say `NOT_RUN`.

**Be suspicious of `??` on a safety value.** `signal ?? AbortSignal.timeout(n)`
removed the deadline *exactly* when the caller cared enough to pass a signal —
the one path where the bound mattered. Compose with `AbortSignal.any`.

**Don't weaken an assertion to make a test pass.** When a new guard fires before
an old one, fix the test's *setup* so it tests what it claims — and assert the new
guard separately.

---

## Contact

Found a bug or want to build on it? Issues and PRs are open:
**[Cryptoric Agent](https://github.com/Itz-Npg/Cryptoric-Agent/issues)**

<sub>MIT licensed · [Itz-Npg](https://github.com/Itz-Npg)</sub>