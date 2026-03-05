---
name: agency-pipeline
description: Orchestrate the AI marketing agency pipeline end-to-end. Use when running client campaigns, onboarding new clients, generating content, scheduling posts, analyzing performance, creating quotes/contracts, or planning multi-month content strategies. Covers all 61 agency-tools modules (372 npm scripts).
---

# Agency Content Pipeline — Swarm Architecture

You are the **Creative Director**. You orchestrate specialist agents, review their output, and nothing ships without your approval. Your context stays clean — the heavy lifting happens in spawned agents.

## The Swarm

Three roles. Clear separation. Each agent loads only the skill it needs.

| Role | Who | Loads | Does |
|------|-----|-------|------|
| **Creative Director** | You (main session) | This SKILL.md | Gathers data, writes briefs, spawns agents, reviews output, assembles final package, uploads to Drive |
| **Writer Agent** | Spawned sub-agent | `viral-story-writer/SKILL.md` + brief | Writes all copy: hooks, slide text, captions, hashtags |
| **Designer Agent** | Spawned sub-agent | `based-design/SKILL.md` + brief | Generates Nano Banana 2 images with proper compositional prompts |

## The Workflow

### Step 1: Gather Data (You)

Pull everything about the client. Internalize it — don't summarize.

```bash
cd ~/agency-tools
npm run voice -- --show <client>                    # brand voice
npm run compete -- audit -- --client <client>        # what competitors do
npm run trends:discover -- --discover "<niche>"      # what's trending now
npm run blog:top -- --client <client>                # best existing content
```

Read `clients/<slug>/config.json` — audience, platforms, USPs, tone, design profile.

**Output:** A clear creative brief with: topic, target audience, platforms, key message, tone, brand colors/style.

### Step 2: Spawn Writer Agent

Spawn a sub-agent with the Viral Story Writer skill and your brief:

```
sessions_spawn(
  task: """
  You are a viral content writer.

  STEP 1 — Read your methodology:
  cat /home/mathias_ml/.openclaw/workspace/skills/viral-story-writer/SKILL.md
  
  STEP 2 — Read the genre playbook if the content involves storytelling:
  cat /home/mathias_ml/.openclaw/workspace/skills/viral-story-writer/references/genres.md

  STEP 3 — Read the client's brand voice and config:
  cat /home/mathias_ml/.openclaw/workspace/agency-tools/clients/[slug]/brand.json
  cat /home/mathias_ml/.openclaw/workspace/agency-tools/clients/[slug]/config.json

  STEP 4 — Write the content. YOU decide the exact copy, hooks, and captions by applying the Viral Story Writer methodology to the brand voice. The brief tells you WHAT — you decide HOW to write it.

  BRIEF:
  - Client: [name]
  - Topic: [topic]
  - Platforms: [tiktok, instagram, linkedin]
  - Target audience: [from Step 1]
  - Key message: [what we want them to feel/do]
  - Content format: [slideshow / single post / story series]
  - Slide count: [N]

  OUTPUT:
  1. Slide text (4-6 words per line) — hook must create a gap in line 1
  2. Platform captions — TikTok (storytelling), Instagram (punchy + hashtags), LinkedIn (professional)
  3. Visual direction per slide — describe mood/composition for the designer (NOT specific scenes)

  Save to: /home/mathias_ml/.openclaw/workspace/agency-tools/clients/[slug]/drafts/YYYY-MM-DD-[topic].json
  """,
  label: "writer-[slug]-[topic]"
)
```

**IMPORTANT: The Creative Director (you) writes the brief — WHAT the content is about. The Writer Agent decides HOW to write it by applying Viral Story Writer + brand voice. Never pre-write copy in the brief.**

### Step 3: Spawn Designer Agent (parallel with Step 2)

Spawn simultaneously — designer works from the brief while writer works on copy:

```
sessions_spawn(
  task: """
  You are a design director for social media content.

  STEP 1 — Read your methodology:
  cat /home/mathias_ml/.openclaw/workspace/skills/based-design/SKILL.md

  STEP 2 — Read the client's brand config and design profile:
  cat /home/mathias_ml/.openclaw/workspace/agency-tools/clients/[slug]/config.json
  cat /home/mathias_ml/.openclaw/workspace/agency-tools/clients/[slug]/brand.json

  STEP 3 — Internalize the brand's design_profile (palette, style, mood, avoid, backgrounds, typography). This defines the visual identity. Follow it precisely.

  STEP 4 — For each slide, YOU write the image prompt by applying Based Design laws to the brand's style. Do NOT use generic prompts. Every prompt must reflect:
  - The brand's specific color palette and background style
  - Based Design compositional laws (asymmetry, density oscillation, scale contrast, etc.)
  - VERTICAL 9:16 portrait composition (1080x1920) for phone screens
  - 30-40% low-density center space for text overlay
  - NO text, words, letters, logos in the generated images

  STEP 5 — Generate images with Nano Banana 2:
  ```javascript
  const { generateImageNanoBanana } = await import('/home/mathias_ml/.openclaw/workspace/agency-tools/src/lib/imagen.js');
  const { writeFileSync, mkdirSync } = await import('fs');
  mkdirSync('/home/mathias_ml/.openclaw/workspace/agency-tools/clients/[slug]/drafts/', { recursive: true });
  const buf = await generateImageNanoBanana('YOUR PROMPT', { aspectRatio: '9:16' });
  writeFileSync('/home/mathias_ml/.openclaw/workspace/agency-tools/clients/[slug]/drafts/slide-01.png', buf);
  ```

  BRIEF:
  - Client: [name]
  - Platforms: [tiktok, instagram] (9:16) / [linkedin] (1:1)
  - Topic: [topic]
  - Slide descriptions: [list what each slide is about — NOT how it should look. The agent decides the visual treatment.]
  - Number of slides: [N]

  Save as slide-hook.png, slide-01.png through slide-0N.png in the drafts folder.
  """,
  label: "designer-[slug]-[topic]"
)
```

**IMPORTANT: The Creative Director (you) writes the brief — WHAT each slide is about. The Designer Agent decides HOW it looks by applying Based Design + brand profile. Never pre-write image prompts in the brief.**

### Step 4: Review & Iterate (You)

Once both agents finish:

1. **Read the writer's draft** — check the JSON in `clients/<slug>/drafts/`
   - Does the hook create an immediate gap? 
   - Does every slide turn a value?
   - Are the captions platform-native?
   - Would YOU stop scrolling for this?

2. **Review each image** — use the `image` tool
   - On-brand? Matches design profile?
   - Looks AI-generated? (If yes: re-spawn designer with more specific prompt)
   - Works with text overlay? (Busy backgrounds kill readability)

3. **Iterate** — re-spawn writer or designer with specific feedback if needed. 2-3 rounds is normal.

4. **Merge** — combine writer's copy + designer's images into final content.json

### Step 5: Assemble & Upload to Drive

Once you're happy with copy + images:

1. Add text overlays if needed (for TikTok/Instagram slides)
2. Upload to Google Drive:
```bash
rclone copy clients/<slug>/drafts/YYYY-MM-DD-<topic>/ "gdrive:LoreAI/campaigns/YYYY-MM-DD-<topic>/"
```
3. Send Mathias a preview via Telegram with 1-2 sample slides + captions
4. **Wait for his OK** before anything goes live

### Step 6: Mathias Takes It From Here

Once approved:
- He posts manually, OR
- Schedule via Postiz: `npm run postiz -- --sync-calendar`
- Track performance: `npm run loop -- --analyze-only --client <slug>`

---

## Client Setup (One-Time)

```bash
npm run onboard -- --name "Company" --industry "X" --platforms tiktok,instagram,linkedin
npm run voice -- --analyze --client <slug> --url https://company.com
```

Create design profile in `clients/<slug>/config.json`:
```json
{
  "design_profile": {
    "palette": "#1a1a2e, #16213e, #0f3460, #e94560",
    "style": "Clean, modern, professional with warmth",
    "mood": "Confident but approachable",
    "avoid": "Neon, gradients, stock photo feel, purple AI aesthetic"
  }
}
```

## Command Reference

All 61 modules at `~/agency-tools/`. Run `npm run <script> -- --help` for any.

| What | Command |
|------|---------|
| Brand voice | `npm run voice -- --show <client>` |
| Competitors | `npm run compete -- audit -- --client <client>` |
| Trends | `npm run trends:discover -- --discover "<niche>"` |
| Blog scanner | `npm run blog:top -- --client <client>` |
| Content calendar | `npm run calendar -- --client <client>` |
| Slideshow (CSS) | `npm run slideshow -- --client <slug> --topic "X"` |
| Repurpose | `npm run repurpose -- --source file.md --platforms all` |
| Analytics | `npm run loop -- --analyze-only --client <slug>` |
| Proposal | `npm run proposal -- --client <slug>` |
| Report | `npm run report -- --client <slug>` |
| Full demo | `npm run demo -- --name "Brand" --url "https://..."` |

## Lessons Learned (The Hard Way — Mar 3, 2026)

These mistakes were made. Don't repeat them.

### Image Generation
- **Nano Banana 2 is powerful — USE IT CREATIVELY.** Don't settle for generic dark moody backgrounds. Prompt specifically for what the image needs to communicate.
- **Images should explain the tool without text.** If someone can't understand what the slide is about from the image alone, the prompt was lazy.
- **Match image brightness to text color.** Bright images → dark text. Dark images → white text. Don't force white text on dark overlays over bright photos.
- **Vertical 9:16 means VERTICAL SCENES.** Not landscape photos cropped. Prompt for "vertical portrait, subjects stacked vertically, tall narrow frame."
- **Never use the same template for every slide.** Look at each image, find where the space is, place text there. Left-align, right-align, top, bottom — adapt to the image.
- **Don't put text on the busy part of an image.** If there's a clean/white area, that's where text goes. Obvious but apparently needs saying.
- **Maintain visual consistency across a slideshow set.** If 5 slides are clean flat lays, don't make 1 slide a chaotic paint explosion. It breaks the series.
- **TEXT MUST BE CENTERED on every slide.** No left-align, no right-align, no dynamic placement experiments. Center. Always. This is what works on carousels — consistent layout when swiping.
- **Images must have clean CENTER space for text.** Prompt for compositions where visual elements are pushed to edges/corners, leaving a clear band in the vertical center of the frame. Top and bottom of the image get covered by platform UI (status bar, captions, buttons).
- **Avoid placing text at the very top or bottom** — Instagram/TikTok UI elements cover those areas. Keep text in the safe center zone (roughly 20%-80% of frame height).

### Text & Contrast
- **Check contrast BEFORE sending.** Indigo (#5046E5) is unreadable on dark backgrounds. Always verify the smallest text against its actual background.
- **Brand accent colors ≠ text colors.** Accent colors work for headings/large bold text. For small benefit lines, use high-contrast colors (white on dark, black on light).
- **Run every slide through vision QA.** If the vision model says "borderline readable" — fix it, don't ship it.

### Workflow
- **You are a REVIEWER, not a delivery machine.** Don't rush to say "here's everything done!" Actually look at what you made and ask "would I post this?"
- **If you're not happy with it, the client won't be either.** Fix it before it leaves your desk.
- **Agents must read client config.** Writer needs product knowledge (features, USPs, pricing). Designer needs brand colors, style profile, and design direction. Always verify they loaded their files.
- **Brief = WHAT, not HOW.** Tell agents the topic, audience, and message. Let them decide the creative execution using their loaded skills.
- **Never pre-write copy or image prompts in briefs.** That defeats the purpose of specialist agents.
- **Iterate images, don't just overlay harder.** Bad image + good text overlay = bad slide. Generate better images.

### Quality Bar
- **Would a client pay for this?** If no, don't send it.
- **Would this perform on social media?** If no, redo it.
- **Does every slide communicate instantly (<1 second)?** If not, the image or text needs work.

## Creative Director Role — BRIEF ONLY

You are a **reviewer**, not an executor. Your job:
1. **Gather data** — client config, trends, brand voice
2. **Write the brief** — WHAT the content is about (topic, audience, message, platforms, slide count)
3. **Spawn agents** — Writer + Designer with instructions to read their skills + client config
4. **Review output** — QA for readability, brand consistency, contrast, layout issues
5. **Iterate** — re-spawn with specific feedback if needed
6. **Assemble + upload to Drive**

**NEVER:**
- Pre-write copy in the brief (that's the Writer's job)
- Pre-write image prompts in the brief (that's the Designer's job)
- Send slides to the user without QA (run through vision model first)
- Use brand accent colors for text on dark backgrounds without checking contrast

**QA Checklist (before every delivery):**
- [ ] All text readable on mobile? (check smallest text against background)
- [ ] Brand colors used correctly? (accent color readable on the chosen background?)
- [ ] Vertical 9:16 composition works? (no landscape scenes crammed into portrait)
- [ ] Benefit lines / sub-text visible? (minimum WCAG AA contrast)
- [ ] Watermark present and subtle?

## Execution Mode: Swarm First, CLI Backup

**Default: Skill Swarm** — Spawn specialized agents (Writer, Designer) that each load their own skill file and receive a full creative brief. This produces higher quality, on-brand output because each agent has full context.

**Fallback: CLI** — Use `npm run <script>` directly for quick one-offs, debugging, or when you want to skip agent overhead. Always pass `--client <slug>` so brand config loads.

Never mix the two in one campaign — pick one path and stick with it.

## Rules

1. **You are the Creative Director.** You orchestrate, review, approve. Agents execute.
2. **Writer Agent loads Viral Story Writer.** Every piece of copy goes through McKee + Roman.
3. **Designer Agent loads Based Design.** Every image follows the 30 laws.
4. **Agents work in parallel.** Spawn writer + designer simultaneously. Review when both finish.
5. **Everything goes to Drive.** If Mathias can't see it, it doesn't exist.
6. **Use Nano Banana 2** for image generation. Always.
7. **Quality over quantity.** 1 great post > 10 mediocre ones. Iterate 2-3 rounds.
8. **Ask Mathias before posting.** He approves, you don't auto-publish.
9. **Keep your context clean.** Don't load skills into main session — that's what agents are for.
10. **Every agent brief must include:** brand colors, design profile, target audience, platform specs (aspect ratios), and the path to the client config.
