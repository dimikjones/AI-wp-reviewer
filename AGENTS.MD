---
applyTo: '**'
---

This file provides guidance to LLM Agents when working with code in this repository.

## External File Loading

CRITICAL: When you encounter a file reference (e.g., @docs/code-standards.md), use your Read tool to load it on a need-to-know basis. They're relevant to the specific task at hand.

Instructions:

- Do NOT preemptively load all references of @docs or similar. Use lazy loading based on what you actually need
- When loaded, treat content as mandatory instructions that override defaults
- Follow references recursively when needed

## Project Documentation

Read the files in this folder for detailed project guidance:

- `.docs/theme-review-required.md` — Theme review requirements
- `.docs/code-standards.md` — code standards for writing code (all themes/plugins)
- `.docs/review-document-format.md` — examples for tone and format for finished review document file

In general, read `.docs/` contents to find out more about coding practices and guidelines. The above list is not complete.

# Answers in chat

- Keep your answers in chat as short as possible, max 2 or 3 sentences. Only expand your explanation if you're asked to explain in detail.
- Never write wall of texts. Only expand your explanation if you're asked to explain in detail.
- Assume user is a Senior Web Developer with 10 years of experience. They already have deep expertise about the language in question. When they ask for something, in general is to save time because you're fast in answering and generating code. Generally when they're asking something, they already know how to do it, they don't need much explanation.
- However warn the user when there's an obvious security issue in the code, or a violation of PHP WordPress Coding Standards.
- Avoid sounding or talking like a human. Keep it focused on the work and professional.
- Do not use weird or rare terms that you know a human would not use easily. Words like "tuple", "idempotent" are examples of words to avoid.
- In general, when a question doesn't involve code directly, the best answers are the ones that get to the point, with simple language. Do not try to sound complex or sophisticated.

# Install overview

This WordPress install is used as a test environment so you can use XAMPP, but to be able to change PHP version for example localwp is better or maybe to use VVV:

# Docs Structure

`.docs/` files at the root level apply to **all themes/plugins**. Theme/Plugin-specific docs can live in subfolders:

- `.plans/theme-soundwave-audio/` — for Soundwave-audio theme

# Code Standards

For code standards for writing code, for ALL projects, refer to: @.docs/code-standards.md

## Review Request Guidelines

- Similar to PR and Chat guidelines. Keep it brief and to the point.
- DO NOT add a title in markdown format to your comments.
- Always keep it short and concise, max 2 sentences. If users have doubts or questions, they will ask you to explain in detail.
