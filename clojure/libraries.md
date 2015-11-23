# Libraries In Use

## Application Bootstrapping

Stuart Sierra's [component library](https://github.com/stuartsierra/component) provides a simple, effective abstraction for modeling the lifecycle of components of a Clojure application, as well as encoding dependencies between components.

## Benchmarking & Profiling

Use [Criterium](https://github.com/hugoduncan/criterium) for writing Clojure benchmarks. If possible, don't invoke your benchmarks via Leiningen.

For profiling, [YourKit](https://www.yourkit.com/) is best in class, but requires purchasing a license (unless you do open source work, in which case they'll give you a free license). [VisualVM](https://visualvm.java.net/) is a free alternative, which is often packaged with the JDK itself.

## CLI Apps

The [tools.cli](https://github.com/clojure/tools.cli) library works in both Clojure and ClojureScript and provides facilities for parsing command-line arguments into Clojure data structures.

The [fs](https://github.com/Raynes/fs) library provides functions for working with the file system from Clojure.

## CSV/TSV Parsing

If you don't need much from your parser, [data.csv](https://github.com/clojure/data.csv) is fine. If your CSV/TSV parsing needs will be performance-bound, you should look at [uniVocity parsers](https://github.com/uniVocity/univocity-parsers).

## Databases/Datastores

This section includes libraries both for relational databases and non-relational stores.

### Migrations

Use [Joplin](https://github.com/juxt/joplin) to organize your application's migrations.

### Relational Databases

Clojure defers to JDBC for relational database handling. Use [java.jdbc](https://github.com/clojure/java.jdbc) for the basic JDBC abstraction, then pull in a database-specific driver to talk with Oracle, PostgreSQL, etc.

Use [iws-oracle](https://github.com/rentpath/iws-oracle) for a higher-level interface and convenience facilities for calling stored functions and procedures.

For building prepared statements in your Clojure code, [Honey SQL](https://github.com/jkk/honeysql) provides a data-driven Clojure API for doing so.

For connection pooling, use [c3p0](http://www.mchange.com/projects/c3p0/).

### Redis

Use [Jedis](https://github.com/xetorthio/jedis) to interact with Redis from Clojure via (minimal) Java interop.

## Data Validation

Prismatic's [Schema](https://github.com/Prismatic/schema) library provides a Clojure-based schema language and validation facilities.

## Documentation Generation

For API-style docs, [Codox](https://github.com/weavejester/codox) is solid. For semi-literate-style docs, try [Marginalia](https://github.com/gdeer81/marginalia).

## Logging

Java's logging story is confusing. Unless you have a compelling reason to do otherwise, just use [Logback](http://logback.qos.ch/) for application logging configuration and include JAR's from slf4j that consolidate competing logging frameworks (org.slf4j/jul-to-slf4j, org.slf4j/jcl-over-slf4j, and org.slf4j/log4j-over-slf4j).

For integrating with Sentry alerts, use [Paul Revere](https://github.com/rentpath/paul-revere).

## Property-based/Generative Testing

The [test.check](https://github.com/clojure/test.check) library provides a more complete story than [test.generative](https://github.com/clojure/test.generative). Prismatic Schema has integration with test.check as well, making the two a good pair.

The [test.chuck](https://github.com/gfredericks/test.chuck) library (developed by the same person as test.check) provides extensions to test.check that are also useful.

## JSON

[Cheshire](https://github.com/dakrone/cheshire) is preferred to Clojure's [data.json](https://github.com/clojure/data.json) for reasons of performance.

[Transit](https://github.com/cognitect/transit-clj) provides a story for type-aware, legal JSON with Transit-aware JSON libraries available for Ruby, Clojure, JavaScript, and other languages.

## Parsing

The [Instaparse](https://github.com/Engelberg/instaparse) library provides solutions to almost all parsing needs.

## State Machines & Finite State Automata ##

Zach Tellman's [Automat](https://github.com/ztellman/automat) library provides facilities for FSM's

## Web

The fundamental request/response abstraction is based on [Ring](https://github.com/mmcgrana/ring) (which is a Ruby Rack-inspired web server interface).

We use [Pedestal](https://github.com/pedestal/pedestal) to configure our web server backend. It uses an embedded [Jetty 9.x](http://www.eclipse.org/jetty/) for the actual web server.

For generating HTML from Clojure, we use [Hiccup](https://github.com/weavejester/hiccup). If more designer-friendly HTML templates are needed, try [Enlive](https://github.com/cgrand/enlive).

## XML

Clojure's [data.xml](https://github.com/clojure/data.xml) library parses XML in such a way that Clojure core's `clojure.zip/xml-zip` can consume and turn into a zipper. From [Clojure's documentation](http://clojure.org/Other%20included%20Libraries-Zippers%20-%20Functional%20Tree%20Editing%20(clojure.zip)):

> A zipper is a data structure representing a location in a hierarchical data structure, and the path it took to get there. It provides down/up/left/right navigation, and localized functional 'editing', insertion and removal of nodes.

If you need more performant XML handling or greater flexibility, the Java standard library includes support for handling XML, including XPath querying and streaming API's. See the [official tutorial](https://docs.oracle.com/javase/tutorial/jaxp/index.html) for a starting point.
