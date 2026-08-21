# FJU IM Spec Writer

🇬🇧 English | 🇹🇼 [繁體中文](README.md)

An Agent Skill for Claude that generates and edits capstone-project specification documents ("SA documents") for a Systems Analysis & Design course at the Department of Information Management, Fu Jen Catholic University — following the instructor's chapter structure, formatting, and review standards.

> **Note**: This skill helps with structure and formatting. It does **not** fabricate project content (survey data, interview results, system logic) — you still need to supply or confirm all substantive content. The skill's job is to organize what you provide into the correct format, and to flag "needs confirmation" when information is missing.
>
> **About `examples/` and `templates/`**: A local copy of this skill package may include the instructor's official templates and a past high-quality capstone example obtained via a faculty advisor. These files are **excluded from this public repo** (see `.gitignore`) since their copyright/privacy belongs to the original course and authors. If you clone this repo, these folders will be empty — this doesn't break the skill, but you'll need to supply your own copies to actually use it (see "What you need before using it" below).

---

## Who this is for

- FJU IM students currently taking the Systems Analysis & Design course or writing their capstone spec document
- Anyone who wants to reuse this "skill-generates-spec-documents" architecture for their own course/instructor's requirements
- Anyone curious about a real-world Claude Agent Skill design case study

## What it does / doesn't do

**Does**:
- Organizes content you provide into the instructor's required chapter structure and formatting (table vs. prose, User Story card format, database table format, etc.)
- Checks for logical inconsistencies across chapters (e.g., a problem stated in the problem statement with no corresponding feature in system scope)
- During the deep-interview phase, asks a structured list of questions based on what the spec requires, helping you catch things you hadn't thought through
- Produces a properly formatted Word document (fonts, sizes, page numbers, TOC, auto-numbered figures/tables, etc.)

**Doesn't**:
- **Won't** invent "what feature modules your system should have"
- **Won't** invent "who your competitors are or their pros/cons"
- **Won't** invent "what your stakeholders expect"

These require your own (or your team's) research (interviews, market research, technical evaluation) — the skill only organizes your ideas into the correct format. If you want creative brainstorming help, just talk to Claude directly — that's a general conversational capability, separate from this skill.

---

## What you need before using it

For copyright/privacy reasons, this public repo does **not** include:

1. **Official templates**: `111_SA.docx` (111-113 academic year version) and `114_SA.docx` (114+ academic year version) — request these from your instructor and place them in `templates/`
2. **(Optional) Past high-quality examples**: If you have a past capstone example obtained via your instructor, place it in `examples/` — this significantly improves the quality of generated content, since the skill analyzes real examples to infer "review preferences the instructor never wrote down explicitly, but are visible from quality differences between examples"

**You can still use the skill without example documents** — `reference/` already contains rules extracted from existing examples (`chapter_requirements.md`, `content_synthesis_guide.md`, etc.). If your instructor's template or rules have changed, see "Rules may be outdated — how to update" below.

---

## Installing on Claude.ai (web/app)

1. Open Claude.ai, go to **Settings → Capabilities**, and confirm "**Code execution and file creation**" is enabled
2. Go to **Customize → Skills**
3. Click "**+**" → "**+ Create skill**"
4. **Zip the entire `fju-im-spec-writer/` folder** and upload it. **Zipping note**: the ZIP's top level must be the `fju-im-spec-writer/` folder itself — don't wrap it in an extra outer folder, and don't zip the loose files directly
5. After uploading, go back to the Skills list and **make sure the skill's toggle is turned on** (some interfaces default to off after upload)

Available on Free, Pro, Max, Team, and Enterprise plans. Personal uploads are private to your account; on Team/Enterprise you can share with colleagues (though see "Collaborating with teammates" below for the package's collaboration limitations).

---

## First-time use: a full walkthrough

**You don't need to manually "invoke" this skill.** Once installed and enabled, just tell Claude what you want to do — it auto-detects relevance from keywords in your message (like "spec document", "User Story", "database design").

### Step 1: Start the conversation

Something like: "I'm starting my capstone spec document, this is my first time using this skill."

### Step 2: Deep project interview (one-time only)

Claude will run a complete background interview covering:
- Project name, core concept, the problem being solved
- Stakeholders, requirements-gathering methods, competitor analysis
- Feature module overview, technical plan
- Team members and division of labor

Your answers get compiled into `state/project_context.md` (a knowledge base). **All future chapter generation reads from this file first — you won't be asked to re-explain background you've already covered.**

### Step 3: Confirm default vs. custom reference sources

Claude will ask whether to apply the skill's built-in default reference strategy (derived from the 111/114 templates and past examples), or whether you have different reference documents to specify instead. **This is only asked once**; the answer is recorded and won't be re-asked.

### Step 4: Generate chapter by chapter

Tell Claude which chapter to write (e.g., "write the problem statement for Chapter 1"). Claude will:
- First confirm you've supplied the details this section needs, and ask for anything missing
- Generate section by section, self-checking against the rules after each small section before moving to the next
- Let you interrupt and correct at any point, rather than discovering a wrong direction after a huge block is written

### Step 5: Assemble and export

Once all needed sections are confirmed, Claude assembles them into one complete Word document (with correct fonts, page numbers, TOC formatting) for you to download.

### Later revisions

If your instructor gives feedback, just tell Claude which chapter to revise — **no need to rewrite the whole document**. Claude will handle just that chapter and log the feedback to avoid repeating the same issue.

---

## Collaborating with teammates

**Important limitation — read this first**: Claude.ai custom skills are uploaded to an **individual account**. The files under `state/` are just the static skeleton shipped inside the skill package. Updates made to these files during a conversation **do not** automatically sync back into your uploaded skill package, and **do not** sync to a teammate's package either. Real collaboration requires an **external shared space outside the skill package** for the "live" versions of `project_context.md` and `project_state.md`, e.g.:

- A shared Google Drive folder (if connected, Claude can read/write it directly)
- A GitHub repo your team already uses
- Or simply: whoever updates the file sends the latest version to the other person, who pastes it in at the start of their next conversation

**Practical workflow**:
1. Decide on a shared location
2. Whoever does the deep interview first produces `project_context.md` and saves it there
3. Before the other person starts working, they paste in the latest `project_context.md` and `project_state.md` from the shared location
4. Before writing any chapter, check the progress table to make sure no one else is already working on it
5. After finishing, save the updated files back to the shared location so the other person sees the latest state

---

## Directory structure

```
fju-im-spec-writer/
├── SKILL.md                       # Core logic: triggers, five-stage workflow, rules
├── README.md / README.en.md    # This file
├── LICENSE                        # MIT (excludes templates/examples)
├── CHANGELOG.md                   # Version history
├── .gitignore                     # Excludes copyright/privacy-sensitive files
├── templates/
│   ├── 111_SA.docx                # Official template, 111-113 (obtain yourself)
│   ├── 114_SA.docx                # Official template, 114+ (obtain yourself)
│   └── README.md                  # Explains why this is empty on GitHub
├── reference/
│   ├── chapter_requirements.md    # Per-chapter rules, common deductions
│   ├── content_synthesis_guide.md # Per-section source priority & inferred preferences (current default)
│   ├── style_guide.md             # Exact table/card formatting templates
│   ├── database_design_rules.md   # Database normalization conventions
│   ├── format_pattern_guide.md    # Prose vs. table vs. list per section
│   ├── document_formatting.md     # Fonts, sizes, page numbers, TOC, punctuation
│   └── review_checklist.md        # Post-generation quality checklist
├── examples/
│   ├── (past examples, obtain yourself)
│   └── README.md
└── state/
    ├── project_context.md         # Project knowledge base (one interview, reused long-term)
    └── project_state.md           # Chapter progress tracker (for collaboration)
```

---

## Rules may be outdated — how to update (for future students)

Instructor requirements change over time (e.g., the 111 → 114 version revision). The analysis in `reference/content_synthesis_guide.md` was built from **the three documents available at the time** (111 template, 114 template, the Beaton music-festival example) — it is **not permanently fixed**.

If you find:
- The instructor has switched to a new template
- You have a newer past example
- The instructor's requirements website has been updated

**Recommended approach**: give the new template/example to Claude and say "I want to re-analyze `content_synthesis_guide.md` using these new reference documents." Claude will apply the same analysis method (comparing completeness per chapter, extracting writing strengths, inferring unwritten preferences) to produce an updated analysis, replacing or supplementing the current one. The skill also proactively asks whether to use the default or a custom source before generating content (see "Stage One" in SKILL.md).

---

## FAQ

**Q: Do I need to re-explain my project background every time?**
No — after the first deep interview, it's saved to `project_context.md`. As long as that file persists (or you paste it in), you won't be asked again.

**Q: Will it hallucinate or make things up?**
The skill is designed to ask for missing information rather than invent it. But it's still an LLM at heart and can still hallucinate (e.g., misattributing details from one feature to another) — you should double-check important numbers and scenario details after generation.

**Q: If two teammates each upload the skill, can we see each other's edits?**
No — see "Collaborating with teammates" above.

**Q: Do I have to follow the skill's suggestions exactly?**
No — the analysis in `content_synthesis_guide.md` is a **suggestion**, not an official instructor ruling. Anywhere it diverges from the instructor's stated rules, the skill flags it explicitly, and you should confirm with your instructor or TA before final submission.

---

## Version history

See CHANGELOG.md

## Acknowledgements & References

Parts of this skill's design were inspired by two open-source projects (both MIT licensed):

- **[obra/superpowers](https://github.com/obra/superpowers)** (MIT License) — inspired the "clarify before you write" (brainstorming) and "small-unit generate-then-verify" (TDD-style red-green loop) workflow design
- **[mattpocock/skills](https://github.com/mattpocock/skills)** (MIT License) — inspired the "one-time deep interview, then reuse the context long-term" pattern (`grill-with-docs`), reflected here in `state/project_context.md`

No code or file content was copied directly from either project; only the design concepts were referenced and independently reimplemented for a different domain (academic spec-document generation vs. software engineering workflows).

## License

This skill's original content (SKILL.md, reference/, README, etc.) is licensed under MIT — see LICENSE. Template files (`templates/`) and past examples (`examples/`) belong to the original course/authors and are not covered by this license — see the note at the bottom of LICENSE and `.gitignore`.
