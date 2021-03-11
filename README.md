# cloverage-experiment

lein cloverage @1.2.2 bug's report

## detail

isolated code is not watched by cloverage.

e.g. the namespace #'cloverage-experiment.error is the below code.

```clojure
(ns cloverage-experiment.utils.error)

(defn bind-error [f [val err]]
  (if (nil? err)
    (f val)
    [nil err]))

(defmacro err->> [val & fns]
  (let [fns (for [f fns] `(bind-error ~f))]
    `(->> [~val nil]
          ~@fns)))
```

this namespace is isolated because it has no requiements.

and then, the namespace #'cloverage-experiment.core is the below code.

```clojure
(ns cloverage-experiment.core
  (:require [clojure.core]) ;; need here code!!!
  )

(defn foo
  "I don't do a whole lot."
  [x]
  (println x "Hello, World!"))

(defn main []
  (print "hello"))
```

this namespace is not isolated because it requires #'clojure.core

so that, cloverage result is here.

```text
lein cloverage                                                                           ✘ 1
OpenJDK 64-Bit Server VM warning: Options -Xverify:none and -noverify were deprecated in JDK 13 and will likely be removed in a future release.
Loading namespaces:  (cloverage-experiment.core)
Test namespaces:  (cloverage-experiment.core-test)
Instrumented cloverage-experiment.core
Instrumented 1 namespaces in 0.0 seconds.

Testing cloverage-experiment.core-test

FAIL in (a-test) (core_test.clj:7)
FIXME, I fail.
expected: (= 0 1)
  actual: (not (= 0 1))

Ran 1 tests containing 1 assertions.
1 failures, 0 errors.
Ran tests.
Writing HTML report to: /run/media/meguru/P/Github/cloverage-experiment/target/coverage/index.html

|---------------------------+---------+---------|
|                 Namespace | % Forms | % Lines |
|---------------------------+---------+---------|
| cloverage-experiment.core |   30.00 |   60.00 |
|---------------------------+---------+---------|
|                 ALL FILES |   30.00 |   60.00 |
|---------------------------+---------+---------|
```
