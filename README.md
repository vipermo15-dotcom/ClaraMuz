<p align="center"><a href="README.ko.md">한국어</a> · <b>English</b> · <a href="README.ja.md">日本語</a></p>

<p align="center"><img src="plugins/clara-muz/assets/logo.png" alt="ClaraMuz logo" width="120"></p>

# ClaraMuz

### A plugin that lets Claude and ChatGPT work as one team

![ClaraMuz at a glance](docs/img/en/hero.svg)

## In one line

**Split the work between two AIs, and hand unfinished work over through notes.** Install it once in the Claude app or the ChatGPT app.

## When to use it

| Situation | What ClaraMuz does |
|---|---|
| "ChatGPT should build it and Claude should review it…" | Splits the task, hands the pieces to both AIs, and **opens the files to check the results itself** before reporting back |
| "I ran out of Claude usage!" | Writes a **handoff note** of what is done and what is left, so you can continue in ChatGPT ![handoff](docs/img/en/handoff.svg) |
| "I don't want to explain the same rules again" | Put them in the **shared notes** that both AIs read |

## How it works

- You install **three skills** (plain written instructions). It is not a program, so it is light and needs no extra tools such as Node.js.
- Shared notes are ordinary files on your computer (`~/.clara-muz/memory/`). You can open and edit them in any text editor.
- To split work, the lead AI calls the other AI's command tool (`codex` or `claude`). If it is missing, **the AI asks "may I install it?" first.**

| Skill | Say something like |
|---|---|
| `clara-muz-team` | "Do this together", "have the other AI review it too", "GPT builds, Claude reviews" |
| `ai-memory` | "Take a note", "show the shared notes", "write a handoff document" |
| `clara-muz-image` | "이미지생성", "make an image for an Instagram / Threads / LinkedIn post", "make practice material images" (GPT draws, Claude checks the file) |

## Use cases

![Ways to use it](docs/img/en/usecases.svg)

The app you type in is the **main** (lead). The other AI is the **sub**.

### 1. GPT plans, Claude builds

*You want ChatGPT as the main for planning and Claude as the sub for building.*

- **Start in:** the ChatGPT app, Codex mode.
- **Say:** `Use clara-muz-team: you write the plan for a signup page, then have Claude build it`
- **What happens:** GPT writes a plan with goals, steps and acceptance criteria, hands it to Claude to build (edits only the files it owns), then reviews the result against the plan.

### 2. Claude builds, GPT makes the images

*Claude is the main for building, but it cannot generate images, so GPT does that part.*

- **Start in:** the Claude app, Code tab.
- **Say:** `Use clara-muz-team: build the landing page, and have GPT make the hero image (16:9, flat illustration) and save it in assets/`
- **What happens:** Claude builds the page and sends GPT a precise brief (subject, style, size, file name). GPT saves the image; Claude checks the file exists and uses it.
- **Note:** this needs image generation to be available to the GPT side in your setup. If it is not, the AI tells you plainly instead of faking an image.

### 3. Cross-check a document with both AIs

*You want a claim, a contract summary or a report checked by two different AIs.*

- **Start in:** either app.
- **Say:** `Use clara-muz-team to cross-check this report with both AIs and show where they disagree`
- **What happens:** both AIs get the same prompt and answer independently. The lead then lists what they agree on, where they differ and what neither supports, checks the disputed points against sources, and reports in a short table. Answers are never merged by averaging.

### 4. Hit your usage limit? Hand it over

- **Say:** `Use ai-memory to write a handoff document`, then open the other app and say `Use ai-memory and continue from the handoff`.

## Install

![Prepare the apps](docs/img/en/step1.svg)

First install and sign in to the **Claude desktop app** or the **ChatGPT desktop app** (whichever you use). With both, the two AIs can share work.

| Guide | Who is it for? |
|---|---|
| **[GUI install guide](docs/INSTALL-GUI.md)** | Anyone who wants to install by clicking in the app, no terminal (**recommended**) |
| [CLI install guide](docs/INSTALL-CLI.md) | Developers who use the terminal |

### At a glance: install in the app

**Claude app:** open **Customize → Plugins → Add → Add marketplace**, enter `Jay-Hyun/ClaraMuz`, then add `clara-muz`.

![Install in Claude](docs/img/en/install-claude.svg)

**ChatGPT app:** open **Plugins → Add plugin marketplace**, enter `Jay-Hyun/ClaraMuz` as the **Source** (leave the other fields empty), add it, then install `clara-muz`. A company admin can roll it out to everyone with **Admin → Plugins → Import marketplace**.

![Install in ChatGPT](docs/img/en/install-chatgpt.svg)

> The pictures are **illustrations** based on the official docs and the real app. Menu names may differ by app version. Details and troubleshooting are in the [GUI install guide](docs/INSTALL-GUI.md).

### Is one app enough?

Yes. But for the two AIs to **work together, you need to use a mode that can run the other AI's command** (Code in Claude, Codex in ChatGPT). In a plain chat window, files may not be saved and commands may not run, so "write a handoff note" is the most reliable feature there.

## Try it

![Try it](docs/img/en/step4.svg)

Just ask in plain words.

```
Use clara-muz-team to read the README and find typos
Use ai-memory to note "buttons are blue"
Use ai-memory to write a handoff document
```

In the Claude app you can also use the slash forms `/clara-muz:clara-muz-team` and `/clara-muz:ai-memory`.

- At first the AI may ask "may I read files?" or "may I run a command?". Check what it wants and allow it.
- If the other AI's command tool is missing, it asks "may I install it?". Say **yes** and it installs it with the official method. Then just make sure you are signed in to that app.

> The pictures are **illustrations**. The real screens may look slightly different.

## Stay safe

- Work handed to the other AI is **read-only by default**; write mode is used only when files must change.
- The AI is told not to touch anything outside the folder you are working in. (This is an instruction, not a hard block.)
- **Never put passwords, keys or personal data in the shared notes.** They are plain text files.
- Notes location: Mac/Linux `~/.clara-muz/memory/`, Windows `C:\Users\<you>\.clara-muz\memory\`

## FAQ

- **Does it cost money?** ClaraMuz is free. Claude and ChatGPT use your existing subscriptions.
- **Do I need Node.js or a server?** No.
- **Can I use just one AI?** Yes. Notes and handoffs work, and the work is split among that AI's own sub-workers. If the other AI was not used, the AI tells you so.
- **How do notes get passed along?** One side writes a note file, the other reads it. A handoff note changes to `claimed` once taken, so it is never taken twice. The idea is borrowed lightly from [ai-memory](https://github.com/akitaonrails/ai-memory); the two projects are independent.
- **I want notes to be read automatically every time.** Ask "run init with ai-memory" and it adds a one-line rule to the app's global instructions file (after asking you).

## Troubleshooting

| Symptom | Try this |
|---|---|
| The command does nothing | Quit the app completely and start it again |
| "`codex` / `claude` not found" | Say yes when the AI offers to install it, or follow the app's official install guide |
| "Sign-in required" | Sign in to that app (Claude or ChatGPT) and try again |
| Not visible on the ChatGPT Plugins tab | Check that adding the marketplace finished (on a company account, that the admin finished the import) and restart the app |
| Claude will not add a marketplace | Your admin may block external marketplaces. Ask them |

## License

MIT
