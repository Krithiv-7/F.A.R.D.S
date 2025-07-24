# 🤝 Contributing to F.A.R.D.S
Welcome to the **Fire Analysis and Response Drone System (F.A.R.D.S)** project! 🚁🔥  
We’re excited you’re here and want to contribute. Whether it's code, design, documentation, or ideas — **all contributions are welcome**.

---

## 📌 Table of Contents
- [Getting Started](#-getting-started)
- [How to Contribute](#-how-to-contribute)
- [Code Style Guide](#-code-style-guide)
- [Branch Naming](#-branch-naming)
- [Commit Messages](#-commit-messages)
- [Pull Request Process](#-pull-request-process)
- [Reporting Issues](#-reporting-issues)
- [Community Conduct](#-community-conduct)

---

## 🚀 Getting Started

1. **Fork the repository** and clone it locally:
   ```bash
   git clone https://github.com/your-username/fards.git
   cd fards
   ```

2. **Set up your environment** using:
   - `docs/SETUP.md` for backend setup
   - `docs/AI_MODEL.md` for ML model environment
   - `docs/DRONE_SETUP.md` for drone simulation

3. Install dependencies:
   ```bash
   # Example (customize per section)
   pip install -r requirements.txt
   npm install --prefix dashboard/
   ```

4. Make sure everything works:
   ```bash
   python main.py
   ```

---

## 💡 How to Contribute

You can help by:
- 🐛 Reporting bugs
- 🚀 Proposing new features
- 📦 Adding functionality
- 🧠 Improving AI accuracy
- 🛠️ Enhancing documentation
- 🎨 Designing the UI/UX

---

## 🎯 Code Style Guide

Follow these conventions:
- **Python:** PEP8
- **JS/TS:** Prettier + ESLint
- **Markdown:** Wrap at 120 chars, consistent formatting
- **Comments:** Explain logic clearly, especially in AI/model code

Run formatters:
```bash
black .         # Python
prettier --write .   # JS/TS
```

---

## 🌱 Branch Naming

Use the following structure:

| Type      | Prefix     | Example                  |
|-----------|------------|--------------------------|
| Feature   | `feat/`    | `feat/detection-module`  |
| Bug Fix   | `fix/`     | `fix/api-timeout`        |
| Docs      | `docs/`    | `docs/setup-readme`      |
| Refactor  | `refactor/`| `refactor/ui-cleanup`    |
| Test      | `test/`    | `test/model-inference`   |

---

## ✍️ Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/) style:

```
<type>(scope): short summary

[optional body]
[optional footer]
```

Examples:
```
feat(dashboard): add map-based fire alert view
fix(api): resolved 500 error on drone POST
docs(readme): update setup instructions
```

---

## 🔁 Pull Request Process

1. Push your branch:  
   ```bash
   git push origin feat/new-feature
   ```

2. Open a pull request on GitHub:
   - Use clear PR title and description
   - Link related issues (e.g. `Closes #5`)
   - Include screenshots or logs if needed

3. Wait for review and CI checks.

---

## 🐞 Reporting Issues

Use the [Issues tab](https://github.com/your-org/fards/issues) to report:
- Bugs 🐛
- Crashes or errors 💥
- Suggestions or feature requests 💡

Follow the issue template when available.

---

## 🌐 Community Conduct

We follow a **Code of Conduct** inspired by [Contributor Covenant](https://www.contributor-covenant.org/).  
Be respectful, inclusive, and supportive of each other.

---

Thank you for helping build **F.A.R.D.S** — let's make wildfire response smarter and safer together 🔥🛰️
