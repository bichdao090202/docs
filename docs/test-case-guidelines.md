# Test Case Guidelines

## 1. Test Class Structure

```java
@Feature("Login")
public class LoginTest extends BaseTest {

    private final LoginBusiness loginBusiness = new LoginBusiness();

    /**
     * TC01 - Login with valid account
     * Precondition: Account "valid_user" exists
     * Expected: User is logged in and welcome message is displayed
     */
    @Test(description = "TC01 - Login with valid account", groups = {"smoke", "login"})
    @Severity(SeverityLevel.CRITICAL)
    public void tc01_loginWithValidAccount() {
        Account account = AccountDAO.getAccount("valid_user");

        // Step 1: Login with valid account
        HomePage homePage = loginBusiness.loginWithAccount(account);

        // Step 2: Verify user is logged in
        Assert.assertTrue(homePage.isWelcomeMessageDisplayed(), "Welcome message is not displayed");
        Assert.assertEquals(homePage.getWelcomeMessage(), "Welcome " + account.getUsername(),
                "Welcome message is incorrect");
    }
}
```

## 2. Rules

| # | Rule |
|---|---|
| 1 | Test uses **Business / Page Object** methods only — no `WebDriver`, no `By`, no `findElement` |
| 2 | Assertions **only in `TestCases`** |
| 3 | 1 test = 1 objective, with its own ID (`TC<nn>`) |
| 4 | Tests are **independent** — any order, any thread |
| 5 | Test data from DAO — no hardcode |
| 6 | Every test has a description block + `// Step n:` comments |
| 7 | Every assertion has a failure message |

## 3. Checkpoints

| ✅ Do | ❌ Don't |
|---|---|
| `Assert.assertEquals(actual, expected, "message")` | `Assert.assertTrue(true)` |
| Verify the real AUT state (text, URL, data in table) | Verify only "no exception thrown" |
| `SoftAssert` for several checks in one TC, end with `assertAll()` | Hide failures in `try/catch` |
| Wait for the state before checking | Check immediately (false fail) |
| Check data actually saved (e.g., item in cart table) | Check only a success toast (false pass) |

> Result must always reflect the AUT: **no wrong pass, no wrong fail.**

## 4. Setup & Cleanup

| Phase | Where | What |
|---|---|---|
| Environment prepare | `@BeforeMethod` (BaseTest) | New driver (ThreadLocal), open base URL, maximize |
| Data prepare | `@BeforeMethod` / `@BeforeClass` | Create required data (API / DB / UI) |
| Data cleanup | `@AfterMethod(alwaysRun = true)` | Delete created data, logout |
| Environment cleanup | `@AfterMethod(alwaysRun = true)` | Screenshot if failed, `driver.quit()`, remove ThreadLocal |
| Global | `@BeforeSuite` / `@AfterSuite` | Clean old results, init dirs |

> Cleanup must run even if the test fails (`alwaysRun = true`).

## 5. Organization

```
TestCases/
├── login/
│   └── LoginTest.java
└── cart/
    └── CartTest.java
resources/suites/
├── smoke-suite.xml
├── regression-suite.xml
└── regression-parallel-suite.xml
```

| Item | Rule |
|---|---|
| Folder | 1 module = 1 folder, 1 feature = 1 test class |
| Groups | `smoke`, `regression`, plus module name (`login`, `cart`) |
| Suite | Smoke / Regression / Parallel — files in `resources/suites/` |
| Priority | Use `priority` only when needed; never depend on execution order |

## 6. Parallel-Safe Rules

- WebDriver in `ThreadLocal` (`DriverManager`). No `static WebDriver`.
- No shared mutable state between tests.
- Unique test data per test (`RandomUtils`), or one account per thread.
- Retry only through `RetryAnalyzer` (max 1), never to hide real failures.

## 7. Test Case Checklist

- [ ] ID, description, steps, expected result documented
- [ ] Uses Business / Page methods only
- [ ] Assertions with messages, correct expected / actual
- [ ] Data from DAO, prepared and cleaned
- [ ] Runs alone and in parallel
- [ ] In the correct module folder + suite + group
