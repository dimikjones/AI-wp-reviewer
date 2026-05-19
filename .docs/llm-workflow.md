# LLM Workflow

Follow these steps for every issue or task. The user will might just add a track ticket link or write prompt like "Do theme review `theme-name-slug`".

## 1. Before Starting — Check for a Plan

**Always check `.plans/{theme-name-slug}/` first** before using any external tools (e.g. `gh`). The plan file is the primary source of truth for an issue. Use `gh` to look up issue details if no local plan exists.

These folders already exist, do NOT run `mkdir` or attempt to create them.

If we have a plan, inform the user. Always ask for confirmation before starting to implement an existing plan.

If no plan exists and the task is non-trivial, create one before proceeding.

## 2. Do the Work

Ask the user if plan is OK. If it is, then implement the fix or feature as described in the plan.

### Save Review File - CRITICAL

⚠️ **IMPORTANT: Never save review files inside the theme/plugin folder being reviewed.**

For themes/plugins reviews, after finishing the entire review, save the .txt file in:
- **Location:** `AI-wp-reviewer/.reviews/{theme-name-slug}-{theme-version}.txt`
- **Filename:** Use the theme slug (e.g., `logistic-transport-1.6.4.txt`, not `theme-review-1.6.4.txt`)
- **Format:** Use WordPress Trac WikiFormatting so reviews can be directly copied/pasted to Trac comments. See [review-document-format.md](review-document-format.md) for formatting rules.

❌ **DON'T:** Save in `wp-content/themes/{theme-name}/theme-review-1.6.4.txt`
✅ **DO:** Save in `AI-wp-reviewer/.reviews/{theme-name-slug}-{theme-version}.txt`
Tone in review file needs to be polite and to sound like a human and experienced WordPress developer.

## 4. After Completing — Check Docs

Once the work is done, check `.docs/` to see if any documentation needs updating:

- `.docs/` root files → general docs valid for all Melapress plugins
- `.docs/{plugin-slug}/` → plugin-specific docs

Update or create a doc file if the change introduces a new feature, modifies an existing behaviour that is documented, or adds something other developers or LLMs should know about.

Very important: docs name must help future LLMs to find information fast. LLMs will never be able to read ALL docs or that will fill their context too fast. You must name files in a way that makes it super fast for LLMs to find documentation about projects. They can just use ls or similar, find the file name, understand the category of the information, and consult and edit information as needed.

Bad example: github-issues.md
Good example: how-to-create-github-issues.md
