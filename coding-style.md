# C++ Coding Style and Conventions Guide

This document outlines the coding standards and best practices for C++ projects targeting **C++11 to C++26** standards.

Adherence to these guidelines try to ensure code is correct, readable, maintainable, and highly performant.

## I. General Principles

1.  **Readability and Maintainability:** Code must be clear, self-documenting, and easy to maintain.
    *   Group sub-expressions with parentheses `()` to avoid any ambiguity regarding operator precedence.
    *   Use brace-initialization `{}` for zero-initialization of variables. Use `()` for explicit constructor calls.

2.  **Modern C++ and Standard Library Usage:**
    *   Leverage the standard library's algorithms and data structures instead of creating custom implementations for common functionalities.
    *   Always use the `std::` namespace prefix for all standard library types, including fundamental types like `std::size_t`, `std::uint8_t`, and `std::int64_t`.
    *   Use modern C++ features (`if constexpr`, `std::format`, `std::span`, spaceship operator `<=>`, etc.) to improve type safety, performance, and clarity.

3.  **Minimalism and Expressiveness:** Strive for concise code that leverages C++ features to eliminate boilerplate and clearly express intent. Minimalism should enhance readability, not obscure it with overly clever or terse syntax.
    *   **Leverage Modern Initialization:** Use uniform initialization `{}` to return default-constructed or empty-state objects where applicable. This is more concise and expressive than alternatives.
        *   For `std::optional`, prefer `return {};` over `return std::nullopt;`.
        *   For containers, prefer `return {};` over `return std::vector<T>();`.
    *   **Defaulted and Deleted Functions:** Use `= default` and `= delete` to explicitly control the generation of special member functions, letting the compiler provide the implementation where appropriate.
    *   **Avoid Redundant Abstractions:** Do not create a complex class hierarchy when a simple `struct` or a free function will suffice.

4.  **Correctness and Robustness:** Prioritize code that is verifiably correct.
    *   **Const Correctness:** All variables, parameters, and member functions that are not intended to be modified **must** be declared `const`.
    *   **Exception Safety:** All functions **must** have a correct `noexcept` specifier. Functions that can fail due to invalid input or runtime conditions throw exceptions and are marked `noexcept(false)`. Functions that handle errors via other means (e.g., return codes) and are guaranteed not to throw are marked `noexcept`.

## II. Naming Conventions

A strict `snake_case` convention is enforced for all identifiers, with the exception of template parameters.

| Entity Type | Convention | Example |
| :--- | :--- | :--- |
| **Namespaces** | `lowercase_with_underscores` | `project_name::data_model` |
| **Classes & Structs** | `lowercase_with_underscores` | `widget_manager`, `coordinate` |
| **Functions & Methods** | `lowercase_with_underscores` | `calculate_total_value`, `parse_input` |
| **Variables & Data Members**| `lowercase_with_underscores` | `item_count`, `is_valid` |
| **Private Data Members** | `lowercase_with_underscores_` (trailing underscore) | `buffer_`, `current_index_` |
| **Constants (`constexpr`)**| `lowercase_with_underscores` | `max_iterations`, `default_tolerance` |
| **Type Aliases (`using`)**| `lowercase_with_underscores_t` | `value_t`, `id_t` |
| **Template Parameters** | `PascalCase` | `template <typename T, typename Allocator>` |

## III. Formatting and Layout

1.  **Indentation:** Use **4 spaces** for each indentation level. The use of tabs is forbidden.

2.  **Brace Style:** Use the **Allman brace style**, where the opening brace `{` is placed on a new, aligned line. Braces are **exclusively** used for scopes containing multiple statements. For single-statement blocks within control structures (`if`, `for`, `while`, etc.), braces **must be omitted**.

3.  **Spacing:**
    *   Use a single space around binary and ternary operators.
    *   Do not place a space between a function name and its opening parenthesis.
    *   Place a single space after control-flow keywords (`if`, `for`, `while`).
    *   Do not place a space after a cast.

4.  **Header Organization:**
    *   Use `#pragma once` for include guards.
    *   Use `#ifndef PCH` guards to support optional precompiled headers.
    *   Organize includes logically: project-specific headers, then standard library headers.
    *   Use forward declarations where possible to minimize header dependencies.

## IV. API and Language Design

1.  **Error Handling:**
    *   **Exceptions (`noexcept(false)`):** Use for reporting precondition violations (e.g., invalid arguments) and unrecoverable runtime errors (e.g., convergence failure, out-of-range values).
    *   **Return Codes (`noexcept`):** Use for performance-critical functions where failure is a predictable and frequent outcome. An `enum class` should define the possible error states.

2.  **Data Structures:**
    *   Use `struct` for simple aggregate data types (Plain Old Data).
    *   Use `class` when invariants must be maintained through a public interface with private data members.

3.  **Namespaces:**
    *   **Hierarchy:** Structure namespaces hierarchically from general to specific concepts (e.g., `kmx::gis::coordinate::wgs84`).
    *   **Unique Naming:** Words within a namespace hierarchy should be unique.
    *   **Anonymous Namespaces:** The use of anonymous namespaces is forbidden. Use a nested `detail` or `internal` namespace for internal-linkage entities.
    *   **Inline Namespaces:** Use `inline` namespaces for versioning or to export a specific set of functionality from a nested implementation namespace, making it part of the parent's interface.

4.  **`std::hash` Specialization:** Provide a specialization of `std::hash` for any custom type intended to be used as a key in an unordered associative container.

## V. Documentation

Thorough documentation is mandatory for all code.

1.  **API Documentation:** All public declarations (namespaces, types, functions, and public members) **must** be documented using Doxygen.
    *   Use triple-slash (`///`) comments for Doxygen blocks.
    *   Use Doxygen's `@` commands for structured documentation.

2.  **Required Doxygen Tags:**
    *   `@brief`: A concise one-line summary.
    *   `@param`: For every function parameter.
    *   `@tparam`: For every template parameter.
    *   `@return`: Description of the return value.
    *   `@throws`: For each type of exception a function can throw.
    *   `@note`, `@warning`: For important remarks or potential issues.
    *   `@reference`: To cite external standards, documents, or sources.

3.  **Implementation Comments:** Use standard `//` comments within implementation files to clarify complex algorithms, non-obvious logic, or the purpose of "magic" constants. Comments should explain the *why*, not re-state the *what*.

---
Copyright © 2025 - present KMX Systems. All rights reserved.
