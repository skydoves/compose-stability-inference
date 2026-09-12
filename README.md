# Compose Compiler Stability Inference System

A comprehensive study of how the Compose compiler determines type stability for recomposition optimization. All details that [Optimize App Performance By Mastering Stability in Jetpack Compose](https://medium.com/proandroiddev/optimize-app-performance-by-mastering-stability-in-jetpack-compose-69f40a8c785d) and [compose-performance](https://github.com/skydoves/compose-performance) couldn't take in.

## 💝 Sponsors

<a href="https://coderabbit.link/Jaewoong" target="_blank"> <img width="300" alt="coderabbit" src="https://github.com/user-attachments/assets/9823e1d3-8467-4d4d-8a53-94b3c0adc630" /></a>

<a href="https://getstream.io/chat/sdk/android/?utm_source=github&utm_medium=referral&utm_content=&utm_campaign=Jaewoong_github_2025" target="_blank"> <img width="260" alt="stream" src="https://github.com/user-attachments/assets/87a69228-4fef-4f48-ad98-1e2c606c5b7e" /></a>

<a href="https://howcomposeworks.com/">
<img src="https://github.com/user-attachments/assets/0f0f72fc-49ce-48b5-b3dd-f2c04e907f80" width="13%" align="right"/>
</a>

## 📗 Jetpack Compose Mechanisms Book

[Jetpack Compose Mechanisms](https://howcomposeworks.com/) takes you from "how to use Compose" into "how Compose actually works," tracing the AOSP source line by line through the compiler, runtime, and UI layers beneath every Composable, with practical, production-ready examples from the author's own Compose tooling and libraries. It then ties all three layers together into deep, real-world performance tuning, from stability inference to the skip decision. Fully updated for Kotlin 2.4.0 and Compose Compiler 2.4.0.

## Table of Contents

- [Compose Compiler Stability Inference System](#compose-compiler-stability-inference-system)
  - [💝 Sponsors](#-sponsors)
  - [📗 Jetpack Compose Mechanisms Book](#-jetpack-compose-mechanisms-book)
  - [Table of Contents](#table-of-contents)
  - [Chapter 1: Foundations](#chapter-1-foundations)
    - [1.1 Introduction](#11-introduction)
    - [1.2 Core Concepts](#12-core-concepts)
      - [Stability Definition](#stability-definition)
      - [Recomposition Mechanics](#recomposition-mechanics)
    - [1.3 The Role of Stability](#13-the-role-of-stability)
      - [Performance Impact](#performance-impact)
    - [1.4 Strong Skipping](#14-strong-skipping)
  - [Chapter 2: Stability Type System](#chapter-2-stability-type-system)
    - [2.1 Type Hierarchy](#21-type-hierarchy)
    - [2.2 Compile Time Stability](#22-compile-time-stability)
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
    - [3.2 Type Level Analysis](#32-type-level-analysis)
      - [Phase 1: Fast Path Type Checks](#phase-1-fast-path-type-checks)
      - [Phase 2: Type Parameter Handling](#phase-2-type-parameter-handling)
      - [Phase 3: Nullable Type Unwrapping](#phase-3-nullable-type-unwrapping)
      - [Phase 4: Value Class Handling](#phase-4-value-class-handling)
    - [3.3 Class Level Analysis](#33-class-level-analysis)
      - [Phase 5: Cycle Detection](#phase-5-cycle-detection)
      - [Phase 6: Annotation and Marker Checks](#phase-6-annotation-and-marker-checks)
      - [Phase 7: Known Constructs](#phase-7-known-constructs)
      - [Phase 8: External Configuration](#phase-8-external-configuration)
      - [Phase 9: Runtime Stability for Separately Compiled Classes](#phase-9-runtime-stability-for-separately-compiled-classes)
      - [Phase 10: Java Type Handling](#phase-10-java-type-handling)
      - [Phase 11: General Interface Handling](#phase-11-general-interface-handling)
      - [Phase 12: Field by Field Analysis](#phase-12-field-by-field-analysis)
    - [3.4 Expression Level Analysis](#34-expression-level-analysis)
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
    - [5.1 Primitive and Standard Library Types](#51-primitive-and-standard-library-types)
      - [Integer Types](#integer-types)
      - [String Type](#string-type)
      - [Function Types](#function-types)
    - [5.2 User Defined Classes](#52-user-defined-classes)
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
      - [Compiler Level Differences: @Stable vs @Immutable](#compiler-level-differences-stable-vs-immutable)
      - [@StableMarker Meta Annotation](#stablemarker-meta-annotation)
    - [6.2 Configuration Files](#62-configuration-files)
      - [File Format](#file-format)
      - [Pattern Syntax](#pattern-syntax)
      - [Gradle Configuration](#gradle-configuration)
      - [Feature Flags](#feature-flags)
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
      - [Example: Self Referential Type](#example-self-referential-type)
      - [Limitation](#limitation)
    - [7.3 Special Cases](#73-special-cases)
      - [Protobuf Types](#protobuf-types)
      - [Delegated Properties](#delegated-properties)
      - [Value Classes with Markers](#value-classes-with-markers)
  - [Chapter 8: Compiler Analysis System](#chapter-8-compiler-analysis-system)
    - [8.1 Analysis Infrastructure](#81-analysis-infrastructure)
      - [IR Attributes: Backend Data Flow](#ir-attributes-backend-data-flow)
      - [FIR Session Components: Frontend Data Flow](#fir-session-components-frontend-data-flow)
    - [8.2 Composable Call Validation](#82-composable-call-validation)
      - [Scope Walking Algorithm](#scope-walking-algorithm)
      - [Validation Order](#validation-order)
      - [Readonly Composables](#readonly-composables)
      - [Property Getters and Delegates](#property-getters-and-delegates)
      - [Propagating @DisallowComposableCalls](#propagating-disallowcomposablecalls)
    - [8.3 Declaration Validation](#83-declaration-validation)
      - [Composable Function Rules](#composable-function-rules)
      - [Property Restrictions](#property-restrictions)
      - [Composable Type Positions](#composable-type-positions)
      - [Diagnostic Severity](#diagnostic-severity)
    - [8.4 Applier Target System](#84-applier-target-system)
      - [Scheme Structure](#scheme-structure)
      - [Target Inference Algorithm](#target-inference-algorithm)
      - [Where Targets Come From](#where-targets-come-from)
      - [Cross Target Validation](#cross-target-validation)
    - [8.5 Composable Function Types](#85-composable-function-types)
    - [8.6 Analysis Pipeline](#86-analysis-pipeline)
      - [Compilation Phases](#compilation-phases)
      - [Extension Registration](#extension-registration)
      - [Data Flow Through Phases](#data-flow-through-phases)
    - [8.7 Practical Examples](#87-practical-examples)
      - [Example: Composable Context Validation](#example-composable-context-validation)
      - [Example: Inline Lambda Analysis](#example-inline-lambda-analysis)
      - [Example: Stability and Skipping](#example-stability-and-skipping)
  - [Conclusion](#conclusion)
  - [📘 Manifest Android Interview](#-manifest-android-interview)
  - [🕊️ Dove Letter](#️-dove-letter)
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

Without stability information, the runtime has to recompose on every invocation, whether or not the parameters changed. That was the whole story until strong skipping landed, which section 1.4 covers.

### 1.3 The Role of Stability

#### Performance Impact

Stability inference affects recomposition in three ways:

**Skipping**: a composable whose parameter values did not change can be skipped instead of executed. Which parameters count as unchanged depends on stability, so this is where the inference pays off.

**Comparison Propagation**: the compiler passes what it knows about a parameter down to child composable calls through the `$changed` mask, so a value already proven unchanged is not compared again further down the tree.

**Comparison Strategy**: the runtime picks structural equality (`equals()`) for stable types and referential equality (`===`) for unstable ones. Section 1.4 follows this thread, because since strong skipping became the default this is the part that still decides the outcome.

Consider this example:

```kotlin
// Unstable parameter type: an interface with unknown stability
@Composable
fun ExpensiveList(items: List<String>) {
    // List is an interface, so it has Unknown stability
    // Comparison falls back to the instance
}

// Stable parameter type: an immutable collection
@Composable
fun ExpensiveList(items: ImmutableList<String>) {
    // ImmutableList is in KnownStableConstructs
    // Comparison uses equals()
}

// The expression and the type are two different questions
@Composable
fun Caller() {
    // listOf() is a known stable function, so this expression is stable
    val items = listOf("a", "b")
    // but the parameter type is still List, which is Unknown
    ExpensiveList(items)
}
```

`List` and `MutableList` are both interfaces, so both have `Unknown` stability. To get a stable parameter, use one of:

1. `ImmutableList` from kotlinx.collections.immutable, which is registered in `KnownStableConstructs`
2. `kotlin.collections.List` added to your stability configuration file
3. The `@Stable` annotation on the class that holds the list

### 1.4 Strong Skipping

Everything above describes the classic skipping rule: a restartable composable is skippable only when every parameter type is stable. Strong skipping changes that rule, and it has been enabled by default since the compiler shipped `FeatureFlag.StrongSkipping` with `default = true`:

```kotlin
enum class FeatureFlag(val featureName: String, val default: Boolean) {
    StrongSkipping("StrongSkipping", default = true),
    IntrinsicRemember("IntrinsicRemember", default = true),
    OptimizeNonSkippingGroups("OptimizeNonSkippingGroups", default = true),
    PausableComposition("PausableComposition", default = true),
    ;
}
```

With strong skipping on, every restartable composable becomes skippable, whatever the stability of its parameters. Stability no longer decides *whether* the function can skip. It decides *how the parameter is compared*:

- A stable parameter is compared with `Composer.changed()`, which uses structural equality (`equals()`)
- An unstable parameter is compared with `Composer.changedInstance()`, which uses referential equality (`===`)

Strong skipping also memoizes lambdas that capture unstable values, which previously blocked lambda reuse.

So stability still matters, just for a different reason. A `data class` that is unstable will be compared by identity, and a fresh instance built on every recomposition will never compare equal, so the child recomposes every time even though its contents are identical. To opt a composable out of skipping entirely, annotate it with `@NonSkippableComposable`.

## Chapter 2: Stability Type System

### 2.1 Type Hierarchy

The compiler represents stability through a sealed class hierarchy defined in `Stability.kt`:

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

### 2.2 Compile Time Stability

#### Stability.Certain

This type represents stability the compiler can settle completely at compile time.

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

**Implementation:** See `Stability.kt` for the `knownStable()` extension function.

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

// Compiler generated code
@StabilityInferred(parameters = 0b1)
class Box<T>(val value: T) {
    // a synthetic static final int placed directly on the class,
    // not inside a companion object
    val $stable: Int = 0
}
```

The `StabilityInferred` KDoc in the Compose runtime describes the field the same way: "there will be a synthetic static final int `$stable` added to the class."

**When Applied:**
- Classes compiled in a separate module, which arrive as external stubs carrying an `@StabilityInferred` bitmask
- Public or internal classes declared in a different file than the one being compiled, on JVM

Only those two. A generic class in the same file resolves to `Stability.Parameter` instead, covered in 2.5.

**Runtime Behavior:**

The `$stable` field holds the class's own contribution. The call site combines it with the stability of the type arguments the bitmask selects:

```kotlin
Box<Int>              // $stable contributes 0, Int is stable       -> stable
Box<MutableList<Int>> // $stable contributes 0, MutableList is not  -> unstable
```

**Implementation:** See `Stability.kt` (the `Runtime` handling in `StabilityInferencer.stabilityOf`) and `ClassStabilityTransformer.kt` (the generated `$stable` field).

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
- Interface types, since the implementation is not known
- `open` and `abstract` classes, which seed as `Unknown` because a subclass could add unstable state

**Runtime Behavior:**

`Unknown` is the one stability that cannot be expressed at runtime. `isExpressible()` returns `false` for it, so there is no `$stable` read to emit and no bit to set, and the class falls back to `Unstable` at the use site. Comparison then goes through `changedInstance`, which uses `===`.

**Implementation:** See `Stability.kt` (the `Stability.Unknown` branch in `StabilityInferencer.stabilityOf`).

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

**Implementation:** See `Stability.kt` for type parameter handling (the `isTypeParameter()` branch and `applyTypeParameterMask`).

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

The compiler combines stabilities using the `plus` operator (defined in `Stability.kt`):

```kotlin
operator fun plus(other: Stability): Stability = when {
    other is Certain -> if (other.stable) this else other
    this is Certain -> if (stable) other else this
    else -> Combined(listOf(this, other))
}
```

Worked examples:

```kotlin
Stable     + Stable     = Stable
Stable     + Unstable   = Unstable
Unstable   + Stable     = Unstable
Stable     + Parameter  = Parameter                     // a stable Certain is absorbed
Parameter  + Parameter  = Combined([Parameter, Parameter])
Runtime    + Parameter  = Combined([Runtime, Parameter])
```

**The key observation:** adding a stable `Certain` returns the *other* operand unchanged, so only genuinely uncertain factors (`Parameter`, `Runtime`, `Unknown`) accumulate into a `Combined`. A `Combined` is only produced when neither operand is `Certain`.

**Key Property:** unstable stability dominates all combinations. A single unstable component makes the entire result unstable.

### 2.7 Stability Decision Tree

The Compose compiler follows a systematic decision tree when determining stability. This tree represents the actual logic flow implemented in the compiler.

#### Complete Decision Tree

```
┌─────────────────────────────────┐
│     Start: Analyze a type       │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  Error or dynamic type?         │───Yes──→ [UNSTABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Unit, primitive, String, or    │───Yes──→ [STABLE]
│  a function type?               │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Type parameter?                │───Yes──→ substitute, else
└────────────┬────────────────────┘          [PARAMETER]
             │ No
             ▼
┌─────────────────────────────────┐
│  Nullable?                      │───Yes──→ analyze the
└────────────┬────────────────────┘          non null type
             │ No
             ▼
┌─────────────────────────────────┐
│  Value class?                   │───Yes──→ marker → [STABLE],
│  (multi field, then inline)     │          else analyze the
└────────────┬────────────────────┘          underlying types
             │ No
             ▼
┌─────────────────────────────────┐
│     Now analyze the class       │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  Already being analyzed?        │───Yes──→ [UNSTABLE]
│  (cycle)                        │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Has a stable marked descendant?│───Yes──→ [STABLE]
│  (@Stable, @Immutable, any      │
│   @StableMarker annotation,     │
│   or a known stable marker)     │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Enum class or enum entry?      │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Object (singleton)?            │───Yes──→ [STABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Primitive, or a Protobuf type? │───Yes──→ [STABLE]
│  (GeneratedMessage/Lite)        │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  In KnownStableConstructs?      │───Yes──→ [STABLE] combined with
│  (Pair, Triple, ImmutableList…) │          the masked type params
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Matches the stability config?  │───Yes──→ [STABLE] combined with
│  (stability_config.conf)        │          the masked type params
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  External Java stub?            │───Yes──→ [UNSTABLE]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Interface?                     │───Yes──→ [UNKNOWN]
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  External stub with no          │───Yes──→ [UNSTABLE]
│  @StabilityInferred bitmask?    │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  JVM, public or internal, and   │───Yes──→ [RUNTIME] combined with
│  declared in a different file?  │          every type parameter
│  (incremental compilation)      │          (mask = null)
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  External stub?                 │───Yes──→ [RUNTIME] combined with
│  (separately compiled module)   │          the masked type params
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Seed stability:                │
│  final class → STABLE           │
│  non final   → UNKNOWN          │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  Any non delegated var          │───Yes──→ [UNSTABLE]
│  property with a backing field? │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Combine every backing field    │
│  type, then the superclass      │
│  (dropped if it is Unknown)     │
└────────────┬────────────────────┘
             │
             ▼
        [STABLE / UNSTABLE / COMBINED / UNKNOWN]
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
│  (a val, not a var)             │          stability
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  A local delegated property     │───Yes──→ [STABLE]
│  reference?                     │
└────────────┬────────────────────┘
             │ No
             ▼
┌─────────────────────────────────┐
│  Is it a composite with all     │───Yes──→ [STABLE]
│  stable subexpressions?         │
└────────────┬────────────────────┘
             │ No
             ▼
   Fall back to the type's stability
```

Note the last box. An expression the analysis cannot say anything extra about does not become unstable, it keeps whatever its type already said. Expression analysis only ever improves on the type result, never worsens it.

#### Key Decision Points Explained

**1. Early Exit Conditions:**
- Primitives, String, Unit, and function types are immediately stable
- Stability annotations override all other checks, including the value class branches
- Enums, both classes and entries, are always stable because their instances are singletons and their state is fixed after initialization
- Objects are always stable, since there is exactly one instance and identity comparison always holds

**2. Interface Handling:**
- Interfaces return `Unknown` stability because implementations can vary
- Exception: Interfaces with `@Stable` marker are trusted

**3. External Types:**
- Java types default to unstable, since Java has no `val` guarantee
- Protobuf types are special cased as stable, because generated messages present an immutable API
- External Kotlin modules use `@StabilityInferred` bitmasks, and an external stub without one is unstable

**4. Member Analysis:**
- The seed stability is `Stable` for `final` classes and `Unknown(declaration)` for non final (`open` or `abstract`) classes
- Any non delegated `var` property makes the entire class unstable
- Delegated properties are analyzed through their delegate type instead
- Superclass stability is combined into the result, but an `Unknown` superclass result is dropped, so an open superclass does not poison the subclass

**5. Generic Type Resolution:**
- The bitmask encodes which type parameters affect stability
- At most 32 type parameters are considered, since the mask is an `Int`
- Type arguments are substituted and analyzed recursively

This decision tree is implemented across several functions in the compiler, with the main entry point being `StabilityInferencer.stabilityOf()`.

## Chapter 3: The Inference Algorithm

### 3.1 Algorithm Overview

The algorithm follows the decision tree above. `StabilityInferencer` carries three pieces of state through the whole recursion, and each phase below makes more sense once you know what they are:

- **`substitutions`**: a map from type parameter symbol to the type argument currently bound to it, grown as the walk descends through generic types
- **`currentlyAnalyzing`**: the set of symbols on the current analysis stack, which is what stops a recursive type from recursing forever
- **`analysisEntryFile`**: the file that started this whole request, which decides whether a class can be inferred concretely or has to fall back to a runtime read

Results are cached in `cache`, keyed by `SymbolForAnalysis`, but only when the declaration lives in `analysisEntryFile`. A result that depended on which file asked the question cannot be reused by a different question.

The algorithm short circuits as soon as it can settle stability definitively, so most types never reach the full member walk.

### 3.2 Type Level Analysis

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
    val symbol = SymbolForAnalysis(classifier, emptyList(), analysisEntryFile)
    if (arg != null && symbol !in currentlyAnalyzing) {
        stabilityOf(arg, substitutions, currentlyAnalyzing + symbol, analysisEntryFile)
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

Nullable types defer to their non null counterpart:

```kotlin
type.isNullable() -> stabilityOf(
    type.makeNotNull(),
    substitutions,
    currentlyAnalyzing,
    analysisEntryFile
)
```

**Examples:**
- `Int?` → analyze `Int` → Stable
- `User?` → analyze `User` → depends on User structure

#### Phase 4: Value Class Handling

Kotlin has two shapes of value class, and the compiler checks them in order. A **multi field value class** holds more than one underlying property, and the compiler reports it through `isFullValueClassType()`:

```kotlin
type.isFullValueClassType() -> {
    val valueClassDeclaration = type.getClass()
        ?: error("Failed to resolve the class definition of full value class type $type")
    if (valueClassDeclaration.hasStableMarker()) {
        Stability.Stable
    } else {
        val primaryProperties = valueClassDeclaration.valueClassRepresentation
            ?.underlyingPropertyNamesToTypes
            ?: return Stability.Unstable // is abstract value class
        primaryProperties
            .map { (_, type) -> stabilityOf(type, substitutions, currentlyAnalyzing, analysisEntryFile) }
            .let { Stability.Combined(it) }
    }
}
```

Every underlying property contributes, and the results are folded into a `Combined`. An abstract value class has no `valueClassRepresentation`, so it falls back to `Unstable`.

A single field **inline class** unwraps to its one underlying type instead:

```kotlin
type.isInlineClassType() -> {
    val inlineClassDeclaration = type.getClass()
        ?: error("Failed to resolve the class definition of inline type $type")

    if (inlineClassDeclaration.hasStableMarker()) {
        Stability.Stable
    } else {
        stabilityOf(
            type = getInlineClassUnderlyingType(
                inlineClassDeclaration,
                treatCompatibleFullValueClassesAsInline = false
            ),
            substitutions = substitutions,
            currentlyAnalyzing = currentlyAnalyzing,
            analysisEntryFile
        )
    }
}
```

The `treatCompatibleFullValueClassesAsInline = false` argument is what keeps the two branches apart. Without it, a multi field value class that happens to be layout compatible with an inline class would be unwrapped to a single type and the other properties would go unchecked.

**Examples:**

```kotlin
@JvmInline
value class UserId(val value: Int)
// Checks: stabilityOf(Int) = Stable

@JvmInline
value class Token(val value: String)
// Checks: stabilityOf(String) = Stable

value class Range(val start: Int, val end: Int)
// Multi field value class
// Checks both: Combined([Stable, Stable])

@JvmInline
@Stable
value class SpecialId(val list: MutableList<Int>)
// @Stable marker overrides underlying type analysis
// Result: Stable (by annotation)
```

A stable marker short circuits both branches, so the annotation wins over whatever the underlying types say.

### 3.3 Class Level Analysis

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
if (declaration.isObject) return Stability.Stable
if (declaration.defaultType.isPrimitiveType()) return Stability.Stable
if (declaration.isProtobufType()) return Stability.Stable
```

**Stable Markers:**
- `@Stable` annotation
- `@Immutable` annotation
- Annotations marked with `@StableMarker`
- Annotations listed in `KnownStableConstructs.stableMarkers`

That last entry exists because some annotations outside Compose carry the same guarantee but cannot be annotated with `@StableMarker`:

```kotlin
val stableMarkers = setOf(
    ClassId(
        FqName("com.google.errorprone.annotations"),
        Name.identifier("Immutable")
    )
)
```

The check itself resolves the annotation class and tests both paths:

```kotlin
private fun IrAnnotation.isStableMarker(): Boolean {
    val owner = annotationClass?.owner ?: return false
    return owner.hasAnnotation(ComposeFqNames.StableMarker) ||
            owner.classId in KnownStableConstructs.stableMarkers
}
```

`hasStableMarkedDescendant()` then walks the supertypes, so a class inherits the marker from any annotated ancestor other than `Any`.

**Enum Handling:**

All enum classes and enum entries are considered stable because:
1. Enum instances are singletons (referential equality works)
2. Enum state is immutable after initialization
3. Enum equality is based on identity

**Object Handling:**

`object` declarations (singletons) are always stable. There is exactly one instance, so identity comparison is always valid regardless of the object's members.

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
    Pair::class.qualifiedName!! to 0b11,
    Triple::class.qualifiedName!! to 0b111,
    Comparator::class.qualifiedName!! to 0b1,
    Result::class.qualifiedName!! to 0b1,
    ClosedRange::class.qualifiedName!! to 0b1,
    ClosedFloatingPointRange::class.qualifiedName!! to 0b1,
    // Guava
    "com.google.common.collect.ImmutableList" to 0b1,
    "com.google.common.collect.ImmutableEnumMap" to 0b11,
    "com.google.common.collect.ImmutableMap" to 0b11,
    "com.google.common.collect.ImmutableEnumSet" to 0b1,
    "com.google.common.collect.ImmutableSet" to 0b1,
    // Kotlinx immutable
    "kotlinx.collections.immutable.ImmutableCollection" to 0b1,
    "kotlinx.collections.immutable.ImmutableList" to 0b1,
    "kotlinx.collections.immutable.ImmutableSet" to 0b1,
    "kotlinx.collections.immutable.ImmutableMap" to 0b11,
    "kotlinx.collections.immutable.PersistentCollection" to 0b1,
    "kotlinx.collections.immutable.PersistentList" to 0b1,
    "kotlinx.collections.immutable.PersistentSet" to 0b1,
    "kotlinx.collections.immutable.PersistentMap" to 0b11,
    // Dagger
    "dagger.Lazy" to 0b1,
    // Coroutines
    EmptyCoroutineContext::class.qualifiedName!! to 0,
    // Java types
    BigInteger::class.qualifiedName!! to 0,
    BigDecimal::class.qualifiedName!! to 0,
    Locale::class.qualifiedName!! to 0,
)
```

The integer value represents a bitmask indicating which type parameters affect stability (covered in Chapter 4). A mask of `0` means the type is stable regardless of its type arguments (e.g. `BigInteger`, `Locale`).

#### Phase 8: External Configuration

Users can provide configuration files declaring types as stable:

```kotlin
if (declaration.isExternalStableType()) {
    val baseStability = Stability.Stable
    return baseStability.applyTypeParameterMask(
        mask = externalTypeMatcherCollection
            .maskForName(declaration.fqNameWhenAvailable) ?: 0,
        typeParameters = typeParameters,
        substitutions,
        analyzing,
        analysisEntryFile,
    )
}
```

The same `applyTypeParameterMask` helper used for `KnownStableConstructs` (Phase 7) combines the base stability with the stability of the type arguments selected by the configured bitmask. Configuration file format is covered in Chapter 6.

#### Phase 9: Runtime Stability for Separately Compiled Classes

Stability inference has to hold up under **incremental compilation**, which is separated by file. If the compiler inferred concrete stability for a class declared in another file, a later edit to that file could silently invalidate the result without recompiling the dependents. To avoid that, a class that is part of the public or internal API and lives in a **different file** than the one that started the analysis is forced to use *runtime* stability: the value of its generated `$stable` field is read at runtime instead of being decided at compile time.

Before that check, an external stub with no bitmask at all is rejected outright:

```kotlin
if (declaration.origin == IrDeclarationOrigin.IR_EXTERNAL_DECLARATION_STUB &&
    declaration.stabilityParamBitmask() == null
) {
    return Stability.Unstable
}
```

The order matters. A class compiled without the Compose compiler has no `$stable` field, so returning `Runtime` for it would emit a read of a field that does not exist. Catching the missing bitmask first is what keeps that from happening.

```kotlin
// `analysisEntryFile` is the file containing the element that started this
// stabilityOf() call tree; `fileContainingDeclaration` is where `declaration` lives.
val forcedToUseRuntimeStability = isTargetJvm &&
    (declaration.visibility.isPublicAPI ||
        declaration.visibility == DescriptorVisibilities.INTERNAL) &&
    (fileContainingDeclaration == null || fileContainingDeclaration != analysisEntryFile)

if (forcedToUseRuntimeStability) {
    val baseStability = Stability.Runtime(declaration)
    return baseStability.applyTypeParameterMask(
        mask = null, // null = consider every type parameter
        typeParameters = typeParameters,
        substitutions,
        analyzing,
        analysisEntryFile,
    )
}

// Classes that come from a separately compiled module arrive as external stubs.
// Their stability is encoded in the @StabilityInferred bitmask.
if (declaration.origin == IrDeclarationOrigin.IR_EXTERNAL_DECLARATION_STUB) {
    val mask = declaration.stabilityParamBitmask() ?: return Stability.Unstable
    val baseStability = Stability.Runtime(declaration)
    return baseStability.applyTypeParameterMask(
        mask,
        typeParameters = typeParameters,
        substitutions,
        analyzing,
        analysisEntryFile,
    )
}
```

**Key points:**

1. The decision is driven by the **file** the declaration lives in, through `analysisEntryFile`, not by a "current module" check. That is what makes the result safe under incremental compilation.
2. `Stability.Runtime(declaration)` means "emit a read of `declaration.$stable` at runtime". It is combined with the stability of the relevant type arguments through `applyTypeParameterMask`.
3. Under `forcedToUseRuntimeStability` the mask is `null`, so **every** type parameter is taken into account. The compiler has no bitmask to consult yet, since the class is being compiled in this same module, so it assumes the worst. For a genuine external stub the recorded `@StabilityInferred` bitmask is used instead.
4. An external stub with no `@StabilityInferred` bitmask, such as a third party class compiled without the Compose compiler, is `Unstable`.
5. The check only applies when `isTargetJvm` is true. Other targets have no static field to read, which is why Chapter 4 describes a separate scheme for Native and JS.

Because an external stub has no `IrFile` parent, `fileContainingDeclaration` is `null` for it, and the file comparison is true. So on JVM a public class from another module takes this branch rather than the external stub branch below, and its recorded bitmask is never read. Chapter 5.4 walks through what that looks like at a call site.

> Note: the order of checks in the source is Java stub, then interface, then external stub without a bitmask, then `forcedToUseRuntimeStability`, then external stub. Phases 10 and 11 below actually execute *before* this runtime stability logic.

#### Phase 10: Java Type Handling

```kotlin
if (declaration.origin == IrDeclarationOrigin.IR_EXTERNAL_JAVA_DECLARATION_STUB) {
    return Stability.Unstable
}
```

Java types default to unstable because:
1. Java allows unrestricted mutability
2. There is no equivalent of Kotlin's `val` guarantee
3. The Java standard library carries no stability annotations

#### Phase 11: General Interface Handling

```kotlin
if (declaration.isInterface) {
    // `Stability.Unknown` is always used for interfaces because stability bitmasks
    // aren't populated for them.
    return Stability.Unknown(declaration)
}
```

An interface has no implementation to inspect, and `ClassStabilityTransformer` skips interfaces, so there is no `$stable` field to fall back on either. That is why the result is `Unknown` rather than `Runtime`.

#### Phase 12: Field by Field Analysis

For concrete classes in the current module:

```kotlin
var stability = if (declaration.modality == Modality.FINAL) {
    Stability.Stable
} else {
    Stability.Unknown(declaration)
}

for (member in declaration.declarations) {
    when (member) {
        is IrProperty -> {
            member.backingField?.let {
                if (member.isVar && !member.isDelegated) return Stability.Unstable
                stability += stabilityOf(it.type, substitutions, analyzing, analysisEntryFile)
            }
        }

        is IrField -> {
            stability += stabilityOf(member.type, substitutions, analyzing, analysisEntryFile)
        }
    }
}

declaration.superClass?.let {
    val superClassStability = stabilityOf(it, substitutions, analyzing, analysisEntryFile)
    if (superClassStability !is Stability.Unknown) {
        stability += superClassStability
    }
}

return stability
```

**Key Points:**

1. The seed is `Stable` only for `final` classes. A non final `open` or `abstract` class seeds as `Unknown(declaration)`, since an unknown subclass could add unstable state
2. Any non delegated `var` property immediately returns `Unstable`
3. Combine the stability of every backing field type
4. Include superclass stability, but only when it is **not** `Unknown`, since an `Unknown` superclass result is dropped rather than propagated
5. Use the `+` operator for combination (see 2.6)

### 3.4 Expression Level Analysis

Beyond type stability, the compiler analyzes expression stability:

```kotlin
fun stabilityOf(expr: IrExpression, fileContainingDependent: IrFile?): Stability {
    // look at type first. if type is stable, whole expression is
    val stability = stabilityOf(expr.type, fileContainingDependent)
    if (stability.knownStable()) return stability

    return when (expr) {
        is IrConst -> Stability.Stable
        is IrCall -> stabilityOf(expr, stability, fileContainingDependent)
        is IrGetValue -> /* analyze variable */
        is IrLocalDelegatedPropertyReference -> Stability.Stable
        is IrComposite -> /* analyze all statements */
        else -> stability
    }
}
```

The type is checked first, and a stable type ends it there. Everything below only runs when the type alone was not enough.

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
    "kotlin.collections.listOfNotNull" to 0b1,
    "kotlin.collections.mapOf" to 0b11,
    "kotlin.collections.emptyMap" to 0,
    "kotlin.collections.setOf" to 0b1,
    "kotlin.collections.emptySet" to 0,
    "kotlin.to" to 0b11,
    // Kotlinx immutable
    "kotlinx.collections.immutable.immutableListOf" to 0b1,
    "kotlinx.collections.immutable.immutableSetOf" to 0b1,
    "kotlinx.collections.immutable.immutableMapOf" to 0b11,
    "kotlinx.collections.immutable.persistentListOf" to 0b1,
    "kotlinx.collections.immutable.persistentSetOf" to 0b1,
    "kotlinx.collections.immutable.persistentMapOf" to 0b11,
)
```

#### Variable Reference Expressions

A reference to a local `val` inherits its initializer's stability. A `var` cannot, since the value may have been reassigned since:

```kotlin
is IrGetValue -> {
    val owner = expr.symbol.owner
    if (owner is IrVariable && !owner.isVar) {
        owner.initializer?.let { stabilityOf(it, fileContainingDependent) } ?: stability
    } else {
        stability
    }
}
```

This is where expression analysis earns its place, because the type on its own would say nothing useful:

```kotlin
val items = listOf("a", "b")  // listOf is in stableFunctions → Stable
val alias = items             // IrGetValue over a val → reads the initializer → Stable

var mutable = listOf("a", "b")
val alias2 = mutable          // IrGetValue over a var → falls back to List → Unknown
```

The check is limited to `IrVariable`, so it applies to local variables only. A property read goes through a getter and is handled by the `IrCall` branch instead.

## Chapter 4: Implementation Mechanisms

### 4.1 Bitmask Encoding

Generic types use bitmasks to encode type parameter dependencies.

#### Encoding Scheme

Each type parameter is represented by a single bit:
- Bit N = 1: Type parameter N affects stability
- Bit N = 0: Type parameter N does not affect stability

The mask is an `Int`, so only the first 32 type parameters are represented. `applyTypeParameterMask` skips anything at index 32 or higher.

**Examples**, using the masks `KnownStableConstructs` records for the standard library types:

```kotlin
Pair::class.qualifiedName!! to 0b11
// Bit 0: A affects stability
// Bit 1: B affects stability

Triple::class.qualifiedName!! to 0b111
// Bit 0: A affects stability
// Bit 1: B affects stability
// Bit 2: C affects stability

Result::class.qualifiedName!! to 0b1
// Bit 0: T affects stability

Locale::class.qualifiedName!! to 0
// No type parameters, and stable unconditionally
```

A class the compiler infers itself carries the same encoding in its `@StabilityInferred(parameters = ...)` annotation. Past 32 the encoding simply saturates: a class with 33 type parameters compiles to `@StabilityInferred(parameters = -1)`, every bit set, because `0b1 shl 32` wraps back to bit 0.

#### Special Bit: Known Stable

`ClassStabilityTransformer` sets one extra bit, at index `typeParameters.size`, when the class turned out to be stable on its own:

```kotlin
if (stability.knownStable() && symbols.size < 32) {
    parameterMask = parameterMask or (0b1 shl symbols.size)
}
```

A class is either known stable or it is not, so this bit never coexists with parameter bits. These are the values the compiler actually emits, taken from its own golden tests:

```kotlin
class EmptyClass
// @StabilityInferred(parameters = 1)
// no type parameters, known stable, so bit 0 is the known stable bit

class SingleParamProp<T>(val p1: T)
// @StabilityInferred(parameters = 1)
// bit 0 means T affects stability

class SingleParamNonProp<T>(p1: T) { val p2 = p1.hashCode() }
// @StabilityInferred(parameters = 2)
// T is never stored, so the class is known stable and bit 1 is set

class DoubleParamSingleProp<T, V>(val p1: T, p2: V) { val p3 = p2.hashCode() }
// @StabilityInferred(parameters = 1)
// only T is stored, so only bit 0 is set

class X<T>(val p1: List<T>)
// @StabilityInferred(parameters = 0)
// List is an interface, so the class is unstable and no bit is set
```

For a class with no type parameters the rule collapses to a single bit: `parameters = 1` means stable, `parameters = 0` means not.

#### Bitmask Application

This is implemented by the `Stability.applyTypeParameterMask` extension function. Note that `mask` is nullable. A `null` mask means "consider every type parameter", which is what the incremental compilation `Runtime` path passes, while a concrete mask selects parameters one bit at a time.

```kotlin
private fun Stability.applyTypeParameterMask(
    mask: Int?,
    typeParameters: List<IrTypeParameter>,
    substitutions: Map<IrTypeParameterSymbol, IrTypeArgument>,
    currentlyAnalyzing: Set<SymbolForAnalysis>,
    analysisEntryFile: IrFile?,
): Stability {
    return when {
        mask == 0 || typeParameters.isEmpty() -> this
        else -> this + Stability.Combined(
            typeParameters.mapIndexedNotNull { index, irTypeParameter ->
                if (index >= 32) return@mapIndexedNotNull null
                if (mask == null || mask and (0b1 shl index) != 0) {
                    val sub = substitutions[irTypeParameter.symbol]
                    if (sub != null)
                        stabilityOf(sub, substitutions, currentlyAnalyzing, analysisEntryFile)
                    else
                        Stability.Parameter(irTypeParameter)
                } else null
            }
        )
    }
}
```

**Process:**
1. For each type parameter at index I (capped at 32)
2. If `mask` is `null`, or bit I is set in `mask`, include that parameter
3. When included, add the stability of the substituted type argument (or `Parameter` if not yet substituted)
4. Otherwise, ignore that parameter

### 4.2 Runtime Field Generation

#### JVM Platform

For JVM targets, `makeStabilityField()` builds a `$stable` property whose backing field is static, final, and annotated with `@JvmField`. The field sits directly on the class, not inside a companion object:

```kotlin
// Source
class Stable(val bar: Int)
class Unstable(var bar: Int)

// Transformed IR, as printed by the compiler's own golden tests
@StabilityInferred(parameters = 1)
class Stable(val bar: Int) {
  val %stable: Int = 0
}
@StabilityInferred(parameters = 0)
class Unstable(var bar: Int) {
  val %stable: Int = 8
}
```

The `@JvmField` annotation tells `JvmPropertiesLowering` to skip the getter and rewrite reads as direct field accesses, so from Java the field is plain `Stable.$stable`.

**Stability Values:**

```kotlin
enum class StabilityBits(val bits: Int) {
    UNSTABLE(0b100),
    STABLE(0b000);

    fun bitsForSlot(slot: Int): Int = bits shl (1 + slot * 3)
}
```

`bitsForSlot` positions the stability bits for a given parameter slot. The `$stable` field stored on a class uses slot `0`, so `UNSTABLE` becomes `0b100 shl 1` = `0b1000` = `8`. That is where the `8` above comes from.

#### Non-JVM Platforms

For Native and JS targets, `buildStabilityPropNonJvm()` puts everything at the package level instead, because there is no static field slot to hang it on. Three declarations are generated, all named from the class FQN with dots replaced by underscores:

1. A private backing field named `<mangled fqName>$stable`
2. A property named `<mangled fqName>$stableprop` that owns the field
3. A separate getter **function** named `<mangled fqName>$stableprop_getter`, registered as metadata visible

```kotlin
// Generated for Native and JS, for a class com.example.Box
private val `com_example_Box$stable`: Int = /* computed */

// Registered via metadataDeclarationRegistrar.registerFunctionAsMetadataVisible(...)
@Deprecated(
    level = DeprecationLevel.HIDDEN,
    message = "Synthetic declaration generated by the Compose compiler. Please do not use."
)
// @HiddenFromObjC is added only on Native targets
fun `com_example_Box$stableprop_getter`(): Int = `com_example_Box$stable`
```

**Rationale:**

A separate getter function is used instead of a plain field getter because `registerFunctionAsMetadataVisible` does not work for a field getter, and there is no API to register properties as metadata visible. Making the getter metadata visible is what lets another module read the stability value at all.

Reading the value back goes through `getRuntimeStabilityValue()`, which looks the getter up by name in the dependency's metadata. When the getter is missing, the dependency was built with an older plugin, and the raw field cannot be trusted: on Native there is no guarantee the package initializer has run, so the field may still hold uninitialized data. The one exception is a dependency whose compiler version string starts with `1.9`, where the field was emitted as a constant and can be read directly. Everything else is treated as `Unstable`, and `ClassStabilityTransformer` collects those classes and reports a `COMPOSE_CONFIGURATION_WARNING` listing them, advising an upgrade to avoid extra recompositions.

### 4.3 Annotation Processing

#### @StabilityInferred Annotation

```kotlin
private fun IrAnnotationContainer.stabilityParamBitmask(): Int? =
    annotations.findAnnotation(ComposeFqNames.StabilityInferred)?.getConstArgument("parameters")
```

The annotation carries a single integer parameter, declared in the Compose runtime as `StabilityInferred(val parameters: Int)`, and the lookup reads it by name.

#### Annotation Generation

```kotlin
val annotation = IrAnnotationImpl(
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

if (cls.hasFirDeclaration()) {
    context.metadataDeclarationRegistrar.addMetadataVisibleAnnotationsToElement(
        cls,
        annotation,
    )
} else {
    cls.annotations += annotation
}
```

The annotation is created as an `IrAnnotationImpl`, which replaced the `IrConstructorCallImpl` earlier compiler versions used. A class that has a FIR declaration behind it gets the annotation attached as metadata visible, so it survives into the module's metadata and can be read from another module. Classes without one, such as declarations synthesized later in the pipeline, get the annotation added to the IR node directly.

The field itself is only emitted for declarations another module could see:

```kotlin
if (cls.visibility.isPublicAPI || cls.visibility == DescriptorVisibilities.INTERNAL) {
    cls.addStabilityMarkerField(stableExpr)
}
```

`visitClass` skips the whole transform for enums, enum entries, interfaces, annotation classes, anonymous objects, `expect` declarations, inner classes, file classes, companions, inline class types, and anything neither public nor internal.

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
4. **Short circuit on Unstable**: return immediately if any `Certain(false)` is found
5. **Collect Runtime and Parameter**: Preserve these for runtime checks

**Result Types:**
- `Stability.Unstable` if any component is unstable
- `Stability.Combined([...])` with deduplicated elements otherwise

## Chapter 5: Case Studies

### 5.1 Primitive and Standard Library Types

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
3. Function equality is defined by reference

### 5.2 User Defined Classes

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

A generic field whose type is itself generic is where the inference most often surprises people:

```kotlin
class Container<T>(val items: List<T>)

// Analysis:
// 1. Field analysis:
//    - items: List<T>
//      - List is an interface, so it never reaches member analysis
//      - Result: Unknown(List)
// 2. Stable + Unknown(List) = Unknown(List)
// 3. Unknown is not expressible, so there is no runtime check to emit
// Result: @StabilityInferred(parameters = 0), $stable = 8 (unstable)
```

`List` is not in `KnownStableConstructs`, and it is an interface, so the type argument is never even examined. `Container<String>` is unstable for the same reason `Container<Counter>` is. The compiler's own golden test records exactly this for `class X<T>(val p1: List<T>)`.

Swapping in a type the compiler does know changes the outcome:

```kotlin
import kotlinx.collections.immutable.ImmutableList

class Container<T>(val items: ImmutableList<T>)

// Analysis:
// 1. Field analysis:
//    - items: ImmutableList<T>
//      - "kotlinx.collections.immutable.ImmutableList" to 0b1 in KnownStableConstructs
//      - bit 0 is set, so the type argument is checked
//      - T has no substitution here → Parameter(T)
// 2. Result: Combined([Parameter(T)])

// Instantiation:
val container: Container<String>
// Substitute T → String, stabilityOf(String) = Stable
// Result: Stable
```

### 5.4 External Dependencies

#### External Class with Annotation

```kotlin
// Library module (compiled separately)
@StabilityInferred(parameters = 0b1)
class LibraryBox<T>(val value: T) {
    val $stable: Int = 0 // synthetic static final field on the class
}

// Your module
@Composable
fun UseLibraryBox(box: LibraryBox<StableClass>) { /* ... */ }

// Analysis:
// 1. LibraryBox is public and lives in another file, and the target is JVM
// 2. forcedToUseRuntimeStability is true -> Stability.Runtime(LibraryBox)
// 3. mask is null, so every type argument is folded in
// 4. stabilityOf(StableClass) -> Runtime(StableClass)
// Result, after normalize(): Combined([Runtime(LibraryBox), Runtime(StableClass)])
```

At the call site that becomes a single expression, which the compiler's golden tests show verbatim:

```kotlin
UseLibraryBox(LibraryBox(StableClass()), %composer, LibraryBox.%stable or StableClass.%stable)
```

There is a detail here worth stopping on. On JVM the recorded `@StabilityInferred` bitmask is not what selected the type argument. `forcedToUseRuntimeStability` is checked *before* the external stub branch, and it passes `mask = null`, so **every** type argument is folded in regardless of what the bitmask says. The compiler's own golden output makes this visible: `SingleParamNonProp` is compiled with `@StabilityInferred(parameters = 2)`, meaning no type parameter affects its stability, and the call site still emits `SingleParamNonProp.%stable or StableClass.%stable`.

The recorded bitmask is consulted on the branch below it, which is reached when the target is not JVM, or when the class is neither public nor internal. The extra type arguments on JVM cost a few `or` operations and can only make a result more conservative, never less, which is the trade the compiler takes for incremental safety.

#### External Class Without Annotation

```kotlin
// Third party library (no Compose compiler)
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
// 1. BaseViewModel is abstract (not an interface), so it reaches member analysis
// 2. modality != FINAL -> seed stability is Unknown(BaseViewModel)
// 3. `abstract val state` has no backing field, so no member contributes
// Result: Stability.Unknown(BaseViewModel)
```

A non final class seeds as `Unknown`. Concrete `val` properties with backing fields are still combined in, but the `Unknown` seed keeps the overall result uncertain unless something resolves it.

This trace assumes `BaseViewModel` and `Screen` are in the same file. On JVM, a public `BaseViewModel` in a different file never reaches member analysis at all: Phase 9 intercepts it and returns `Runtime` instead.

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

// Analysis of Base:
// 1. Base is open, so modality != FINAL → seed is Unknown(Base)
// 2. Field id: Int → Stable
// 3. Unknown(Base) + Stable = Unknown(Base)
// Result: Stability.Unknown(Base)

// Analysis of Derived:
// 1. Derived is final → seed is Stable
// 2. Field name: String → Stable
// 3. Check superclass: Base → Unknown, so it is dropped
// Result: Stability.Certain(stable = true)
```

An open superclass resolving to `Unknown` is dropped rather than combined in. Without that rule every subclass of an open class would inherit the uncertainty of a class that is perfectly stable on its own.

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

#### Compiler Level Differences: @Stable vs @Immutable

Both annotations mark types as stable for recomposition skipping, and there is one difference in how the compiler treats them.

**Stability Inference Treatment**

Both annotations are processed identically through `hasStableMarker()`:

```kotlin
fun IrAnnotationContainer.hasStableMarker(): Boolean =
    annotations.any { it.isStableMarker() }
```

Both result in:
- The same stability inference, `Stability.Certain(stable = true)`
- The same `$stable` field, emitted as the constant `STABLE`
- The same recomposition skipping behavior

Neither gets a `@StabilityInferred` annotation. `ClassStabilityTransformer.visitClass` returns as soon as it sees a marker:

```kotlin
if (declaration.hasStableMarker()) {
    metrics.recordClass(declaration, marked = true, stability = Stability.Stable)
    cls.addStabilityMarkerField(irConst(STABLE))
    return cls
}
```

There is nothing to infer, so there is no bitmask to record. The compiler's golden output shows exactly that, a `@Stable` class carrying `val %stable: Int = 0` and no `@StabilityInferred` line.

**Static Expression Optimization (Key Difference)**

`@Immutable` has special treatment for static expression detection.

```kotlin
private fun IrConstructorCall.isStatic(fileContainingDependent: IrFile?): Boolean {
    // special case constructors of inline classes as static if their underlying
    // value is static.
    if (type.isInlineClassType()) {
        return stabilityInferencer.stabilityOf(
            type.unboxInlineClass(), fileContainingDependent
        ).knownStable() &&
                arguments[0]?.isStatic(fileContainingDependent) == true
    }

    // If a type is immutable, then calls to its constructor are static if all of
    // the provided arguments are static.
    if (symbol.owner.parentAsClass.hasAnnotation(ComposeFqNames.Immutable)) {
        return areAllArgumentsStatic(fileContainingDependent)
    }
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
fun Chart(
    // @static in the compiler report: the constructor call is evaluated once
    origin: ImmutablePoint = ImmutablePoint(0, 0),
    // @dynamic: the call is re-evaluated whenever the default is taken
    anchor: StablePoint = StablePoint(0, 0),
) { /* ... */ }
```

**What staticness buys:**

1. **Default parameters**: a `@static` default is hoisted out of the defaults group, so the composable does not re-evaluate it on every composition. The `-composables.txt` report tags every default as `@static` or `@dynamic`, which makes the difference easy to see.
2. **Intrinsic remember**: a static argument is known not to change, so the comparison it would need can be folded away instead of costing a slot.
3. **Lambda memoization**: a lambda that only captures static values has nothing that can change, so it does not need a `remember` wrapper keyed on its captures.

**Summary Table:**

| Aspect | @Stable | @Immutable |
|--------|---------|------------|
| Stability inference | Stable | Stable |
| Recomposition skipping | Enabled | Enabled |
| Constructor staticness | No | Yes (with static args) |
| Lambda capture optimization | Standard | Enhanced |
| Semantic contract | Allows private mutability | Truly immutable |

The compiler treats `@Immutable` as the stronger guarantee, and that extra guarantee buys compile time evaluation of constructor calls, which then feeds static expression detection and lambda memoization.

#### @StableMarker Meta Annotation

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

Create a [stability configuration file](https://developer.android.com/develop/ui/compose/performance/stability/fix#configuration-file), `stability_config.conf`. `StabilityConfigParser` reads it line by line, skipping blank lines and lines that start with `//`. A comment after a pattern is a parse error, not a comment:

```
// Single class
com.example.ExternalType

// Package wildcard
com.example.models.**

// Single segment wildcard
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
- `*`: matches a single package segment
- `**`: matches multiple package segments
- `<*>`: the generic parameter affects stability
- `<_>`: the generic parameter is ignored for stability

A pattern matches more than the class named. `FqNameMatcherCollection.matches` checks the class's own FQN *and* every supertype FQN, so listing a base class or an interface makes every type that extends it stable too:

```kotlin
fun matches(name: FqName?, superTypes: List<IrType>): Boolean {
    // ...
    return matcherTree.findFirstPositiveMatcher(name) != null ||
            superTypeNames.any { matcherTree.findFirstPositiveMatcher(it) != null }
}
```

That is convenient for a sealed hierarchy and a trap for a broad interface. Patterns are stored in a tree keyed by package segment, so a large configuration file does not slow compilation down much.

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

Pass the configuration file to the `composeCompiler` block of the [Compose compiler Gradle plugin](https://developer.android.com/develop/ui/compose/compiler):

```kotlin
composeCompiler {
    stabilityConfigurationFiles.addAll(
        rootProject.layout.projectDirectory.file("stability_config.conf"),
    )
}
```

The older singular `stabilityConfigurationFile` property still exists, but it is deprecated at `DeprecationLevel.ERROR` and is scheduled for removal in Kotlin 2.5.0, so new builds should use `stabilityConfigurationFiles`.

#### Feature Flags

The same block carries the compiler's feature flags. Each flag has a default in the plugin, and the Gradle option only records a deviation from it:

```kotlin
enum class FeatureFlag(val featureName: String, val default: Boolean) {
    StrongSkipping("StrongSkipping", default = true),
    IntrinsicRemember("IntrinsicRemember", default = true),
    OptimizeNonSkippingGroups("OptimizeNonSkippingGroups", default = true),
    PausableComposition("PausableComposition", default = true),
    ;
}
```

To turn one off, add its disabled form:

```kotlin
composeCompiler {
    featureFlags = setOf(ComposeFeatureFlag.StrongSkipping.disabled())
}
```

The older single purpose options that used to control these, such as `enableStrongSkippingMode`, `enableIntrinsicRemember`, and `enableNonSkippingGroupOptimization`, are deprecated at `DeprecationLevel.ERROR` alongside `stabilityConfigurationFile`.

### 6.3 Compiler Reports

#### Enabling Reports

```kotlin
composeCompiler {
    reportsDestination = layout.buildDirectory.dir("compose_reports")
    metricsDestination = layout.buildDirectory.dir("compose_metrics")
}
```

#### Generated Files

`reportsDestination` produces `<module>-classes.txt`, `<module>-composables.txt`, and `<module>-composables.csv`, plus `<module>-composables.log` when the compiler logged anything. `metricsDestination` produces `<module>-module.json`. In the prefix, dots in the module name become underscores and angle brackets are removed.

**`<module>-classes.txt`**: class stability analysis, one entry per class, each field marked `stable`, `unstable`, or `runtime`. Inferred classes also get a `<runtime stability>` line showing the expression used to resolve stability at runtime:

```
stable class com.example.User {
  stable val id: Int
  stable val name: String
  <runtime stability> = Stable
}

unstable class com.example.Counter {
  unstable var count: Int
  <runtime stability> = Unstable
}

runtime class com.example.Box {
  runtime val value: T
  <runtime stability> = Parameter(T)
}
```

A class marked with `@Stable` or `@Immutable` is printed without the `<runtime stability>` line, since nothing was inferred.

**`<module>-composables.txt`**: composable function analysis, printed in pseudo Kotlin. Each parameter sits on its own line with no separator, and each default expression is tagged `@static` or `@dynamic`:

```
restartable skippable fun com.example.Image(
  unstable bitmap: ImageBitmap
  stable contentDescription: String?
  stable modifier: Modifier? = @static Companion
  stable alignment: Alignment? = @dynamic Companion.Center
)
```

`restartable` marks a function that can serve as a recomposition scope, and `skippable` marks one that can be skipped when its arguments compare equal. The two are related but separate. A function has to be restartable to be skippable, and `shouldBeRestartable()` already rules out inline functions, functions with a non Unit return type, `@NonRestartableComposable`, and functions with explicit groups. Among what is left, a `restartable` entry with no `skippable` usually means `@NonSkippableComposable`, since strong skipping makes the rest skippable by default.

**`<module>-composables.csv`**: the same per function data in a form you can drop into a spreadsheet.

**`<module>-module.json`**: module wide counters, useful mostly as a number to track across builds:

```json
{
  "skippableComposables": 53,
  "restartableComposables": 60,
  "readonlyComposables": 1,
  "totalComposables": 100,
  "restartGroups": 60,
  "totalGroups": 139,
  "staticArguments": 25,
  "certainArguments": 138,
  "knownStableArguments": 377,
  "knownUnstableArguments": 25,
  "unknownStableArguments": 24,
  "totalArguments": 426,
  "markedStableClasses": 8,
  "inferredStableClasses": 28,
  "inferredUnstableClasses": 0,
  "inferredUncertainClasses": 0,
  "effectivelyStableClasses": 36,
  "totalClasses": 36,
  "memoizedLambdas": 40,
  "singletonLambdas": 6,
  "singletonComposableLambdas": 4,
  "composableLambdas": 49,
  "totalLambdas": 81,
  "featureFlags": {
    "StrongSkipping": true,
    "IntrinsicRemember": true,
    "OptimizeNonSkippingGroups": true,
    "PausableComposition": true
  }
}
```

The ratio between `certainArguments` and `totalArguments` tells you how much stability metadata is actually reaching composable calls, which is usually the most actionable number in the file.

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
// MutableList is an interface, so it resolves to Unknown,
// which makes the enclosing class uncertain
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
// To make it stable, add to stability_config.conf:
// kotlin.collections.List
```

Declaring `kotlin.collections.List` stable is a promise you make on behalf of every list in the module, including the `MutableList` instances that are also `List`. It is only safe if you never mutate a list after handing it to a composable.

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
// Third party library without Compose support
class LibraryClass(val data: String)

@Composable
fun Display(obj: LibraryClass) {
    // LibraryClass marked unstable
}
```

**Solution:**

Add to `stability_config.conf`:

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
data class SymbolForAnalysis(
    val symbol: IrClassifierSymbol,
    val typeParameters: List<IrTypeArgument?>,
    // The file containing the element that initiated this stabilityOf request tree.
    // Two identical symbols analyzed from different entry files are distinct keys,
    // which is what keeps the per file caching and runtime stability behavior correct.
    val analysisEntryFile: IrFile?,
)

// In stabilityOf(declaration: IrClass)
val fullSymbol = SymbolForAnalysis(symbol, typeArguments, analysisEntryFile)

if (currentlyAnalyzing.contains(fullSymbol))
    return Stability.Unstable
```

The `currentlyAnalyzing` set tracks the analysis stack to detect cycles. The `analysisEntryFile` is part of the key because the same type can resolve to different stability depending on which file initiated the analysis (see Phase 9).

#### Example: Self Referential Type

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

A delegated `var` escapes the immediate `Unstable` return, and the backing field it does have holds the delegate, not the value. So the class inherits the delegate's stability:

```kotlin
@Stable
class StableDelegate { /* getValue, setValue */ }

class UnstableDelegate {
    var value: Int = 0
    /* getValue, setValue */
}

class StableDelegateProp {
    var p1 by StableDelegate()
}
// @StabilityInferred(parameters = 1), $stable = 0

class UnstableDelegateProp {
    var p1 by UnstableDelegate()
}
// @StabilityInferred(parameters = 0), $stable = 8
```

This is what makes `by mutableStateOf(...)` work. `MutableState` is `@Stable`, so a `var` delegated to it leaves the enclosing class stable, and the runtime still learns about writes because the state object notifies composition itself.

#### Value Classes with Markers

Both value class branches start with the same check:

```kotlin
if (inlineClassDeclaration.hasStableMarker()) {
    Stability.Stable
}
```

A marker therefore overrides whatever the underlying types say:

```kotlin
@JvmInline
@Stable
value class Wrapper(val list: MutableList<Int>)
// Underlying type (MutableList) is unstable
// But @Stable annotation overrides
// Result: Stable (developer responsibility)
```

Note that `ClassStabilityTransformer` skips inline class types entirely, so a value class never gets a `$stable` field of its own. Its stability is resolved wherever it is used, by unwrapping it again.

## Chapter 8: Compiler Analysis System

Stability inference is one half of what the Compose plugin does. The other half is a set of checkers that validate how composable functions are declared and called. Both halves used to be split across two frontends, and the older one is now gone: there is no `k1` package in the plugin anymore, and with it went `BindingTrace`, `WritableSlice`, and `TypeResolutionInterceptorExtension`. Everything the frontend does today runs on FIR, and everything the backend does runs on IR.

### 8.1 Analysis Infrastructure

The plugin needs to write down what it learned in one phase and read it back in a later one. It uses a different mechanism on each side of the compiler.

#### IR Attributes: Backend Data Flow

`lower/ComposePluginAttributes.kt`

In the IR phase, metadata is attached directly to IR nodes through delegated attribute and flag properties:

```kotlin
internal var IrExpression.isStaticExpression: Boolean by irFlag(copyByDefault = true)
internal var IrExpression.isStaticFunctionExpression: Boolean by irFlag(copyByDefault = true)
internal var IrElement.isComposableSingleton: Boolean by irFlag(copyByDefault = true)
internal var IrElement.isComposableSingletonClass: Boolean by irFlag(copyByDefault = true)
internal var IrElement.durableFunctionKey: KeyInfo? by irAttribute(copyByDefault = true)
internal var IrElement.hasTransformedLambda: Boolean by irFlag(copyByDefault = true)
internal var IrFunction.functionMetrics: FunctionMetrics? by irAttribute(copyByDefault = true)
```

What each one carries:

- **isStaticExpression**: marks expressions whose value can be computed once instead of on every composition
- **durableFunctionKey**: stores the stable identity of a function, which is what makes hot reload possible
- **isComposableSingleton** and **isComposableSingletonClass**: mark composable lambdas that were hoisted into singletons
- **functionMetrics**: the per function record that ends up in the compiler reports

They read and write as plain properties on the node, so an analysis pass does `expr.isStaticExpression = true` and a later lowering does `if (expr.isStaticExpression) { ... }`. No lookup table sits in between. `copyByDefault = true` means the attribute survives when an IR node is copied, which matters because several lowerings rebuild functions wholesale.

#### FIR Session Components: Frontend Data Flow

The frontend has no trace to record into. When a checker needs to remember something between calls, it stores it in a session component instead:

```kotlin
internal class ComposableTargetSessionStorage(session: FirSession) : FirExtensionSessionComponent(session) {
    // parent links, lambda to expression links, and a cache of LazyScheme per FirElement
}

private val FirSession.composableTargetSessionStorage by FirSession.sessionComponentAccessor<ComposableTargetSessionStorage>()
```

The component is registered with the rest of the plugin's FIR extensions and lives for the duration of the session. Applier inference is the only part of the frontend that needs this; the call and declaration checkers are stateless.

### 8.2 Composable Call Validation

`k2/ComposableCallChecker.kt`

This file holds two checkers, one for calls and one for property reads, both wired into the FIR checker infrastructure:

```kotlin
object ComposablePropertyAccessExpressionChecker : FirPropertyAccessExpressionChecker(MppCheckerKind.Common)
object ComposableFunctionCallChecker : FirFunctionCallChecker(MppCheckerKind.Common)
```

The function call checker dispatches on what the callee turned out to be:

```kotlin
context(context: CheckerContext, reporter: DiagnosticReporter)
override fun check(expression: FirFunctionCall) {
    val calleeFunction = expression.calleeReference.toResolvedFunctionSymbol()
        ?: return

    // K2 propagates annotation from the fun interface method to the constructor.
    // https://youtrack.jetbrains.com/issue/KT-47708.
    if (calleeFunction.origin == FirDeclarationOrigin.SamConstructor) return

    if (calleeFunction.isComposable(context.session)) {
        checkComposableCall(expression, calleeFunction, context, reporter)
    } else if (calleeFunction.callableId.isInvoke()) {
        checkInvoke(expression, context, reporter)
    }
}
```

#### Scope Walking Algorithm

The question the checker has to answer is whether the call sits inside something composable. It answers it by walking outward from the call site through `CheckerContext.containingElements`, which is the stack of FIR elements the checker is currently nested in. There is no PSI involved: the compiler stopped generating PSI outside the IDE, and the last PSI condition was removed from this checker along with it.

```kotlin
private inline fun CheckerContext.visitCurrentScope(
    visitInlineLambdaParameter: (FirValueParameter) -> Unit,
    visitAnonymousFunction: (FirAnonymousFunction) -> Unit = {},
    visitFunction: (FirFunction) -> Unit = {},
    visitTryExpression: (FirTryExpression, FirElement) -> Unit = { _, _ -> },
    visitFunctionCall: (FirFunctionCall) -> Unit = {},
) {
    for ((elementIndex, element) in containingElements.withIndex().reversed()) {
        when (element) {
            is FirAnonymousFunction -> {
                if (element.inlineStatus == InlineStatus.Inline) {
                    findValueParameterForLambdaAtIndex(elementIndex)?.let(visitInlineLambdaParameter)
                }
                visitAnonymousFunction(element)
                if (element.inlineStatus != InlineStatus.Inline) return
            }
            is FirFunction -> {
                visitFunction(element)
                return
            }
            is FirTryExpression -> { /* ... */ }
            is FirFunctionCall -> visitFunctionCall(element)
            // ...
            is FirDeclaration -> return
        }
    }
}
```

Two details make this work. The walk goes `reversed()`, so it visits the innermost element first and moves outward. And the function is `inline`, so a bare `return` inside one of the callbacks returns from the enclosing checker function, not just from the loop. That is how the checker says "this call is fine, stop looking" without threading a result value back out.

An `Inline` lambda is transparent: the walk passes straight through it, because a composable call inside an inline lambda executes in the caller's scope. `NoInline` and `CrossInline` lambdas stop the walk, because their body may run at any time.

Everything else falls into the final `is FirDeclaration -> return` branch, which ends the walk without finding a composable scope. A few element kinds are listed above it precisely so they do *not* end the walk: `FirProperty` and `FirValueParameter`, because the call may have come from an initializer or a default value; `FirAnonymousObject` and `FirAnonymousInitializer`; and a `FirField` whose origin is `Synthetic.DelegateField`, which FIR creates for constructor delegation.

#### Validation Order

`checkComposableCall` runs the walk with five callbacks, and each one handles a rule:

1. **Zero argument `key`**: a call to `androidx.compose.runtime.key` with a single argument has a group key but no body, which is always a mistake. Reports `KEY_CALL_WITH_NO_ARGUMENTS`.
2. **`@DisallowComposableCalls` lambdas**: if the enclosing inline lambda's parameter type carries the annotation, reports `CAPTURED_COMPOSABLE_INVOCATION`.
3. **Composable scopes**: a lambda whose function type kind is `ComposableFunction`, or a function annotated `@Composable`, ends the check successfully.
4. **try blocks**: reports `ILLEGAL_TRY_CATCH_AROUND_COMPOSABLE`.
5. **runCatching**: reports `ILLEGAL_RUN_CATCHING_AROUND_COMPOSABLE`.
6. **Fall through**: if the walk finished without finding a composable scope, reports `COMPOSABLE_INVOCATION`.

The try check is narrower than its name suggests. Composable calls are legal inside `catch` and `finally`, and only the `try` block itself is rejected:

```kotlin
visitTryExpression = { tryExpression, container ->
    // Only report an error if the composable call happens inside of the `try`
    // block. Composable calls are allowed inside of `catch` and `finally` blocks.
    if (container !is FirCatch && tryExpression.finallyBlock != container) {
        reporter.reportOn(
            tryExpression.source,
            ComposeErrors.ILLEGAL_TRY_CATCH_AROUND_COMPOSABLE,
            context
        )
    }
}
```

`container` is the child of the try expression through which the walk arrived, which is what lets the checker tell the three blocks apart. The reason for the rule is that composition state is written as the composable executes. An exception thrown mid execution leaves the slot table partly updated, and the `catch` block would then be running against a composition that no longer matches the code that produced it. `runCatching` is rejected for the same reason, since it is a `try` in disguise.

#### Readonly Composables

`@ReadOnlyComposable` promises that the body only performs read operations on the composer, which lets the compiler emit no group around it at all. Calling a normal composable from one would break that promise, so `checkComposableFunction` carries the call site source down and reports it:

```kotlin
if (function.hasComposableAnnotation(session)) {
    if (function.hasReadOnlyComposableAnnotation(session) && nonReadOnlyCallInsideFunction != null) {
        reporter.reportOn(nonReadOnlyCallInsideFunction, NONREADONLY_CALL_IN_READONLY_COMPOSABLE, context)
    }
    return ComposableCheckForScopeStatus.STOP
}
```

The source is only non null when the callee is itself not readonly, so a readonly composable calling another readonly composable passes.

#### Property Getters and Delegates

A composable property is a getter, not a field, and a delegated composable property has extra limits:

```kotlin
if (function is FirPropertyAccessor && function.propertySymbol.hasDelegate) {
    if (function.propertySymbol.isVar) {
        reporter.reportOn(function.source, COMPOSE_INVALID_DELEGATE, context)
    } else if (function.propertySymbol is FirRegularPropertySymbol) {
        // Only local variables can be implicitly composable, for top-level or
        // class-level declarations we require an explicit annotation.
        reporter.reportOn(function.propertySymbol.source, COMPOSABLE_EXPECTED, context)
    }
    return ComposableCheckForScopeStatus.STOP
}
```

A `var` delegate needs `setValue`, and a composable `setValue` has nowhere to run, so it is rejected outright. A `val` delegate at top level or class level has to be annotated rather than inferred, because its type is part of the declaration's public shape.

#### Propagating @DisallowComposableCalls

`checkInvoke` handles the inverse problem. When an inline function's lambda parameter is invoked from inside a `@DisallowComposableCalls` lambda, that restriction has to propagate to the parameter, otherwise a composable call could sneak through the invocation:

```kotlin
val param = (expression.dispatchReceiver as? FirPropertyAccessExpression)
    ?.calleeReference
    ?.toResolvedValueParameterSymbol()
    ?: return
if (param.resolvedReturnTypeRef.hasDisallowComposableCallsAnnotation(context.session) ||
    !param.containingDeclarationSymbol.let { it is FirCallableSymbol && it.isInline }
) {
    return
}
```

If the parameter is not already annotated, the checker reports `MISSING_DISALLOW_COMPOSABLE_CALLS_ANNOTATION` naming both the parameter that needs the annotation and the one that imposed the restriction.

### 8.3 Declaration Validation

#### Composable Function Rules

`k2/ComposableFunctionChecker.kt` is a `FirFunctionChecker`, and it runs the override checks before it even asks whether the function is composable:

1. **Override consistency**: an override must match its parent on composability, otherwise `FirErrors.CONFLICTING_OVERLOADS`.
2. **Applier scheme on overrides**: when both are composable, `!override.toScheme().canOverride(declaration.symbol.toScheme())` reports `COMPOSE_APPLIER_DECLARATION_MISMATCH`.
3. **expect and actual**: a mismatch between the expect declaration and its actual reports `MISMATCHED_COMPOSABLE_IN_EXPECT_ACTUAL`.

The rest applies only to `@Composable` declarations:

4. **suspend**: `COMPOSABLE_SUSPEND_FUN`. A suspend function can resume anywhere, and composition has to stay on the composition thread with its slot table positioned where it left off.
5. **main**: `COMPOSABLE_FUN_MAIN`. There is no composer to pass in at the process entry point.
6. **Default parameter values on open and abstract functions**: these need runtime support, so they are gated on language version through `ComposeLanguageFeature`, which declares `DefaultParametersInAbstractFunctions(LanguageVersion.KOTLIN_2_1)` and `DefaultParametersInOpenFunctions(LanguageVersion.KOTLIN_2_2)`. Below the required version they report `ABSTRACT_COMPOSABLE_DEFAULT_PARAMETER_VALUE` or `OPEN_COMPOSABLE_DEFAULT_PARAMETER_VALUE`. A dependency compiled before the support existed reports the warning `DEPRECATED_OPEN_COMPOSABLE_DEFAULT_PARAMETER_VALUE`.
7. **`setValue` operator**: a composable `setValue` reports `COMPOSE_INVALID_DELEGATE`, matching the delegate rule in the call checker.

#### Property Restrictions

`k2/ComposablePropertyChecker.kt` holds two checkers. The first runs on any property whose getter or setter is annotated:

```kotlin
if (declaration.isVar) {
    reporter.reportOn(declaration.source, ComposeErrors.COMPOSABLE_VAR, context)
}
if (declaration.hasBackingField) {
    reporter.reportOn(declaration.source, ComposeErrors.COMPOSABLE_PROPERTY_BACKING_FIELD, context)
}
```

Both come down to the same thing: a composable property is a function that runs during composition, so there is nothing for a field to hold and nothing for a setter to do.

The second, `ComposablePropertyReferenceChecker`, reports `COMPOSABLE_PROPERTY_REFERENCE` for a `::property` reference to a non delegated composable property. A property reference produces a `KProperty` object whose getter would have to be invoked outside composition.

#### Composable Type Positions

`k2/ComposableAnnotationChecker.kt` is a `FirResolvedTypeRefChecker`. It catches `@Composable` written on something that is not a function type:

```kotlin
if (typeRef !is FirErrorTypeRef && !typeRef.coneType.isComposableFunction(session)) {
    reporter.reportOn(composableAnnotation.source, COMPOSABLE_INAPPLICABLE_TYPE, typeRef.coneType, context)
}
```

It makes one exception, for the synthetic array type FIR creates around a `vararg` parameter, since the annotation there belongs to the element type.

#### Diagnostic Severity

`k2/ComposeErrors.kt` declares 24 diagnostic factories, and the severity comes from which builder they use: `error0` through `error4` produce errors, `warning0` through `warning4` produce warnings. The split is worth knowing, because the applier diagnostics are the ones people most often expect to fail a build:

| Diagnostic | Severity |
|---|---|
| `COMPOSABLE_INVOCATION` | Error |
| `COMPOSABLE_EXPECTED` | Error |
| `CAPTURED_COMPOSABLE_INVOCATION` | Error |
| `NONREADONLY_CALL_IN_READONLY_COMPOSABLE` | Error |
| `ILLEGAL_TRY_CATCH_AROUND_COMPOSABLE` | Error |
| `ILLEGAL_RUN_CATCHING_AROUND_COMPOSABLE` | Error |
| `MISSING_DISALLOW_COMPOSABLE_CALLS_ANNOTATION` | Error |
| `COMPOSABLE_SUSPEND_FUN` | Error |
| `COMPOSABLE_FUN_MAIN` | Error |
| `COMPOSABLE_VAR` | Error |
| `COMPOSABLE_PROPERTY_BACKING_FIELD` | Error |
| `COMPOSABLE_PROPERTY_REFERENCE` | Error |
| `COMPOSE_INVALID_DELEGATE` | Error |
| `COMPOSABLE_INAPPLICABLE_TYPE` | Error |
| `KEY_CALL_WITH_NO_ARGUMENTS` | Error |
| `MISMATCHED_COMPOSABLE_IN_EXPECT_ACTUAL` | Error |
| `OPEN_COMPOSABLE_DEFAULT_PARAMETER_VALUE` | Error |
| `ABSTRACT_COMPOSABLE_DEFAULT_PARAMETER_VALUE` | Error |
| `COMPOSE_APPLIER_CALL_MISMATCH` | Warning |
| `COMPOSE_APPLIER_PARAMETER_MISMATCH` | Warning |
| `COMPOSE_APPLIER_DECLARATION_MISMATCH` | Warning |
| `DEPRECATED_OPEN_COMPOSABLE_DEFAULT_PARAMETER_VALUE` | Warning |
| `COMPOSE_CONFIGURATION_ERROR` | Error, no source |
| `COMPOSE_CONFIGURATION_WARNING` | Warning, no source |

`CONFLICTING_OVERLOADS` does not appear here because it is not a Compose diagnostic. The plugin reuses `FirErrors.CONFLICTING_OVERLOADS` from the Kotlin compiler itself. `COMPOSE_CONFIGURATION_WARNING` is the sourceless factory used for plugin level messages, including the non JVM stability warning from Chapter 4.

### 8.4 Applier Target System

A composable function does not produce UI directly. It emits nodes into whatever applier the composition was started with, and an Android UI node means nothing to a canvas or a terminal renderer. The applier target system tracks which applier each composable expects, and reports when two that disagree meet.

#### Scheme Structure

`inference/Scheme.kt` models a target as a sealed `Item` with two implementations, both top level rather than nested:

```kotlin
sealed class Item

class Token(val value: String) : Item()

class Open(
    val index: Int,
    val constraints: Constraints = Constraints.UNRESTRICTED,
    override val isUnspecified: Boolean = false,
) : Item()
```

A `Token` is a target that is already decided, carrying the fully qualified name of a marker annotation. An `Open` is a target still to be determined. Its `index` is what ties positions together: two `Open` items with the same non negative index have to resolve to the same token, while a negative index means the item is independent of every other. `Constraints` narrows an open target to a set of allowed tokens, which is how a function that declares more than one acceptable target is represented.

A `Scheme` is the target of a declaration plus the schemes of its composable lambda parameters and result:

```kotlin
class Scheme(
    val target: Item,
    val parameters: List<Scheme> = emptyList(),
    val result: Scheme? = null,
    val anyParameters: Boolean = false,
) {
    fun canOverride(other: Scheme): Boolean =
        alphaRename().simpleCanOverride(other.alphaRename())
}
```

`equals`, `hashCode`, and `canOverride` all compare modulo alpha renaming, so `[0, [0]]` and `[2, [2]]` are the same scheme. What matters is which positions share an index, not which numbers were used. `canOverride` is what `ComposableFunctionChecker` calls when validating an override.

The debug form is `[target, parameter, parameter: result]`. Serialization into the `@ComposableInferredTarget` annotation is separate and produces three strings:

```kotlin
data class SerializedScheme(val scheme: String, val positional: String, val indexed: String)
```

The extra two carry the constraint sets, which the main string has no room for.

#### Target Inference Algorithm

`ApplierInferencer` is generic over the node and type representation, so the same engine serves both the FIR frontend and the IR backend:

```kotlin
class ApplierInferencer<Type, Node>(
    private val typeAdapter: TypeAdapter<Type>,
    private val nodeAdapter: NodeAdapter<Type, Node>,
    private val lazySchemeStorage: LazySchemeStorage<Node>,
    private val errorReporter: ErrorReporter<Node>,
)
```

It exposes `visitVariable`, `visitCall`, and `toFinalScheme`. Callers supply four adapters: `TypeAdapter` reads a declared scheme off a type, `NodeAdapter` navigates containers and parameter positions, `LazySchemeStorage` caches partially resolved schemes, and `ErrorReporter` receives conflicts.

The algorithm is unification, the same technique type inference uses:

1. Read the declared scheme from `@ComposableTarget` and `@ComposableInferredTarget`, or start fully open when there is none.
2. Convert each scheme to `CallBindings`, a tree of `Binding` objects mirroring the scheme's shape.
3. Unify the call's bindings with the callee's, position by position.
4. When an open binding meets a token, bind it. When two open bindings meet, merge them so they resolve together.

`Bindings` keeps unified bindings in a circular list and always merges the smaller group into the larger, which keeps the work bounded as a call graph grows. Binding fails when the intersected constraints allow nothing:

```kotlin
fun unify(a: Binding, b: Binding): Boolean
```

Failure comes back as `false` and reaches the user through `ErrorReporter`. Nothing is thrown, and no substitution map is built: the whole state lives in `Bindings` and `LazyScheme`.

#### Where Targets Come From

Nothing in the compiler hardcodes a target name. A token is either the `applier` string of a `@ComposableTarget` annotation, or the fully qualified name of an annotation class that is itself annotated `@ComposableTargetMarker`. `androidx.compose.ui.UiComposable` is just what the UI library happens to call its marker; the compiler never mentions it.

```kotlin
fun FirCallableSymbol<*>.schemeItem(): Item {
    val targets = targetsFromAnnotations()
    val explicitOpen = compositionOpenTarget()
    return when {
        targets.size == 1 -> Token(targets.first())
        targets.size > 1 -> Open(explicitOpen ?: -1, constraints = Constraints.restrictedTo(targets))
        explicitOpen != null -> Open(explicitOpen)
        else -> Open(-1, isUnspecified = true)
    }
}
```

The annotations all live in `androidx.compose.runtime`: `ComposableTarget(applier)`, `ComposableOpenTarget(index)`, `ComposableInferredTarget(scheme)`, `ComposableInferredTargetConstraints(positional, indexed)`, and `ComposableTargetMarker(description)`. That `description` is what turns a token back into readable text in a diagnostic.

#### Cross Target Validation

`k2/ComposableTargetChecker.kt` is a `FirFunctionCallChecker` that runs the inferencer over each composable call:

```kotlin
override fun check(expression: FirFunctionCall) {
    val calleeFunction = expression.calleeReference.toResolvedCallableSymbol() ?: return
    if (calleeFunction.isComposable(context.session)) {
        updateParents(context)
        val infer = FirApplierInferencer(context, reporter)
        val call = inferenceNodeOf(expression, context)
        val target = callableInferenceNodeOf(expression, calleeFunction, context)
        // ... map arguments to inference nodes ...
        infer.visitCall(call, target, arguments)
    }
}
```

Its `ErrorReporter` turns the failed token sets into prose using each marker's `description`, and reports one of two diagnostics:

- `COMPOSE_APPLIER_CALL_MISMATCH`: "Calling a {1} composable function where a {0} composable was expected"
- `COMPOSE_APPLIER_PARAMETER_MISMATCH`: "A {1} composable parameter was provided where a {0} composable was expected"

**Example Error:**

```kotlin
@Composable
@ComposableTarget("androidx.compose.ui.UiComposable")
fun UiButton(text: String, content: @Composable () -> Unit) { /* ... */ }

@Composable
@ComposableTarget("com.example.CustomApplier")
fun CustomWidget() { /* ... */ }

@Composable
fun Screen() {
    UiButton("Click") {
        CustomWidget()  // COMPOSE_APPLIER_CALL_MISMATCH
    }
}
```

All three applier diagnostics are warnings rather than errors. A mismatch usually is a real bug, but inference here spans the whole call graph, and a library that never declared a target can pull an unrelated build into a conflict it cannot fix. Reporting without blocking is the compromise.

### 8.5 Composable Function Types

The frontend has no interceptor rewriting lambda descriptors anymore. `@Composable` function types are a first class function type kind in FIR, contributed by an extension:

```kotlin
class ComposableFunctionTypeKindExtension(session: FirSession) : FirFunctionTypeKindExtension(session) {
    override fun FunctionTypeKindRegistrar.registerKinds() {
        registerKind(ComposableFunction, KComposableFunction)
    }
}

object ComposableFunction : FunctionTypeKind(
    FqName("androidx.compose.runtime.internal"),
    "ComposableFunction",
    ComposeClassIds.Composable,
    isReflectType = false,
    isInlineable = true,
) {
    override val prefixForTypeRender: String get() = "@Composable"
    override fun reflectKind(): FunctionTypeKind = KComposableFunction
}
```

Because the kind is registered with the type system, `@Composable () -> Unit` is a distinct type rather than a function type with an annotation on it. Ordinary type inference then does the work that used to need an interceptor:

```kotlin
// Expected type is @Composable () -> Unit, so the lambda is inferred
// with the ComposableFunction kind and composable calls are allowed inside it
val content: @Composable () -> Unit = {
    Text("Hello")
}

Column(
    content = {
        Text("Item 1")
        Text("Item 2")
    }
)
```

This is also the check `ComposableCallChecker` performs when it asks whether a lambda opens a composable scope: `function.typeRef.coneType.functionTypeKind(context.session) === ComposableFunction`.

One compatibility detail is worth noting. The kind sets `serializeAsFunctionWithAnnotationUntil`, so composable function types are written into metadata as plain function types carrying `@Composable`. That keeps libraries built by the current compiler readable by older plugin versions.

### 8.6 Analysis Pipeline

#### Compilation Phases

```
┌─────────────────────────┐
│    1. PARSING           │
│  Source to light tree   │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   2. FIR RESOLUTION     │
│  Types, symbols, and    │
│  composable type kinds  │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   3. FIR CHECKERS       │
│  ┌──────────────────┐   │
│  │ Annotation check │   │
│  │ Function check   │   │
│  │ Property check   │   │
│  │ Call check       │   │
│  │ Target check     │   │
│  └──────────────────┘   │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   4. FIR2IR             │
│  Build the IR tree      │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   5. IR ANALYSIS        │
│  Stability inference    │
│  Static detection       │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│   6. IR LOWERING        │
│  Transform composables  │
└─────────────────────────┘
```

#### Extension Registration

Everything the frontend contributes is registered in one place:

```kotlin
class ComposeFirExtensionRegistrar : FirExtensionRegistrar() {
    override fun ExtensionRegistrarContext.configurePlugin() {
        +::ComposableFunctionTypeKindExtension
        +::ComposeFirCheckersExtension
        +::ComposableTargetSessionStorage

        registerDiagnosticContainers(ComposeErrors)
    }
}
```

`ComposeFirCheckersExtension` is where each checker is attached to its slot:

```kotlin
override val declarationCheckers = object : DeclarationCheckers() {
    override val functionCheckers = setOf(ComposableFunctionChecker)
    override val propertyCheckers = setOf(ComposablePropertyChecker)
}
override val typeCheckers = object : TypeCheckers() {
    override val resolvedTypeRefCheckers = setOf(ComposableAnnotationChecker)
}
override val expressionCheckers = object : ExpressionCheckers() {
    override val functionCallCheckers = setOf(ComposableFunctionCallChecker, ComposableTargetChecker)
    override val propertyAccessExpressionCheckers = setOf(ComposablePropertyAccessExpressionChecker)
    override val callableReferenceAccessCheckers = setOf(ComposablePropertyReferenceChecker)
}
```

There is no declaration generator, supertype generator, or status transformer. The frontend only inspects and reports; every declaration the plugin synthesizes, including the `$stable` field, is created in the IR phase.

#### Data Flow Through Phases

**FIR checkers to session components:**

```kotlin
// During applier target checking
session.composableTargetSessionStorage.storeLazyScheme(node, lazyScheme)
```

**IR analysis to IR attributes:**

```kotlin
// During static expression analysis
expression.isStaticExpression = isStatic

// During key generation
function.durableFunctionKey = keyInfo
```

**IR lowering reading attributes back:**

```kotlin
// During composable transformation
val isStatic = expr.isStaticExpression
val key = function.durableFunctionKey
```

### 8.7 Practical Examples

#### Example: Composable Context Validation

```kotlin
class MainActivity {
    fun onCreate() {
        Text("Hello")  // ERROR: COMPOSABLE_INVOCATION
    }

    @Composable
    fun Content() {
        Text("Hello")  // OK: inside a composable function

        runBlocking {
            Text("Error")  // ERROR: COMPOSABLE_INVOCATION
        }

        LaunchedEffect(Unit) {
            Text("Error")  // ERROR: the block is suspend, not composable
        }

        try {
            Text("Error")  // ERROR: ILLEGAL_TRY_CATCH_AROUND_COMPOSABLE
        } catch (e: Exception) {
            Text("Fine")   // OK: catch blocks are allowed
        }
    }
}
```

**Analysis flow for the first call:**

1. `ComposableFunctionCallChecker` resolves `Text` and finds it composable
2. `visitCurrentScope` walks outward from the call through `containingElements`
3. It reaches `onCreate`, a `FirFunction` without `@Composable`, so `checkComposableFunction` reports `COMPOSABLE_EXPECTED` on the declaration and returns `CONTINUE`
4. The walk ends, and `checkComposableCall` reports `COMPOSABLE_INVOCATION` on the call

The developer sees two diagnostics for one mistake, one pointing at the call and one at the function that should have been annotated.

The `runBlocking` and `LaunchedEffect` cases end the same way for a different reason. Neither function is inline, so their lambdas arrive with an `inlineStatus` other than `Inline`. Their function type kind is not `ComposableFunction` either, so the walk visits the lambda, finds nothing composable about it, and the non inline status stops it right there. Suspend lambdas are never special cased anywhere in the checker; they simply fail the composable type kind test like any other ordinary lambda.

#### Example: Inline Lambda Analysis

```kotlin
inline fun <T> withoutComposables(
    @DisallowComposableCalls block: () -> T
): T = block()

@Composable
fun Screen() {
    withoutComposables {
        val data = loadData()  // OK
        Text(data)  // ERROR: CAPTURED_COMPOSABLE_INVOCATION
    }
}
```

**Analysis flow:**

1. The walk from `Text` reaches the lambda, which is `Inline`
2. `findValueParameterForLambdaAtIndex` maps the lambda back to `block` through the resolved argument list
3. `block`'s type carries `@DisallowComposableCalls`
4. `CAPTURED_COMPOSABLE_INVOCATION` is reported, naming both `block` and `withoutComposables`

The rule exists because an inline lambda passed to a function like this may be stored and invoked later, outside composition, where there is no composer to call into.

#### Example: Stability and Skipping

```kotlin
class Foo(var value: Int = 0)

@Composable
fun Test(x: Int) {
    A(x)
}

@Composable
fun Test(x: Foo) {
    used(x)
}
```

**Stability analysis:**

- `Int` is a primitive, so `Certain(stable = true)`
- `Foo` has a `var` property, so `Certain(stable = false)`, and it gets `@StabilityInferred(parameters = 0)` with `$stable = 8`

**Generated code**, as the compiler's golden tests record it:

```kotlin
fun Test(x: Int, %composer: Composer?, %changed: Int) {
  %composer = %composer.startRestartGroup(<>)
  val %dirty = %changed
  if (%changed and 0b0110 == 0) {
    %dirty = %dirty or if (%composer.changed(x)) 0b0100 else 0b0010
  }
  if (%composer.shouldExecute(%dirty and 0b0011 != 0b0010, %dirty and 0b0001)) {
    A(x, %composer, 0b1110 and %dirty)
  } else {
    %composer.skipToGroupEnd()
  }
  %composer.endRestartGroup()?.updateScope { %composer: Composer?, %force: Int ->
    Test(x, %composer, updateChangedFlags(%changed or 0b0001))
  }
}
```

```kotlin
fun Test(x: Foo, %composer: Composer?, %changed: Int) {
  %composer = %composer.startRestartGroup(<>)
  val %dirty = %changed
  if (%changed and 0b0110 == 0) {
    %dirty = %dirty or if (%composer.changedInstance(x)) 0b0100 else 0b0010
  }
  if (%composer.shouldExecute(%dirty and 0b0011 != 0b0010, %dirty and 0b0001)) {
    used(x)
  } else {
    %composer.skipToGroupEnd()
  }
  %composer.endRestartGroup()?.updateScope { %composer: Composer?, %force: Int ->
    Test(x, %composer, updateChangedFlags(%changed or 0b0001))
  }
}
```

The two bodies have the same shape. Both open a restart group, both compute a `%dirty` mask, both gate the body on `shouldExecute`, and both skip to the end of the group when nothing changed. There is exactly one difference: the stable parameter goes through `%composer.changed(x)` and the unstable one through `%composer.changedInstance(x)`.

That single call is where all of stability inference lands. `changed` compares with `equals()`, so an equal value skips even when it is a different instance. `changedInstance` compares with `===`, so a rebuilt object never compares equal and the body runs again. A `data class` that is unstable does not lose the ability to skip; it loses the ability to *match*, which in a screen that rebuilds its state on every emission amounts to the same cost.

`shouldExecute` came in with pausable composition. Its first argument is whether any used parameter differs from last time, and its second is `%dirty and 1`, the low bit that says the scope was restarted rather than reached normally. That second argument is what lets a paused composition resume a scope it had already decided to skip. When `FeatureFlag.PausableComposition` is off, or the runtime on the classpath is too old to have the function, `irShouldExecute` falls back to the older form:

```kotlin
irOrOr(
    parametersChanged,
    irNot(irIsSkipping())
)
```

One more branch depends on the flags. With strong skipping disabled, a function that has both unstable parameters and default values gets an extra guard, `defaultParam.irHasAnyProvidedAndUnstable(unstableMask)`, forcing execution whenever an unstable parameter was actually passed. Strong skipping makes that guard unnecessary, since `changedInstance` already handles those parameters, so with the default configuration it is never generated.
## Conclusion

Most day to day work with stability comes down to two habits. Read the `-classes.txt` report before you guess, because the compiler will tell you exactly which field made a class unstable and the answer is often a `var` you forgot about or an interface typed property. And when you reach for a fix, prefer giving the compiler something it can infer, a `val` of a type it already knows, over declaring a type stable in the configuration file, since that declaration is a promise the compiler cannot check.

What is worth carrying away is that stability was never really about skipping. Since strong skipping became the default, every restartable composable can skip. Stability decides how the runtime *compares* a parameter, `equals()` or `===`, and everything in this repository, the bitmasks, the `$stable` field, the file scoped runtime fallback, exists to answer that one question as precisely as separate compilation allows. Once you read it that way, an unstable class stops being a thing that blocks an optimization and becomes a thing that compares by identity, which is a far easier property to reason about in your own code.

If you want more on Compose performance, check out the [compose-performance repository](https://github.com/skydoves/compose-performance).

<a href="https://www.android.skydoves.me/">
<img src="https://github.com/user-attachments/assets/e014ce01-3461-40af-bb2a-eb44f3f55f36" width="13%" align="right"/>
</a>

## 📘 Manifest Android Interview

[Manifest Android Interview](https://www.android.skydoves.me/) is a comprehensive guide designed to enhance your Android development expertise through 108 interview questions with detailed answers, 162 additional practical questions, and 50+ "Pro Tips for Mastery" sections. The interview questions primarily focus on Android development, including the Framework, UI, Jetpack Libraries, and Business Logic, as well as Jetpack Compose, covering Fundamentals, Runtime, and UI.

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
