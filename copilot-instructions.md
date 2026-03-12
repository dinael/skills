# 🧠 Copilot Instructions — Main Manifest

This file acts as the **configuration core** for your AI Skills system. Here you define which skills the AI must load before executing any request.

Every time you give an instruction such as:

``` text
@workspace follow the instructions in copilot-instructions.md and create X
```

The AI will:

1. Open this file.
2. Load all the skills listed below.
3. Apply those rules to your request.

---

## 📘 List of Active Skills

Include here all the skills you want the AI to use automatically.

> **Important:** Each line must point to a `.md` file containing clear rules.

``` text
skills/component-driven-development.md
skills/typescript-pro.md
skills/accessibility-guidelines.md
skills/design-system-architecture.md
skills/testing-expert.md
```

Add or remove skills as your project evolves.

---

## 🧩 How it works

Each skill is a module that defines:

- Development patterns
- Technical standards
- Best practices
- Folder structures
- Design rules
- Accessibility rules
- Documentation guidelines
- Anything you want the AI to follow consistently

---

## ➕ How to add new skills

1. Create a file inside `/skills/`, e.g.: `skills/performance-optimization.md`.
2. Add it to the list:

``` text
skills/performance-optimization.md
```

The AI will automatically use it in the next request.

---

## 🪄 Usage example

``` text
@workspace follow the instructions in copilot-instructions.md and create an accessible and documented Login component.
```

The AI will apply **all** listed skills without you having to mention them.

---

## ✔️ Recommendation

Keep this file simple: only list skills and define how they should be interpreted.

Complex content should live inside each individual skill.
