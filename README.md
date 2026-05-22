# base-python-architecture

A reusable Python project template based on Clean Architecture (also known as Hexagonal Architecture). The goal of this repository is to provide a solid, opinionated starting point that can be cloned and extended for any new Python project without having to redesign the folder structure from scratch each time.

## Purpose

Starting a new project always involves the same structural decisions: where does business logic live, how are external dependencies isolated, how are entry points organized. This template encodes those decisions once so future projects can focus on the domain rather than the scaffolding.

## Architecture

The project is organized into four layers, each with a clearly defined responsibility. Dependencies flow inward: outer layers depend on inner ones, never the other way around.

```
base-python-architecture/
├── domain/
│   ├── entities/        # Core business objects, no external dependencies
│   └── exceptions/      # Domain-specific exceptions
├── application/
│   ├── use_cases/       # Orchestration of domain logic
│   └── ports/           # Abstract interfaces (input/output boundaries)
├── infrastructure/
│   ├── repositories/    # Concrete implementations of domain ports
│   └── models/          # ORM models or data-layer representations
└── interface/
    └── api/             # Entry points (REST, CLI, events, etc.)
```

### Layer responsibilities

**domain** — The heart of the application. Contains entities and business rules. Has no knowledge of frameworks, databases, or external services.

**application** — Coordinates use cases by orchestrating domain objects. Defines ports (abstract interfaces) that infrastructure must implement. This layer is also framework-agnostic.

**infrastructure** — Implements the ports defined in the application layer. All I/O concerns live here: database access, external APIs, caching, messaging.

**interface** — The outermost layer. Adapts incoming requests (HTTP, CLI, queues) into calls to the application layer and formats responses.

## Usage

Clone the repository and rename it to match your project:

```bash
git clone <repo-url> my-project
cd my-project
```

Then:

1. Replace placeholder `__init__.py` files with actual code starting from the `domain` layer.
2. Define your entities and exceptions in `domain/`.
3. Write use cases in `application/use_cases/` and declare the interfaces they need in `application/ports/`.
4. Implement those interfaces in `infrastructure/repositories/`.
5. Wire everything together and expose it in `interface/api/`.

## Principles

- **Dependency inversion**: the domain and application layers never import from infrastructure or interface.
- **Testability**: business logic can be tested without a database or HTTP server by substituting port implementations with in-memory fakes.
- **Replaceability**: swapping a database engine or a web framework only requires changes in the outermost layers.

## Requirements

- Python 3.10+

Dependencies are intentionally left out of this template. Add your own (`requirements.txt`, `pyproject.toml`, etc.) once you start building on top of it.
