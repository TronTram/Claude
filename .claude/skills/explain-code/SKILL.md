---
name: explain-code
description: Explain code by showing visual call chains, role descriptions, and summaries. Use this skill whenever the user asks to explain a function, module, code flow, or how a piece of code works — even if they just say "explain this", "walk me through this", "how does this work", or "what does this do". Trigger broadly on any code explanation request.
---

# Code Explanation Skill

When explaining code, follow this structured format so the explanation is visual, layered, and easy to follow. The goal is to make complex code feel intuitive by showing *how data flows* through the system, *what each piece does*, and *why it matters* — all at a glance.

## The Formula

**Visual Flow (with relative paths) → Root Cause / Purpose → Role Descriptions → Summary**

Always follow this order. Each section builds on the previous one.

## 1. Visual Call Chain

Show the flow as an ASCII diagram using **relative paths from the project root**. This is the anchor of the explanation — readers should be able to trace the entire flow visually before reading any prose.

```
User Action / Entry Point
  ↓
`src/routes/order.py → create_order()`
  — receives order data from request ↓

`src/services/order_service.py → validate_and_create()`
  — validates business rules, enriches data ↓

`src/repositories/order_repo.py → save()`
  — persists to database
```

Rules for the diagram:
- Use **relative paths** from the project root (e.g. `src/services/foo.py`, not just `foo.py` or `/Users/me/project/src/services/foo.py`)
- Include the **function name** after the arrow: `file.py → function_name()`
- Add a brief **one-line description** of what each step does, prefixed with `—`
- Use `↓` arrows to show direction of flow
- If the flow branches (e.g. conditionals, error paths), show both paths

## 2. Root Cause / Purpose

Explain **why** this code exists or what problem it solves. This grounds the reader before diving into details. Keep it to 1-3 sentences.

Examples:
- "This handles the checkout flow — when a user submits their cart, it validates inventory, calculates totals, and creates the order record."
- "This function exists because raw API responses need to be normalized before the UI can render them."

## 3. Role Descriptions

Describe each layer's role using plain-language labels. This helps readers build a mental model of the architecture. Use labels like:

- **Entry point** — where the request comes in
- **Orchestrator** — coordinates multiple steps
- **Validator** — checks rules and constraints
- **Transformer** — reshapes data
- **Messenger** — passes data between layers
- **Final hand-off** — where the result lands (DB write, API response, etc.)

Format:
```
- `src/routes/order.py` — **Entry point**: receives the HTTP request and extracts parameters
- `src/services/order_service.py` — **Orchestrator**: validates rules, then delegates to the repository
- `src/repositories/order_repo.py` — **Final hand-off**: writes the order to the database
```

## 4. Summary

One sentence tying it all together. The reader should be able to read *only* this sentence and understand the gist.

Example: "A user's checkout request flows from the route handler through validation and pricing logic, ultimately persisting the order in the database."