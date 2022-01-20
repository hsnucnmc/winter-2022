---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# GitOps

siriuskoan

---

# Outline
- Introduction
- Priciples
- Pros & Cons

---

# Introduction

GitOps = IaC + MRs/PRs + CI/CD

It is a concept, and developers can choose to follow this concept to develop an application.

It is first pioneered by weaveworks in 2017.

<!--

IaC: Infrastructure as Code

-->

---

# Priciples

- The entire system described declaratively.
- The canonical desired system state versioned in Git.
- Approved changes that can be automatically applied to the system.
  - There is a controller watching the git repo (desired state) and make the current state migrate to desired state.
- Software agents to ensure correctness and alert on divergence.
  - Self-healing for human error.

<!--

Declaratively: We don't specify how to do it, instead, we just say what we want.

-->

---

# Pros & Cons

Pros
- Only use git to control version.
  - Developers can use familiar tools.
  - When debugging, the developers only need to check out the code in git.
  - Other pros of git, such as security and history tracking.
- The repo is the only standard, so if anyone modify the pods illegally, the controller can restore it.

---

# Pros & Cons

Cons
- When the scale gets larger, the repo(s) get more complex.
  - There may be many teams, and each of them may have many environments (at least production and development).
  - This may make the repo too big (all in one) or too many (one repo each environment per team), but both of them still have their problems.
- Developers need to solve git version conflicts (needs git pull) manually.
- Hard to audit.
  - If we want to audit a component, the developers have to track back which repo the component is in, find the config file, and finally look at the history.
  - More tools are needed, but it contradicts the simplicity of GitOps.

