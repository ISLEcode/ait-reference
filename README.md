![](https://avatars0.githubusercontent.com/u/20255067?s=400&u=67ee0ca71c13b0c52d93c1d5f4b08b47ffd9a5c7&v=4)

## AIT reference package (`ait-reference`)

### About

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

### Build & installation

Our build is based on GNU Autotools. We practice _out of source_ builds in a subdirectory named, by convention, `out`; this
essentially means that all actions, beyond the first step below, are performed in the aforementioned subdirectory.

1.  Initial checkout from GitHub and re-generation of GNU Autotools base scripts:

    ``` {.sh}
    git clone https://github.com/ISLEcode/ait-reference
    cd ait-reference
    ./aitbuild prepare
    ```

1.  Since this is a data-only package we can directly perform an installation

    ``` {.sh}
    ./aitbuild install
    ```

If you are a package maintainer the following tasks may apply.

-   Before commiting back the master branch, lint the distribution as follows:

    ``` {.sh}
    ./aitbuild check
    ```

    If all goes well you should have a distribution tarball in the `out` subdirectory. You coulud also create tarballs using
    specific archive formats and compression schemes — e.g. `make dist-gzip` or `make dist-bzip2`; this however is irrelevant in
    our case since we use GitHub's release mechanisme which automatically creates source code tarballs.

-   Building a binary distribution has not yet been automated, this has to be done manually — we assume here we are building
    a binary tarball to be added to a new GitHub release and that the released version is _1.0.0_. All shell commands and scripts
    are assumed to run under the Kornshell.

    1.  Create the tarball distribution tarball — a.k.a. _binary tarball_, even though this package only contains data.

        ``` {.sh}
        ./aitbuild distrib
        ```

    1.  Either from the command line or from a GitHub-capable frontend, create a new GitHub release tagged `v1.0.0`, collect
        input from the `ChangeLog` file, and append the previously created distribution tarball.

    1.  The build is not fully _out of source_ and some GNU build-time files make their way into the source tree, at the top
        level with configure and m4 temporary files and scripts, and in sub-directories with transient `Makefile.in` files.
        These are excluded from revision control through `.gitignore`. Nonetheless, cleaning up your source tree is good practice.
        This hasn't yet been automated, so here are the steps, from the repository's top-level directory.

        ``` {.sh}
        ./aitbuild clean
        ```

<!-- vim: set digraph et nospell syn=md :-->
