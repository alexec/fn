# fn

A fun programming languge.

Maybe easier to say what we hate about different languages, than what we love:

* Tuning GC.
* Formatting code. I don't care about opinions.
* Verbosity. Long words used when short words would be fine. My screen is only so big.
* Keyboard acrobatics. Code that looks like math. Syntax more like Ruby than C++.
* Exception handling. Not try/catch or magic numbers.

* Tools (especially compilers and type systems) that put barriers in the way of me solving my problem
  * I should be able to compile and run a tiny bit of my program to find out if it works without refactoring the whole codebase, and only when it works should I need to care about all the places that I need to fix
* Complex syntax
  * I should be able to read the code and understand what's being expressed
    * Counter examples
      * Scala
      * Perl
* Significant whitespace
  * This breaks in too many situations. Eg. copy/paste, email
* async/await
  * This is the lowest common denominator of async programming
  * Better options include
    * CSP + transducers
    * first class continuations
  
Things that need to be supported:
* Literal data structures (at least maps, vectors/arrays, numbers, strings, nouns, verbs ... maybe sets, queues, stacks?)
  * all data is serialisable / round tripable
  * by nouns, I mean names for bits of information are not strings. Ie. in JSON
```
{
  name: "name"
}
```
the key `name` is should not be the same as the value `"name"`. They should have distict types.
   * by verbs I mean names for bits of code that are equally distict from strings (and names for bits of data)
   * nouns and verbs need to have namespaces the same entity will need to use the same name for different values in different bounded contexts - namespaces fix this
```
{
  customer.id: "123"
  product.id: "456"
}
```
   * all data is immutable
* transaction system to handle atomic updates to data in memory
* monoidal operations on each of the collection types (identity value + operation)
  * need to have a meaningful algebra for combining data
* collections lib should be built around transducers (for optional lazy evaluation)
  * collections should be persistent functional data structures a la Bagwell/Hickey
    * https://docs.rs/im/latest/im/
    * https://github.com/orium/rpds
* mulitple dispatch
  * predicate dispatch?
* module system
  * library fetching
  * versioning
  * identifying breaking changes
* Tail call optimization

Good ideas that could be pilfered:
* Content addressable code from unison: https://www.unison-lang.org/learn/the-big-idea/
* Commas are whitespace
* Common lisp condition system for error handling?

Dumb questions:
* Do you need classes for anything other than state machines? Can we avoid "classes" altogether?
  * Yes ;P

* Use cases for programming tools
  * Information processing (eg. take this order, calculate the delivery charge & tax, store it in a db ready for picking/shipping)
    * there are 3 bits to this
      - what an order looks like
      - the domain model of tax calculation, etc
      - the mechanical part of storing for retrieval later
    * standard java style classes are bad at all of this
    * maps / lists / etc are better
      - they have an abstract information model that allows introspection, combination, etc
      - they have occurrence typing and can process information in shapes they didn't know about when created
        - eg. the tax calculation can apply to orders and purchase orders without being re-written if the po has the relevant info in the right place and extra info can just be returned from a function call
      - there's a meaningful implementation of equals
      - RDF has a lot to offer / inform here
      - namespaces on names are really useful
    * everything this does should be a pure function
    * this builds a domain model
    * there should be some kind of schema library to describe and validate the shape of the data
  * Mechanical processes
    * essentially this is a state machine
    * eg. writing a web server or updating a db record
    * java style classes are better at this, but not great
    * these can often be built out of smaller pieces but they're generally impure and should provide a transactional way of going from the past into the future
      * examples:
        - CSP (like go channels, clojure.core.async, etc)
        - Clojure's STM / OCaml's ref type
        - Actors (like erlang otp, and scala akka) ( - probably buildable out of the previous two?)
    * These should apply a domain model transactionally to some data
    * This sounds a lot like the disruptor pattern / cqrs / event sourcing as a fundamental design principal
    * support for runtime manipulation of processes and data
      * processes come in a few shapes
        * examples
          - request / response (+ middleware / interceptors?)
          - map / reduce
          - transducers
          - state machines
          - concurrent execution
          - rules engine
          - logic engine / minikanren / prolog
        * these sound a bit like monads and should be combinable in some way????
  * loose coupling
    * a lot of langs have high coupling in their default idioms
      * examples
        - mutable by default collections
        - not differentiating between the name, identity and value of something. ie.
```
val x = new Customer("Bob") // this says nothing about the immutablity of the Customer object, but says that `x` cannot refer to any other object, which is pointless
```
        - full coverage pattern matching
          - this is localised polymorphism, if you add a new value to an enum (for example) then that becomes a breaking change to any users of that enum
          - better served by destructuring + generic polymorphism??
        - async/await and other structural patterns
  * Serialisation
    * json is not good enough - needs more data types ... and to be extensible
    * code needs to be serialisable - or at least resolvable (content based addressing) - so we can send code to the data
    * if we're serialising code, then the compiler needs to be available at runtime
  * Typo's
    * dynamic langs tend to not provide tools that help catch typos
  * Encapsulation can fuck off
    * it only creates problems and solves none
  * Static analysis
    * type systems are both too weak and too strong
      * a map is a function with very specific mappings between it's input and output
        * static analysis should be able to track this and tell you about specific values, not just classes of values
        * see idris / coq and dependent typing
      * static analysis should be optional
        * you should be able to compile and run parts of your program without caring about warnings from a stat.anal tool
        * you should be able to query the stat.anal results for usages / etc
        * stat.anal tools should be canonical - the core lang team should provide "linters"
      * type inference is essential, however you need a different type system different use cases
        * possible use casess:
          - the compiler (if we're targeting wasm, then inferring something is a "Customer" isn't very useful - we need to get down to 64-bit ints)
          - occurrence typing for keys in a map useful for information tracking
  * Small simple language made out of composable pieces
    * the less syntax the better
    * the less obscure dsl's the better
    * the less difficult reasoning the better
    * the core lang should define interfaces for it's core pieces and be built to those abstractions, not concrete types
    * starter for 10 as to the bits of the lang that we should build
      - reader (deserialises data and code from some kind of stream not just files, into in memory data structures (AST ?))
      - writer (serialises data and code)
      - resolver - loads code / dependencies (from git???)
      - compiler - compiles in memory data into executable code (macro expansion?)
      - evaluator - evals compiled code - on a wasm runtime?
      - linter
      - core data structures
      - core libs - divided into pure and impure
