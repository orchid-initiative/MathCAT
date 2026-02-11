# Contributing to MathCAT

Thank you for your interest in contributing to MathCAT! This project makes math accessible to people who use screen readers and braille displays, and every contribution helps expand that access.

MathCAT is maintained by [Neil Soiffer](https://github.com/NSoiffer) under the [DAISY Consortium](https://daisy.org/mathcat). Contributions are welcome from everyone — whether you want to add a new language translation, fix a bug in the Rust code, improve tests, or help with documentation.

## How to Get Started

### 1. Set Up Your Environment

**Prerequisites:**
- [Git](https://git-scm.com/downloads)
- [Rust](https://www.rust-lang.org/tools/install) (for building and testing)
- [Python 3 + uv](https://docs.astral.sh/uv/) (only if working with the translation audit tool)

**Clone and build:**

```bash
git clone https://github.com/daisy/MathCAT.git
cd MathCAT
cargo build
```

**Run the tests:**

```bash
cargo test
```

This runs the full test suite (~183 test files covering speech, braille, and language output). It takes roughly 90 seconds. You can run a subset of tests by name:

```bash
# Run only English language tests
cargo test Languages::en

# Run a specific test by name
cargo test test_simple_fraction
```

### 2. Pick Your Contribution Path

MathCAT has several types of contributions, each with different skill requirements:

| Path | Skills needed | Difficulty | Where to start |
|------|--------------|------------|----------------|
| **Translation** | Fluency in a language + basic YAML | Beginner | [Translator Guide](docs/helpers.md#language-translators) |
| **Braille code** | Knowledge of a braille math code + YAML | Intermediate | [Braille Guide](docs/helpers.md#braille-translators) |
| **YAML rules** | Understanding of MathCAT's rule engine | Intermediate | [Rule format docs](docs/helpers.md#the-basic-parts-of-a-speech-rule) |
| **Rust code** | Rust programming | Advanced | [Developer Guide](docs/developers.md) |
| **Documentation** | Clear writing | Beginner | This file + `docs/` directory |

### 3. Make Your Changes

1. **Fork** the MathCAT repository on GitHub.
2. **Create a branch** for your work (e.g., `add-french-translation`, `fix-fraction-speech`).
3. Make your changes.
4. **Run the tests** to make sure nothing is broken: `cargo test`
5. **Commit** with a clear message describing what you changed and why.
6. **Open a pull request** against the `main` branch.

## Contribution Paths in Detail

### Translations

MathCAT supports speech output in multiple languages. Adding or improving a translation is one of the most impactful contributions you can make — libraries and schools around the world need MathCAT in their language.

**To start a new translation:**
1. Contact [@NSoiffer](https://github.com/NSoiffer) — Neil can generate initial translation files that save significant time. These use existing translations from MathPlayer, SRE, and machine translation as a starting point.
2. Your translation files go in `Rules/Languages/xx/` where `xx` is your language code (e.g., `fr`, `de`, `el`).
3. Work through the files in this order:
   - `definitions.yaml` — number words (cardinal, ordinal)
   - One speech style (`SimpleSpeak_Rules.yaml` or `ClearSpeak_Rules.yaml`) plus `SharedRules/`
   - `unicode.yaml` — the ~270 most common math symbols
   - `unicode-full.yaml` — thousands of less common symbols (do what you can)
   - `navigate.yaml` — navigation prompts

**Translation conventions:**
- Lowercase keys (`t:`, `ot:`, `ct:`) mean "not yet verified"
- Uppercase keys (`T:`, `OT:`, `CT:`) mean "translated and verified"
- Verify each translation, then change the key to uppercase

**Testing your translation** is required before it can be merged. See the [translator testing guide](docs/helpers.md#automatic-tests-for-your-translation) for instructions on adapting the English tests for your language.

**Translation audit tool:** Moritz Gross has built a tool to compare translations against the English source files and flag gaps. See [audit_translations README](PythonScripts/audit_translations/README.md) for usage:

```bash
uv run --project PythonScripts audit-translations --list    # see available languages
uv run --project PythonScripts audit-translations es         # audit Spanish
```

For the full translator guide, see [docs/helpers.md](docs/helpers.md#language-translators).

### YAML Rule Changes

MathCAT's speech and braille output is driven by YAML rule files. If you find a case where MathCAT speaks or brailles something incorrectly, the fix is often a rule change rather than a Rust code change.

Before making rule changes, read the [rule format documentation](docs/helpers.md#the-basic-parts-of-a-speech-rule) to understand the structure.

### Rust Code Changes

For changes to MathCAT's core Rust code, see the [Developer Guide](docs/developers.md).

**Key source files:**
- `src/` — core library code
- `build.rs` — build configuration

**Important:** MathCAT is used by screen readers (JAWS, NVDA) and braille translation software (BrailleBlaster) in production. Changes to core logic should be well-tested. When in doubt, open an issue to discuss your approach before writing code.

## Pull Request Guidelines

- **One concern per PR.** A translation update, a bug fix, and a new test should be separate PRs.
- **Include tests.** Bug fixes should include a test that fails without the fix. New features should include tests for expected behavior.
- **Describe your changes.** Explain what you changed and why. For translations, note which files you translated and whether the translations are verified (uppercase keys).
- **Keep PRs focused.** Don't mix style changes with functional changes.

## Communication

- **GitHub Issues:** For bug reports, feature requests, and questions.
- **DAISY MathCAT mailing list:** For broader discussions about the project direction. Sign up at [daisy.org/mathcat](https://daisy.org/mathcat).
- **Working group meetings:** The MathCAT project working group meets regularly. Contact the DAISY Consortium for meeting details.

## Project Structure Overview

```
MathCAT/
  src/              # Rust source code
  tests/            # Test files (organized by language/braille)
  Rules/
    Languages/      # Speech rules per language (en/, fr/, de/, ...)
    Braille/        # Braille rules per code (Nemeth/, UEB/, ...)
    Intent/         # Language-independent intent inference rules
  docs/             # Documentation (users, callers, helpers, developers)
  PythonScripts/    # Translation audit tool and other utilities
```

## Questions?

If you're unsure where to start or need help, [open an issue](https://github.com/daisy/MathCAT/issues) or reach out on the mailing list. We're happy to help new contributors find their way.
