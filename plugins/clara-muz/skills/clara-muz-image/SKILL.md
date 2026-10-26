---
name: clara-muz-image
description: 이미지 생성이 필요할 때 사용. "이미지생성", "이미지 만들어줘", "카드뉴스 이미지", "인스타/스레드/링크드인 게시물 이미지", "실습용 이미지"처럼 요청하면 요청서를 만들고 ChatGPT(GPT)에게 맡겨 이미지를 만든 뒤 파일을 직접 확인한다. Make images for posts, practice material or personal work by briefing GPT, then verify the file.
---

# clara-muz-image: 이미지 생성 요청서 → 생성 → 확인

You are the **lead**. Task from the user: $ARGUMENTS

Trigger word: **이미지생성**. Main generator: **ChatGPT (GPT image generation)**. ComfyUI is a later add-on (see section 7).

## 0. Know which app you are

- **Claude** (Claude Code / desktop Code mode): cannot make images itself. Brief GPT with the `codex` command (see `clara-muz-team` section 2). Tested: Claude -> `codex exec` -> built-in image_gen -> file in the working folder, exact size 1080 x 1350.
- **Codex / GPT**: you may be able to generate the image yourself. If your environment has no image generation, say so plainly and use the manual path in section 4.
- Plain chat window that cannot run commands: write the brief (section 2) and give it to the user to paste into the ChatGPT app.

## 1. Ask only what is missing

Ask in one short message, and only for what the task did not already say:
1. **Purpose** (pick one): `post` (Threads / Instagram / LinkedIn), `lesson` (student practice material), `personal`.
2. **Subject and mood** in one or two sentences, and any text that must appear inside the image (Korean text is error-prone, see section 5).
3. **How many** images (default 1; a card-news set is 4 and must share one style).
4. Folder to save in (default: `images/` inside the working folder).

## 2. Write the brief (one file per image)

Save to `~/.clara-muz/jobs/<id>/prompt.md`. Use exactly these fields:

```
Subject:
Style: (flat illustration / photo / 3D / ... , palette as hex codes)
Size: (see the table, pixels and ratio)
Text in image: (exact string, or "none")
Do not include: real people, logos or brands of others, watermarks
Save as: <folder>/<name>.png
End with: the saved path, the final prompt you used, and what you could not do.
```

| Purpose | Size (px) | Ratio |
|---|---|---|
| Instagram feed / card news | 1080 x 1350 | 4:5 |
| Threads | 1080 x 1350 or 1080 x 1080 | 4:5 or 1:1 |
| LinkedIn single image | 1200 x 627 | 1.91:1 |
| LinkedIn carousel page / document | 1080 x 1350 | 4:5 |
| Lesson sheet | 1600 x 1200 | 4:3 |

For a set (card news), give every image the same Style and palette lines and number the file names `card1.png`, `card2.png`, ...

## 3. Run

Add these lines to the brief so GPT uses the right tool and leaves the file where you can find it:

```
Use the imagegen skill with the built-in image_gen tool (not the CLI/API fallback; no OPENAI_API_KEY is needed).
The built-in tool saves under $CODEX_HOME/generated_images/. Copy the final image to <folder>/<name>.png before finishing. Never leave it only in generated_images. Do not overwrite an existing file; use -v2 etc.
```

- You are Claude: run GPT limited to the image folder (background, same as `clara-muz-team`):
  `codex exec -C "<folder>" --sandbox workspace-write --skip-git-repo-check -o "<job dir>/result.md" - < "<job dir>/prompt.md"`
- **macOS**: if `codex` is not on the PATH, the ChatGPT desktop app ships it at `/Applications/ChatGPT.app/Contents/Resources/codex-cli/bin/codex` (signed in with the ChatGPT account, no API key). The shell has no `timeout` command; just run it in the background and wait.
- One `codex exec` run per image (a set of 4 = 4 runs, same Style lines). Do not ask for "variants" when you need distinct assets.
- Image generation counts against the user's ChatGPT/Codex usage. For more than about 8 images in one task, tell the user first.
- If GPT answers that it cannot generate images, or no file appears, do **not** draw a substitute and do not fake it. Go to section 4. Using `OPENAI_API_KEY` (CLI/API fallback) only if the user asks for it.

## 4. Manual path (when GPT cannot generate here)

Give the user the brief as plain text to paste into the ChatGPT app (image generation on), and ask them to save the result into the folder and tell you the file name. Then continue with section 5 as usual.

## 5. Check, don't trust

Open the file yourself and report:
- exists, format (PNG/JPG), pixel size matches the table (a different size is a fail unless the user accepts it)
- not blank, not cropped, no stray watermark
- **Korean or other text in the image is exactly the requested string** (read it character by character. In one test a short Korean headline came out exactly right, but one test is not a guarantee: long or small text, numbers and dates are the likely failures. If it is wrong, either regenerate once with the exact text quoted in the prompt, or put the text on afterwards with a design tool, and say which)
- no real person, no third-party logo or brand
- for a set: same style and palette across all images

Fail → send one correction run with the specific problems. After two failed tries, stop and tell the user what is left.

## 6. Record

Save a note (see `ai-memory`) with: purpose, final prompt, style and palette, file names, date. Never put keys, passwords or personal data in it. For card news, this note lets the next set keep the same look.

Mode-specific:
- `post`: say that the image is **AI-generated** if the platform or the user's rule requires a label, and remind the user to check the post text with their own approval step before publishing. Do not publish.
- `lesson`: also give the student the final prompt split into subject / style / size / text, so they can see how it was built.
- `personal`: nothing extra.

## 7. ComfyUI (later)

Not installed yet. When it is, add it as a second generator: you (Claude) call the ComfyUI MCP tools directly, using the same brief fields, and sections 5 and 6 stay the same. Until then, do not mention ComfyUI as available.

## 8. Finish

Tell the user in plain language: what was made, where the files are, what you checked, what failed or is left, and whether GPT was actually used.
