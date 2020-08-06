
## AIT reference package (`ait-reference`)

This repository contains miscellaneous data sets which are used to assemble the `ait-reference` package. It contains general
purpose word lists, control lists, glossaries and abbreviations. It is a new package intended to consolidate into one dedicatєd
location all such reference data from previous incarnations of the AIT framework — in particular, the _Dragonfly project_,
the _Deutsch Bank PBSC factory_, and the _IBM Tahtum factory_. Reference data was scattered in multiple packages and often
redundantly hard coded in compiled software; for instance the command line `qotd` (_quote of the day_), where all quotes are
hardcoded in the C binary.

This is _work in progress_ where standardisation efforts still need thought and organisation — See the [strategy](Strategy.md)
article where related conclusions are written up. Reuse, maintainability and availability are the KPIs of this strategy.

In addition to consolidating existing reference data, we wish to extend this repository with new datasets providing world facts
and figures in various domains — see the [roadmap](Roadmap.md) article for the current wishlist. For instance, we have been
maintaining over more than a decade, a copy of the CIA's World Factbook; this has primarily been used to create _go to market_
country books. In the future, we would like to transform the exising factbooks into a set of YAML files managed within the
scope of this repository.

This **is not, and shall not be, a big data junk yard**. Information collected here is analysed, sorted and structured by
_humans_. That said, maintaining large sets of structured information requires time and effort. So all helping hands are more than
welcome. In return, if YAML is not the _lingua franca_ for such data, we are happy to maintain builds in alternate formats, be
they based on markup languages (e.g. JSON, XML, …) or custom binary formats.

<!-- vim: set digraph et nospell syn=md :-->
