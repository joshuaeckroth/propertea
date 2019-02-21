# propertea

*Simple property loading, coercing, and validating*


## An Example

propertea can be used to load a property file, convert, and validate. The
following snippet shows loading a file and converting a few of the properties
to their desired types.

Given the properties file `test/fake.properties` with the following contents:

```
string-example=hello-string
int-example=1
boolean-example=true
empty-string=
nested.example.depth=5
nested.example.leaves=2
nested.withCamelCase=get-dashed

```

```clj
(require '[propertea.core :as prop])

(prop/read-properties "test/fake.properties"
                      :as-int [:int-example]
                      :as-bool [:boolean-example])
```
```clj
{:int-example 1, :string-example "hello-string", :boolean-example true}
```

An input stream can be used instead of a filename. This is useful to read
property files in a Jar:

```clj
(prop/read-properties
  (.getResourceAsStream
    (.getContextClassLoader (Thread/currentThread))
    "some.properties"))
```

or

```clj
(require '[clojure.java.io :as io])

(-> (io/resource "some.properties")
    (io/input-stream)
    (prop/read-properties))
```

propertea can also validate that required properties are specified. Assuming
the same properties file as above:

```clj
(prop/read-properties "test/fake.properties" :required [:req-prop]))
```
```clj
java.lang.RuntimeException: (:req-prop) are required, but not found
```


## Installing

The easiest way is via Leiningen. Add the following dependency to your
`project.clj` file:

```clj
[clojusc/propertea "1.4.2"]
```

To build from source and install locally, run the following commands:

```
$ lein deps
$ lein jar
$ lein install
```


## Version History

| Release     | Clojure     | Maintainer     | Notes
| ----------- | ----------- | -------------- | ---------------------------------- |
| 1.5.0       | 1.10.0      | clojusc        | Backwards-compatible function changes; breaking arg changes |
| 1.4.2       | 1.10.0      | clojusc        | 100% compatible with 1.4.1         |
| 1.4.1       | 1.5.1       | Joshua Eckroth | Version from which clojusc forked  |
| 1.3.1       | 1.2.0       | Jay Fields     | Version from which Josh forked     |


## License

Copyright © 2010-2012, Jay Fields

Copyright © 2013-2014, Joshua Eckroth

Copyright © 2019, Clojure-Aided Enrichment Center

All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

* Neither the name Jay Fields nor the names of the contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
