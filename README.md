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
  - [Separation of Concerns](#separation-of-concerns)
  - [Design Quality](#design-quality)
    - [Coupling & Cohesion](#coupling--cohesion)
    - [Information Hiding](#information-hiding)
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

Conceptual integrity means designing and implementing software in a consistent manner — so that even if multiple people worked on it, it feels as if a single mind guided all the work.

> "It is better to have a system omit certain anomalous features and improvements, but to reflect one set of design ideas, than to have one that contains many good but independent and uncoordinated ideas." — Fred Brooks, *The Mythical Man-Month*

This does not mean developers cannot voice opinions. It means the team agrees on design principles and conventions, and follows them consistently throughout the project.

**Why it matters:**

Think of software as a building. Conceptual integrity is the consistency of its structure and design. Without a clear blueprint and a guiding architect, different workers may use different materials and structures — resulting in an unorganized, inconsistent, and potentially unstable system. A consistent codebase is easier to read, easier to extend, and easier to hand off to new team members.

**How to achieve it:**

| Approach | Description |
|---|---|
| Communication | Agile practices like daily stand-ups and sprint retrospectives help teams agree on libraries, methods, and naming conventions |
| Code reviews | Systematic peer examination of written code keeps developers consistent with each other and catches deviations early |
| Design principles & patterns | Using agreed interfaces and design patterns creates conventional, predictable class structures |
| Strong architecture | A well-defined underlying design guides how all parts of the system are organized and interact |
| Unifying concepts | Finding common ground between seemingly different things reduces special cases — e.g., in Unix, every resource is treated as a file, so the same operations apply everywhere |
| Core commit group | Restricting code merges to a small core group ensures all changes align with the overall architecture and design vision |

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

### Separation of Concerns

A **concern** is anything that matters in providing a solution to a problem. Separation of concerns is the principle of addressing each concern in its own dedicated section of the system, rather than tangling multiple responsibilities together.

> Think of a supermarket: butchering meat, baking bread, accepting payment, and stocking shelves are all handled by **separate departments** — each focused on its own concern. Well-designed software works the same way.

Separation of concerns is not a standalone rule — it is the underlying idea that runs through all four OO design principles:

| Principle | How it applies separation of concerns |
|---|---|
| **Abstraction** | Each concept in the problem space becomes its own abstraction with relevant attributes and behaviors |
| **Encapsulation** | Each abstraction is contained in its own class; implementation details are hidden behind an interface |
| **Decomposition** | A whole can be split into separate, focused parts |
| **Generalization** | Common traits are separated out and moved into a superclass |

---

#### Example — Smartphone Design

Consider a `SmartPhone` class that handles both camera and phone functionality in a single class:

```java
// Poor design — low cohesion, no modularity
class SmartPhone {
    // Camera attributes + behaviors
    // Phone attributes + behaviors
    // All mixed together
}
```

**Problems with this design:**
- Low cohesion — camera and phone behaviors are unrelated but bundled together.
- No modularity — the camera and phone cannot be accessed, reused, or replaced independently.
- Any change to one concern risks breaking the other.

---

**Better design:** Separate the two concerns into their own interfaces and implementing classes, and let `SmartPhone` act as a **coordinator**:

```java
// Define concerns as interfaces
interface Camera { void takePicture(); }
interface Phone  { void makeCall();    }

// Implement each concern independently
class FirstGenCamera    implements Camera { ... }
class TraditionalPhone  implements Phone  { ... }

// SmartPhone composes both — knows nothing about their internals
class SmartPhone {
    private Camera camera;
    private Phone  phone;

    public SmartPhone(Camera camera, Phone phone) {
        this.camera = camera;
        this.phone  = phone;
    }

    public void takePicture() { camera.takePicture(); }
    public void makeCall()    { phone.makeCall();     }
}
```

Now the `SmartPhone` class simply **delegates** to each component. The camera and phone know nothing about each other, but are composed together by the smartphone.

To swap the camera for a newer model, only the instantiation needs to change — the `SmartPhone` class itself is untouched:

```java
// Swapping components without touching SmartPhone
SmartPhone phone = new SmartPhone(new HDCamera(), new TraditionalPhone());
```

---

#### Trade-offs

Applying separation of concerns improves **cohesion** within each class, but introduces a trade-off:

| Effect | Description |
|---|---|
| ✅ Higher cohesion | Each class has a single, clear responsibility |
| ✅ More modularity | Components can be reused, swapped, or extended independently |
| ✅ Easier maintenance | Changes to one concern do not ripple into others |
| ⚠️ Increased coupling | `SmartPhone` now depends on the `Camera` and `Phone` interfaces |

The goal is not to eliminate coupling entirely, but to ensure that dependencies are on **interfaces** (stable contracts) rather than concrete implementations — keeping the system flexible.

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

#### Information Hiding

Information hiding is the practice of giving each module access to only the information it needs to do its job — and nothing more. It directly supports loose coupling by ensuring modules depend on stable interfaces rather than changeable implementation details.

Things that are likely to change (implementation details) should be hidden. Things that should stay stable (assumptions and contracts) should be revealed through interfaces.

This allows developers to work on a module independently — others can use it through its interface without needing to know how it works internally.

Information hiding is applied in practice through encapsulation and access modifiers.

##### Access Modifiers in Java

Java provides four levels of access control:

| Modifier | Accessible By |
|---|---|
| `public` | Any class in the system |
| `protected` | The encapsulating class, its subclasses, and classes in the same package |
| `default` (no keyword) | The encapsulating class and classes in the same package only |
| `private` | The encapsulating class only |

```java
public class Person {
    private String name;       // hidden — only Person can access this directly
    protected int age;         // accessible to subclasses and same-package classes
    public String nationality; // accessible by any class

    public void sleep() {      // behavior exposed through public interface
        // implementation hidden — callers invoke it but cannot change how it works
    }
}
```

> **Note:** A public method exposes a behavior to the outside world, but the implementation of that behavior remains hidden through encapsulation. Callers can invoke it — they cannot change how it works.

##### Why Information Hiding Matters

| Benefit | Description |
|---|---|
| **Flexibility** | Implementation details can change without affecting other modules |
| **Reusability** | Modules expose clean interfaces that others can depend on |
| **Maintainability** | Bugs are contained — internal changes don't ripple outward |
| **Parallel development** | Teams can work on separate modules simultaneously, relying only on agreed interfaces |

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
