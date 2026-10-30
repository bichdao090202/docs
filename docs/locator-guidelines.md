# Locator Guidelines

Goals: **comprehensive**, **robust**, **fast**, **clean**.

## 1. Locator Priority

| Priority | Locator | Use when |
|---|---|---|
| 1 | `id` | Stable and unique |
| 2 | `name` / `data-testid` / `aria-label` | Stable test attributes |
| 3 | CSS selector | No id; simple structure |
| 4 | XPath | Need text match, parent/sibling traversal |
| ❌ | Absolute XPath, index-based, auto-generated ids/classes | Never |

## 2. Rules

- **Unique:** verify exactly 1 match in DevTools (`$$("css")` / `$x("xpath")`).
- **Short & relative:** start from a nearby stable anchor, not from `/html/body`.
- **Stable attributes only:** id, name, data-*, aria-*, visible text. Not position, color, or generated class names.
- **Scoped:** narrow with a parent container, avoid `//*`.
- **Text match:** use `normalize-space()` instead of raw `text()` with spaces.
- **Location:** only at the top of the Page Object. No inline locators in methods, Business, or tests.
- **No duplicates:** one element = one locator.

## 3. Bad vs Good

| ❌ Bad | ✅ Good |
|---|---|
| `/html/body/div[2]/div/form/input[1]` | `input[name='username']` |
| `//div[3]/button` | `//button[normalize-space()='Login']` |
| `//*[@id='submit']` | `#submit` |
| `//div[@class='btn btn-primary btn-lg x7sd9']` | `button[data-testid='submit']` |
| `(//input)[2]` | `//label[normalize-space()='Email']/following::input[1]` |
| `//div[contains(@class,'a')]//span//a` | `//nav[@id='main']//a[normalize-space()='Orders']` |

## 4. Dynamic Locators

Use when the same control repeats (menu items, table cells, product buttons). Define a **template** `String` with `%s` / `%d`, build with `byDynamic()`.

```java
// Template (top of the page)
private final String dynItmMenu = "//nav[@id='main']//a[normalize-space()='%s']";
private final String dynBtnAddToCart = "//h3[normalize-space()='%s']/following::button[1]";
private final String dynCelOrder = "//table[@id='orders']//tr[%d]/td[%d]";

// Usage
public void selectMenu(String menuName) {
    click(byDynamic(dynItmMenu, menuName));
}
```

```java
// BasePage
protected By byDynamic(String template, Object... args) {
    return By.xpath(String.format(template, args));
}
```

| Rule | |
|---|---|
| 1 template = 1 purpose | ✅ |
| Parameters only for varying part | ✅ |
| Template named `dyn<Prefix><Purpose>` | ✅ |
| Concatenating strings inside methods | ❌ |

## 5. Performance

- Prefer `id` / CSS over XPath.
- Avoid XPath starting with `//*` or deep `//div//div//div`.
- No `contains()` on large containers when `=` works.
- Avoid locators that match hundreds of nodes then filter by index.

## 6. Checklist

- [ ] Unique in DevTools
- [ ] No absolute / index-based XPath
- [ ] Works after page reload / data change
- [ ] Declared at top of Page Object, correct prefix
- [ ] Dynamic locator used for repeated controls
