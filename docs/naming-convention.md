# Naming Convention

## 1. Branch Naming

**Format:** `<type>/<module-or-purpose>`

| Type | Use for |
|---|---|
| `feature` | New test case, page, or framework function |
| `fix` | Fix script, locator, or framework bug |
| `refactor` | Improve code, no behavior change |
| `docs` | Documentation only |

**Examples**
- `feature/login-module`
- `fix/login-error-locator`
- `refactor/base-page-wait`
- `docs/naming-convention`

Rules: lowercase, kebab-case, no spaces, no personal names.

## 2. Pull Request Naming

**Format:** `[Type][Module] Short description`

| Part | Values |
|---|---|
| Type | `Feature`, `Fix`, `Refactor`, `Docs` |
| Module | `Login`, `Cart`, `Common`, ... |

**Examples**
- `[Feature][Login] Implement TC01 - Login with valid account`
- `[Feature][Login] Add LoginPage and LoginBusiness`
- `[Fix][Login] Fix locator of error message`
- `[Refactor][Common] Move wait methods to WaitUtils`

## 3. Commit Message

**Format:** `<type>(<module>): <short description>` — English, imperative, ≤ 72 chars.

| Type | Use for |
|---|---|
| `feat` | New test / page / function |
| `fix` | Bug fix |
| `refactor` | Refactor |
| `test` | Update test only |
| `docs` | Documentation |
| `chore` | Build, config, dependency |

**Examples**
- `feat(login): implement TC01 login with valid account`
- `fix(login): update lblErrorMessage locator`
- `chore(pom): upgrade selenium version`

## 4. Package / Folder

| Level | Rule | Example |
|---|---|---|
| Top-level layer | As defined in [project-structure.md](project-structure.md) | `PageObjects`, `TestCases` |
| Sub-folder (module) | lowercase, singular | `TestCases/login`, `Common/utils` |

## 5. Class Name

PascalCase, noun.

| Type | Format | Example |
|---|---|---|
| Page Object | `<Page>Page` | `LoginPage`, `CartPage` |
| Component (shared UI part) | `<Name>Component` | `HeaderComponent` |
| Business | `<Module>Business` | `LoginBusiness` |
| Test class | `<Module>Test` | `LoginTest`, `CartTest` |
| Base class | `Base<Type>` | `BasePage`, `BaseTest` |
| Data object | `<Entity>` | `Account`, `Product` |
| DAO | `<Entity>DAO` | `AccountDAO` |
| Enum | `<Name>` (singular) | `Browser`, `Environment` |
| Constants | `<Topic>Constants` | `TimeoutConstants` |
| Custom component | `<Control>Element` | `TableElement`, `ComboBoxElement` |
| Utility | `<Purpose>Utils` | `WaitUtils`, `JsonUtils` |
| Listener | `<Purpose>Listener` | `TestListener` |

## 6. Test Case Name

| Item | Format | Example |
|---|---|---|
| Test ID | `TC<nn>` (unique in project) | `TC01` |
| Test method | `tc<nn>_<camelCaseDescription>` | `tc01_loginWithValidAccount` |
| `@Test(description)` | `TC<nn> - <Sentence case>` | `TC01 - Login with valid account` |

**Example (Login)**

| ID | Method | Description |
|---|---|---|
| TC01 | `tc01_loginWithValidAccount` | TC01 - Login with valid account |
| TC02 | `tc02_loginWithInvalidPassword` | TC02 - Login with invalid password |
| TC03 | `tc03_loginWithEmptyFields` | TC03 - Login with empty username and password |

## 7. Method Name

camelCase, starts with a verb.

| Layer | Pattern | Example |
|---|---|---|
| Page – action | `verb` + `Element` | `clickLoginButton()`, `enterUsername()`, `selectCountry()` |
| Page – get data | `get` + `Data` | `getErrorMessage()` |
| Page – state | `is` / `has` + `State` | `isWelcomeMessageDisplayed()` |
| Page – navigation | `open` + `Page` | `openLoginPage()` |
| Business | `verb` + `User goal` | `loginWithAccount()`, `clearShoppingCart()` |
| DAO | `get` / `getAll` + `Entity` | `getAccount()`, `getAllProducts()` |

## 8. Variable & Constant

| Kind | Rule | Example |
|---|---|---|
| Variable / parameter | camelCase, meaningful | `expectedMessage` |
| Boolean | `is` / `has` / `can` | `isLoggedIn` |
| Collection | plural | `products` |
| Constant | `UPPER_SNAKE_CASE`, `static final` | `DEFAULT_TIMEOUT` |
| ❌ Avoid | `a`, `tmp`, `data1`, unclear abbreviations | |

## 9. Element Name

**Format:** `<prefix><PurposeInPascalCase>` — e.g. `btnLogin`.

| Control | Prefix | Example |
|---|---|---|
| Button | `btn` | `btnLogin`, `btnSubmit` |
| Textbox / Input | `txt` | `txtUsername`, `txtPassword` |
| Textarea | `txa` | `txaDescription` |
| Link | `lnk` | `lnkForgotPassword` |
| Checkbox | `chk` | `chkRememberMe` |
| Radio button | `rdo` | `rdoMale` |
| Combobox / Dropdown | `cbo` | `cboCountry` |
| List / List box | `lst` | `lstProducts` |
| Label / Text / Message | `lbl` | `lblWelcomeMessage`, `lblErrorMessage` |
| Image | `img` | `imgLogo` |
| Icon | `ico` | `icoCart` |
| Table | `tbl` | `tblOrders` |
| Table row | `row` | `rowOrder` |
| Menu | `mnu` | `mnuMain` |
| Menu item | `itm` | `itmLogout` |
| Tab | `tab` | `tabProfile` |
| Date picker | `dtp` | `dtpBirthDate` |
| Dialog / Modal / Popup | `dlg` | `dlgConfirmDelete` |
| Form | `frm` | `frmLogin` |
| Iframe | `ifr` | `ifrPayment` |
| File upload | `upl` | `uplAvatar` |
| Slider | `sld` | `sldPrice` |
| Pagination | `pgn` | `pgnOrders` |
| Loading spinner | `spn` | `spnLoading` |
| Container (div / section / panel) | `pnl` | `pnlFilter` |

**Dynamic locator (String template):** `dyn` + `<Prefix><Purpose>`

| Example | Meaning |
|---|---|
| `dynItmMenu` | Menu item by text |
| `dynBtnAddToCart` | Add-to-cart button by product name |
| `dynCelOrder` | Table cell by row / column |

**Rules**
- Name by **purpose**, not by position / color / index (`btnLogin` ✅, `btn1` ❌, `btnBlue` ❌).
- Same element on many pages → one shared component, not duplicated names.
- Names are unique within a page.

## 10. File Name

| File | Format | Example |
|---|---|---|
| Test data | `<module>.json` | `login.json` |
| TestNG suite | `<scope>-suite.xml` | `smoke-suite.xml`, `regression-parallel-suite.xml` |
| Env config | `<env>.properties` | `qa.properties` |
| Screenshot | `<testMethod>_<yyyyMMdd_HHmmss>.png` | `tc01_loginWithValidAccount_20250101_101530.png` |
| Log | `test-run.log` (rolling by date) | `logs/test-run.log` |
