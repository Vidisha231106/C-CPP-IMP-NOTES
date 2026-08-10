# Object-Oriented Programming — Interview-Grade Deep Reference

> Built for someone who already knows the basics and wants both the "why," down to
> implementation mechanics, and the kind of gnarly, multi-concept examples that actually show
> up in system-design and technical interviews. Every section pairs a precise definition with
> a worked example complex enough to expose the edge cases, because that's where interviewers
> actually probe.

---

## Table of Contents

1. [What OOP Is Actually Solving](#1-what-oop-is-actually-solving)
2. [Classes and Objects, Precisely](#2-classes-and-objects-precisely)
3. [Encapsulation — Deep Dive](#3-encapsulation--deep-dive)
4. [Abstraction — Deep Dive](#4-abstraction--deep-dive)
5. [Inheritance — Deep Dive](#5-inheritance--deep-dive)
6. [Polymorphism — Deep Dive](#6-polymorphism--deep-dive)
7. [Constructors, Destructors, Object Lifecycle](#7-constructors-destructors-object-lifecycle)
8. [Static vs Instance Members](#8-static-vs-instance-members)
9. [Abstract Classes vs Interfaces](#9-abstract-classes-vs-interfaces)
10. [Composition vs Inheritance — Full Worked Refactor](#10-composition-vs-inheritance--full-worked-refactor)
11. [SOLID — Full Worked Refactor of a God Class](#11-solid--full-worked-refactor-of-a-god-class)
12. [Design Patterns — Combined, Realistic Systems](#12-design-patterns--combined-realistic-systems)
13. [UML Class Diagrams](#13-uml-class-diagrams)
14. [Advanced Gotchas (Where Interviewers Actually Dig)](#14-advanced-gotchas-where-interviewers-actually-dig)
15. [Common Pitfalls and Anti-Patterns](#15-common-pitfalls-and-anti-patterns)
16. [Interview System-Design Drills (OOP-Focused)](#16-interview-system-design-drills-oop-focused)
17. [Self-Check and Mock Interview Questions](#17-self-check-and-mock-interview-questions)

---

## 1. What OOP Is Actually Solving

Procedural code breaks down at scale for two reasons: **state and the logic that guards it
drift apart** (any function can mutate any data, so nothing guarantees validity), and
**change is expensive** (new variants mean editing a big conditional scattered across the
codebase instead of adding a self-contained unit). Every pillar below answers one of these
two problems: protect state, or make variation cheap by *adding* code instead of *editing*
it.

Interviewers care about this framing because they'll often ask "why not just use a
dictionary and functions here?" — and the strong answer is always traceable back to one of
these two problems, not "because OOP is how it's done."

---

## 2. Classes and Objects, Precisely

A **class** is a blueprint; an **object** is a concrete instance with its own state living at
a specific address. Worth being crisp about, because interviewers test it directly:

- **Stack vs heap**: in Java/C#, objects always live on the heap; local variables are
  references on the stack. In C++, `Point p;` allocates on the stack (destroyed
  automatically at scope end), while `Point* p = new Point();` allocates on the heap
  (you own cleanup). This single distinction is the root of most C++ memory-management
  interview questions.
- **A class is itself an object** in Python/Ruby/Smalltalk — an instance of a metaclass
  (`type` in Python). This is why `type(MyClass)` returns something, and why frameworks like
  Django's ORM can dynamically generate model classes at import time.
- **Identity vs equality**: `a is b` (Python) / `a == b` (Java, reference identity by
  default) asks "same object in memory?" `a == b` (Python, calls `__eq__`) / `a.equals(b)`
  (Java) asks "same logical value?" Getting this backwards is a classic interview bug —
  e.g., comparing boxed `Integer` objects in Java with `==` works for small cached values
  (-128 to 127) and silently breaks for larger ones, because the JVM caches small boxed
  integers. This exact gotcha ("why did `Integer a = 200; Integer b = 200; a == b` return
  false?") is a real, frequently-asked interview question.

### Complex example: value object with correct equality

```java
public final class Money {
    private final long cents;
    private final String currency;

    public Money(long cents, String currency) {
        this.cents = cents;
        this.currency = currency;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Money)) return false;
        Money other = (Money) o;
        return cents == other.cents && currency.equals(other.currency);
    }

    @Override
    public int hashCode() {
        return Objects.hash(cents, currency);
    }
}
```

Why this is harder than it looks: **the `equals`/`hashCode` contract** requires that if
`a.equals(b)` is true, `a.hashCode() == b.hashCode()` must also be true — break this and
`Money` objects silently vanish from `HashSet`/`HashMap` lookups (you insert one, look it up
with an "equal" object, and get a miss, because it hashed to a different bucket). This is one
of the most common "gotcha" interview questions in Java specifically, and the discipline
generalizes: any language with hash-based collections and custom equality has the same trap.

---

## 3. Encapsulation — Deep Dive

Encapsulation protects **invariants** — facts that must always hold about an object's state —
by forcing every mutation through methods the class controls, and it also hides
*implementation*, not just data, enabling internal changes without breaking callers.

A shallow version (private fields + a getter and setter for literally every field) provides
none of this protection — it's public fields with extra ceremony. Real encapsulation means
asking, per field: does external code need to *read* this, does it need to *write* it
directly, or should it only change indirectly through a method that enforces a rule?

### Complex example: immutable value object + defensive copying + validated builder

This is a favorite interview scenario because it stacks three encapsulation techniques at
once: immutability, defensive copying of mutable fields, and a validating builder.

```java
public final class FlightBooking {
    private final String passengerName;
    private final List<String> segments;      // mutable type inside an immutable object
    private final LocalDateTime bookedAt;

    private FlightBooking(Builder b) {
        this.passengerName = b.passengerName;
        // Defensive copy: without this, the caller's list reference leaks out,
        // and mutating their original list would silently corrupt this "immutable" object.
        this.segments = new ArrayList<>(b.segments);
        this.bookedAt = b.bookedAt;
    }

    public List<String> getSegments() {
        // Defensive copy on the way OUT too, or callers can mutate our internal list.
        return new ArrayList<>(segments);
    }

    public static class Builder {
        private String passengerName;
        private List<String> segments = new ArrayList<>();
        private LocalDateTime bookedAt = LocalDateTime.now();

        public Builder passenger(String name) {
            if (name == null || name.isBlank())
                throw new IllegalArgumentException("Passenger name required");
            this.passengerName = name;
            return this;
        }

        public Builder addSegment(String segment) {
            this.segments.add(segment);
            return this;
        }

        public FlightBooking build() {
            if (segments.isEmpty())
                throw new IllegalStateException("Booking needs at least one segment");
            return new FlightBooking(this);
        }
    }
}
```

The interview-relevant insight: **immutability without defensive copying is fake
immutability.** If `FlightBooking` just stored `b.segments` directly (no `new
ArrayList<>(...)`), the caller could do:

```java
List<String> segs = new ArrayList<>(List.of("JFK-LHR"));
FlightBooking booking = new FlightBooking.Builder().passenger("A. Turing").addSegment("JFK-LHR").build();
segs.add("LHR-CDG");   // if the builder didn't copy, this could corrupt booking's internal state
```

This exact pattern — "is this class actually immutable, or does it just look immutable?" —
is a standard senior-level interview probe.

---

## 4. Abstraction — Deep Dive

**Encapsulation** hides data/implementation state. **Abstraction** hides *complexity* by
exposing only the essential interface. You can have one without much of the other — a class
riddled with mechanical getters is encapsulated but poorly abstracted, since callers still
have to think in terms of internal fields.

### Complex example: layered abstraction hiding an unreliable dependency

A common interview ask is "design a caching layer in front of a flaky external API." This
forces you to abstract across multiple concerns at once: the caller shouldn't know whether
data came from cache, a retry, or a live call.

```python
from abc import ABC, abstractmethod
import time

class ExchangeRateProvider(ABC):
    @abstractmethod
    def get_rate(self, currency_pair: str) -> float: ...

class FlakyThirdPartyProvider(ExchangeRateProvider):
    """The messy, unreliable real implementation."""
    def get_rate(self, currency_pair: str) -> float:
        # imagine this sometimes throws, sometimes times out
        return self._call_external_api(currency_pair)

    def _call_external_api(self, currency_pair):
        ...

class RetryingProvider(ExchangeRateProvider):
    """Abstracts away flakiness behind retries."""
    def __init__(self, delegate: ExchangeRateProvider, max_attempts=3):
        self._delegate = delegate
        self._max_attempts = max_attempts

    def get_rate(self, currency_pair: str) -> float:
        last_error = None
        for attempt in range(self._max_attempts):
            try:
                return self._delegate.get_rate(currency_pair)
            except Exception as e:
                last_error = e
                time.sleep(0.1 * (attempt + 1))   # backoff
        raise last_error

class CachingProvider(ExchangeRateProvider):
    """Abstracts away latency and staleness behind a TTL cache."""
    def __init__(self, delegate: ExchangeRateProvider, ttl_seconds=60):
        self._delegate = delegate
        self._ttl = ttl_seconds
        self._cache = {}   # currency_pair -> (rate, timestamp)

    def get_rate(self, currency_pair: str) -> float:
        cached = self._cache.get(currency_pair)
        if cached and (time.time() - cached[1]) < self._ttl:
            return cached[0]
        rate = self._delegate.get_rate(currency_pair)
        self._cache[currency_pair] = (rate, time.time())
        return rate

# Client code only ever sees the abstraction:
provider: ExchangeRateProvider = CachingProvider(RetryingProvider(FlakyThirdPartyProvider()))
rate = provider.get_rate("USD/EUR")
```

Why this is the "hard" version of abstraction: each layer implements the **same interface**
it wraps (this is the **Decorator pattern**, Section 12), so the client's mental model never
grows more complex no matter how much resilience machinery you stack underneath. This is
also a clean, concrete answer to "what's the difference between abstraction and
encapsulation" in an interview — point at `ExchangeRateProvider` as the abstraction and at
each class's private state (`_cache`, `_max_attempts`) as the encapsulation.

---

## 5. Inheritance — Deep Dive

Inheritance models **"is-a."** Violate that and you get Liskov Substitution Principle
failures (Section 11). Beyond the basics, three things separate people who've memorized
syntax from people who understand the mechanism:

### 5.1 The Diamond Problem, worked precisely

```
      Animal
      /    \
   Swimmer  Runner
      \    /
    Triathlete
```

If both `Swimmer` and `Runner` inherit a `stamina` field from `Animal` and `Triathlete`
inherits from both, does `Triathlete` have one `stamina` or two conflicting copies?

- **C++** actually lets this ambiguity happen unless you use **virtual inheritance**:
  `class Swimmer : virtual public Animal`. Without `virtual`, `Triathlete` gets two separate
  `Animal` subobjects (two copies of `stamina`), and `triathlete.stamina` won't even compile
  — ambiguous. With `virtual`, both paths share a single `Animal` subobject.
- **Python** resolves this via **C3 linearization** (`Triathlete.__mro__`), producing a
  single deterministic, consistent method resolution order. Run `Triathlete.mro()` and
  you'll see the exact order Python searches — this is directly inspectable, unlike C++.
- **Java/C#** sidestep it: no multiple inheritance of classes at all, only of interfaces —
  and even there, Java 8's `default` methods reopened a smaller version of the same problem
  (two interfaces providing conflicting default implementations of the same method forces
  the implementing class to explicitly override and disambiguate).

### 5.2 Method hiding vs overriding — the C++/Java trap

```cpp
class Base {
public:
    void greet() { std::cout << "Base\n"; }             // NOT virtual: hides, doesn't override
    virtual void greetVirtual() { std::cout << "Base\n"; }
    virtual ~Base() {}   // virtual destructor -- see Section 14
};
class Derived : public Base {
public:
    void greet() { std::cout << "Derived\n"; }
    void greetVirtual() override { std::cout << "Derived\n"; }
};

Base* b = new Derived();
b->greet();          // prints "Base"   <- surprising, and a classic interview trick question
b->greetVirtual();   // prints "Derived"
delete b;
```

The interview version of this question is usually phrased as "what does this print, and
why?" — the correct answer requires explaining vtables (Section 14), not just memorizing the
output.

### 5.3 Constructor/destructor order in inheritance chains

A frequently-asked interview question: **in what order do constructors and destructors run
in a hierarchy?**

```
Construction: Base → Derived → MostDerived   (top-down: parent first)
Destruction:  MostDerived → Derived → Base    (bottom-up: reverse of construction)
```

This matters practically: a `Derived` constructor can rely on `Base`'s fields already being
initialized, and a `Base` destructor should never call an overridden virtual method
expecting `Derived`'s behavior — by the time `~Base()` runs during destruction, the
`Derived` part of the object has *already been torn down*, so a virtual call from `~Base()`
resolves to `Base`'s own implementation, not `Derived`'s (in C++; other languages differ but
the underlying gotcha — "the derived-most part is gone" — is broadly true).

---

## 6. Polymorphism — Deep Dive

Two core kinds, plus a third worth knowing at interview level.

### 6.1 Compile-time (overloading) vs runtime (overriding) — precisely

Overloading is resolved by the **compiler**, using the **static (declared) type** of the
arguments at the call site. Overriding is resolved at **runtime**, using the **actual
(dynamic) type** of the object. This distinction is the entire answer to a very common
trick question:

```java
class Animal {
    void speak(Animal a) { System.out.println("Generic speak"); }
    void speak(Dog d) { System.out.println("Dog-specific speak"); }
}

Animal a = new Dog();       // static type: Animal, dynamic type: Dog
a.speak(a);                 // which overload runs?
```

Answer: **`speak(Animal a)`** runs — overload resolution happens at compile time based on
the *declared* type of the argument (`Animal a`, since the variable is declared as
`Animal`), completely ignoring the fact that the runtime object is a `Dog`. This surprises
almost everyone the first time they see it and is a genuinely good interview differentiator.

### 6.2 Covariant return types (an advanced override rule)

An override can narrow its return type to a subtype of the original method's return type —
this is legal and doesn't break the contract, because callers expecting the base type still
get something compatible:

```java
class Animal {
    Animal reproduce() { return new Animal(); }
}
class Dog extends Animal {
    @Override
    Dog reproduce() { return new Dog(); }   // legal: Dog is-a Animal (covariant return)
}
```

### 6.3 Double dispatch — the Visitor pattern

Regular polymorphism is **single dispatch**: the method that runs depends on the runtime
type of *one* object (the receiver). Some problems need the behavior to depend on the
runtime types of **two** objects simultaneously — e.g., collision handling between two
different shape types. This needs **double dispatch**, and the classic OOP solution is the
**Visitor pattern**:

```java
interface Shape {
    void accept(ShapeVisitor visitor);
}
class Circle implements Shape {
    public void accept(ShapeVisitor v) { v.visit(this); }   // dispatch #1: which visit() overload
}
class Square implements Shape {
    public void accept(ShapeVisitor v) { v.visit(this); }
}
interface ShapeVisitor {
    void visit(Circle c);
    void visit(Square s);
}
class CollisionVisitor implements ShapeVisitor {
    public void visit(Circle c) { /* circle-specific collision logic */ }
    public void visit(Square s) { /* square-specific collision logic */ }
}
```

The first dispatch (`shape.accept(visitor)`) resolves based on the shape's runtime type; the
second (`visitor.visit(this)`, where `this` has a concrete compile-time type inside each
`accept` override) resolves the overload based on that concrete type. Interviewers who ask
"how would you handle behavior that depends on two objects' types without a giant
`instanceof` chain" are fishing for exactly this pattern.

### 6.4 Parametric polymorphism (generics) with bounded types — a harder example

```java
// A generic repository that only accepts types that are Comparable,
// so it can keep entries sorted -- bounded type parameter
class SortedRepository<T extends Comparable<T>> {
    private final List<T> items = new ArrayList<>();

    public void add(T item) {
        int index = Collections.binarySearch(items, item);
        int insertAt = index >= 0 ? index : -(index + 1);
        items.add(insertAt, item);
    }
}
```

`T extends Comparable<T>` (an interview-common piece of generics syntax) says "T can be any
type, as long as that type can compare itself to other instances of itself" — this is
**F-bounded polymorphism**, and being able to explain why the type parameter appears inside
its own bound (`Comparable<T>`, not just `Comparable`) is a solid signal of generics fluency.

---

## 7. Constructors, Destructors, Object Lifecycle

Beyond the basics: **object slicing** (C++-specific but conceptually important) happens when
a derived object is assigned by value into a base-typed variable — only the base part gets
copied, silently discarding the derived-specific data and behavior:

```cpp
void process(Base b) {   // pass BY VALUE, not by reference/pointer
    b.greetVirtual();    // always calls Base's version -- polymorphism is lost!
}
Derived d;
process(d);   // d is "sliced" down to just its Base portion when copied into the parameter
```

The fix is to always pass polymorphic objects by reference or pointer (`Base&` or `Base*`),
never by value — a very standard C++ interview trap.

**RAII (Resource Acquisition Is Initialization)**, C++'s core resource-management idiom: tie
a resource's lifetime to an object's lifetime, so the destructor guarantees cleanup even if
an exception is thrown mid-function — this is the conceptual ancestor of Java's
try-with-resources and Python's context managers, and interviewers sometimes ask you to
connect the three.

---

## 8. Static vs Instance Members

### Complex example: thread-safe lazy singleton (a genuinely tricky interview question)

"Implement a thread-safe singleton" is one of the most common OOP+concurrency crossover
interview questions, because the naive version is subtly broken:

```java
// BROKEN under multi-threading: two threads can both see instance == null
// and both construct separate instances.
class NaiveSingleton {
    private static NaiveSingleton instance;
    public static NaiveSingleton getInstance() {
        if (instance == null) {
            instance = new NaiveSingleton();
        }
        return instance;
    }
}

// CORRECT: double-checked locking with `volatile`
class Singleton {
    private static volatile Singleton instance;
    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {                 // 1st check: avoid locking on the common path
            synchronized (Singleton.class) {
                if (instance == null) {          // 2nd check: only one thread constructs it
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

Why `volatile` is non-negotiable here (a favorite "explain why" follow-up): without it, the
JVM/compiler can reorder instructions so another thread sees a **non-null but
not-yet-fully-constructed** `instance` reference (object reference assigned before the
constructor body finishes running) — a partially-built object leaking out. `volatile`
prevents that specific reordering. Most languages have a cleaner idiom for this
(Python's module-level singletons rely on the GIL and import system; C#'s `Lazy<T>`; Java
itself often prefers an `enum` singleton, which sidesteps this entire problem because enum
instances are guaranteed thread-safe and single by the JVM spec) — being able to name the
cleaner alternative after showing you understand the hard version is a strong interview
signal.

---

## 9. Abstract Classes vs Interfaces

| | Abstract Class | Interface |
|---|---|---|
| State (fields) | Yes | No (constants only, traditionally) |
| Method implementations | Mix of abstract + concrete | Traditionally none; Java 8+/C# 8+ allow `default` methods |
| Constructor | Yes | No |
| Multiple inheritance | Only one abstract superclass | Many interfaces at once |
| Models | "is-a" with shared implementation | "can-do" capability contract |

### Complex example: default-method diamond in Java 8+

Interfaces gained `default` methods to allow adding new methods to an interface without
breaking every existing implementer — but this reopened a mini diamond problem:

```java
interface Flyer {
    default String move() { return "flies"; }
}
interface Swimmer {
    default String move() { return "swims"; }
}
// COMPILE ERROR: Duck must override move() -- Java refuses to silently pick one
class Duck implements Flyer, Swimmer {
    @Override
    public String move() {
        return Flyer.super.move() + " and " + Swimmer.super.move();   // explicit disambiguation
    }
}
```

The `Interface.super.method()` syntax for explicitly choosing (or combining) a default
implementation is obscure enough that bringing it up unprompted is a strong signal in an
interview.

### Complex example: mixin-style capability composition with interfaces

```java
interface Serializable2 { default String serialize() { return "..."; } }
interface Auditable { default void logAccess(String who) { System.out.println(who + " accessed"); } }
interface Cacheable { default String cacheKey() { return "key:" + hashCode(); } }

// A class can pick up several independent capabilities without a rigid single-parent hierarchy
class UserProfile implements Serializable2, Auditable, Cacheable {
    // gets all three behaviors "for free," combinable in ways a single inheritance
    // chain could never express without deep, awkward hierarchies
}
```

This is the practical payoff of "prefer interfaces for capabilities, classes for shared
'is-a' state" — and it's a clean answer to "why would you ever want multiple inheritance"
even in languages that ban it for classes.

---

## 10. Composition vs Inheritance — Full Worked Refactor

The flying-penguin problem, worked all the way through, because "walk me through fixing
this" is an extremely common interview exercise.

### The broken version (inheritance misused)

```java
class Bird {
    void fly() { System.out.println("Flying"); }
}
class Duck extends Bird {}      // fine, ducks fly
class Penguin extends Bird {    // BROKEN: penguins can't fly
    @Override
    void fly() { throw new UnsupportedOperationException("Penguins can't fly"); }
}
```

This violates the **Liskov Substitution Principle**: any code that does
`for (Bird b : birds) b.fly();` now crashes the moment a `Penguin` is in the list, even
though a `Penguin` is, in casual English, still "a bird." The hierarchy encoded a capability
(flying) as if every bird had it.

### The fix: extract flying as a composed, swappable behavior (Strategy pattern)

```java
interface FlightBehavior {
    void fly();
}
class CanFly implements FlightBehavior {
    public void fly() { System.out.println("Flying"); }
}
class CannotFly implements FlightBehavior {
    public void fly() { System.out.println("Can't fly, but can waddle"); }
}

abstract class Bird {
    private final FlightBehavior flightBehavior;   // COMPOSED, not inherited
    protected Bird(FlightBehavior flightBehavior) { this.flightBehavior = flightBehavior; }
    void performFly() { flightBehavior.fly(); }
}
class Duck extends Bird {
    Duck() { super(new CanFly()); }
}
class Penguin extends Bird {
    Penguin() { super(new CannotFly()); }
}
```

Now `performFly()` never throws for any `Bird`, LSP holds, and — the part that really sells
this to interviewers — **you can change a bird's flight behavior at runtime** (e.g., an
injured duck temporarily grounded) by swapping the composed `FlightBehavior` object, which
inheritance could never support, since what a class inherits from is fixed at compile time.

---

## 11. SOLID — Full Worked Refactor of a God Class

A single running example, refactored through all five letters, because "here's some bad
code, fix it" is close to the most common OOP interview format there is.

### Starting point: an order-processing God Class

```java
class OrderProcessor {
    void process(Order order) {
        // validation
        if (order.getItems().isEmpty()) throw new IllegalArgumentException("Empty order");

        // pricing
        double total = 0;
        for (Item i : order.getItems()) total += i.getPrice() * i.getQuantity();
        if (order.hasDiscountCode()) total *= 0.9;

        // payment
        if (order.getPaymentMethod().equals("CREDIT_CARD")) {
            // charge credit card directly here
        } else if (order.getPaymentMethod().equals("PAYPAL")) {
            // call PayPal API directly here
        }

        // persistence
        Connection conn = DriverManager.getConnection("jdbc:mysql://...");
        // raw SQL insert here

        // notification
        // send an email directly here
    }
}
```

Five responsibilities crammed into one method: validation, pricing, payment, persistence,
notification. Any change to any one of these — a new payment provider, a new tax rule, a
switched database — risks breaking the others, and the class is nearly untestable (you can't
unit test pricing without also standing up a real database connection and a real payment
gateway).

### S — Single Responsibility: split by reason to change

```java
class OrderValidator { void validate(Order order) { ... } }
class PricingCalculator { double calculateTotal(Order order) { ... } }
class PaymentProcessor { void charge(Order order, double amount) { ... } }
class OrderRepository { void save(Order order) { ... } }
class NotificationService { void notifyCustomer(Order order) { ... } }
```

### O — Open/Closed: make payment methods extensible without editing existing code

```java
interface PaymentMethod {
    void charge(double amount);
}
class CreditCardPayment implements PaymentMethod {
    public void charge(double amount) { /* ... */ }
}
class PayPalPayment implements PaymentMethod {
    public void charge(double amount) { /* ... */ }
}
// Adding CryptoPayment later means adding a class, never touching the two above,
// and never touching OrderProcessor's logic either.
```

### L — Liskov Substitution: make sure every `PaymentMethod` is truly substitutable

If `CryptoPayment.charge()` needed the caller to first call a nonexistent
`waitForConfirmations()` before trusting the result, it would silently violate the contract
every other `PaymentMethod` honors (charge either succeeds or throws, synchronously as far
as the caller can tell) — the fix is to model asynchronous confirmation explicitly in the
interface (e.g., returning a `PaymentResult` with a `PENDING` state) so *every* implementer,
present and future, honors the same contract rather than one implementer quietly behaving
differently.

### I — Interface Segregation: don't force every payment method to implement refunds if only some support it

```java
interface PaymentMethod { void charge(double amount); }
interface Refundable { void refund(double amount); }

class CreditCardPayment implements PaymentMethod, Refundable { ... }
class CryptoPayment implements PaymentMethod { /* no refund() to stub out */ }
```

### D — Dependency Inversion: `OrderProcessor` depends on abstractions, injected from outside

```java
class OrderProcessor {
    private final OrderValidator validator;
    private final PricingCalculator pricing;
    private final PaymentMethod payment;          // interface, not a concrete class
    private final OrderRepository repository;     // interface, not a concrete JDBC class
    private final NotificationService notifier;

    OrderProcessor(OrderValidator validator, PricingCalculator pricing, PaymentMethod payment,
                    OrderRepository repository, NotificationService notifier) {
        this.validator = validator;
        this.pricing = pricing;
        this.payment = payment;
        this.repository = repository;
        this.notifier = notifier;
    }

    void process(Order order) {
        validator.validate(order);
        double total = pricing.calculateTotal(order);
        payment.charge(total);
        repository.save(order);
        notifier.notifyCustomer(order);
    }
}
```

Now every dependency can be swapped (a `MockPaymentMethod` and `InMemoryOrderRepository` for
unit tests, a real `MySqlOrderRepository` in production) and `OrderProcessor` itself never
changes — the end-to-end interview answer to "how would you make this testable and
extensible," touching all five letters through one coherent example.

---

## 12. Design Patterns — Combined, Realistic Systems

Rather than a flat list, here's how patterns actually combine in a real system, because
"design a notification system" is a common interview prompt that naturally wants three or
four patterns working together.

### Scenario: a multi-channel notification system with retry, logging, and pluggable channels

```java
// STRATEGY: interchangeable delivery channels
interface NotificationChannel {
    void send(String message, String recipient);
}
class EmailChannel implements NotificationChannel {
    public void send(String message, String recipient) { /* SMTP call */ }
}
class SmsChannel implements NotificationChannel {
    public void send(String message, String recipient) { /* SMS gateway call */ }
}

// DECORATOR: add retry behavior to ANY channel without touching its code
class RetryingChannel implements NotificationChannel {
    private final NotificationChannel delegate;
    private final int maxAttempts;
    RetryingChannel(NotificationChannel delegate, int maxAttempts) {
        this.delegate = delegate; this.maxAttempts = maxAttempts;
    }
    public void send(String message, String recipient) {
        for (int attempt = 1; attempt <= maxAttempts; attempt++) {
            try { delegate.send(message, recipient); return; }
            catch (Exception e) { if (attempt == maxAttempts) throw e; }
        }
    }
}

// DECORATOR again: add logging, independently stackable with retry
class LoggingChannel implements NotificationChannel {
    private final NotificationChannel delegate;
    LoggingChannel(NotificationChannel delegate) { this.delegate = delegate; }
    public void send(String message, String recipient) {
        System.out.println("Sending to " + recipient);
        delegate.send(message, recipient);
    }
}

// FACTORY: decides which concrete channel(s) to build, hiding construction detail
class ChannelFactory {
    static NotificationChannel create(String type) {
        NotificationChannel base = switch (type) {
            case "EMAIL" -> new EmailChannel();
            case "SMS" -> new SmsChannel();
            default -> throw new IllegalArgumentException("Unknown channel: " + type);
        };
        return new LoggingChannel(new RetryingChannel(base, 3));
    }
}

// OBSERVER: other parts of the system react to a notification event without NotificationService
// needing to know who's listening
interface NotificationListener { void onSent(String recipient); }
class NotificationService {
    private final NotificationChannel channel;
    private final List<NotificationListener> listeners = new ArrayList<>();

    NotificationService(NotificationChannel channel) { this.channel = channel; }
    void addListener(NotificationListener l) { listeners.add(l); }

    void notify(String message, String recipient) {
        channel.send(message, recipient);
        for (NotificationListener l : listeners) l.onSent(recipient);   // e.g., analytics, audit log
    }
}
```

Four patterns cooperating: **Strategy** (swap channel type), **Decorator** (stack retry +
logging without subclass explosion — imagine trying to get
`LoggingRetryingEmailChannel`/`LoggingRetryingSmsChannel`/`RetryingSmsChannel`/... via
inheritance instead), **Factory** (hide the wiring), **Observer** (let unrelated parts of the
system react to events without coupling). Being able to name *which* pattern is solving
*which specific* problem in a stack like this — not just "I used some patterns" — is what
separates a strong interview answer from a name-dropped one.

### State pattern: modeling an order lifecycle without a sprawling if/else

```java
interface OrderState {
    OrderState next(Order order);
    String name();
}
class PendingState implements OrderState {
    public OrderState next(Order order) { return new PaidState(); }
    public String name() { return "PENDING"; }
}
class PaidState implements OrderState {
    public OrderState next(Order order) { return new ShippedState(); }
    public String name() { return "PAID"; }
}
class ShippedState implements OrderState {
    public OrderState next(Order order) { throw new IllegalStateException("Already shipped"); }
    public String name() { return "SHIPPED"; }
}

class Order {
    private OrderState state = new PendingState();
    void advance() { state = state.next(this); }
    String status() { return state.name(); }
}
```

This is the pattern-level answer whenever an interviewer says "this class has a `status`
field and a giant `switch` on it in five different methods, sprawling every time we add a
status" — each state becomes its own class, and adding a new state means adding a class, not
editing five switch statements (a direct application of Open/Closed).

---

## 13. UML Class Diagrams

```
┌─────────────────────────┐
│      «abstract» Shape     │
├─────────────────────────┤
│ # color: String            │   # = protected
├─────────────────────────┤
│ + area(): double            │
│ + describe(): void          │
└─────────────────────────┘
          △
   ┌──────┴──────┐
   │             │
┌────────┐  ┌────────┐
│ Circle  │  │ Square  │
├────────┤  ├────────┤
│-radius │  │-side   │
├────────┤  ├────────┤
│+area() │  │+area() │
└────────┘  └────────┘
```

Relationship reference:

| Symbol | Relationship | Meaning |
|---|---|---|
| `──▷` hollow triangle, solid | Inheritance | "is-a" |
| `┈┈▷` hollow triangle, dashed | Interface realization | "implements" |
| `──◆` filled diamond | Composition | part's lifetime is owned by whole |
| `──◇` hollow diamond | Aggregation | part can outlive whole |
| `──▶` open arrow | Association | "uses/references" |
| `┈┈▶` dashed open arrow | Dependency | temporary use (e.g., method parameter) |

**Composition vs aggregation, precisely**: a `House` and its `Room`s are composition —
destroy the house, the rooms are gone. A `University` and its `Student`s are aggregation —
close the university, the students still exist. Interviewers use this exact distinction to
check whether you actually understand ownership semantics or just memorized diamond shapes.

---

## 14. Advanced Gotchas (Where Interviewers Actually Dig)

- **Virtual destructors in C++**: if `Base` has a non-virtual destructor and you `delete` a
  `Derived*` through a `Base*`, only `~Base()` runs — `~Derived()` never fires, leaking any
  resources `Derived` owned. Rule: **any class intended to be inherited from and used
  polymorphically through a base pointer must have a virtual destructor.** This is one of
  the single most commonly asked C++ OOP interview questions.
- **Vtable mechanics**: virtual dispatch works by giving each polymorphic class a hidden
  table of function pointers (the vtable) and each object a hidden pointer to its class's
  table (the vptr). A virtual call is "follow vptr, look up the function pointer at this
  slot, call it" — one extra indirection versus a direct call, which is the (usually
  negligible) performance cost of polymorphism, and explains why virtual calls can't be
  inlined by the compiler the way non-virtual calls often can.
- **Python MRO edge cases**: `class D(B, C):` where `B` and `C` both inherit from `A` doesn't
  just "pick B" — it computes a linearization guaranteeing each class appears only once and
  before its own parents (C3 linearization). `D.mro()` shows the exact order; relying on
  "probably left to right" instead of checking is a common bug in real multiple-inheritance
  Python code (e.g., mixins).
- **Constructors calling overridable methods**: calling a method the subclass might override,
  from inside the base constructor, is a classic footgun — the subclass's fields haven't been
  initialized yet when the base constructor runs (construction order is top-down), so the
  overridden method executes against a half-built object and can crash or silently use
  default/zero values instead of what the subclass constructor was about to set.
- **Boxed type identity caching (Java)**: `Integer a = 127, b = 127; a == b` is `true`
  (cached), but `Integer a = 200, b = 200; a == b` is `false` (not cached, different heap
  objects) — always use `.equals()` for boxed-type value comparison, never `==`.
- **LSP violations that only show up under specific usage** (the Square/Rectangle problem,
  Section 11's original framing): a subtype can pass every unit test written against it in
  isolation and still violate LSP, because the violation is about *behavioral contracts
  under substitution*, not about any single method's correctness — this is why LSP is often
  the hardest SOLID letter to actually verify mechanically, and interviewers like probing
  whether you know that.

---

## 15. Common Pitfalls and Anti-Patterns

- **God Object** — one class doing everything (Section 11's starting point).
- **Anemic Domain Model** — pure data-bag classes, all logic pushed into separate "service"
  classes; not automatically wrong, but if unintentional it throws away encapsulation's
  point.
- **Inheritance for code reuse alone** — `Stack extends Vector` in early Java is the
  textbook example: no real "is-a," just reuse, and it leaks list operations a stack
  shouldn't expose (insert-in-the-middle on a stack makes no conceptual sense).
- **The Square/Rectangle problem** — Section 11; "is-a" in English doesn't guarantee "is-a"
  behaviorally.
- **Circular dependencies** — usually fixed by extracting a shared abstraction both classes
  depend on instead (Dependency Inversion again).
- **Premature abstraction** — an interface with exactly one implementation "just in case,"
  paying an indirection cost for a variant that may never arrive.
- **Overusing static/singletons** — reintroduces hidden global state and makes unit testing
  harder (you can't substitute a fake for a static dependency without extra machinery).

---

## 16. Interview System-Design Drills (OOP-Focused)

These are the classic "design X" prompts, framed around which OOP concepts each one is
really testing. Sketch class names and relationships for each before reading the notes.

### Design a Parking Lot
Tests: abstraction (a `Vehicle` hierarchy — `Car`, `Motorcycle`, `Truck` — each with a
`getSize()`), Strategy (a pluggable `FeeStrategy` — hourly vs flat-rate vs event pricing),
composition (`ParkingLot` *has* many `ParkingFloor`s, each *has* many `ParkingSpot`s —
composition, since spots don't outlive their floor), and polymorphism (`ParkingSpot.canFit(Vehicle v)`
dispatching differently per spot type without a giant `instanceof` chain).

### Design an Elevator System
Tests: State pattern (`Idle`, `MovingUp`, `MovingDown`, `DoorsOpen` states, each knowing
valid transitions — directly mirrors Section 12's order-lifecycle example), Observer
(elevator notifies floor-request panels when it arrives), and a subtle SOLID question: should
`Elevator` know about scheduling logic, or should that be a separate `SchedulingStrategy`
injected in (Dependency Inversion, letting you swap FCFS for a nearest-elevator algorithm
without touching `Elevator` itself)?

### Design a Library Management System
Tests: composition vs aggregation precisely (`Library` *aggregates* `Member`s — members exist
independently of the library — but *composes* `Catalog`, which has no meaning without its
library), and Interface Segregation (`Borrowable` vs `Reservable` as separate interfaces,
since reference-only books are `Borrowable` but not `Reservable` the same way).

### Design a Rate Limiter (as a class hierarchy, not just an algorithm)
Tests Strategy sharply: define a `RateLimitingStrategy` interface with `TokenBucket`,
`SlidingWindow`, and `FixedWindow` implementations, all swappable behind the same
`allowRequest(clientId)` contract — a direct, compact demonstration of Open/Closed (new
algorithms added without touching calling code) that's fast to sketch under interview time
pressure.

### Design a Chess Game
Tests inheritance done *correctly* (unlike the flying-penguin trap): `Piece` as an abstract
base with `King`, `Queen`, `Rook`, etc. each overriding `getValidMoves(Board board)` — this
is a genuine "is-a" hierarchy where every subtype really does share the essential contract
(every piece can be asked for its valid moves; none of them need to "throw
UnsupportedOperationException" the way Penguin.fly() did), making it a good example to
contrast against Section 10's broken case in an interview answer.

---

## 17. Self-Check and Mock Interview Questions

**Conceptual:**
1. Why does "private fields + public getter/setter for every field" fail to deliver real
   encapsulation?
2. Walk through exactly what happens, mechanically (vtable/vptr), when a virtual method is
   called through a base-class pointer.
3. Why does a base-class constructor calling an overridable method risk operating on a
   half-initialized object?
4. What specifically does `volatile` prevent in the double-checked-locking singleton, and
   why does the naive version fail only sometimes (i.e., why is it a race condition rather
   than an outright bug)?
5. Explain the `equals`/`hashCode` contract and describe a concrete way violating it breaks
   a `HashMap`.

**Design / refactor prompts (talk through your answer out loud, as in a real interview):**
6. You're handed a `ReportGenerator` class that fetches data, formats it as PDF, emails it,
   and logs the send — all in one 200-line method. Refactor it through SOLID the way Section
   11 did for `OrderProcessor`.
7. A `Rectangle`/`Square` inheritance relationship is causing test failures only in one
   specific code path that resizes shapes independently on each axis. Diagnose why, in LSP
   terms, and propose a fix.
8. You need to support ten different discount rules for an e-commerce checkout, and the
   number is expected to keep growing. Which pattern(s) from Section 12 do you reach for,
   and why not just a big `if/else` chain?
9. A junior engineer proposes `class Employee extends Person` and `class Manager extends
   Employee` and `class Department extends Manager` (a department "is managed by" someone,
   modeled as inheritance). What's wrong with this, and how would you remodel it using
   composition?
10. Pick one of the Section 16 system-design drills and actually sketch the class diagram
    (boxes + relationship arrows, UML-style) before checking the notes above.

Send me your answers to any of these — especially 6, 7, and 9 — and I'll tell you precisely
where the reasoning holds up and where an interviewer would push back, the same way a real
loop would.
