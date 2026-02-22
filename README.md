# DataMonster Claude Skills
**Tableau & data visualization skills for Claude AI**
by [Pablo Gomez](https://Data.Monster) — DataMonster · Tableau Visionary & Ambassador · London, UK

---

## What are Claude Skills?

Skills are Markdown instruction files that teach Claude your personal standards, preferences, and patterns — so you don't explain yourself every single session.

You load a skill at the start of a conversation and Claude immediately knows how you work:

> *"I'm going to give you a skill to follow for this conversation."* → paste the skill → start working.

Skills work in **claude.ai chat** (web or mobile — free account supported) and in **Claude Code** (Anthropic's developer terminal tool, paid plan required). The Color Palette skill in particular is optimised for claude.ai chat, where the visual artifact panel renders the palette in full.

---

## Available Skills

| Skill | What it does | Best used in |
|-------|-------------|--------------|
| [`tableau-color-palette-skill.md`](./tableau-color-palette-skill.md) | Generates harmonious color palettes with visual swatches, hex codes, and Tableau Preferences.tps XML output. Includes PNG & SVG export. | claude.ai chat |
| [`tableau-calculated-fields-skill.md`](./tableau-calculated-fields-skill.md) | Calculated field standards: FIXED/INCLUDE/EXCLUDE decision logic, naming conventions, table calc patterns, NULL safety | Claude Code or chat |
| [`tableau-chart-type-decisions-skill.md`](./tableau-chart-type-decisions-skill.md) | Chart type recommendations, color palette rules, what to never use, dashboard layout principles | Claude Code or chat |

---

## How to use these skills

### In claude.ai chat (recommended for most users)

1. Open [claude.ai](https://claude.ai) and start a new conversation
2. Open the skill `.md` file on GitHub → click **Raw** → select all and copy
3. Paste the skill content into the chat with a message like:
   > *"I'm going to give you a skill to follow for this conversation."*
4. Claude will confirm it understands — then start working

> **Tip for the Color Palette skill:** Make sure **Artifacts** is enabled in your claude.ai settings (Settings → Feature Preview → Artifacts). Without it, the palette won't render visually.

### In Claude Code (for developers)

1. Download the `.md` file(s) you want
2. Place them in your project at `.claude/skills/`
3. Reference them at the start of a session:
   > *"Read `tableau-color-palette-skill.md` and use it to generate a palette for..."*

---

## Color Palette Skill — what it does

This is the most demo'd skill in the set — first shown live at **Tableau Conference 2025** and **DataFam Europe 2025** (originally as a ChatGPT custom GPT, now rebuilt and improved for Claude).

Given a simple prompt describing your visualization, Claude will:
- Ask the right questions about theme, style, and color restrictions
- Generate a harmonious palette of 5 colors (or however many you need)
- Render the palette visually as color swatches with names, hex codes, and roles
- Let you click any swatch or hex code to copy it to clipboard
- Export the palette as a **PNG** (for Canva, presentations) or **SVG** (for Adobe Illustrator, Figma)
- Output the Tableau **Preferences.tps XML** so you can add it directly to Tableau Desktop

Example prompts that work beautifully:
> *"Generate a 5-colour palette for a professional, warm Superstore dashboard. High contrast, colorblind accessible."*

> *"Palette for a climate change infographic — urgency and awareness, 3 harmonious colours."*

> *"Color palette for an F1 infographic about the Alpine team and Franco Colapinto."*

Watch the original TC2025 live demo: [Color Mastery — The Power of Color in Data Storytelling](https://www.salesforce.com/plus/experience/tableau_conference_2025/series/learning_at_tableau_conference_2025/episode/episode-s1e10)

---

## More from DataMonster

🔗 [data.monster](https://Data.Monster) · [LinkedIn](https://www.linkedin.com/in/pablolgomez) · [Tableau Public](https://public.tableau.com/app/profile/pablolgomez)

> These skills are shared freely with the DataFam community. If you use them, adapt them, or build on them — a shoutout is always appreciated but never required. That's the DataFam way. 💙
