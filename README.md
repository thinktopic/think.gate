# thinktopic/gate

Gate is an opinitated library for hacking wepages into your clojure app.

## Usage

In your app's **project.clj**:

```
;; depend on
[thinktopic/gate "0.1.0"]

;; add
:cljsbuild {:builds
	     [{:id "dev"
                :figwheel true
                :source-paths ["src/cljs"]
                :compiler {:main "your.cljs.ns"
                           :asset-path "out"
                           :output-to "resources/public/js/app.js"
                           :output-dir "resources/public/out"}}]}

```

---

In your app's **clojure**:

```
[think.gate.core :as gate]

;; ...

(defn on-foo
  [params]
  (+ (:a params) 41))

(def routing-map
  {"foo" #'on-foo})

(defn gate
  []
  (gate/open #'routing-map))

```

---

In your app's **clojurescript** (i.e., under `src/cljs/your/cljs/ns.cljs`):

```
(ns your.cljs.ns
  (:require-macros [cljs.core.async.macros :refer [go]])
  (:require [cljs.core.async :as async :refer [<!]]
            [reagent.core :refer [atom]]
            [think.gate.core :as gate]
            [think.gate.model :as model]))

(enable-console-print!)

(def state* (atom nil))

(defn interactive-component
  []
  (fn []
    (if-let [n @state*]
      [:div "Server's answer: " n]
      [:button {:on-click #(go (reset! state* (<! (model/put "foo" {:a 1})))
                               (println @state*))}
       "GO"])))

(think.gate.core/set-component [interactive-component])
```

## License

Copyright © 2016 ThinkTopic, LLC.