# AI-wp-reviewer
AI reviewer assistant for WordPress submissions.

This repository should be used for boosing review process for WordPress submissions and to get new ideas on how to integrate a variety of workflows for AI agentic software like Claude Code and Open Code.

The main goal is to save time have more mental space to tackle with thngs that require manual inspection.

## How to use these files and folders.

1. Copy the files and folders you need from this repository to the root of your WordPress local install you use for reviews.
2. Everytime you use a tool like Claude Code, Open Code, etc, it should be opened from the root of your WP install and not from the specific product you're working on, while extensions for VS code like Cline will start from root you may want to include location in your prompt. That is because: AGENTS.md or CLAUDE.md will be read with both the general and product-specific guidelines. If you use agentic tools only in specific product folders you will need to reference the files and folders at the top of the WordPress install. It would be more time wasted prompting. We don't just copy everything in each product folder because some of these files work cross products, or in any case we may use LLMs in different ways.
3. I recommend symlinking AGENTS.md and CLAUDE.md, so they're in sync. Claude Code will read CLAUDE.md first, other tools usually prefer AGENTS.md. Do not depend on just one tool, things change fast and something that is useful today may be obsolete tomorrow.
4. Create your own private repository with this structure so you can edit it for your needs.
5. It's **fundamental** that you read the .md files you download, and change them where needed as some of them need your specific paths.

## General Guidelines

1. Do not interact with agentic tools like you would with a human. Everything should always be neutral and instructive. Some files in this repo tell LLMs to do the same.
2. Always read every line of code that is generated since AI is prone to errors or hallucinations.
3. Use your expertise to steer AI towards the right direction, it will be necessary. Double check everything. Some themes/plugins have bad code patterns that LLMs will imitate and mark as good.
4. Any reference to this repository to save tokens is beyond a matter of budget, it's mostly a matter of saving time and using context as best as possible.
5. Don't expect these files to be something that will be done once and that's it. I suggest saving some time every week to experiment with these tools, and iterate your .md files to improve them. With the goal to save more time and improve output quality.
6. Your development expertise plus the best LLM models and tools should save you development hours. If you notice LLM tools slowing you down, or you don't get a concrete bump in productivity, there's something at some stage of the process that needs to be addressed.

### Tips for prompts

- LLMs need to see: for backend code, make sure to tell them to use eval commands, error logs, wp cli db calls, to let them see the data structure. For front end start by copy pasting images of the UI or use specific extensions for agentic front end dev.
- LLMs need to get memories injected. At some point we will test and try tools like mem0, but for now, consider they always start from zero. In every fresh session they never saw your project, your code, nothing. They know nothing about the project in the first prompt. MD files are there to inject memories they can use to get to the goal faster. At the same time, keeping everything in one single session will not work because it's a waste of context. This is also why LLMs will always have blind spots and why your experience needs to be part of the workflow.

## Safety

Prompt injection is a concrete risk. Do not let the agent run commands without asking for permissions. Open Code needs some extra steps for this compared to Claude Code. You can find ann opencode.json config file in this repository to start.

## Requirements

- A local environment where you and LLMs have easy access to WP CLI.
- Ideally a system where you can easily run unix-based commands: Mac OS, Linux. A workaround on Windows with WSL is possible, but may be more painful to manage, or may require more work on more .md files to help LLMs understand when they should use powershell commands or not.
- In general, it helps to choose tools around your workflow that have a CLI, since agents in the terminal as Open Code can interact with them easily.

## Models to use

At the moment, Opus 4.6 is the best model. Most of the resources in this repository have been used with Opus 4.6.

Other good models (May 2026)

- GLM 5
- Sonnet 4.6
- DeepSeek V2.5
- Kimi k2.5

You can check new model releases here: https://openrouter.ai/models

## Agentic tools to avoid

For the moment I recommend staying away from tools like the following, which should run in isolation on their own virtual machine or VPS.

- Open Claw or any similar tool that can take over your entire system.
- Any kind of "AI-based" browser.
- Any kind of tool that does stuff in the background without asking for clear permissions. CLI agentic tools if configured properly will ask you for permission for any risky commands. In a GUI product this tends to be less likely.

## Agentic tools

- Open Code: I tend to prefer this one to other tools for most of the work. Mostly because it's neutral and independent from brands. It can be expanded with plugins.
- Claude Code: I tend to use it when dealing with something more complex, mostly for budget reasons. One advantage compared to Open Code is that Claude Code often gets new features released and included without you having to do anything.
- Cline: extension for VS code might be easier just for reviewing tasks end ease of prompting with project reference.

### Why using a CLI / Terminal User Interface for development

- It's easy to open several terminal emulators to work on multiple issues.
- Still acts as a chat interface.
- Generally more control and better permission management compared to GUI tools.
- A CLI can run any command you could run. With commands like `gh` this also means less time invested in clicking around the UI with your mouse across different pages.
- You can keep editing code and checking git diffs in VS Code.

### Markdown file types

- Instruction files: like docs, information or guidelines agents should follow.
- Commands: in Open Code you can trigger these with `/` for example, `/theme_create_review`. It's a canned prompt that should fit in context. Helps you save time in copy/pasting or writing.
- Agents: the main mode you're using in the tool. For example in Open Code plan and build are the two default agents.
- Sub-agents: they're called by agents automatically depending on your prompt. You could have a sub agent specialized readme files for example. This could include in its specification a series of specific tools or commands to use, and specific directions on how to update or write new sections in a readme file.

Depending on how you work, you may decide to create new commands or sub agents. E.g. a Github issue can be a command to send a prompt, but maybe you prefer it to be an agent. Whatever works best for you to save you time.

**Lazy loading**: your agentic tool should start by reading only AGENTS.md and CLAUDE.md. I suggest keeping AGENTS.md relatively short, for example around 100 lines to start. Then the agent should automatically read AGENTS.md references from `/docs` or `/plans` depending on what's relevant as you work.

### You can use `@` to invoke files

In opencode you can trigger your custom commands with a slash `/`. It's more direct, however you may be in a tool where opencode commands are not picked up automatically. In that case in tools like Claude Code you can still use `@` to reference a file, which may be a command you saved in opencode.

### Role of folders in this repository

- docs: anything long term about Reveiw process and guidelines. Organized in two different levels: product-specific, and general rules. For example, `code-standards.md` and `theme-requirements.md` is right under `docs`, because it should be valid for all products.
- .opencode or similar: config files, including commands, to be used with a specific product.
- plans: this folder's content should be only on your local machine. I suggest to avoid committing plans. Plans are temporary .md files used in an llm workflow that can be deleted once the PR is merged and the issue is solved. If it's necessary to save new information for the long term, use `/docs`.
- .opencode: can contain `agents`, `sub-agents`, `commands`, `skills`, `plugins` folders. For more details: [Opencode docs](https://opencode.ai/docs/commands/#_top)

### Guidelines for .md file names

The goal is to name files in a way they can be easily understood by you or an LLM in the future, even in years from now, with a simple `ls` command. So the LLM or you can pick specific files to read easily, and avoid wasting context and time by having the LLM read every single doc file.

Bad example: github-issues.md
Good example: how-to-create-github-issues.md

## Main LLM workflow

An example of a process you can use while working on individual issues in a product. I've used this in Activity Log and it helped me to save a lot of time.

### On your local machine

0. Make sure you have installed theme that you need to review.
1. Type prompt DO review on 'theme name'.
2. The agent should follow the `llm-workflow.md` file. It's a series of development steps you can read in `@docs/llm-workflow.md`.
3. After sharing the link in the first prompt most of the time (unless the issue is large) the LLM will create a plan and work on the problem. It will ask for your permission for additional steps if needed, and upon complettition it will create revew file that you can submit to track ticket.

While working on an issue, sometimes LLMs might uncover new issues that needs to be solved that are not related.
