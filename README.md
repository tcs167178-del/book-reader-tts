# Book Reader TTS Preview

An independent experimental fork of [Book Reader by Elton Labs](https://github.com/swayinfo/elton-reader), based on version 3.1.2. This fork is maintained by **tcs167178-del** and is not an official Elton Labs release. Original copyright and MIT license are preserved in LICENSE; dependency notices are in NOTICE.md.

## Install on iPad or desktop with BRAT

1. In Obsidian, install and enable **BRAT** from Community plugins.
2. Open BRAT settings and choose **Add beta plugin**.
3. Enter `tcs167178-del/book-reader-tts`, select release **0.4.8**, and install.
4. Enable **Book Reader TTS Preview** in Community plugins, then use its library command.

The original Book Reader and this fork have different plugin IDs and separate settings. Their reading progress is not automatically migrated. Installing this plugin does not transfer your books.

### 中文安装说明

在 iPad 的 Obsidian 中安装并启用 BRAT，进入 BRAT 设置，点击 **Add beta plugin**，填写 `tcs167178-del/book-reader-tts`，选择 **0.4.8** 并安装。随后在第三方插件中启用 **Book Reader TTS Preview**。无需在“文件”App 中操作隐藏的 `.obsidian` 文件夹。

## Features

- EPUB, FB2 and PDF reading inherited from Book Reader.
- Experimental EPUB system text-to-speech: voice and language selection, speed control, pause/resume and paragraph navigation. PDF speech is not supported.
- Reader appearance controls, font sizing, backgrounds, page/scroll modes and reading progress/time estimates.
- Custom library labels, sorting, tags, archive/restore and batch organization.
- Batch import and local content extraction, with optional AI summaries.
- Chinese/English interface following Obsidian language; other interface languages fall back to English. Speech language is configured separately.
- Optional file-based synchronization of reading progress and library metadata.

## Synchronization

The plugin writes per-device journals into `Book Reader Sync` in your vault. A separate transport, such as Remotely Save, must synchronize that folder and your books. Use the same relative book paths on each device. This plugin does not connect to a NAS by itself. Keep device-specific voice preferences local; avoid simultaneously synchronizing the plugin's data.json with another mechanism. Back up the vault before enabling sync on an existing library.

## Privacy and network use

Book files, settings and sync journals are stored in the vault. System speech uses voices exposed by the device; voice availability and offline behavior depend on the operating system. Optional translation sends selected text to Google's translation endpoint. Optional AI features send requested book text and prompts to the AI provider you configure (including OpenAI-compatible endpoints or a local server). API credentials are stored in local plugin settings. External support links retained from the original plugin belong to the original project; fork issues should be reported in this repository.

## Status and limitations

This is a preview build, not an Obsidian community-directory release. Windows build and automated checks have passed. Installation, speech behavior and background/lock-screen playback on real iPad hardware still require verification. Mobile compatibility is declared in the manifest but is not a claim of completed device testing. Obsidian 1.13.7 was used during development; older versions allowed by the inherited minimum have not been comprehensively tested.

## Build and test

Download and extract `book-reader-tts-source-0.4.8.zip` from the repository or release first; it contains the complete source, tests and build configuration. Use a current Node.js release compatible with the dependencies.

```sh
npm ci
npm test
npm run build
```

Release assets are `main.js`, `manifest.json` and `styles.css`. The bundled PDF worker is embedded in main.js. Source and dependency licenses remain available in this repository and the source archive attached to the release.

## Voice diagnostics (0.4.8)

Open a book → Voice → Manage and install voices → All voices diagnostics (unfiltered). The report includes every voice returned by Web Speech, including IDs and languages outside Chinese/English/Japanese. Export saves a JSON file in the vault root; it contains no book text or credentials, but follows your normal vault sync rules. Test system default voice (Chinese) requests zh-CN without assigning a voice and does not change saved preferences. It cannot guarantee Siri voice access. Returning to the app refreshes the voice list.

## Library and reading insights (0.4.8)

The Collections panel supports adding tag-backed collections, renaming, hiding/restoring and ordering with touch-friendly up/down controls. Hiding a collection does not delete its books. Book actions remain under the ellipsis button; bulk actions appear in Edit mode. Books and PDF filters are available. Bookshelf appearance settings remain accessible from the panel and current-category row.

Reading time starts automatically after opening a book and can be paused in the footer. Only visible foreground reader intervals are counted; long suspended intervals are discarded. Per-book totals start with this update and follow file renames. Reading Insights shows recorded daily/lifetime totals, a seven-day chart, prior-week comparison, reading streak and most-read books by recorded time. Foreground time is an estimate of reading, not an attention measurement. Per-book totals, collection visibility and order remain device-local in this version; existing tag and reading-position synchronization remains unchanged. Historical daily totals are preserved, but per-book history is never fabricated.

51 automated checks passed; tablet-size browser component previews were inspected. Actual iPad WebView behavior remains to be verified.

## 0.4.8: experimental background speech

Voice settings now offer an opt-in Background speech experiment, off by default. It submits up to 32 short utterances to the local system speech queue in advance. This reduces dependency on JavaScript completion callbacks, but does not grant background execution or guarantee iPad lock-screen playback. If JavaScript is suspended, queued speech may run out; the operating system may also stop speech immediately. No cloud, audio export, silent-audio keepalive, or new native permissions.

Pause, stop and paragraph navigation cancel queued utterances. Use Apply to current phrase after changing voice or speed to replace already queued speech. Returning from the background offers Continue if the engine reports no speaking or pending speech.

Validation: 54 automated tests pass and production build succeeds. iPad background playback remains unverified. Test foreground speech first, then switch apps for 60 seconds, then test locking the screen separately. Report whether playback stops immediately, at a sentence boundary, or later. Disable the experiment to return to sentence-at-a-time playback.

## 0.4.9: bookshelves and touch navigation

Book actions and bulk selection now have Add to bookshelves: check multiple shelves, create and select a shelf, or uncheck to remove membership without moving files. Existing tags remain compatible. Mixed bulk selections preserve memberships unless explicitly changed.

Library appearance is separate from collection membership. Reading insights opens from the reading-time summary. Search has its own row, book actions stay visible, cover-fit controls are reserved for Edit mode, and tapping a book in Edit mode selects it. Collection editing uses compact touch controls.

55 automated tests pass. Membership save and dialog layouts checked at a 768 x 1024 browser viewport using the actual components with an Obsidian UI adapter. Native iPad confirmation remains required. Background speech remains experimental as described for 0.4.8.
