# Graduation Project Spec Writer (fju-im-spec-writer)

🇬🇧 English | 🇹🇼 [繁體中文](README.md)

FJU IM Spec Writer — An Agent Skill for Claude that generates and edits a capstone-project specification document ("SA document") following the requirements of a Systems Analysis & Design course at the Department of Information Management, Fu Jen Catholic University.

> **Note**: This skill helps with structure and formatting. It does **not** fabricate project content (survey data, interview results, system logic) — you still need to supply or confirm all substantive content. The skill's job is to organize what you provide into the correct format, and to flag "needs confirmation" when information is missing.

## What it does

- Generates content following the instructor's chapter structure (Ch.1–4 + appendix)
- Applies fixed formats for User Stories, database design, and UI blueprints
- Runs a post-generation quality checklist to catch common consistency issues (e.g., problem statement not matching system scope)
- Supports solo or two-person collaboration via a shared progress-tracking file

## Directory structure

```
fju-im-spec-writer/
├── SKILL.md
├── README.md / README.zh-TW.md
├── LICENSE
├── CHANGELOG.md
├── templates/
│   ├── 111_SA.docx
│   └── 114_SA.docx
├── reference/
│   ├── chapter_requirements.md
│   ├── style_guide.md
│   ├── database_design_rules.md
│   ├── format_pattern_guide.md
│   ├── document_formatting.md
│   └── review_checklist.md
├── examples/
└── state/
    ├── project_context.md
    └── project_state.md
```

## Installing on Claude.ai

1. Settings → Capabilities: enable "Code execution and file creation"
2. Customize → Skills → "+" → "+ Create skill"
3. Zip the entire `fju-im-spec-writer/` folder (the folder itself must be the ZIP root) and upload it
4. Toggle the skill on

Available on Free, Pro, Max, Team, and Enterprise plans. Personal uploads are private to your account; on Team/Enterprise you can share with colleagues.

## Scope

The rules here are extracted from one specific instructor's course requirements. If you're not in this course, the rules won't apply directly — but you can reuse the skeleton (SKILL.md + reference + templates + state) and swap in your own instructor's requirements.

## License

Personal use, see LICENSE. Template files under `templates/` belong to the original course/authors — confirm you have rights to share before publishing publicly.
