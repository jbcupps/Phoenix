# Contributing to Phoenix

Phoenix is the coordination hub for the AI Ethical Stack. Contributions that improve documentation, cross-repo integration, and project coordination are welcome.

## Getting Started

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/your-feature`
3. Make your changes
4. Submit a pull request

## Commit Convention

All repos in the AI Ethical Stack use **conventional commits**:

| Prefix | Usage |
|--------|-------|
| `feat()` | New feature or functionality |
| `fix()` | Bug fix |
| `refactor()` | Code restructuring without behavior change |
| `chore()` | Maintenance, dependencies, tooling |
| `docs()` | Documentation changes |
| `ci()` | CI/CD pipeline changes |

**Examples**:
```
feat(scoring): add memetic dimension to evaluation API
fix(auth): reject expired Ed25519 signatures
docs(architecture): update Mermaid diagram with Orion_dock
chore(deps): bump Flask to 3.x
```

## Documentation Requirements

**Docs must be updated in every PR.** This is non-negotiable.

After any change (code, architecture, phase, integration, feature):
1. Update the relevant section in `README.md`
2. Update the appropriate file in `/docs/`
3. Commit docs in the same PR as code

PR titles should reflect both code and doc changes: `feat: add scoring API | docs: update integration guide`

### Formatting Rules

- Use `#` headings for document structure
- Use code fences (` ``` `) for code blocks and configuration
- Use Mermaid diagrams for architecture and flow visualization
- Use tables for structured data
- Link every mention of another repo (e.g., [Abigail](https://github.com/jbcupps/abigail))
- Reference GitHub issues/projects: `Closes #XX` | `Project board item #YY`

### Cross-Repo Links

When referencing other repositories in the stack, always use full links:

| Repo | Link |
|------|------|
| Abigail | `[Abigail](https://github.com/jbcupps/abigail)` |
| SAO | `[SAO](https://github.com/jbcupps/SAO)` |
| Ethical_AI_Reg | `[Ethical_AI_Reg](https://github.com/jbcupps/Ethical_AI_Reg)` |
| Orion_dock | `[Orion_dock](https://github.com/jbcupps/Orion_dock)` |
| Phoenix | `[Phoenix](https://github.com/jbcupps/Phoenix)` |

## Pull Request Process

1. Ensure your branch is up to date with `main`
2. Verify all documentation is current
3. Use a descriptive PR title following commit conventions
4. Include a summary of changes in the PR body
5. Reference any related issues or project board items
6. Request review from the PO for architecture or docs changes

## Weekly Sync

Every Monday:
1. Run `git pull` on Phoenix
2. Review `/docs/` for staleness
3. Open a PR titled `docs: weekly sync [date]` if updates are needed

## Getting PO Input

When you need Product Owner review:
- Comment `@grok-po` with "DOC REVIEW NEEDED" on the relevant PR or issue
- PO will respond within 1 hour during business hours

## Security

- Never commit API keys, secrets, or credentials
- Follow Ed25519 security boundaries defined in [CLAUDE.md](CLAUDE.md)
- Report security concerns directly to the PO

## License

By contributing to Phoenix, you agree that your contributions will be licensed under the MIT License.
