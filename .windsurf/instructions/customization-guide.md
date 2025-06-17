# Windsurf Instructional Memory – Customization Guide

This guide shows *project teams* how to adapt the Windsurf `.windsurf/` template to their own repositories in **four progressive steps**.

---

## 1. Foundation (≈ 1 hour)
1. Fork / clone the template repository.
2. Read `.windsurf/README.md` for a directory tour.
3. Skim `rules/01-meta-rules.md` to understand rule precedence.

### MUST CUSTOMISE NOW
| Area | File(s) |
|------|---------|
| **Core Context** | `context/architecture.md`, `context/directory_structure.md`, `context/technical.md`, `context/glossary.md` |
| **Project Config** | `.windsurf/README.md`, `rules/52-project-conventions.md`, `config/mcp_config.json` |

---

## 2. Technology Alignment (≈ 2 hours)
1. Update `rules/20-coding-standards.md` for each language.
2. Add language-specific templates under `templates/`.
3. Configure lint / format hooks in `workflows/` and pre-commit.
4. Test a simple `/mode executor` interaction to ensure standards are enforced.

---

## 3. Process Integration (≈ 2-4 hours)
1. Align `rules/50-version-control.md` with your branching strategy.
2. Adapt `workflows/*` for CI, test, deploy.
3. Populate `planning/tasks/TASKS.md` with a realistic backlog.
4. Hold a walkthrough so all contributors understand persona modes.

---

## 4. Advanced Customisation (ongoing)
* Add domain knowledge articles to `knowledge/`.
* Create additional operational modes (e.g., `SECURITY`, `DOCS`).
* Implement vector memory index (see optimisation §14.7).
* Contribute improvements back via PRs.

---

### Common Tech-Stack Patterns
```yaml
web_dev:
  - edit: rules/20-coding-standards.md (JS/TS)
  - add: templates/react-component.template
  - configure: workflows/deploy-staging.yml
python_data:
  - edit: rules/20-coding-standards.md (Python)
  - add: templates/python-class.template
  - add: knowledge/data-science-patterns.md
enterprise:
  - customise: rules/50-version-control.md
  - add: knowledge/compliance-requirements.md
```

---

### Validation Checklist
- [ ] AI reads updated architecture.
- [ ] Code output follows new standards.
- [ ] Task workflow runs end-to-end (Planner → Executor → Reviewer).
- [ ] CI passes on a sample PR.

Need help? Consult proposal docs or open an issue tagged `help wanted`.
