# AGENTS.md

This file provides essential commands and conventions for AI coding agents working with the Thymeleaf codebase.

## Build/Test Commands

```bash
# Full build with tests
mvn clean package

# Run all tests
mvn clean test

# Run single test class
mvn -Dtest=TemplateEngineTest test

# Run single test method
mvn -Dtest=TemplateEngineTest#testBasicExecution test

# Build specific module
cd lib/thymeleaf && mvn clean package

# Skip tests
mvn clean package -DskipTests

# Generate Javadoc
mvn javadoc:javadoc
```

## Architecture

Multi-module Maven project (Java 8+). Key modules:
- `lib/thymeleaf`: Core template engine
- `lib/thymeleaf-spring5`, `lib/thymeleaf-spring6`: Spring integrations
- `lib/thymeleaf-extras-springsecurity5/6`: Spring Security integrations
- `tests/`: Test suites
- `examples/`: Sample applications

Core packages: `engine/`, `context/`, `expression/`, `dialect/`, `processor/`, `templatemode/`, `templateresolver/`

## Code Style

- **Java version**: Java 8 minimum (no Java 9+ APIs)
- **Indentation**: 4 spaces (no tabs)
- **Line length**: Max 120 chars (recommended)
- **Encoding**: Pure ASCII for `.java`, ISO-8859-1 for `.properties`
- **Parameters**: Declare as `final`
- **Autoboxing**: Forbidden (explicit boxing/unboxing required)
- **Null checks**: Validate all non-nullable public method parameters
- **Constructors**: Always explicit with `super()` call
- **Javadoc**: Maintain for public methods, add `@author` for substantial changes
- **Comments**: All code/comments in English
- **Line endings**: UNIX-style (`\n`)
- **Copyright**: Include Apache 2.0 header in all files

## Testing

- Use JUnit 5 for new tests
- Test structure: `src/test/java`
- Unit tests recommended for new/modified code
