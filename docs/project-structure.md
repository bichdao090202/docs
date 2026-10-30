# Project Structure

## 1. Technologies

| Technology | Purpose |
|---|---|
| Java | Programming language |
| Maven | Build + dependency management |
| Selenium WebDriver | Browser automation |
| TestNG | Test runner, suites, groups, parallel execution |
| Page Object Model | UI abstraction |
| Log4j2 | Logging |
| Allure Report | Execution report |

## 2. Folder Structure

```
root
├── docs/                      # Project rules (this folder)
├── pom.xml
├── README.md
└── src/test
    ├── java
    │   ├── Constant/          # Constants
    │   ├── Business/          # Business flows
    │   ├── DataObjects/       # Data models + DAO
    │   ├── Enums/             # Enums
    │   ├── PageObjects/       # Page Objects
    │   ├── TestCases/         # Test classes, grouped by module
    │   └── Common/            # Base classes, driver, elements, utils, listeners
    └── resources
        ├── Data/              # Test data files (JSON)
        ├── config/            # config.properties, <env>.properties
        ├── suites/            # TestNG suite XML files
        └── log4j2.xml
```

## 3. Folder Responsibilities

| Folder | Contains | Must NOT contain |
|---|---|---|
| `Constant` | `TimeoutConstants`, `MessageConstants`, `PathConstants` | Logic |
| `Business` | Business flows (`LoginBusiness`) combining Page Object actions | Assertions, locators |
| `DataObjects` | Models (`Account`) + `dao/` (`AccountDAO`) | UI code |
| `Enums` | `Browser`, `Environment`, `UserRole` | Logic beyond the enum |
| `PageObjects` | Page classes: locators + UI actions | Assertions, test data, business flow |
| `TestCases` | Test classes + assertions | WebDriver calls, locators |
| `Common` | See below | Project-specific business logic |

**Common sub-folders**

| Sub-folder | Contains |
|---|---|
| `base/` | `BasePage`, `BaseTest` |
| `driver/` | `DriverFactory`, `DriverManager` (ThreadLocal) |
| `element/` | Custom components: `TableElement`, `MenuElement`, `ComboBoxElement`, `DatePickerElement` |
| `utils/` | `WaitUtils`, `ConfigReader`, `JsonUtils`, `RandomUtils`, `ScreenshotUtils` |
| `listeners/` | `TestListener`, `RetryAnalyzer` |

## 4. Layer Dependencies

```
TestCases → Business → PageObjects → Common
```

| Layer | May use | Must NOT use |
|---|---|---|
| `TestCases` | Business, PageObjects, DataObjects, Enums, Constant | WebDriver / `org.openqa.selenium.*` |
| `Business` | PageObjects, DataObjects, Common | TestCases, assertions |
| `PageObjects` | Common, Enums, Constant | Business, TestCases, assertions |
| `Common` | Enums, Constant | Business, PageObjects, TestCases |

## 5. Test Design Approach

| Approach | Rule |
|---|---|
| Page Object Model | 1 page / component = 1 class. See [page-object-guidelines.md](page-object-guidelines.md) |
| Business layer abstraction | Tests read like user stories; Business hides step details |
| Parallel execution | ThreadLocal driver, no shared state. See [test-case-guidelines.md](test-case-guidelines.md) |
| Custom components | Complex controls wrapped once, reused everywhere. See [custom-components.md](custom-components.md) |

## 6. Test Flow

```
Test (TC01) → Business (loginWithAccount) → Page (enterUsername, clickLogin) → Common (BasePage.type/click) → WebDriver
     ↑
  Assertion only here
```
