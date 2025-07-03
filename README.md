# Blaze Schema Compiler

The Blaze Schema Compiler provides an API for compiling JSON Schemas into a form that can be used for efficient validation. This document covers the usage of the compiler, its API overloads, important terms, and practical examples.

## Table of Contents

- [Overview](#overview)
- [Key Terms](#key-terms)
- [API Reference](#api-reference)
  - [compile(String schema)](#compilestring-schema)
  - [compile(String schema, String defaultDialect)](#compilestring-schema-string-defaultdialect)
- [Usage Examples](#usage-examples)
- [Best Practices](#best-practices)
- [See Also](#see-also)

## Overview

The Blaze Schema Compiler allows you to compile JSON Schema documents into `CompiledSchema` objects. These objects can then be used for fast and repeated validation of JSON data. The compiler manages resources internally using an `Arena` to ensure efficient memory usage.

## Key Terms

- **JSON Schema**: A declarative language for validating the structure of JSON data.
- **CompiledSchema**: An optimized, pre-processed version of your JSON Schema, ready for validation.
- **Arena**: A memory/resource management context used internally to manage the lifecycle of compiled schemas.
- **Dialect**: The specific version or variant of the JSON Schema specification (e.g., Draft 2020-12).

## API Reference

### `compile(String schema)`

**Compiles a JSON schema string using the default dialect specified within the schema (if present)**.
```java
public static CompiledSchema compile(String schema)

```
- **Parameters:**
  - `schema`: The JSON schema as a string.
- **Returns:** `CompiledSchema` — the compiled schema instance.
- **Behavior:** Internally creates and manages an `Arena` for resource management. Uses the `$schema` property in the schema to determine the dialect.

---

### `compile(String schema, String defaultDialect)`

**Compiles a JSON schema string, allowing you to specify a default dialect if the schema does not declare one**.

@return A compiled schema
*/
```java
public static CompiledSchema compile(String schema, String defaultDialect)
```
- **Parameters:**
  - `schema`: The JSON schema as a string.
  - `defaultDialect`: The default dialect URI to use if the schema does not specify one.
- **Returns:** `CompiledSchema` — the compiled schema instance.
- **Behavior:** Internally creates and manages an `Arena`. If the schema does not specify a `$schema` property, `defaultDialect` is used.

---
## Usage Examples

### Example 1: Compiling with Schema-Declared Dialect
```java
String schemaJson = "{"
+ ""$schema": "https://json-schema.org/draft/2020-12/schema\","
+ ""type": "string""
+ "}";

try (CompiledSchema schema = Blaze.compile(schemaJson)) {
BlazeValidator validator = new BlazeValidator();

text
// Validate a string
boolean validStringResult = validator.validate(schema, "\"hello\"");
System.out.println("Validation result for \"hello\": " + validStringResult);
assertTrue(validStringResult);
}

```
### Example 2: Compiling with Explicit Default Dialect
```java
String schemaJson = "{"
+ ""type": "string""
+ "}";

String defaultDialect = "https://json-schema.org/draft/2020-12/schema";

try (CompiledSchema schema = Blaze.compile(schemaJson, defaultDialect)) {
BlazeValidator validator = new BlazeValidator();

boolean validStringResult = validator.validate(schema, "\"world\"");
System.out.println("Validation result for \"world\": " + validStringResult);
assertTrue(validStringResult);
}

```

## Best Practices

- **Always close `CompiledSchema`**: Use try-with-resources to ensure proper cleanup.
- **Specify dialect when in doubt**: If your schema does not declare a `$schema` property, always provide a `defaultDialect` to avoid ambiguity.
- **Validate input types**: Ensure the JSON data you validate matches the schema's expectations.

## See Also

- [Evaluator Documentation](./evaluator.md)
- [JSON Schema Specification](https://json-schema.org/specification.html)

---

**Happy validating! If you encounter issues, please open an issue or contribute via pull requests.**
