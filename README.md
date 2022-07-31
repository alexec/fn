# fn

A fun programming languge.

Maybe easier to say what we hate about different languages, than what we love:

* Tuning GC.
* Formatting code. I don't care about opinions.
* Verbosity. Long words used when short words would be fine. My screen is only so big.
* Keyboard acrobatics. Code that looks like math. Syntax more like Ruby than C++.

* Tools (especially compilers and type systems) that put barriers in the way of me solving my problem
  * I should be able to compile and run a tiny bit of my program to find out if it works without refactoring the whole codebase, and only when it works should I need to care about all the places that I need to fix
* Complex syntax
  * I should be able to read the code and understand what's being expressed
    * Counter examples
      * Scala
      * Perl
* Significant whitespace
  * This breaks in too many situations. Eg. copy/paste, email
  
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
* mulitple dispatch
  * predicate dispatch?
* module system
  * library fetching
  * versioning
  * identifying breaking changes
