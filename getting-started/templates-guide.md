# Templates Guide

This page explains **what each template contains, how to copy it, and how to mix-and-match modules** to document new Demo solutions quickly.

---

## 1 · Template Overview

| Template | Audience | Core Modules | Optional Modules |
|----------|----------|--------------|------------------|
| **Business** | Managers & stakeholders | Core Info, Executive Summary | Business Impact, Roadmap, Success Stories |
| **Technical** | Developers & architects | Core Info, Technical Summary | Architecture, Integration, Deployment, Testing |

GitBook supports **page duplication** and **block-level editing**, so you can reuse these templates as many times as you need[13].

---

## 2 · Folder Structure

templates/demos/
├── business-template.md # copy → [demo-name]/business.md
└── technical-template.md # copy → [demo-name]/technical.md

## 3 · Create a New Demo Doc (Git Sync)

1. Create a folder for the new POC
mkdir demo-catalog/example

2. Copy base templates
cp templates/demos/business-template.md demo-catalog/example/business.md
cp templates/demos/technical-template.md demo-catalog/example/technical.md

3. Commit & push
git add demo-catalog/example
git commit -m "docs: add example POC"
git push


GitBook will auto-sync the push and render both pages in the correct navigation order.

---

## 4 · Create a New Demo Doc (GitBook UI)

1. Open the **Templates** section in the left panel.
2. Click the **⋮** menu on *Business Template* or *Technical Template* → **Duplicate**.
3. Rename the duplicated page (e.g., `example – Business`).
4. Move the page into **Demo Solutions › example**.
5. Edit placeholders and delete unused sections.

---

## 5 · Cross-Linking Best Practices

- Always link **Business ↔ Technical** docs at the bottom of each page.
For technical details, see: Technical Documentation

text
- Update the **Demo Catalog** table after creating a new POC.
- Keep IDs and filenames identical across business & technical docs (`demo-2025-001`).

---

## 6 · Template Versioning

| Action | When | Owner |
|--------|------|-------|
| **Minor update** (typo, small tweak) | Anytime | Any contributor |
| **Major change** (new section) | Quarterly review | Doc lead |
| **Deprecate** (module no longer used) | When superseded | Steering group |

> Use Git change-requests to review major template updates[15].

---

## 7 · Troubleshooting

| Issue | Cause | Fix |
|-------|-------|-----|
| Template variables show as `{{ placeholder }}` | Missed replacement | Search & replace all placeholders |
| Page not in sidebar | Not listed in `SUMMARY.md` | Add path under the correct heading |
| Diagram not rendering | Mermaid not enabled | Enable **Mermaid** integration in space settings |

---

Happy documenting! Use these templates to keep every Demo clear, consistent, and easy to navigate.


