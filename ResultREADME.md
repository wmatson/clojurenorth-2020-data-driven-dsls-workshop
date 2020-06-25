# `edn-card-executor`

`edn-card-executor` is a Clojure(script) library for managing state in card games along the vein of [Dominion](https://en.wikipedia.org/wiki/Dominion_(card_game) or [MTG](https://en.wikipedia.org/wiki/Magic:_The_Gathering).

## Quick Start

```clojure
  (require '[edn-card-executor.core :as ece])
  (require '[edn-card-executor.helpers :as ece-helpers])
  (require '[edn-card-executor.text-gui :refer [swing-text-gui]])

  (defn money-gain [amount]
    {:ece-helpers/amount 1
     :ece-helpers/resource :dominion/money
     :ece/target :ece/current-player})

  (def cards [{:ece/id :dominion/copper
               :ece/effects [{:ece-helpers/add (money-gain 1)}]}
              {:ece/id :dominion/estate
               :ece/effects []
               :ece/data {:dominion/victory-points 1}
              {:ece/id :dominion/smithy
               :ece/effects [{:ece-helpers/draw {:ece/target :ece/current-player}}]
               :ece/data {:dominion/victory-points 1}}
              {:ece/id :wmatson.dominion/mega-curse
               :ece/effects []
               :ece/data {:dominion/victory-points -5}}
              {:ece/id :wmatson.dominion/multiplying-money
               :ece/effects [{:wmatson.dominion.effects/multiplying-money []}]}])

  (def ^:private money-multiplier (atom 0))

  (def custom-effects
      [{:ece/id :wmatson.dominion.effects/multiplying-money
        :handler (fn [world]
                    (update world :to-resolve conj {:ece-helpers/add (money-gain (swap! money-multiplier inc))}))}])

  (def game
    {:ece/effect-definitions (concat ece-helpers/basic-defaults custom-effects)
     :ece/card-definitions cards
     :ece/on-game-reset   #(reset! money-multiplier 0)
     :ece/turn-management :ece-helpers/standard-clockwise-cycle
     :ece/end-conditions :ece-helpers/draw-pile-empty})

  (ece/run! {:ece/game game
             :ece/ui swing-text-gui})
```


## Limitations / Future Work

[ what would you like to have added or changed in your design? ]
[ any particular challenges? ]
[ was a data-driven approach suitable for this problem? ]
[ any of the reflection questions particularly relevant? ]


