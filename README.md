# Compose Compiler Stability Inference System

A comprehensive study of how the Compose compiler determines type stability for recomposition optimization. All details that [Optimize App Performance By Mastering Stability in Jetpack Compose](https://medium.com/proandroiddev/optimize-app-performance-by-mastering-stability-in-jetpack-compose-69f40a8c785d) and [compose-performance](https://github.com/skydoves/compose-performance) couldn't take in.

## 💝 Sponsors

<a href="https://www.coderabbit.ai/?utm_source=github&utm_medium=referral&utm_content=&utm_campaign=Jaewoong_github_2025" target="_blank"> <img width="300" alt="coderabbit" src="https://www.coderabbit.ai/_next/image?url=https%3A%2F%2Fvictorious-bubble-f69a016683.media.strapiapp.com%2FCr_logo_dark_f656abe8e3.png&w=384&q=75" /></a>

<a href="https://getstream.io/chat/sdk/android/?utm_source=github&utm_medium=referral&utm_content=&utm_campaign=Jaewoong_github_2025" target="_blank"> <img width="260" alt="stream" src="https://github.com/user-attachments/assets/87a69228-4fef-4f48-ad98-1e2c606c5b7e" /></a>

## Table of Contents

- [Compose Compiler Stability Inference System](#compose-compiler-stability-inference-system)
  - [📘 Manifest Android Interview](#-manifest-android-interview)
  - [🕊️ Dove Letter](#️-dove-letter)
  - [Table of Contents](#table-of-contents)
  - [Chapter 1: Foundations](#chapter-1-foundations)
    - [1.1 Introduction](#11-introduction)
    - [1.2 Core Concepts](#12-core-concepts)
      - [Stability Definition](#stability-definition)
      - [Recomposition Mechanics](#recomposition-mechanics)
    - [1.3 The Role of Stability](#13-the-role-of-stability)
      - [Performance Impact](#performance-impact)
  - [Chapter 2: Stability Type System](#chapter-2-stability-type-system)
    - [2.1 Type Hierarchy](#21-type-hierarchy)
    - [2.2 Compile-Time Stability](#22-compile-time-stability)
      - [Stability.Certain](#stabilitycertain)
    - [2.3 Runtime Stability](#23-runtime-stability)
      - [Stability.Runtime](#stabilityruntime)
    - [2.4 Uncertain Stability](#24-uncertain-stability)
      - [Stability.Unknown](#stabilityunknown)
    - [2.5 Parametric Stability](#25-parametric-stability)
      - [Stability.Parameter](#stabilityparameter)
    - [2.6 Combined Stability](#26-combined-stability)
      - [Stability.Combined](#stabilitycombined)
    - [2.7 Stability Decision Tree](#27-stability-decision-tree)
      - [Complete Decision Tree](#complete-decision-tree)
      - [Decision Tree for Generic Types](#decision-tree-for-generic-types)
      - [Expression Stability Decision Tree](#expression-stability-decision-tree)
      - [Key Decision Points Explained](#key-decision-points-explained)
  - [Chapter 3: The Inference Algorithm](#chapter-3-the-inference-algorithm)
    - [3.1 Algorithm Overview](#31-algorithm-overview)
    - [3.2 Type-Level Analysis](#32-type-level-analysis)
      - [Phase 1: Fast Path Type Checks](#phase-1-fast-path-type-checks)
      - [Phase 2: Type Parameter Handling](#phase-2-type-parameter-handling)
      - [Phase 3: Nullable Type Unwrapping](#phase-3-nullable-type-unwrapping)
      - [Phase 4: Inline Class Handling](#phase-4-inline-class-handling)
    - [3.3 Class-Level Analysis](#33-class-level-analysis)
      - [Phase 5: Cycle Detection](#phase-5-cycle-detection)
      - [Phase 6: Annotation and Marker Checks](#phase-6-annotation-and-marker-checks)
      - [Phase 7: Known Constructs](#phase-7-known-constructs)
      - [Phase 8: External Configuration](#phase-8-external-configuration)
      - [Phase 9: External Module Handling](#phase-9-external-module-handling)
      - [Phase 10: Java Type Handling](#phase-10-java-type-handling)
      - [Phase 11: General Interface Handling](#phase-11-general-interface-handling)
      - [Phase 12: Field-by-Field Analysis](#phase-12-field-by-field-analysis)
    - [3.4 Expression-Level Analysis](#34-expression-level-analysis)
      - [Constant Expressions](#constant-expressions)
      - [Function Call Expressions](#function-call-expressions)
      - [Variable Reference Expressions](#variable-reference-expressions)
  - [Chapter 4: Implementation Mechanisms](#chapter-4-implementation-mechanisms)
    - [4.1 Bitmask Encoding](#41-bitmask-encoding)
      - [Encoding Scheme](#encoding-scheme)
      - [Special Bit: Known Stable](#special-bit-known-stable)
      - [Bitmask Application](#bitmask-application)
    - [4.2 Runtime Field Generation](#42-runtime-field-generation)
      - [JVM Platform](#jvm-platform)
      - [Non-JVM Platforms](#non-jvm-platforms)
    - [4.3 Annotation Processing](#43-annotation-processing)
      - [@StabilityInferred Annotation](#stabilityinferred-annotation)
      - [Annotation Generation](#annotation-generation)
    - [4.4 Normalization Process](#44-normalization-process)
  - [Chapter 5: Case Studies](#chapter-5-case-studies)
    - [5.1 Primitive and Built-in Types](#51-primitive-and-built-in-types)
      - [Integer Types](#integer-types)
      - [String Type](#string-type)
      - [Function Types](#function-types)
    - [5.2 User-Defined Classes](#52-user-defined-classes)
      - [Simple Data Class](#simple-data-class)
      - [Class with Mutable Property](#class-with-mutable-property)
      - [Class with Mixed Properties](#class-with-mixed-properties)
    - [5.3 Generic Types](#53-generic-types)
      - [Simple Generic Container](#simple-generic-container)
      - [Multiple Type Parameters](#multiple-type-parameters)
      - [Nested Generic Types](#nested-generic-types)
    - [5.4 External Dependencies](#54-external-dependencies)
      - [External Class with Annotation](#external-class-with-annotation)
      - [External Class Without Annotation](#external-class-without-annotation)
    - [5.5 Interface and Abstract Types](#55-interface-and-abstract-types)
      - [Interface Parameter](#interface-parameter)
      - [Abstract Class](#abstract-class)
      - [Interface with @Stable](#interface-with-stable)
    - [5.6 Inheritance Hierarchies](#56-inheritance-hierarchies)
      - [Stable Inheritance](#stable-inheritance)
      - [Unstable Inheritance](#unstable-inheritance)
  - [Chapter 6: Configuration and Tooling](#chapter-6-configuration-and-tooling)
    - [6.1 Stability Annotations](#61-stability-annotations)
      - [@Stable Annotation](#stable-annotation)
      - [@Immutable Annotation](#immutable-annotation)
      - [Compiler-Level Differences: @Stable vs @Immutable](#compiler-level-differences-stable-vs-immutable)
      - [@StableMarker Meta-Annotation](#stablemarker-meta-annotation)
    - [6.2 Configuration Files](#62-configuration-files)
      - [File Format](#file-format)
      - [Pattern Syntax](#pattern-syntax)
      - [Gradle Configuration](#gradle-configuration)
    - [6.3 Compiler Reports](#63-compiler-reports)
      - [Enabling Reports](#enabling-reports)
      - [Generated Files](#generated-files)
    - [6.4 Common Issues and Solutions](#64-common-issues-and-solutions)
      - [Issue 1: Accidental var Usage](#issue-1-accidental-var-usage)
      - [Issue 2: Mutable Collections](#issue-2-mutable-collections)
      - [Issue 3: Interface Parameters](#issue-3-interface-parameters)
      - [Issue 4: External Library Types](#issue-4-external-library-types)
      - [Issue 5: Inheritance from Unstable Base](#issue-5-inheritance-from-unstable-base)
  - [Chapter 7: Advanced Topics](#chapter-7-advanced-topics)
    - [7.1 Type Substitution](#71-type-substitution)
      - [Substitution Map Construction](#substitution-map-construction)
      - [Substitution Application](#substitution-application)
      - [Nested Substitution](#nested-substitution)
    - [7.2 Cycle Detection](#72-cycle-detection)
      - [Detection Mechanism](#detection-mechanism)
      - [Example: Self-Referential Type](#example-self-referential-type)
      - [Limitation](#limitation)
    - [7.3 Special Cases](#73-special-cases)
      - [Protobuf Types](#protobuf-types)
      - [Delegated Properties](#delegated-properties)
      - [Inline Classes with Markers](#inline-classes-with-markers)
  - [Chapter 8: Compiler Analysis System](#chapter-8-compiler-analysis-system)
    - [8.1 Analysis Infrastructure](#81-analysis-infrastructure)
      - [WritableSlices: Data Flow Storage](#writableslices-data-flow-storage)
      - [BindingContext and BindingTrace](#bindingcontext-and-bindingtrace)
    - [8.2 Composable Call Validation](#82-composable-call-validation)
      - [Context Checking Algorithm](#context-checking-algorithm)
      - [Inline Lambda Restrictions](#inline-lambda-restrictions)
      - [Type Compatibility Checking](#type-compatibility-checking)
    - [8.3 Declaration Validation](#83-declaration-validation)
      - [Composable Function Rules](#composable-function-rules)
      - [Property Restrictions](#property-restrictions)
      - [Override Consistency](#override-consistency)
    - [8.4 Applier Target System](#84-applier-target-system)
      - [Scheme Structure](#scheme-structure)
      - [Target Inference Algorithm](#target-inference-algorithm)
      - [Cross-Target Validation](#cross-target-validation)
    - [8.5 Type Resolution and Inference](#85-type-resolution-and-inference)
      - [Automatic Composable Inference](#automatic-composable-inference)
      - [Lambda Type Adaptation](#lambda-type-adaptation)
    - [8.6 Analysis Pipeline](#86-analysis-pipeline)
      - [Compilation Phases](#compilation-phases)
      - [Data Flow Through Phases](#data-flow-through-phases)
    - [8.7 Practical Examples](#87-practical-examples)
      - [Example: Composable Context Validation](#example-composable-context-validation)
      - [Example: Inline Lambda Analysis](#example-inline-lambda-analysis)
      - [Example: Stability and Skipping](#example-stability-and-skipping)
  - [Appendix: Source Code References](#appendix-source-code-references)
    - [Primary Source Files](#primary-source-files)
  - [Conclusion](#conclusion)
  - [Find this repository useful? :heart:](#find-this-repository-useful-heart)
- [License](#license)

## Chapter 1: Foundations

### 1.1 Introduction

The Compose compiler implements a stability inference system to enable recomposition optimization. This system analyzes types at compile time to determine whether their values can be safely compared for equality during recomposition.

The inference process involves analyzing type declarations, examining field properties, and tracking stability through generic type parameters. The results inform the runtime whether to skip recomposition when parameter values remain unchanged.

### 1.2 Core Concepts

#### Stability Definition

A type is considered stable when it satisfies three conditions:

1. **Immutability**: The observable state of an instance does not change after construction
2. **Equality semantics**: Two instances with equal observable state are equal via `equals()`
3. **Change notification**: If the type contains observable mutable state, all state changes trigger composition invalidation

These properties allow the runtime to make optimization decisions based on value comparison.

#### Recomposition Mechanics

When a composable function receives parameters, the runtime determines whether to execute the function body:

```kotlin
@Composable
fun UserProfile(user: User) {
    // Function body
}
```

The decision process:
1. Compare the new `user` value with the previous value
2. If equal and the type is stable, skip recomposition
3. If different or unstable, execute the function body

Without stability information, the runtime must conservatively recompose on every invocation, regardless of whether parameters changed.

### 1.3 The Role of Stability

#### Performance Impact

Stability inference affects recomposition in three ways:

**Smart Skipping**: Composable functions with stable parameters can be skipped when parameter values remain unchanged. This reduces the number of function executions during recomposition.

**Comparison Propagation**: The compiler passes stability information to child composable calls, enabling nested optimizations throughout the composition tree.

**Comparison Strategy**: The runtime selects between structural equality (`equals()`) for stable types and referential equality (`===`) for unstable types, affecting change detection behavior.

Consider this example:

```kotlin
// Unstable parameter type - interface with unknown stability
@Composable
fun ExpensiveList(items: List<String>) {
    // List is an interface - has Unknown stability
    // Falls back to instance comparison
}

// Stable parameter type - using immutable collection
@Composable
fun ExpensiveList(items: ImmutableList<String>) {
    // ImmutableList is in KnownStableConstructs
    // Can skip recomposition when unchanged
}

// Alternative: Using listOf() result
@Composable
fun ExpensiveList(items: List<String>) {
    // If items comes from listOf(), the expression is stable
    // But the List type itself is still an interface with Unknown stability
}
```

The key insight: `List` and `MutableList` are both interfaces with `Unknown` stability. To achieve stable parameters, use:
1. `ImmutableList` from kotlinx.collections.immutable (in KnownStableConstructs)
2. Add `kotlin.collections.List` to your stability configuration file
3. Use `@Stable` annotation on your data classes containing List

## Chapter 2: Stability Type System

### 2.1 Type Hierarchy

The compiler represents stability through a sealed class hierarchy defined in :

```kotlin
sealed class Stability {
    class Certain(val stable: Boolean) : Stability()
    class Runtime(val declaration: IrClass) : Stability()
    class Unknown(val declaration: IrClass) : Stability()
    class Parameter(val parameter: IrTypeParameter) : Stability()
    class Combined(val elements: List<Stability>) : Stability()
}
```

Each subtype represents a different category of stability information available to the compiler.

### 2.2 Compile-Time Stability

#### Stability.Certain

This type represents stability that can be determined completely at compile time.

**Structure:**
```kotlin
class Certain(val stable: Boolean) : Stability()
```

The `stable` field indicates whether the type is definitely stable (`true`) or definitely unstable (`false`).

**Examples:**

```kotlin
// Certain(stable = true)
class Point(val x: Int, val y: Int)

// Certain(stable = false)
class Counter(var count: Int)
```

**Usage Conditions:**
- Primitive types (`Int`, `Long`, `Boolean`, etc.)
- `String` and `Unit`
- Function types (`FunctionN`, `KFunctionN`)
- Classes with only stable `val` properties
- Classes with any `var` property (immediately unstable)
- Classes marked with `@Stable` or `@Immutable` annotations

**Implementation:** See  for the `knownStable()` extension function.

### 2.3 Runtime Stability

#### Stability.Runtime

This type indicates that stability must be checked at runtime by reading a generated `$stable` field.

**Structure:**
```kotlin
class Runtime(val declaration: IrClass) : Stability()
```

The `declaration` references the class whose stability requires runtime determination.

**Generated Code Example:**

```kotlin
// Source code
class Box<T>(val value: T)

// Compiler-generated code
@StabilityInferred(parameters = 0b1)
class Box<T>(val value: T) {
    companion object {
        @JvmField
        val $stable: Int = /* computed based on type parameters */
    }
}
```

**When Applied:**
- Classes from external modules (separately compiled)
- Generic classes where type parameters affect stability
- Classes with `@StabilityInferred` annotation

**Runtime Behavior:**

At instantiation sites, the runtime computes the `$stable` field value:

```kotlin
Box<Int>         // $stable = STABLE (0b000)
Box<MutableList> // $stable = UNSTABLE (0b100)
```

**Implementation:** See  and .

### 2.4 Uncertain Stability

#### Stability.Unknown

This type represents cases where the compiler cannot determine stability.

**Structure:**
```kotlin
class Unknown(val declaration: IrClass) : Stability()
```

**Examples:**

```kotlin
interface Repository {
    fun getData(): String
}

class Screen(val source: Repository)
// Repository has Unknown stability
```

**Usage Conditions:**
- Interface types (unknown implementations)
- Abstract classes without concrete analysis
- Types in incremental compilation scenarios

**Runtime Behavior:**

When encountering Unknown stability, the runtime falls back to instance comparison (`===`) for change detection. This conservative approach ensures correctness but prevents skipping optimizations.

**Implementation:** See .

### 2.5 Parametric Stability

#### Stability.Parameter

This type represents stability that depends on a generic type parameter.

**Structure:**
```kotlin
class Parameter(val parameter: IrTypeParameter) : Stability()
```

**Example:**

```kotlin
class Wrapper<T>(val value: T)
//              ^^^^^^^^^^^^
// Stability depends on T

// Instantiation examples:
Wrapper<Int>       // Stable (Int is stable)
Wrapper<Counter>   // Unstable (Counter from 2.2 is unstable)
```

**Resolution Process:**

When analyzing `Wrapper<Int>`:
1. Identify `value: T` has `Stability.Parameter(T)`
2. Substitute `T` with `Int`
3. Evaluate `stabilityOf(Int)` = `Stable`
4. Result: `Wrapper<Int>` is stable

**Implementation:** See  for type parameter handling.

### 2.6 Combined Stability

#### Stability.Combined

This type aggregates multiple stability factors from different sources.

**Structure:**
```kotlin
class Combined(val elements: List<Stability>) : Stability()
```

**Examples:**

```kotlin
class Complex<T, U>(
    val primitive: Int,     // Certain(stable = true)
    val param1: T,          // Parameter(T)
    val param2: U           // Parameter(U)
)
// Combined([Certain(true), Parameter(T), Parameter(U)])
```

**Combination Rules:**

The compiler combines stabilities using the `plus` operator ():

```kotlin
Stable     + Stable     = Stable
Stable     + Unstable   = Unstable
Unstable   + Stable     = Unstable
Stable     + Parameter  = Combined([Parameter])
Parameter  + Parameter  = Combined([Parameter, Parameter])
Runtime    + Parameter  = Combined([Runtime, Parameter])
```

**Key Property:** Unstable stability dominates all combinations. A single unstable component makes the entire result unstable.

### 2.7 Stability Decision Tree

The Compose compiler follows a systematic decision tree when determining stability. This tree represents the actual logic flow implemented in the compiler.

#### Complete Decision Tree

```
┌─────────────────────────────────┐
│     Start: Analyze Type/Class   │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  Is it a primitive type?        │───Yes──→ [STABLE]
│  (Int, Boolean, Float, etc.)    │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it String or Unit?          │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it a function type?         │───Yes──→ [STABLE]
│  (Function<*>, KFunction<*>)    │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Has @Stable or @Immutable?     │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Has @StableMarker descendant?  │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it an Enum class/entry?     │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it a Protobuf type?         │───Yes──→ [STABLE]
│  (GeneratedMessage/Lite)        │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it in KnownStableConstructs?│───Yes──→ [STABLE/RUNTIME]
│  (Pair, Triple, etc.)           │          (check type params)
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Matches external config?       │───Yes──→ [STABLE/RUNTIME]
│  (stability-config.conf)        │          (check type params)
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it an interface?            │───Yes──→ [UNKNOWN]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it from external Java?      │───Yes──→ [UNSTABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Has @StabilityInferred?        │───Yes──→ [RUNTIME]
│  (from separate compilation)    │          (use bitmask)
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Analyze class members:         │
│  - Check all properties         │
│  - Check backing fields         │
│  - Check superclass             │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  Any var (mutable) property?    │───Yes──→ [UNSTABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  All members stable?            │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
        [UNSTABLE/COMBINED]
```

#### Decision Tree for Generic Types

When analyzing generic types, the compiler follows an additional decision path:

```
┌─────────────────────────────────┐
│   Generic Type: Class<T1, T2>   │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  Base class stable?             │───No───→ [UNSTABLE]
└────────────┬────────────────────┘
             │ Yes
             ▼
┌─────────────────────────────────┐
│  Has stability bitmask?         │───No───→ Analyze each
│  (from KnownStableConstructs    │         type parameter
│   or external config)           │         individually
└────────────┬────────────────────┘
             │ Yes
             ▼
┌─────────────────────────────────┐
│  For each type parameter Ti:    │
│  Is bit i set in bitmask?       │───No───→ Ti doesn't affect
└────────────┬────────────────────┘         stability
             │ Yes
             ▼
┌─────────────────────────────────┐
│  Check stability of actual      │
│  type argument for Ti           │───→ Combine all results
└─────────────────────────────────┘
```

#### Expression Stability Decision Tree

For expressions (used in default parameters and composable bodies):

```
┌─────────────────────────────────┐
│    Expression to analyze        │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  Is expression type stable?     │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it a constant (IrConst)?    │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it a stable function call?  │───Yes──→ Check function
│  (listOf, mapOf, etc.)          │          type parameters
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it a val reference?         │───Yes──→ Check initializer
│  (non-mutable variable)         │          stability
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it a composite with all     │───Yes──→ [STABLE]
│  stable sub-expressions?        │
└────────────┬────────────────────┘
             │ No
             ▼
         [UNSTABLE]
```

#### Key Decision Points Explained

**1. Early Exit Conditions:**
- Primitives, String, Unit, and function types are immediately stable
- Stability annotations override all other checks
- Enums are always stable (finite, immutable values)

**2. Interface Handling:**
- Interfaces return `Unknown` stability because implementations can vary
- Exception: Interfaces with `@Stable` marker are trusted

**3. External Types:**
- Java types default to unstable (mutable by default in Java)
- Protobuf types are special-cased as stable (immutable messages)
- External Kotlin modules use `@StabilityInferred` bitmasks

**4. Member Analysis:**
- Any `var` property makes the entire class unstable
- Delegated properties are analyzed based on their delegate type
- Superclass stability affects subclass stability

**5. Generic Type Resolution:**
- Bitmask encodes which type parameters affect stability
- Maximum 32 type parameters supported (32-bit bitmask)
- Type arguments are substituted and analyzed recursively

This decision tree is implemented across several key functions in the compiler, with the main entry point being `StabilityInferencer.stabilityOf()`.

## Chapter 3: The Inference Algorithm

### 3.1 Algorithm Overview

The stability inference algorithm operates following the decision tree shown above, with multiple optimization paths for early termination. The implementation spans several key components working together.

**Algorithm Structure:**

The algorithm short-circuits when it can determine stability definitively, avoiding unnecessary analysis.

### 3.2 Type-Level Analysis

#### Phase 1: Fast Path Type Checks

The compiler first checks for common stable types:

```kotlin
when {
    type is IrErrorType -> Stability.Unstable
    type is IrDynamicType -> Stability.Unstable

    type.isUnit() ||
    type.isPrimitiveType() ||
    type.isFunctionOrKFunction() ||
    type.isSyntheticComposableFunction() ||
    type.isString() -> Stability.Stable
}
```

**Primitive Types:**
- Numeric: `Byte`, `Short`, `Int`, `Long`, `Float`, `Double`
- Boolean: `Boolean`
- Character: `Char`

**Function Types:**
- Standard functions: `Function0`, `Function1`, ..., `FunctionN`
- Kotlin functions: `KFunction0`, `KFunction1`, ..., `KFunctionN`
- Composable functions: `ComposableFunction0`, etc.

#### Phase 2: Type Parameter Handling

For generic type parameters:

```kotlin
type.isTypeParameter() -> {
    val classifier = type.classifierOrFail
    val arg = substitutions[classifier]

    if (arg != null && symbol !in currentlyAnalyzing) {
        stabilityOf(arg, substitutions, currentlyAnalyzing + symbol)
    } else {
        Stability.Parameter(classifier.owner as IrTypeParameter)
    }
}
```

**Substitution Example:**

```kotlin
class Container<T>(val item: T)

// Analyzing Container<Int>
// T is IrTypeParameter
// substitutions map: {T: Int}
// Result: stabilityOf(Int) = Stable
```

#### Phase 3: Nullable Type Unwrapping

Nullable types defer to their non-null counterpart:

```kotlin
type.isNullable() ->
    stabilityOf(type.makeNotNull(), substitutions, currentlyAnalyzing)
```

**Examples:**
- `Int?` → analyze `Int` → Stable
- `User?` → analyze `User` → depends on User structure

#### Phase 4: Inline Class Handling



Inline classes (value classes) have special handling:

```kotlin
type.isInlineClassType() -> {
    val inlineClassDeclaration = type.getClass()

    if (inlineClassDeclaration.hasStableMarker()) {
        Stability.Stable
    } else {
        stabilityOf(
            type = getInlineClassUnderlyingType(inlineClassDeclaration),
            substitutions = substitutions,
            currentlyAnalyzing = currentlyAnalyzing
        )
    }
}
```

**Examples:**

```kotlin
@JvmInline
value class UserId(val value: Int)
// Checks: stabilityOf(Int) = Stable

@JvmInline
value class Token(val value: String)
// Checks: stabilityOf(String) = Stable

@JvmInline
@Stable
value class SpecialId(val list: MutableList<Int>)
// @Stable marker overrides underlying type analysis
// Result: Stable (by annotation)
```

### 3.3 Class-Level Analysis

#### Phase 5: Cycle Detection

To prevent infinite recursion with recursive types:

```kotlin
if (currentlyAnalyzing.contains(fullSymbol))
    return Stability.Unstable
```

**Example:**

```kotlin
class Node(val value: Int, val next: Node?)

// Analysis trace:
// 1. stabilityOf(Node) → add to currentlyAnalyzing
// 2. Check field: value: Int → Stable
// 3. Check field: next: Node? → unwrap nullable
// 4. Check field: next: Node → CYCLE DETECTED
// 5. Return Unstable
```

This conservative approach ensures termination while potentially marking some stable recursive types as unstable.

#### Phase 6: Annotation and Marker Checks

Quick checks for annotated or special types:

```kotlin
if (declaration.hasStableMarkedDescendant()) return Stability.Stable
if (declaration.isEnumClass || declaration.isEnumEntry) return Stability.Stable
if (declaration.defaultType.isPrimitiveType()) return Stability.Stable
if (declaration.isProtobufType()) return Stability.Stable
```

**Stable Markers:**
- `@Stable` annotation
- `@Immutable` annotation
- Annotations marked with `@StableMarker`

**Enum Handling:**

All enum classes and enum entries are considered stable because:
1. Enum instances are singletons (referential equality works)
2. Enum state is immutable after initialization
3. Enum equality is based on identity

**Protobuf Detection:**

```kotlin
private fun IrClass.isProtobufType(): Boolean {
    if (!isFinalClass) return false
    val directParentClassName = superTypes
        .lastOrNull { !it.isInterface() }
        ?.classOrNull?.owner?.fqNameWhenAvailable?.toString()
    return directParentClassName == "com.google.protobuf.GeneratedMessageLite" ||
           directParentClassName == "com.google.protobuf.GeneratedMessage"
}
```

Generated protobuf classes are marked stable despite potentially containing mutable implementation details.

#### Phase 7: Known Constructs

The compiler maintains a registry of known stable types:

```kotlin
val stableTypes = mapOf(
    "kotlin.Pair" to 0b11,
    "kotlin.Triple" to 0b111,
    "kotlin.Result" to 0b1,
    "kotlin.Comparator" to 0b1,
    "kotlin.ranges.ClosedRange" to 0b1,
    "com.google.common.collect.ImmutableList" to 0b1,
    "kotlinx.collections.immutable.ImmutableList" to 0b1,
    "dagger.Lazy" to 0b1,
    "java.math.BigInteger" to 0,
    "java.math.BigDecimal" to 0,
    // ... more entries
)
```

The integer value represents a bitmask indicating which type parameters affect stability (covered in Chapter 4).

#### Phase 8: External Configuration

Users can provide configuration files declaring types as stable:

```kotlin
if (declaration.isExternalStableType()) {
    mask = externalTypeMatcherCollection
        .maskForName(declaration.fqNameWhenAvailable) ?: 0
    stability = Stability.Stable
}
```

Configuration file format (covered in Chapter 6).

#### Phase 9: External Module Handling

For classes from external modules:

```kotlin
if (declaration.isInterface && declaration.isInCurrentModule()) {
    return Stability.Unknown(declaration)
} else {
    val bitmask = declaration.stabilityParamBitmask() ?: return Stability.Unstable

    val knownStableMask = if (typeParameters.size < 32) 0b1 shl typeParameters.size else 0
    val isKnownStable = bitmask and knownStableMask != 0
    mask = bitmask and knownStableMask.inv()

    stability = if (isKnownStable && declaration.isInCurrentModule()) {
        Stability.Stable
    } else {
        Stability.Runtime(declaration)
    }
}
```

**Rationale for Interface Handling:**

Interfaces in the current module return `Unknown` to support incremental compilation. Since implementations may change across compilation units, the compiler cannot safely infer interface stability.

#### Phase 10: Java Type Handling

```kotlin
if (declaration.origin == IrDeclarationOrigin.IR_EXTERNAL_JAVA_DECLARATION_STUB) {
    return Stability.Unstable
}
```

Java types default to unstable because:
1. Java allows unrestricted mutability
2. No equivalent of Kotlin's `val` guarantee
3. No stability annotations in Java standard library

#### Phase 11: General Interface Handling

```kotlin
if (declaration.isInterface) {
    return Stability.Unknown(declaration)
}
```

Without concrete implementation details, interfaces have unknown stability.

#### Phase 12: Field-by-Field Analysis

For concrete classes in the current module:

```kotlin
var stability = Stability.Stable

for (member in declaration.declarations) {
    when (member) {
        is IrProperty -> {
            member.backingField?.let {
                if (member.isVar && !member.isDelegated)
                    return Stability.Unstable

                stability += stabilityOf(it.type, substitutions, analyzing)
            }
        }

        is IrField -> {
            stability += stabilityOf(member.type, substitutions, analyzing)
        }
    }
}

declaration.superClass?.let {
    stability += stabilityOf(it, substitutions, analyzing)
}

return stability
```

**Key Points:**

1. Start with `Stable` assumption
2. Any `var` property immediately returns `Unstable`
3. Combine stability of all `val` property types
4. Include superclass stability
5. Use `+` operator for combination (see 2.6)

### 3.4 Expression-Level Analysis

Beyond type stability, the compiler analyzes expression stability:

```kotlin
fun stabilityOf(expr: IrExpression): Stability {
    val stability = stabilityOf(expr.type)
    if (stability.knownStable()) return stability

    return when (expr) {
        is IrConst -> Stability.Stable
        is IrCall -> stabilityOf(expr, stability)
        is IrGetValue -> /* analyze variable */
        is IrLocalDelegatedPropertyReference -> Stability.Stable
        is IrComposite -> /* analyze all statements */
        else -> stability
    }
}
```

#### Constant Expressions

Literal constants are always stable:

```kotlin
val x = 42           // IrConst(42) → Stable
val s = "text"       // IrConst("text") → Stable
val b = true         // IrConst(true) → Stable
```

#### Function Call Expressions

The compiler checks known stable functions:

```kotlin
private fun stabilityOf(expr: IrCall, baseStability: Stability): Stability {
    val function = expr.symbol.owner
    val fqName = function.kotlinFqName

    return when (val mask = KnownStableConstructs.stableFunctions[fqName.asString()]) {
        null -> baseStability
        0 -> Stability.Stable
        else -> Stability.Combined(/* check type arguments */)
    }
}
```

**Known Stable Functions:**

```kotlin
val stableFunctions = mapOf(
    "kotlin.collections.emptyList" to 0,
    "kotlin.collections.listOf" to 0b1,
    "kotlin.collections.mapOf" to 0b11,
    "kotlin.collections.emptyMap" to 0,
    "kotlin.collections.setOf" to 0b1,
    "kotlinx.collections.immutable.immutableListOf" to 0b1,
    // ... more functions
)
```

#### Variable Reference Expressions

For `val` variables, the compiler checks initializer stability:

```kotlin
val x = 42              // Stable initializer
val y = x               // IrGetValue(x) → Stable

var z = 42              // Mutable variable
val w = z               // IrGetValue(z) → use type stability
```

## Chapter 4: Implementation Mechanisms

### 4.1 Bitmask Encoding

Generic types use bitmasks to encode type parameter dependencies.

#### Encoding Scheme

Each type parameter is represented by a single bit:
- Bit N = 1: Type parameter N affects stability
- Bit N = 0: Type parameter N does not affect stability

**Maximum Limit:** 32 type parameters (Int size constraint)

**Examples:**

```kotlin
// Pair<A, B>
@StabilityInferred(parameters = 0b11)
// Binary: 00000000000000000000000000000011
// Bit 0: A affects stability
// Bit 1: B affects stability

// Triple<A, B, C>
@StabilityInferred(parameters = 0b111)
// Binary: 00000000000000000000000000000111
// Bit 0: A affects stability
// Bit 1: B affects stability
// Bit 2: C affects stability

// Result<T>
@StabilityInferred(parameters = 0b1)
// Binary: 00000000000000000000000000000001
// Bit 0: T affects stability
```

#### Special Bit: Known Stable

If bit position `typeParams.size` is set, the class is known stable:

```kotlin
@StabilityInferred(parameters = 0b101)
class Container<T, U>
// Binary: 00000000000000000000000000000101
// Bit 0: T affects stability
// Bit 1: U does not affect stability
// Bit 2: Known stable bit (1 shl 2 where typeParams.size = 2)
```

This indicates the class is stable regardless of type parameter instantiation.

#### Bitmask Application

```kotlin
return when {
    mask == 0 || typeParameters.isEmpty() -> stability
    else -> stability + Stability.Combined(
        typeParameters.mapIndexedNotNull { index, irTypeParameter ->
            if (index >= 32) return@mapIndexedNotNull null
            if (mask and (0b1 shl index) != 0) {
                val sub = substitutions[irTypeParameter.symbol]
                if (sub != null)
                    stabilityOf(sub, substitutions, analyzing)
                else
                    Stability.Parameter(irTypeParameter)
            } else null
        }
    )
}
```

**Process:**
1. For each type parameter at index I
2. Check if bit I is set in mask
3. If set, add stability of that parameter
4. If not set, ignore that parameter

### 4.2 Runtime Field Generation

#### JVM Platform

For JVM targets, the compiler generates a static field:

```kotlin
// Source
class Box<T>(val value: T)

// Generated
@StabilityInferred(parameters = 0b1)
class Box<T>(val value: T) {
    companion object {
        @JvmField
        public static final int $stable = 0
    }
}
```

The field name is always `$stable`, and it is initialized with a computed stability value.

**Stability Values:**

```kotlin
enum class StabilityBits(val bits: Int) {
    UNSTABLE(0b100),
    STABLE(0b000)
}
```

#### Non-JVM Platforms

For Native and JS targets, the compiler generates:
1. A top-level property with a mangled name
2. A getter function for metadata visibility

```kotlin
// Generated for Native/JS
private val `com_example_Box$stabilityflag`: Int = /* ... */

@HiddenFromObjC
@Deprecated(level = DeprecationLevel.HIDDEN, message = "...")
internal fun `com_example_Box$stabilityflag_getter`(): Int =
    `com_example_Box$stabilityflag`
```

**Rationale:**

Non-JVM platforms require getter functions for metadata visibility. The getter is registered as metadata-visible to enable cross-module stability checks.

### 4.3 Annotation Processing

#### @StabilityInferred Annotation

```kotlin
private fun IrAnnotationContainer.stabilityParamBitmask(): Int? =
    (annotations.findAnnotation(ComposeFqNames.StabilityInferred)?.arguments[0] as? IrConst)
        ?.value as? Int
```

The annotation carries a single integer parameter representing the bitmask.

#### Annotation Generation

```kotlin
val annotation = IrConstructorCallImpl(
    UNDEFINED_OFFSET,
    UNDEFINED_OFFSET,
    StabilityInferredClass.defaultType,
    StabilityInferredClass.constructors.first(),
    typeArgumentsCount = 0,
    constructorTypeArgumentsCount = 0,
    origin = null
).also {
    it.arguments[0] = irConst(parameterMask)
}

if (useK2 && cls.hasFirDeclaration()) {
    context.metadataDeclarationRegistrar.addMetadataVisibleAnnotationsToElement(
        cls,
        annotation,
    )
} else {
    cls.annotations += annotation
}
```

### 4.4 Normalization Process

Before using stability for code generation, the compiler normalizes it:

```kotlin
fun Stability.normalize(): Stability {
    when (this) {
        is Stability.Certain,
        is Stability.Parameter,
        is Stability.Runtime,
        is Stability.Unknown,
        -> return this

        is Stability.Combined -> { /* normalize */ }
    }

    val parameters = mutableSetOf<IrTypeParameterSymbol>()
    val parts = mutableListOf<Stability>()
    val stack = mutableListOf<Stability>(this)

    while (stack.isNotEmpty()) {
        when (val stability: Stability = stack.removeAt(stack.size - 1)) {
            is Stability.Combined -> {
                stack.addAll(stability.elements)
            }

            is Stability.Certain -> {
                if (!stability.stable)
                    return Stability.Unstable
            }

            is Stability.Parameter -> {
                if (stability.parameter.symbol !in parameters) {
                    parameters.add(stability.parameter.symbol)
                    parts.add(stability)
                }
            }

            is Stability.Runtime -> parts.add(stability)
            is Stability.Unknown -> { /* ignore */ }
        }
    }

    return Stability.Combined(parts)
}
```

**Normalization Operations:**

1. **Flatten Combined**: Recursively expand nested `Combined` instances
2. **Remove Unknown**: `Unknown` elements are discarded (treated as uncertain)
3. **Deduplicate Parameters**: Keep only unique type parameters
4. **Short-circuit on Unstable**: Return immediately if any `Certain(false)` found
5. **Collect Runtime and Parameter**: Preserve these for runtime checks

**Result Types:**
- `Stability.Unstable` if any component is unstable
- `Stability.Combined([...])` with deduplicated elements otherwise

## Chapter 5: Case Studies

### 5.1 Primitive and Built-in Types

#### Integer Types

```kotlin
val x: Int = 42
// Analysis: type.isPrimitiveType() → true
// Result: Stability.Certain(stable = true)
```

All primitive numeric types follow the same pattern.

#### String Type

```kotlin
val s: String = "text"
// Analysis: type.isString() → true
// Result: Stability.Certain(stable = true)
```

`String` receives special treatment due to its immutability guarantees.

#### Function Types

```kotlin
val f: (Int) -> String = { it.toString() }
// Analysis: type.isFunctionOrKFunction() → true
// Result: Stability.Certain(stable = true)
```

Function types are stable because:
1. Function references are immutable
2. Capturing lambdas capture immutable values (or create new closures)
3. Function equality is well-defined

### 5.2 User-Defined Classes

#### Simple Data Class

```kotlin
data class User(
    val id: Int,
    val name: String
)

// Analysis:
// 1. Not in fast path
// 2. No annotations
// 3. Not in known constructs
// 4. Field analysis:
//    - id: Int → Stable
//    - name: String → Stable
// 5. Combine: Stable + Stable = Stable
// Result: Stability.Certain(stable = true)
```

#### Class with Mutable Property

```kotlin
class Counter(
    var count: Int
)

// Analysis:
// 1. Field analysis:
//    - count is var → immediate return
// Result: Stability.Certain(stable = false)
```

#### Class with Mixed Properties

```kotlin
class Mixed(
    val stable: String,
    var unstable: Int
)

// Analysis:
// 1. Field analysis:
//    - stable: String → Stable
//    - unstable is var → immediate return
// Result: Stability.Certain(stable = false)
```

The presence of any `var` property makes the entire class unstable.

### 5.3 Generic Types

#### Simple Generic Container

```kotlin
class Box<T>(val value: T)

// Analysis:
// 1. Field analysis:
//    - value: T → Stability.Parameter(T)
// 2. Generate annotation: @StabilityInferred(parameters = 0b1)
// Result: Stability.Combined([Stability.Parameter(T)])

// Instantiation:
val intBox: Box<Int>
// Substitute T → Int
// stabilityOf(Int) = Stable
// Result: Stable

val counterBox: Box<Counter>
// Substitute T → Counter
// stabilityOf(Counter) = Unstable (from 5.2)
// Result: Unstable
```

#### Multiple Type Parameters

```kotlin
class Pair<A, B>(val first: A, val second: B)

// Analysis:
// 1. Field analysis:
//    - first: A → Parameter(A)
//    - second: B → Parameter(B)
// 2. Generate annotation: @StabilityInferred(parameters = 0b11)
// Result: Combined([Parameter(A), Parameter(B)])

// Instantiation:
val pair: Pair<Int, String>
// Substitute A → Int, B → String
// stabilityOf(Int) = Stable
// stabilityOf(String) = Stable
// Result: Stable
```

#### Nested Generic Types

```kotlin
class Container<T>(val items: List<T>)

// Analysis:
// 1. Field analysis:
//    - items: List<T>
//      - List is known stable construct (mask = 0b1)
//      - Check type arg T → Parameter(T)
// 2. Result: Combined([Parameter(T)])

// Instantiation:
val container: Container<String>
// items: List<String>
// List stability depends on String
// stabilityOf(String) = Stable
// Result: Stable
```

### 5.4 External Dependencies

#### External Class with Annotation

```kotlin
// Library module (compiled separately)
@StabilityInferred(parameters = 0b1)
class LibraryBox<T>(val value: T) {
    companion object {
        @JvmField
        val $stable: Int = 0
    }
}

// Your module
fun useLibraryBox(box: LibraryBox<Int>) {
    // Analysis:
    // 1. box: LibraryBox<Int>
    // 2. LibraryBox is external
    // 3. Has @StabilityInferred(0b1)
    // 4. Create Stability.Runtime(LibraryBox)
    // 5. Check bit 0 (T parameter)
    // 6. Add stabilityOf(Int) = Stable
    // Result: Combined([Runtime(LibraryBox), Stable])
    // At runtime: check LibraryBox.$stable field
}
```

#### External Class Without Annotation

```kotlin
// Third-party library (no Compose compiler)
class ThirdPartyType(val data: String)

// Your module
fun useThirdParty(obj: ThirdPartyType) {
    // Analysis:
    // 1. ThirdPartyType is external
    // 2. No @StabilityInferred annotation
    // 3. stabilityParamBitmask() returns null
    // Result: Stability.Unstable
}
```

### 5.5 Interface and Abstract Types

#### Interface Parameter

```kotlin
interface Repository {
    fun getData(): String
}

class Screen(val repo: Repository)

// Analysis of Repository:
// 1. Repository is interface
// 2. Unknown implementations
// Result: Stability.Unknown(Repository)

// Analysis of Screen:
// 1. Field repo: Repository → Unknown
// Result: Combined([Unknown(Repository)])
// Runtime: use instance comparison for repo
```

#### Abstract Class

```kotlin
abstract class BaseViewModel {
    abstract val state: String
}

class Screen(val viewModel: BaseViewModel)

// Analysis:
// 1. BaseViewModel is abstract
// 2. isInterface check fails
// 3. Field analysis cannot be performed (abstract)
// Result: Stability.Unknown(BaseViewModel)
```

#### Interface with @Stable

```kotlin
@Stable
interface StableRepository {
    fun getData(): String
}

class Screen(val repo: StableRepository)

// Analysis of StableRepository:
// 1. Check annotations
// 2. Has @Stable marker
// Result: Stability.Certain(stable = true)

// Analysis of Screen:
// 1. Field repo: StableRepository → Stable
// Result: Stable
```

### 5.6 Inheritance Hierarchies

#### Stable Inheritance

```kotlin
open class Base(val id: Int)
class Derived(val name: String) : Base(0)

// Analysis of Derived:
// 1. Field analysis:
//    - name: String → Stable
// 2. Check superclass: Base
//    - Field id: Int → Stable
// 3. Combine: Stable + Stable = Stable
// Result: Stability.Certain(stable = true)
```

#### Unstable Inheritance

```kotlin
open class Base(var state: Int)
class Derived(val data: String) : Base(0)

// Analysis of Base:
// 1. Field state is var
// Result: Unstable

// Analysis of Derived:
// 1. Field data: String → Stable
// 2. Check superclass: Base → Unstable
// 3. Combine: Stable + Unstable = Unstable
// Result: Stability.Certain(stable = false)
```

The unstable superclass makes all derived classes unstable.

## Chapter 6: Configuration and Tooling

### 6.1 Stability Annotations

#### @Stable Annotation

Declares that a type's public API is stable:

```kotlin
@Stable
class MutableCounter(private var count: Int) {
    fun increment() {
        count++
        // Must trigger recomposition
    }

    override fun equals(other: Any?): Boolean {
        return other is MutableCounter && count == other.count
    }
}
```

**Contract:**
1. Public API appears immutable (private var is internal)
2. `equals()` implements structural equality
3. State changes trigger composition invalidation

**Warning:** Incorrect usage violates runtime assumptions.

#### @Immutable Annotation

Stronger guarantee than `@Stable`:

```kotlin
@Immutable
class ImmutableData(val value: String)
```

**Contract:**
1. All observable state is truly immutable
2. No mutable fields (even private)
3. `equals()` implements structural equality

#### Compiler-Level Differences: @Stable vs @Immutable

While both annotations mark types as stable for recomposition skipping, there is one significant compiler-level difference.

**Stability Inference Treatment**

Both annotations are processed identically through `hasStableMarker()`:

```kotlin
fun IrAnnotationContainer.hasStableMarker(): Boolean =
    annotations.any { it.isStableMarker() }
```

Both result in:
- Same stability inference (types marked as `Stability.Certain(stable = true)`)
- Same `@StabilityInferred` annotation generation
- Same `$stable` runtime field generation
- Same recomposition skipping behavior

**Static Expression Optimization (Key Difference)**

`@Immutable` has special treatment for static expression detection.

```kotlin
private fun IrConstructorCall.isStatic(): Boolean {
    // special case for inline classes
    if (type.isInlineClassType()) {
        return stabilityInferencer.stabilityOf(type.unboxInlineClass()).knownStable() &&
                arguments[0]?.isStatic() == true
    }

    // @Immutable constructors with static args are static
    if (symbol.owner.parentAsClass.hasAnnotationSafe(ComposeFqNames.Immutable)) {
        return areAllArgumentsStatic()
    }

    // @Stable constructors are NOT considered static
    return false
}
```

**Practical Impact:**

```kotlin
@Immutable
data class ImmutablePoint(val x: Int, val y: Int)

@Stable
data class StablePoint(val x: Int, val y: Int)

@Composable
fun Example() {
    // Static expression - enables additional optimizations
    val immutable = ImmutablePoint(10, 20)

    // NOT a static expression - standard optimizations only
    val stable = StablePoint(10, 20)

    // Lambda with immutable capture can be more aggressively memoized
    val lambda1 = { immutable.x }  // Better optimization

    // Lambda with stable capture has standard memoization
    val lambda2 = { stable.x }      // Standard optimization
}
```

**Optimization Benefits of Static Expressions:**

1. **Lambda Memoization**: Static captures don't prevent lambda singleton optimization
2. **Default Parameters**: Static defaults can be computed at compile time
3. **Remember Optimization**: Compiler may skip unnecessary remember calls for static values

**Summary Table:**

| Aspect | @Stable | @Immutable |
|--------|---------|------------|
| Stability inference | Stable | Stable |
| Recomposition skipping | Enabled | Enabled |
| Constructor staticness | No | Yes (with static args) |
| Lambda capture optimization | Standard | Enhanced |
| Semantic contract | Allows private mutability | Truly immutable |

The compiler treats `@Immutable` as a stronger guarantee that enables additional compile-time optimizations, particularly for static expression evaluation and lambda memoization.

#### @StableMarker Meta-Annotation

Create custom stability markers:

```kotlin
@StableMarker
annotation class MyStable

@MyStable
class CustomType(val data: String)
// Treated as @Stable
```

### 6.2 Configuration Files

#### File Format

Create a [Stability configuration file](https://developer.android.com/develop/ui/compose/performance/stability/fix#configuration-file), `stability_config.conf`:

```
// Single class
com.example.ExternalType

// Package wildcard
com.example.models.**

// Single-segment wildcard
com.example.*.data

// Generic parameter inclusion
com.example.Container<*>

// Generic parameter exclusion
com.example.Wrapper<*,_>

// Mixed generic parameters
com.example.Complex<*,_,*>
```

#### Pattern Syntax

**Wildcard Rules:**
- `*`: Matches single package segment
- `**`: Matches multiple package segments
- `<*>`: Generic parameter affects stability
- `<_>`: Generic parameter ignored for stability

**Generic Parameter Encoding:**

```
Container<*,_,*>
          │ │ │
          │ │ └─ Bit 2 set (third param matters)
          │ └─── Bit 1 clear (second param ignored)
          └───── Bit 0 set (first param matters)
// Bitmask: 0b101 = 5
```

#### Gradle Configuration

To enable this feature, pass the path of the configuration file to the composeCompiler options block of the [Compose compiler Gradle plugin](https://developer.android.com/develop/ui/compose/compiler) configuration.

```kotlin
composeCompiler {
  stabilityConfigurationFile = rootProject.layout.projectDirectory.file("stability_config.conf")
}
```

### 6.3 Compiler Reports

#### Enabling Reports

```kotlin
composeCompiler {
    reportsDestination = layout.buildDirectory.dir("compose_reports")
    metricsDestination = layout.buildDirectory.dir("compose_metrics")
}
```

#### Generated Files

**`<module>-classes.txt`**: Class stability analysis

```
stable class User {
  stable val id: Int
  stable val name: String
}

unstable class Counter {
  unstable var count: Int
}

runtime stable class Box {
  stable val value: T
}
```

**`<module>-composables.txt`**: Composable function analysis

```
restartable skippable scheme("[androidx.compose.ui.UiComposable]") fun UserProfile(
  stable user: User
)

restartable scheme("[androidx.compose.ui.UiComposable]") fun Counter(
  unstable counter: Counter
)
```

**`<module>-module.json`**: Metrics summary

```json
{
  "skippableComposables": 45,
  "restartableComposables": 50,
  "readonlyComposables": 5,
  "totalComposables": 100,
  "restartGroups": 50,
  "totalGroups": 75
}
```

### 6.4 Common Issues and Solutions

#### Issue 1: Accidental var Usage

**Problem:**

```kotlin
data class UserState(var loading: Boolean)
// Unstable due to var
```

**Solution:**

```kotlin
data class UserState(val loading: Boolean)
// Stable
```

#### Issue 2: Mutable Collections

**Problem:**

```kotlin
class ViewModel(val items: MutableList<String>)
// MutableList is unstable
```

**Solution:**

```kotlin
import kotlinx.collections.immutable.ImmutableList

class ViewModel(val items: ImmutableList<String>)
// ImmutableList is stable (in KnownStableConstructs)
```

**Alternative (requires configuration):**

```kotlin
class ViewModel(val items: List<String>)
// List is an interface with Unknown stability by default
// To make it stable, add to stability-config.txt:
// kotlin.collections.List
```

#### Issue 3: Interface Parameters

**Problem:**

```kotlin
interface DataSource { }

@Composable
fun Screen(source: DataSource) {
    // DataSource has Unknown stability
    // Falls back to instance comparison
}
```

**Solution A: Add @Stable**

```kotlin
@Stable
interface DataSource { }
```

**Solution B: Use concrete type**

```kotlin
@Composable
fun Screen(source: ConcreteDataSource) {
    // Concrete class can have known stability
}
```

#### Issue 4: External Library Types

**Problem:**

```kotlin
// Third-party library without Compose support
class LibraryClass(val data: String)

@Composable
fun Display(obj: LibraryClass) {
    // LibraryClass marked unstable
}
```

**Solution:**

Add to `stability-config.txt`:

```
com.thirdparty.LibraryClass
```

#### Issue 5: Inheritance from Unstable Base

**Problem:**

```kotlin
open class MutableBase(var state: Int)

class DerivedData(val name: String) : MutableBase(0)
// Unstable due to base class
```

**Solution:**

Restructure to avoid mutable inheritance:

```kotlin
open class Base(val id: Int)

class DerivedData(val name: String, val state: Int) : Base(0)
// Stable if all fields are stable
```

## Chapter 7: Advanced Topics

### 7.1 Type Substitution

#### Substitution Map Construction

```kotlin
private fun IrSimpleType.substitutionMap(): Map<IrTypeParameterSymbol, IrTypeArgument> {
    val cls = classOrNull ?: return emptyMap()
    val params = cls.owner.typeParameters.map { it.symbol }
    val args = arguments
    return params.zip(args).filter { (param, arg) ->
        param != (arg as? IrSimpleType)?.classifier
    }.toMap()
}
```

**Example:**

```kotlin
class Container<T, U>(val first: T, val second: U)

// Analyzing Container<Int, String>
// typeParameters = [T, U]
// arguments = [Int, String]
// substitutionMap = {T: Int, U: String}
```

#### Substitution Application

When analyzing `Container<Int, String>`:

```kotlin
// Field: first: T
stabilityOf(T, substitutions = {T: Int, U: String})
// Lookup T in substitutions → Int
// Result: stabilityOf(Int) = Stable

// Field: second: U
stabilityOf(U, substitutions = {T: Int, U: String})
// Lookup U in substitutions → String
// Result: stabilityOf(String) = Stable
```

#### Nested Substitution

```kotlin
class Outer<T>(val inner: Inner<T>)
class Inner<U>(val value: U)

// Analyzing Outer<Int>
// 1. Field inner: Inner<T>
// 2. Inner has type parameter U
// 3. U is substituted with T
// 4. T is substituted with Int
// 5. Result: stabilityOf(Int) = Stable
```

### 7.2 Cycle Detection

#### Detection Mechanism

```kotlin
private data class SymbolForAnalysis(
    val symbol: IrClassifierSymbol,
    val typeParameters: List<IrTypeArgument?>,
)

// In stabilityOf(declaration: IrClass)
val fullSymbol = SymbolForAnalysis(symbol, typeArguments)

if (currentlyAnalyzing.contains(fullSymbol))
    return Stability.Unstable
```

The `currentlyAnalyzing` set tracks the analysis stack to detect cycles.

#### Example: Self-Referential Type

```kotlin
class Node(val value: Int, val next: Node?)

// Analysis trace:
// stabilityOf(Node, currentlyAnalyzing = {})
//   analyzing = {Node}
//   field: value: Int → Stable
//   field: next: Node?
//     unwrap nullable
//     stabilityOf(Node, currentlyAnalyzing = {Node})
//       CYCLE DETECTED: Node in currentlyAnalyzing
//       return Unstable
```

#### Limitation

Some recursive types that could be stable are marked unstable:

```kotlin
class TreeNode(val value: Int, val left: TreeNode?, val right: TreeNode?)
// Could be stable (immutable structure)
// Marked unstable due to cycle detection
```

This conservative approach ensures algorithm termination.

### 7.3 Special Cases

#### Protobuf Types

**Detection:** 

```kotlin
private fun IrClass.isProtobufType(): Boolean {
    if (!isFinalClass) return false
    val directParentClassName = superTypes
        .lastOrNull { !it.isInterface() }
        ?.classOrNull?.owner?.fqNameWhenAvailable?.toString()
    return directParentClassName == "com.google.protobuf.GeneratedMessageLite" ||
           directParentClassName == "com.google.protobuf.GeneratedMessage"
}
```

**Rationale:**

Generated protobuf classes use internal mutability for builder patterns but present an immutable API. The compiler treats them as stable based on their parent class.

#### Delegated Properties

```kotlin
if (member.isVar && !member.isDelegated)
    return Stability.Unstable
```

Delegated `var` properties are not automatically unstable. The stability depends on the delegate implementation.

**Example:**

```kotlin
class WithDelegate {
    var value: String by mutableStateOf("")
    // Delegated to MutableState
    // Compose tracks state changes
    // Can be stable with proper delegate
}
```

#### Inline Classes with Markers

```kotlin
if (inlineClassDeclaration.hasStableMarker()) {
    Stability.Stable
}
```

Inline classes can override underlying type stability with annotations:

```kotlin
@JvmInline
@Stable
value class Wrapper(val list: MutableList<Int>)
// Underlying type (MutableList) is unstable
// But @Stable annotation overrides
// Result: Stable (developer responsibility)
```

## Chapter 8: Compiler Analysis System

### 8.1 Analysis Infrastructure

The Compose compiler uses Kotlin's binding context and custom writable slices to store metadata during compilation. This data flows through the compilation pipeline, influencing code generation decisions.

#### WritableSlices: Data Flow Storage

`analysis/ComposeWritableSlices.kt`, `k1/FrontendWritableSlices.kt`

WritableSlices act as key-value stores where the compiler records analysis results:

**IR-Level Slices**

```kotlin
object ComposeWritableSlices {
    val IS_SYNTHETIC_COMPOSABLE_CALL = WritableSlice<IrFunctionAccessExpression, Boolean>()
    val IS_STATIC_FUNCTION_EXPRESSION = WritableSlice<IrFunctionExpression, Boolean>()
    val IS_STATIC_EXPRESSION = WritableSlice<IrExpression, Boolean>()
    val IS_COMPOSABLE_SINGLETON = WritableSlice<IrAttributeContainer, Boolean>()
    val IS_COMPOSABLE_SINGLETON_CLASS = WritableSlice<IrClass, Boolean>()
    val DURABLE_FUNCTION_KEY = WritableSlice<IrSimpleFunction, KeyInfo>()
    val HAS_TRANSFORMED_LAMBDA = WritableSlice<IrSimpleFunction, Boolean>()
}
```

**Frontend Slices**

```kotlin
object ComposeFrontEndWritableSlices {
    val INFERRED_COMPOSABLE_DESCRIPTOR = WritableSlice<FunctionDescriptor, Boolean>()
    val LAMBDA_CAPABLE_OF_COMPOSER_CAPTURE = WritableSlice<IrFunction, Boolean>()
    val INFERRED_COMPOSABLE_LITERAL = WritableSlice<IrFunctionExpression, Boolean>()
    val COMPOSE_LAZY_SCHEME = WritableSlice<IrAttributeContainer, Scheme>()
}
```

These slices store critical metadata:
- **IS_STATIC_EXPRESSION**: Marks expressions that can be evaluated at compile time
- **DURABLE_FUNCTION_KEY**: Stores unique keys for functions to enable hot reload
- **INFERRED_COMPOSABLE_DESCRIPTOR**: Tracks lambdas automatically inferred as composable

#### BindingContext and BindingTrace

The slices are stored in a `BindingContext` that persists throughout compilation:

```kotlin
// During analysis
bindingTrace.record(ComposeWritableSlices.IS_STATIC_EXPRESSION, expression, true)

// During lowering
val isStatic = context.bindingContext[ComposeWritableSlices.IS_STATIC_EXPRESSION, expression]
```

### 8.2 Composable Call Validation

`k1/ComposableCallChecker.kt`

The composable call checker ensures composable functions are only called from valid contexts.

#### Context Checking Algorithm

The checker walks up the PSI tree from each composable call site:

```kotlin
override fun check(resolvedCall: ResolvedCall<*>, reportOn: PsiElement, context: CallCheckerContext) {
    // Walk up PSI tree to find composable context
    var node: PsiElement? = reportOn
    while (node != null) {
        when (node) {
            is KtLambdaExpression -> {
                val descriptor = bindingContext[BindingContext.FUNCTION, node.functionLiteral]
                if (descriptor?.isComposableCallable() == true) {
                    return // Valid: inside composable lambda
                }
            }

            is KtFunction -> {
                val descriptor = bindingContext[BindingContext.FUNCTION, node]
                if (descriptor?.isComposableCallable() == true) {
                    return // Valid: inside composable function
                }
            }

            is KtPropertyAccessor -> {
                val descriptor = bindingContext[BindingContext.PROPERTY_ACCESSOR, node]
                if (descriptor?.isComposableCallable() == true) {
                    return // Valid: inside composable property
                }
            }

            is KtTryExpression -> {
                // Check if composable call is inside try/catch
                if (node.tryBlock.isAncestor(reportOn)) {
                    context.trace.report(ILLEGAL_TRY_CATCH_AROUND_COMPOSABLE.on(reportOn))
                    return
                }
            }

            is KtClass, is KtFile -> {
                // Reached non-composable boundary
                context.trace.report(COMPOSABLE_INVOCATION.on(reportOn))
                return
            }
        }
        node = node.parent
    }
}
```

#### Inline Lambda Restrictions

Special handling for inline functions with composable lambda parameters:

```kotlin
private fun checkInlineLambdaCall(
    resolvedCall: ResolvedCall<*>,
    reportOn: PsiElement,
    context: CallCheckerContext
) {
    val descriptor = resolvedCall.resultingDescriptor

    for ((parameter, argument) in resolvedCall.valueArguments) {
        if (!parameter.type.isComposableType()) continue

        if (parameter.hasDisallowComposableCalls()) {
            // This lambda cannot contain composable calls
            val lambda = argument.getLambdaExpression()
            lambda?.forEachDescendantOfType<KtCallExpression> { call ->
                if (call.isComposableCall()) {
                    context.trace.report(
                        CAPTURED_COMPOSABLE_INVOCATION.on(call, parameter, descriptor)
                    )
                }
            }
        }
    }
}
```

**Example:**

```kotlin
inline fun runWithoutComposables(
    @DisallowComposableCalls block: () -> Unit
) = block()

@Composable
fun MyComposable() {
    runWithoutComposables {
        Text("Error")  // ERROR: Composable calls not allowed
    }
}
```

#### Type Compatibility Checking

Validates that composable and non-composable function types match expected signatures:

```kotlin
override fun checkType(
    expression: KtExpression,
    expressionType: KotlinType,
    expectedType: KotlinType,
    context: CallCheckerContext
) {
    val isComposableExpression = expressionType.isComposableType()
    val isComposableExpected = expectedType.isComposableType()

    when {
        isComposableExpression && !isComposableExpected -> {
            // Trying to pass composable lambda where non-composable expected
            if (!isInlineConversion(expression)) {
                context.trace.report(
                    TYPE_MISMATCH.on(expression, expectedType, expressionType)
                )
            }
        }

        !isComposableExpression && isComposableExpected -> {
            // Trying to pass non-composable where composable expected
            context.trace.report(
                TYPE_MISMATCH.on(expression, expectedType, expressionType)
            )
        }
    }
}
```

### 8.3 Declaration Validation

Validates composable function and property declarations follow Compose rules.

#### Composable Function Rules

Key validation rules enforced:

```kotlin
class ComposableDeclarationChecker : DeclarationChecker {
    override fun check(declaration: KtDeclaration, descriptor: DeclarationDescriptor, context: DeclarationCheckerContext) {
        when (descriptor) {
            is FunctionDescriptor -> checkFunction(descriptor, declaration, context)
            is PropertyDescriptor -> checkProperty(descriptor, declaration, context)
        }
    }

    private fun checkFunction(descriptor: FunctionDescriptor, declaration: KtDeclaration, context: DeclarationCheckerContext) {
        // Rule 1: Composable functions cannot be suspend
        if (descriptor.isComposableCallable() && descriptor.isSuspend) {
            context.trace.report(COMPOSABLE_SUSPEND_FUN.on(declaration))
        }

        // Rule 2: Main function cannot be composable
        if (descriptor.isComposableCallable() && descriptor.name.asString() == "main") {
            context.trace.report(COMPOSABLE_FUN_MAIN.on(declaration))
        }

        // Rule 3: Composability must be consistent in overrides
        descriptor.overriddenDescriptors.forEach { overridden ->
            if (descriptor.isComposableCallable() != overridden.isComposableCallable()) {
                context.trace.report(
                    CONFLICTING_OVERLOADS.on(declaration, listOf(descriptor, overridden))
                )
            }
        }

        // Rule 4: Open composable functions with default params (pre-Kotlin 2.0)
        if (descriptor.isComposableCallable() &&
            descriptor.modality != Modality.FINAL &&
            descriptor.valueParameters.any { it.hasDefaultValue() }) {
            if (!context.languageVersionSettings.supportsFeature(LanguageFeature.ComposeOpenFunctions)) {
                context.trace.report(OPEN_FUNCTION_WITH_DEFAULT_PARAMETERS.on(declaration))
            }
        }
    }
}
```

#### Property Restrictions

Composable properties have specific limitations:

```kotlin
private fun checkProperty(descriptor: PropertyDescriptor, declaration: KtDeclaration, context: DeclarationCheckerContext) {
    if (!descriptor.isComposableCallable()) return

    // Rule 1: Composable properties cannot have backing fields
    if (descriptor.hasBackingField()) {
        context.trace.report(COMPOSABLE_VAR.on(declaration))
    }

    // Rule 2: Composable properties cannot be var (have setters)
    if (descriptor.isVar) {
        context.trace.report(COMPOSABLE_VAR.on(declaration))
    }

    // Rule 3: Composable delegates restricted to getValue only
    if (declaration is KtProperty && declaration.hasDelegate()) {
        if (descriptor.isVar) {
            // setValue on composable delegate not allowed
            context.trace.report(COMPOSABLE_PROPERTY_CANNOT_BE_VAR.on(declaration))
        }
    }
}
```

#### Override Consistency

Ensures override hierarchies maintain consistent composability:

```kotlin
private fun checkOverrideConsistency(
    descriptor: CallableDescriptor,
    context: DeclarationCheckerContext
) {
    val isComposable = descriptor.isComposableCallable()

    for (overridden in descriptor.overriddenDescriptors) {
        val overriddenComposable = overridden.isComposableCallable()

        if (isComposable != overriddenComposable) {
            context.trace.report(
                CONFLICTING_OVERLOADS.on(
                    descriptor.source.getPsi()!!,
                    listOf(descriptor, overridden)
                )
            )
        }

        // Check applier compatibility
        val applierInferencer = ApplierInferencer(descriptor.module)
        if (!applierInferencer.areCompatible(descriptor, overridden)) {
            context.trace.report(
                COMPOSE_APPLIER_DECLARATION_MISMATCH.on(
                    descriptor.source.getPsi()!!
                )
            )
        }
    }
}
```

### 8.4 Applier Target System

The applier target system ensures composable functions target compatible UI frameworks.

#### Scheme Structure

Applier schemes encode which UI framework a composable targets:

```kotlin
sealed class Item {
    class Token(val value: String) : Item()  // Concrete applier
    class Open(val index: Int) : Item()      // Generic/unknown applier
}

data class Scheme(
    val target: Item,
    val parameters: List<Scheme> = emptyList(),
    val result: Scheme? = null
) {
    fun isOpen() = target is Item.Open
    fun isConcrete() = target is Item.Token
}
```

**Example Schemes:**

```kotlin
// UI Composable targeting Android UI
Scheme(Item.Token("androidx.compose.ui.UiComposable"))

// Generic composable (works with any applier)
Scheme(Item.Open(-1))

// Composable with lambda expecting UI target
Scheme(
    target = Item.Token("androidx.compose.ui.UiComposable"),
    parameters = listOf(
        Scheme(Item.Token("androidx.compose.ui.UiComposable"))
    )
)
```

#### Target Inference Algorithm

The `ApplierInferencer` class performs unification to resolve applier targets:

```kotlin
class ApplierInferencer(private val module: ModuleDescriptor) {
    fun inferredScheme(descriptor: CallableDescriptor): Scheme {
        // 1. Extract scheme from annotations
        val annotationScheme = descriptor.getComposableTargetAnnotation()?.let { annotation ->
            schemeFromAnnotation(annotation)
        }

        // 2. Build call graph
        val callGraph = buildCallGraph(descriptor)

        // 3. Create constraint system
        val constraints = mutableListOf<Constraint>()
        for ((caller, callee) in callGraph) {
            constraints.add(UnifyConstraint(caller.scheme, callee.scheme))
        }

        // 4. Solve constraints via unification
        val substitution = unify(constraints)

        // 5. Apply substitution to get concrete scheme
        return annotationScheme?.apply(substitution) ?: Scheme(Item.Open(-1))
    }

    private fun unify(constraints: List<Constraint>): Substitution {
        val subst = mutableMapOf<Int, Item>()

        for (constraint in constraints) {
            when (constraint) {
                is UnifyConstraint -> {
                    when {
                        constraint.left.isOpen() && constraint.right.isConcrete() -> {
                            subst[(constraint.left.target as Item.Open).index] = constraint.right.target
                        }
                        constraint.left.isConcrete() && constraint.right.isOpen() -> {
                            subst[(constraint.right.target as Item.Open).index] = constraint.left.target
                        }
                        constraint.left.isConcrete() && constraint.right.isConcrete() -> {
                            if (constraint.left.target != constraint.right.target) {
                                throw IncompatibleApplierException(constraint.left, constraint.right)
                            }
                        }
                    }
                }
            }
        }

        return Substitution(subst)
    }
}
```

#### Cross-Target Validation

Validates that composable calls use compatible appliers:

```kotlin
private fun checkApplierCompatibility(
    caller: CallableDescriptor,
    callee: CallableDescriptor,
    context: CallCheckerContext
) {
    val callerScheme = inferredScheme(caller)
    val calleeScheme = inferredScheme(callee)

    if (!areCompatible(callerScheme, calleeScheme)) {
        context.trace.report(
            COMPOSE_APPLIER_CALL_MISMATCH.on(
                context.element,
                calleeScheme.target.toString(),
                callerScheme.target.toString()
            )
        )
    }
}
```

**Example Error:**

```kotlin
@Composable
@ComposableTarget("androidx.compose.ui.UiComposable")
fun UiButton(text: String) { /* ... */ }

@Composable
@ComposableTarget("com.example.CustomApplier")
fun CustomWidget() { /* ... */ }

@Composable
fun Screen() {
    UiButton("Click") {
        CustomWidget()  // ERROR: Incompatible applier targets
    }
}
```

### 8.5 Type Resolution and Inference

Automatically infers `@Composable` annotation on lambda expressions based on expected type.

#### Automatic Composable Inference

```kotlin
class ComposeTypeResolutionInterceptorExtension : TypeResolutionInterceptorExtension {
    override fun interceptFunctionLiteralDescriptor(
        expression: KtLambdaExpression,
        context: TypeResolutionContext,
        descriptor: FunctionDescriptor
    ): FunctionDescriptor {
        val expectedType = context.expectedType ?: return descriptor

        if (!expectedType.isComposableType()) return descriptor
        if (descriptor.isComposableCallable()) return descriptor

        // Infer @Composable on lambda
        val composableDescriptor = descriptor.copy(
            annotations = descriptor.annotations + ComposableAnnotation
        )

        // Record inference
        context.trace.record(
            ComposeFrontEndWritableSlices.INFERRED_COMPOSABLE_DESCRIPTOR,
            composableDescriptor,
            true
        )

        context.trace.record(
            ComposeFrontEndWritableSlices.INFERRED_COMPOSABLE_LITERAL,
            expression.functionLiteral,
            true
        )

        return composableDescriptor
    }
}
```

#### Lambda Type Adaptation

The system automatically adapts lambda types to match composable expectations:

```kotlin
// Expected: @Composable () -> Unit
val content: @Composable () -> Unit = {
    // Lambda automatically becomes @Composable
    Text("Hello")  // Composable call allowed
}

// Without inference, this would be an error
Column(
    content = {  // Automatically @Composable
        Text("Item 1")
        Text("Item 2")
    }
)
```

### 8.6 Analysis Pipeline

#### Compilation Phases

The analysis system operates through distinct compilation phases:

```
┌─────────────────────────┐
│    1. PARSING           │
│  PSI Tree Construction  │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│  2. RESOLUTION (K1/K2)  │
│  Type & Symbol Binding  │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   3. DIAGNOSTIC PHASE   │
│  Checkers & Validators  │
│  ┌──────────────────┐   │
│  │ Annotation Check │   │
│  │ Declaration Check│   │
│  │ Call Check       │   │
│  │ Target Check     │   │
│  └──────────────────┘   │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   4. IR GENERATION      │
│  Convert PSI to IR Tree │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   5. IR ANALYSIS        │
│  Stability Inference    │
│  Static Detection       │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   6. IR LOWERING        │
│  Transform Composables  │
└─────────────────────────┘
```

#### Data Flow Through Phases

**Frontend Analysis → WritableSlices:**

```kotlin
// During type resolution
ComposeTypeResolutionInterceptor {
    record(INFERRED_COMPOSABLE_DESCRIPTOR, descriptor, true)
}

// During call checking
ComposableCallChecker {
    record(LAMBDA_CAPABLE_OF_COMPOSER_CAPTURE, lambda, true)
}

// During target checking
ComposableTargetChecker {
    record(COMPOSE_LAZY_SCHEME, function, scheme)
}
```

**IR Analysis → WritableSlices:**

```kotlin
// During stability analysis
StabilityInferencer {
    irTrace.record(IS_STATIC_EXPRESSION, expression, isStatic)
}

// During key generation
DurableFunctionKeyTransformer {
    irTrace.record(DURABLE_FUNCTION_KEY, function, keyInfo)
}
```

**IR Lowering → Read Slices:**

```kotlin
// During composable transformation
ComposableFunctionBodyTransformer {
    val isStatic = irTrace[IS_STATIC_EXPRESSION, expr]
    val key = irTrace[DURABLE_FUNCTION_KEY, function]

    if (isStatic) {
        // Generate optimized code
    } else {
        // Generate standard code
    }
}
```

### 8.7 Practical Examples

#### Example: Composable Context Validation

```kotlin
// Source code
class MainActivity {
    fun onCreate() {
        Text("Hello")  // ERROR: Not in composable context
    }

    @Composable
    fun Content() {
        Text("Hello")  // OK: In composable function

        runBlocking {
            Text("Error")  // ERROR: In non-composable lambda
        }

        LaunchedEffect(Unit) {
            Text("Error")  // ERROR: LaunchedEffect block is suspend, not composable
        }
    }
}
```

**Analysis Flow:**

1. `ComposableCallChecker` sees `Text("Hello")` call
2. Walks up PSI tree from call site
3. In `onCreate`: Reaches `KtFunction` without `@Composable` → Reports `COMPOSABLE_INVOCATION`
4. In `Content`: Finds `@Composable` function → Valid
5. In `runBlocking`: Lambda not composable → Reports `COMPOSABLE_INVOCATION`
6. In `LaunchedEffect`: Suspend lambda context → Reports `COMPOSABLE_INVOCATION`

#### Example: Inline Lambda Analysis

```kotlin
inline fun <T> withoutComposables(
    @DisallowComposableCalls noinline block: () -> T
): T = block()

@Composable
fun Screen() {
    withoutComposables {
        val data = loadData()  // OK
        Text(data)  // ERROR: CAPTURED_COMPOSABLE_INVOCATION
    }
}
```

**Analysis Flow:**

1. `ComposableCallChecker.checkInlineLambdaCall()` examines `withoutComposables` call
2. Finds parameter `block` has `@DisallowComposableCalls`
3. Traverses lambda body AST
4. Finds composable call to `Text()`
5. Reports `CAPTURED_COMPOSABLE_INVOCATION` with parameter name and function

#### Example: Stability and Skipping

```kotlin
// Source
data class StableUser(val name: String, val age: Int)
data class UnstableUser(var name: String, var age: Int)

@Composable
fun StableUserCard(user: StableUser) {
    Text(user.name)
}

@Composable
fun UnstableUserCard(user: UnstableUser) {
    Text(user.name)
}
```

**Analysis and Generation:**

1. **Stability Analysis:**
   - `StableUser`: All `val` properties of stable types → `Certain(stable = true)`
   - `UnstableUser`: Has `var` properties → `Certain(stable = false)`

2. **WritableSlice Recording:**
   ```kotlin
   irTrace.record(IS_STATIC_EXPRESSION, stableUserParam, false)
   irTrace.record(IS_STATIC_EXPRESSION, unstableUserParam, false)
   ```

3. **Code Generation Difference:**

   **StableUserCard (can skip):**
   ```kotlin
   fun StableUserCard(user: StableUser, $composer: Composer, $changed: Int) {
       $composer.startRestartGroup(key)
       if ($changed and 0x1 == 0 && $composer.skipping) {
           $composer.skipToGroupEnd()  // Skip if user unchanged
       } else {
           Text(user.name, $composer, 0)
       }
       $composer.endRestartGroup()?.updateScope {
           StableUserCard(user, it, $changed or 0x1)
       }
   }
   ```

   **UnstableUserCard (always executes):**
   ```kotlin
   fun UnstableUserCard(user: UnstableUser, $composer: Composer, $changed: Int) {
       $composer.startRestartGroup(key)
       // No skipping logic - always executes
       Text(user.name, $composer, 0)
       $composer.endRestartGroup()?.updateScope {
           UnstableUserCard(user, it, 0x1)
       }
   }
   ```

The analysis system's decisions directly impact runtime performance through intelligent code generation based on stability inference and validation results.

## Appendix: Source Code References

### Primary Source Files

The source code is available as part of the Kotlin compiler project:

**Repository:** https://github.com/JetBrains/kotlin/tree/master/plugins/compose

1. **`Stability.kt`** (Line count: 479)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/analysis/Stability.kt`
   - Contains: Stability type definitions, inference algorithm, helper functions

2. **`KnownStableConstructs.kt`** (Line count: 107)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/analysis/KnownStableConstructs.kt`
   - Contains: Registry of known stable types and functions

3. **`ClassStabilityTransformer.kt`** (Line count: 219+)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/lower/ClassStabilityTransformer.kt`
   - Contains: Stability field generation, annotation application

4. **`StabilityConfigParser.kt`** (Line count: 81)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/analysis/StabilityConfigParser.kt`
   - Contains: Configuration file parsing

5. **`StabilityExternalClassNameMatching.kt`** (Line count: 237)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/analysis/StabilityExternalClassNameMatching.kt`
   - Contains: Pattern matching for external type configuration

6. **`AbstractComposeLowering.kt`** (Line count: 1785)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/lower/AbstractComposeLowering.kt`
   - Contains: Runtime field generation utilities (lines 820-950)

7. **`ComposableCallChecker.kt`** (Line count: 673)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/k1/ComposableCallChecker.kt`
   - Contains: Composable call validation logic

8. **`ComposableDeclarationChecker.kt`** (Line count: 271)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/k1/ComposableDeclarationChecker.kt`
   - Contains: Declaration validation rules

9. **`ComposableTargetChecker.kt`** (Line count: 452)
   - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/k1/ComposableTargetChecker.kt`
   - Contains: Applier target inference and validation

10. **`ComposeWritableSlices.kt`** (Line count: 31)
    - Location: `compiler-hosted/src/main/java/androidx/compose/compiler/plugins/kotlin/analysis/ComposeWritableSlices.kt`
    - Contains: IR-level data storage definitions

## Conclusion

The Compose compiler's stability inference system operates through a multi-phase analysis process that examines types, classes, and expressions. The system balances compile-time analysis with runtime checks, enabling optimization while maintaining correctness guarantees.

Key principles:
1. Stability enables recomposition skipping through value comparison
2. The algorithm proceeds from fast paths to detailed analysis
3. Bitmasks encode generic type parameter dependencies
4. External modules require runtime stability fields
5. Configuration files extend stability to external types
6. Conservative handling ensures correctness over optimization

Understanding this system allows developers to structure code for optimal Compose performance while maintaining type safety and correctness. If you want to get more information about the Compose performance, check out [compose-performance repository](https://github.com/skydoves/compose-performance).

<a href="https://www.android.skydoves.me/">
<img src="https://github.com/user-attachments/assets/e014ce01-3461-40af-bb2a-eb44f3f55f36" width="13%" align="right"/>
</a>

## 📘 Manifest Android Interview

[Manifest Android Interview](https://www.android.skydoves.me/) is a comprehensive guide designed to enhance your Android development expertise through 108 interview questions with detailed answers, 162 additional practical questions, and 50+ "Pro Tips for Mastery" sections. The interview questions primarily focus on Android development—including the Framework, UI, Jetpack Libraries, and Business Logic—as well as Jetpack Compose, covering Fundamentals, Runtime, and UI.

<a href="https://github.com/doveletter">
<img src="https://github.com/user-attachments/assets/3ecd2a7b-9713-40cd-8817-fa568271cefa" width="13%" align="right"/>
</a>

## 🕊️ Dove Letter

If you're eager to dive deeper into Kotlin and Android, explore [Dove Letter](https://github.com/doveletter), a private subscription repository where you can learn, discuss, and share knowledge. To get more details about this unique opportunity, check out the [Learn Kotlin and Android With Dove Letter](https://medium.com/@skydoves/learn-kotlin-and-android-with-dove-letter-26265da11903) article.

## Find this repository useful? :heart:
Support it by joining __[stargazers](https://github.com/skydoves/compose-stability-inference/stargazers)__ for this repository. :star: <br>
Also, __[follow me](https://github.com/skydoves)__ on GitHub for my next creations! 🤩

# License
```xml
Designed and developed by 2025 skydoves (Jaewoong Eum)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
