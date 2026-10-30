# Page Object & Business Layer Guidelines

## 1. Page Object Rules

| # | Rule |
|---|---|
| 1 | 1 page (or shared UI part) = 1 class. Extends `BasePage` |
| 2 | Locators declared **only** at the top of the class, `private final` |
| 3 | Methods represent **user actions** or **data getters** (not locator details) |
| 4 | **No assertions / verifications** in Page Objects |
| 5 | **No test data** or business flow in Page Objects |
| 6 | Interact through `BasePage` / custom element wrappers, not raw `driver.findElement` |
| 7 | Waits and logging live in `BasePage` wrappers, not repeated in each page |
| 8 | Action returns the page it leads to (`HomePage`) or `this` (for chaining) |
| 9 | State methods return `boolean` / `String` — the **test** decides pass / fail |
| 10 | Shared UI (header, menu, footer) → `Component`, not copied into pages |
| 11 | Interface is clean: no duplicated pages, elements, or methods |

## 2. Page Object Template

```java
public class LoginPage extends BasePage {

    // ===== Locators =====
    private final By txtUsername = By.id("username");
    private final By txtPassword = By.id("password");
    private final By btnLogin = By.cssSelector("button[type='submit']");
    private final By lblErrorMessage = By.cssSelector(".alert-error");

    // ===== Actions =====
    @Step("Enter username: {0}")
    public LoginPage enterUsername(String username) {
        type(txtUsername, username);
        return this;
    }

    @Step("Enter password")
    public LoginPage enterPassword(String password) {
        type(txtPassword, password);
        return this;
    }

    @Step("Click Login button")
    public HomePage clickLoginButton() {
        click(btnLogin);
        return new HomePage();
    }

    // ===== Getters / States =====
    public String getErrorMessage() {
        return getText(lblErrorMessage);
    }

    public boolean isErrorMessageDisplayed() {
        return isDisplayed(lblErrorMessage);
    }
}
```

## 3. BasePage (Common/base)

| Method | Purpose |
|---|---|
| `click(By)` | Wait clickable → click → log |
| `type(By, String)` | Wait visible → clear → sendKeys → log |
| `getText(By)` | Wait visible → return text |
| `isDisplayed(By)` | Return `true/false`, no exception |
| `waitForVisible(By)` / `waitForClickable(By)` | Explicit waits |
| `byDynamic(String template, Object... args)` | Build dynamic locator (see [locator-guidelines.md](locator-guidelines.md)) |
| `scrollTo(By)` | Scroll element into view |

Driver comes from `DriverManager.getDriver()` (ThreadLocal) — pages never receive or store a static driver.

## 4. Business Layer

| | Page Object | Business |
|---|---|---|
| Represents | One page's UI | A user goal across pages |
| Example | `enterUsername()` | `loginWithAccount(Account)` |
| Locators | ✅ | ❌ |
| Assertions | ❌ | ❌ |
| Returns | Page / data / boolean | Page / data |

```java
public class LoginBusiness {

    @Step("Login with account: {0.username}")
    public HomePage loginWithAccount(Account account) {
        return new LoginPage()
                .enterUsername(account.getUsername())
                .enterPassword(account.getPassword())
                .clickLoginButton();
    }
}
```

## 5. Do / Don't

| ✅ Do | ❌ Don't |
|---|---|
| `loginPage.getErrorMessage()` | `Assert.assertEquals(...)` inside page |
| One `HeaderComponent` for all pages | Same header locators in 5 pages |
| Page method takes data as parameter | Page method with hardcoded text |
| `clickLoginButton()` | `clickButtonByCss()` (leaks implementation) |
