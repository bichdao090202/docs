# Project Documentation

Rules for everyone contributing to this Selenium test automation project.

**Stack:** Java · Maven · Selenium WebDriver · TestNG · Page Object Model · Log4j2 · Allure

## Documents

| # | Document | Content |
|---|----------|---------|
| 1 | [project-structure.md](project-structure.md) | Tech stack, folders, layers, dependency rules |
| 2 | [naming-convention.md](naming-convention.md) | Branch, PR, commit, class, method, variable, element names |
| 4 | [page-object-guidelines.md](page-object-guidelines.md) | Page Object & Business layer rules |
| 5 | [locator-guidelines.md](locator-guidelines.md) | Locator priority, XPath rules, dynamic locators |
| 6 | [test-case-guidelines.md](test-case-guidelines.md) | Test design, checkpoints, setup/cleanup, suites |

## Golden Rules

1. Tests call **Business / Page Object** methods only — never WebDriver directly.
2. **No assertions** in Page Objects, Business, or Common.
3. **No hardcode**: URLs, credentials, timeouts, test data → config / Data files.
4. **Explicit waits** only. No `Thread.sleep()`.
5. Locators are short, relative, unique — declared only at the top of a Page Object.
6. One element = one definition. No duplicated pages / elements / methods.
7. Every test is independent, parallel-safe, and prepares/cleans its own data.
8. Every action is logged; every failure has a report entry + screenshot.
9. Follow naming convention. Comments in English, no typos.
10. A PR must pass the [code review checklist](code-review-checklist.md) before merge.

## Reading Order for New Members

`project-structure` → `naming-convention` → `page-object-guidelines` → `test-case-guidelines`
