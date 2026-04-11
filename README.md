# OO-AnalysisAndDesign

## Table of Contents
- [🏗️ Object-Oriented Analysis & Design](#oo-analysis-design)
  - [Foundations](#foundations)
  - [Design Process](#design-process)
    - [Stage 1 — Eliciting Requirements](#stage-1--eliciting-requirements)
    - [Stage 2 — Conceptual Design](#stage-2--conceptual-design)
    - [Stage 3 — Technical Design](#stage-3--technical-design)
    - [Conceptual Integrity](#conceptual-integrity)
  - [The Four Pillars of OO Design](#the-four-pillars-of-oo-design)
    - [1. Abstraction](#1-abstraction)
    - [2. Encapsulation](#2-encapsulation)
    - [3. Decomposition](#3-decomposition)
    - [4. Generalization](#4-generalization)
  - [Design Quality](#design-quality)
    - [Coupling & Cohesion](#coupling--cohesion)
    - [UML Diagram Types](#uml-diagram-types)
    - [Model Checking](#model-checking)

---

<a id="oo-analysis-design"></a>

## 🏗️ Object-Oriented Analysis & Design

This section documents the design philosophy, process, and principles used to build this system — from initial requirements through to technical implementation.

---

### Foundations

**Software Design** addresses lower-level concerns (individual components and their interactions), while **Software Architecture** handles the big picture — high-level structure, system organization, and quality trade-offs.

The architect acts as the bridge between clients and engineers, balancing competing attributes such as performance, security, and time to market.

**Core goals of good design:**

| Goal | Description |
|---|---|
| **Flexible** | Adapts to changing requirements without major rework |
| **Reusable** | Components can be used across different contexts |
| **Maintainable** | Easy to modify, extend, and fix over time |

> ⚠️ Skipping the design phase is a leading cause of project failure — approximately 13% of projects fail due to incomplete or poorly elicited requirements.

---

### Design Process

Design is iterative, moving from a fuzzy client vision down to concrete technical blueprints.

#### Stage 1 — Eliciting Requirements

Client needs are often vague. Requirements are uncovered through probing questions and structured using **User Stories**:

```text
As a [role], I want to [goal] so that [reason].
```

#### Stage 2 — Conceptual Design

High-level components and responsibilities are outlined without technical detail.

**CRC Cards (Class-Responsibility-Collaborator)** are used as a physical prototyping tool to simulate object interactions and refine class responsibilities before writing any code.

#### Stage 3 — Technical Design

Detailed blueprints are produced for developers to implement directly.

**UML Class Diagrams** specify:
- Class names
- Attributes (member variables / properties)
- Methods (operations)
- Relationships between classes

#### Conceptual Integrity

The system should feel as if it were designed by a single mind. This is maintained through consistent conventions and regular code reviews across the team.

---

### The Four Pillars of OO Design

#### 1. Abstraction

Real-world entities are simplified to only the attributes and behaviors relevant within the system's context. Irrelevant details are deliberately excluded.

> *Example: A grocery item is abstracted to just `name` and `price` — its colour or origin are irrelevant to the system.*

---

#### 2. Encapsulation

Data and behaviour are bundled into self-contained objects. Internal state is protected via **information hiding**:

| Access Modifier | Visibility |
|---|---|
| `public` | Accessible by any class |
| `protected` | Accessible within the class and its subclasses |
| `private` | Accessible only within the class itself |

**Getter** and **Setter** methods serve as controlled access points ("gates"), ensuring that internal data is only modified in valid, expected ways.

---

#### 3. Decomposition

Systems are broken into smaller, manageable parts. The nature of each part-whole relationship is expressed through three relationship types:

| Relationship | Strength | Description | Example |
|---|---|---|---|
| **Association** | Loose | Independent partnership — neither owns the other | Student ↔ Course |
| **Aggregation** | Weak "has-a" | Parts can exist independently of the whole | Crew ↔ Airliner |
| **Composition** | Strong "has-a" | Parts are destroyed when the whole is destroyed | House ↔ Room |

---

#### 4. Generalization

Common attributes and behaviors are factored into a **Superclass** (parent), which **Subclasses** (children) inherit — reducing redundancy and centralizing shared logic.

- **Inheritance** — Expresses an *"is-a"* relationship. In Java: `extends`.
- **Interfaces** — Define *what* an object can do (method signatures only), not *how*. In Java: `implements`. This enables **polymorphism** — the same behavior can be implemented differently by different classes.

```java
// Inheritance
class Dog extends Animal { ... }

// Interface + Polymorphism
interface Speakable { void speak(); }
class Cat implements Speakable { public void speak() { ... } }
class Dog implements Speakable { public void speak() { ... } }
```

> **Note on Java:** Multiple class inheritance is not supported (to avoid data ambiguity), but a class may implement multiple interfaces — achieving the same flexibility without conflict.

---

### Design Quality

#### Coupling & Cohesion

Two key metrics are used to evaluate design complexity. Since complexity applies to both classes and the methods within them, the term **module** is used to refer to any program unit.

> 💡 The average person can hold roughly **7 things** in short-term memory (Miller, 1956). Once design complexity exceeds what a developer can mentally handle, bugs become more frequent. Keeping modules simple is critical.

---

**Coupling** measures the complexity of connecting a module to other modules.

- **Tight coupling** — a module is highly reliant on others (hard to reuse or replace, like puzzle pieces).
- **Loose coupling** — a module connects easily to others (interchangeable and flexible, like Lego blocks).

**Aim for loose (low) coupling.** When evaluating coupling, consider three factors:

| Factor | Definition | Goal |
|---|---|---|
| **Degree** | Number of connections between the module and others | Keep it small — few parameters or narrow interfaces |
| **Ease** | How obvious the connections are | Understandable without reading the implementation |
| **Flexibility** | How interchangeable the connected modules are | Other modules should be easily replaceable |

---

**Cohesion** measures the clarity of purpose *within* a module.

- **High cohesion** — the module performs one task with a single, clear responsibility.
- **Low cohesion** — the module serves more than one purpose or has an unclear role.

**Aim for high cohesion.** If a module has more than one responsibility, it should be split.

---

#### Example — Refactoring for Cohesion & Coupling

**❌ Poor design:** A single `Sensor` class handles both humidity and temperature via a flag parameter:

```java
// Low cohesion — unclear purpose
// Tight coupling — caller must know internal flag values
sensor.get(0); // humidity?
sensor.get(1); // temperature?
```

The `get(flag)` method hides behavior behind a control flag, forcing callers to break encapsulation just to use it. This produces **low cohesion** and **tight coupling** at the same time.

**✅ Better design:** Split into two focused classes:

```java
// High cohesion — one clear purpose each
// Loose coupling — intent is obvious, no hidden flags
humiditySensor.get();    // clearly returns humidity
temperatureSensor.get(); // clearly returns temperature
```

Each class now has a single well-defined responsibility, and callers no longer need to understand internals to use them correctly.

---

#### The Coupling–Cohesion Trade-off

These two metrics exist in tension with each other:

- Simplifying a module to achieve **high cohesion** may increase its dependence on other modules → raises coupling.
- Simplifying connections to achieve **low coupling** may force a module to take on more responsibilities → lowers cohesion.

Good design finds the right balance — distributing responsibility across modules without creating excessive interdependencies.

---

#### UML Diagram Types

Beyond class diagrams, two additional UML diagrams are used to capture dynamic behavior:

| Diagram | Purpose |
|---|---|
| **Sequence Diagram** | Shows how objects interact and exchange messages over time |
| **State Diagram** | Maps the states an object can be in and the events that trigger transitions between them |

---

#### Model Checking

A formal verification process that systematically examines all possible system states to detect flaws such as:
- **Deadlocks** — where two or more processes block each other indefinitely
- **Race conditions** — where the system's behavior depends on unpredictable timing of events
