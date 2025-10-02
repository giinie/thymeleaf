# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Thymeleaf is a modern server-side Java template engine for both web and standalone environments. This is a multi-module Maven project with the following key modules:

- **lib/thymeleaf**: Core template engine
- **lib/thymeleaf-spring5**: Spring Framework 5.x integration
- **lib/thymeleaf-spring6**: Spring Framework 6.x integration
- **lib/thymeleaf-extras-springsecurity5**: Spring Security 5.x integration
- **lib/thymeleaf-extras-springsecurity6**: Spring Security 6.x integration
- **lib/testing/**: Testing framework modules
- **tests/**: Test suites for core and integrations
- **examples/**: Sample applications
- **dist/**: Distribution assembly

## Build Commands

### Full Build
```bash
mvn clean package
```
Builds all modules, runs tests, and generates a distribution archive in `dist/target/`.

### Build Specific Module
```bash
# Core engine only
cd lib/thymeleaf && mvn clean package

# Spring 6 integration
cd lib/thymeleaf-spring6 && mvn clean package
```

### Run Tests
```bash
# All tests
mvn clean test

# Specific module tests
cd tests/thymeleaf-tests-core && mvn test

# Single test class
mvn -Dtest=TemplateEngineTest test

# Single test method
mvn -Dtest=TemplateEngineTest#testBasicExecution test
```

### Skip Tests
```bash
mvn clean package -DskipTests
```

### Generate Javadoc
```bash
mvn javadoc:javadoc
```

## Architecture

### Core Template Engine (`lib/thymeleaf`)

The core engine is organized into several key packages:

- **engine/**: Template processing engine implementation (parsing, execution, model manipulation)
- **context/**: Template execution contexts and variable scopes
- **expression/**: Expression evaluation system (OGNL, SpringEL support)
- **dialect/**: Dialect system for extending template functionality
- **cache/**: Template and expression caching infrastructure
- **model/**: Template model representation (DOM-like structure)
- **templatemode/**: Template modes (HTML, XML, TEXT, JAVASCRIPT, CSS, RAW)
- **templateresolver/**: Template resolution and loading
- **templateparser/**: Template parsing infrastructure
- **processor/**: Template processor interfaces and base implementations
- **exceptions/**: Exception hierarchy

### Spring Integration

Spring integration modules follow parallel structure:
- `spring5`/`spring6`: Core Spring integration (view resolvers, reactive support)
- `extras-springsecurity5`/`extras-springsecurity6`: Security dialect for authentication/authorization

### Key Design Patterns

1. **Template Processing Pipeline**: Templates are parsed into model → processed by dialect processors → rendered to output
2. **Dialect System**: Extensibility through dialects that contribute processors, expression objects, and configuration
3. **Dual-Mode Support**: Both Jakarta EE and legacy javax.* APIs supported for servlet/JSP/validation
4. **Reactive Support**: Spring 6 integration includes reactive template processing with Project Reactor

## Code Conventions

### Java Requirements
- **Minimum Java version**: Java 8
- All code must compile with Java 8 source/target compatibility
- Avoid Java 9+ specific APIs in core modules

### Code Style
- **Indentation**: 4 spaces (no tabs)
- **Line length**: Maximum 120 characters (recommended)
- **Line endings**: UNIX-style (`\n`)
- **Encoding**: Pure ASCII for `.java` files, ISO-8859-1 for `.properties` files
- **Method parameters**: Declare as `final`
- **Constructors**: Always explicit, including `super()` call
- **Autoboxing**: Forbidden (explicit boxing/unboxing required)
- **Null checks**: Validate all non-nullable public method parameters

### Naming
- Follow standard Java Code Conventions
- All code, comments, documentation in English
- Meaningful variable/method/class names

### Documentation
- Maintain existing Javadoc when modifying code
- Add Javadoc for new public methods
- Add yourself as `@author` for substantial changes/additions

### Copyright Headers
- All contributed files must include standard Thymeleaf copyright header (Apache 2.0)

## Testing

- Unit tests welcome for new/modified code
- For new algorithms or substantial modifications, unit tests may be required
- Test classes follow standard Maven structure: `src/test/java`
- Use JUnit 5 (Jupiter) for new tests

## Module Dependencies

When working across modules, note these dependency relationships:
- Spring integration modules depend on `thymeleaf` core
- Spring Security extras depend on corresponding Spring integration modules
- Testing modules provide test utilities for dialect/integration testing
- All modules share parent POM configuration from root `pom.xml`

## Common Development Workflows

### Adding a New Processor
1. Create processor class in appropriate dialect module
2. Extend appropriate base processor class from `org.thymeleaf.processor`
3. Register in dialect configuration
4. Add unit tests in corresponding test module
5. Update Javadoc

### Modifying Template Engine Behavior
1. Identify affected package in `lib/thymeleaf/src/main/java/org/thymeleaf`
2. Understand interaction with engine pipeline (parsing → processing → rendering)
3. Consider cache invalidation impacts
4. Test with multiple template modes (HTML, XML, TEXT, etc.)
5. Verify both servlet and standalone contexts

### Working with Spring Integration
1. Changes to Spring 5 integration often need parallel changes in Spring 6
2. Consider reactive vs non-reactive code paths for Spring 6
3. Test with Spring Framework test utilities in corresponding test module
4. Verify view resolution, i18n, and validation integration

## Important Notes

- This is an Apache 2.0 licensed open-source project
- Contributor License Agreement required for non-trivial contributions
- Contact project maintainers before starting significant work
- GPG signing configured for releases (key: releases@thymeleaf.org)
- Dual support for Jakarta EE (jakarta.*) and legacy javax.* namespaces