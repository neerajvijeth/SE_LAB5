## SE_Lab5

### Identified Issues Table

| Issue | Type | Line(s) | Description | Fix Strategy |
|--------|------|----------|--------------|---------------|
| Mutable default argument `[]` (Pylint: W0102) | Bug | 8 | The function uses `logs=[]` as a default parameter, which is mutable and shared between all calls to `addItem`, possibly causing incorrect behavior. | Replace with `logs=None` and initialize inside the function: `if logs is None: logs = []`. |
| Usage of `eval()` (Bandit: B307 / Pylint: W0123) | Security (Medium) | 59 | The use of `eval()` is unsafe since it can run arbitrary code, leading to security risks. | Remove the `eval()` line entirely since it’s unnecessary for the program’s functionality. |
| Bare `except` block (Flake8: E722 / Pylint: W0702) | Bug | 19 | The code uses a bare `except:` statement, which captures every possible exception and conceals real issues. | Replace it with a specific exception type, e.g., `except KeyError:` to handle missing dictionary keys safely. |
| Unused import `logging` (Flake8: F401 / Pylint: W0611) | Style / Cleanup | 2 | The `logging` module is imported but never utilized anywhere in the script. | Simply delete the unused import line. |
| Missing `with` statement for file operations (Pylint: R1732) | Style / Reliability | 26, 32 | Files are opened without using a `with` context manager, risking unclosed file handles if an error occurs. | Refactor the file handling code to use `with open(..., encoding="utf-8") as f:` for automatic closure. |
| Old string formatting (Pylint: C0209) | Style | 12 | The script uses outdated `%` string formatting instead of modern f-strings. | Update to an f-string: `f"{datetime.now()}: Added {qty} of {item}"`. |

---

### Lab 5 Reflection

#### 1. Which issues were easiest and hardest to resolve?
The simplest fixes involved removing the unused `logging` import and the insecure `eval()` function call—these required only deleting lines of code. The most challenging issue was the mutable default argument (`logs=[]`). Understanding why mutable defaults persist across multiple function calls took deeper reasoning, and fixing it required adjusting both the function signature and initialization logic.

#### 2. Did any of the tools produce false positives?
Yes. Pylint flagged the `global` statement used for `stock_data`. While global variables are generally avoided, it was needed here for maintaining state in this small script. In this case, the warning was contextually acceptable and was suppressed using a comment.

#### 3. How would you incorporate these static analysis tools into a real project workflow?
I would integrate them in two main ways:
- **During development:** Configure the code editor (e.g., VS Code) to automatically run Flake8 each time a file is saved. This ensures immediate feedback on code style and minor issues.
- **In CI/CD pipelines:** Use GitHub Actions or another CI service to automatically execute Pylint and Bandit on every push or pull request. The build pipeline would fail if any critical Bandit security warnings or major Pylint issues are detected, preventing insecure or faulty code from being merged.

#### 4. What improvements were observed after applying the fixes?
- **Reliability:** Removing `eval()` eliminated a potential security vulnerability. Replacing the bare `except` with a specific one improved transparency in error handling, while adding file operation error handling made the program safer to run even if files were missing.
- **Accuracy:** Fixing the mutable list argument ensured each call to `addItem` maintains its own log, preventing cross-call interference and hidden bugs.
- **Readability:** The final codebase is more organized and consistent. Using `with open()` for file handling is cleaner, and adopting f-strings improved clarity and modernized the syntax.

