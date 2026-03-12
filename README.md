
# 🧠 IA Skills System — README

Modular skills system to standardize and automate collaboration with AI

## 📌 Project Description

This repository contains a centralized system of **AI Skills**, designed so that any agent (such as Copilot) can work while following your rules, development patterns, and technical standards **without requiring manual intervention in each request**.

The main goal is to allow you to **define your guidelines, workflows, and conventions once**, and have the AI automatically apply them in all future developments.

This system is based on a root file called **`copilot-instructions.md`**, which acts as the **central manifest** for all your skills.

---

## 🚀 How this system works

### 1. `copilot-instructions.md` → The brain of the system

This is the main file listing **all the skills, rules, and standards** that you want the AI to apply in each request.

The AI automatically performs the following steps:

1. Opens `copilot-instructions.md`.
2. Reads all linked skills (e.g., `component-driven-development.md`, `typescript.md`, etc.).
3. Builds a **complete context** based on your rules.
4. Executes your request applying **all** those rules without you having to repeat them.

---

## 🧩 Advantages of the system

### ✔️ Less manual intervention

You only need to give instructions like:

``` text
@workspace follow the instructions in copilot-instructions.md and generate a login component.
```

The AI will automatically apply:

- CDD (Component-Driven Development)
- Strict TypeScript
- Architecture patterns
- DX best practices
- Your Design System standards
- Accessibility
- Testing
- And all defined skills

---

### ✔️ Absolute consistency

All generated components, functions, documentation, or scripts follow the same standards.

### ✔️ Unlimited scalability

If you create new skills:

- Simply add them to `copilot-instructions.md`.
- The AI will use them immediately in the next request.

No extra configuration needed.

---

## 📂 Recommended project structure

``` text
/
├── copilot-instructions.md        # Main manifest
├── skills/
│   ├── component-driven-development.md
│   ├── typescript.md
│   ├── accessibility.md
│   ├── design-system-architecture.md
│   └── ...other skills
└── README.md
```

You can organize your skills into thematic folders as the system grows.

---

## 🛠️ How to use the system

### 1. Write your request referencing the manifest

Example:

``` text
@workspace follow the instructions in copilot-instructions.md and generate an accessible form.
```

### 2. The AI will automatically

- Load the manifest
- Load all associated skills
- Apply all standards
- Generate the final output

You only give the command.
The AI applies your entire development environment.

---

## ✨ Adding new skills

To expand your capabilities:

1. Create a skill under `/skills/skill-name.md`.
2. Add it as a new line inside `copilot-instructions.md`.

Example:

``` text
skills/testing-expert.md
```

And that's it — it is now active for all future requests.

---

## 💡 Practical examples

### Generate a component

``` text
@workspace follow the instructions in copilot-instructions.md and create a responsive Header for the Design System.
```

### Document a component

``` text
@workspace use the full context described in copilot-instructions.md and create the MDX documentation for the Switch component.
```

### Create a script

``` text
@workspace apply the standards in copilot-instructions.md and generate a bash script to publish packages in a monorepo.
```

---

## 🧱 Project objectives

- Reduce friction when working with AI
- Centralize standards, guidelines, and best practices
- Increase consistency in code, design, and architecture
- Scale a rule system that evolves with your expertise

---

## 📜 License

Include any license you prefer (MIT, Apache 2.0, etc.).
