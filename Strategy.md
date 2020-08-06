## Repository management strategy

### Introduction

Іt's been a (very) long time since the reference lists used over the years have been checked and consolidated.
This is what has started here. The reorganisation strategy is detailed hereafter.

### Top-level directories

The repository will be organised as a regular source code repository with the following top-level directories:

`_in`
:   The conventional directory name for locally stored data which is ignored in the Git-based version control workflow.

`bin`
:   Store for dedicated build-time utilities used to download/extract/convert datasets and reference material.

`src`
:   Store for hand-maintained datasets and/or for datasets which are _spooled_ through this repository — that is datasets
    which cannot be auto-magically generated with no human interaction.

`out`
:   The conventional directory used at build-time to spool build intermediate and final products. This directory is not
    under version control and should never be part of distributed tarballs.

`wip`
:   This _work in progess_ repository should no exist in the master branch. It is however recommended to use this directory
    in feature branches to collect input material that needs to be further _digested_ before migrating it to the _src_
    directory. In such cases, files and folders should be placed in a sub-directory whose name is the name of the branch.

### Collections

The datasets will be organised in collections. The following collections are foreseen:

**acronyms**
:   Acronyms, initialisms, and abbreviations of all kinds are maintained here as simple and ready to use YAML files
    essentially consisting of the term and its definition; when used in conjunction with dictionary, the dataset's
    front matter should indicated the bound dataset and each term should have the necessary cross-referencing code
    appended to its definition.

**clists** (or _controlled lists_)
:   Controlled lists are used as building blocks of a broader taxonomy which is represented by this repository as a whole.
    Each controlled list representes a normalised concept or series; most of which are pretty obvious. Where ever possible,
    when standardised material exists — in particular those maintained by official organisations; the we should do our best
    to abide to those ѕtandards.

    Note: the rich collection of such lists created circa 2003 for Interwage's international payroll and expatriate management
    system have been passed over to the SAMinfo ERP except for generic lists such as countries, currencies and timeuzones. My
    inted however is to bring back to this repository what could be classified as _lorem ipsum_ datasets such as abstract
    contact names, addresses and phone numbers which are useful at design and testing phases.

**dictionaries** (abbreviated as _dict_)
:   Reference lists of terms and their definitions organised into collections such as ITIL, computer science, or general
    knowledge dictionaries. Are also included in this category the word lists of spoken languages used by spell checking
    software, such as _aspell(1)_.

    Note: in previous incarnations of the AIT framework — the _Dragonfly project_, the _Deutsch Bank PBSC factory_, or the
    _IBM Tahtum factory_, we used to have `etc/lists` and `etc/wlists` directories for various word lists. Such lists, that
    can't be classified in other categories, will be placed here.

**quotes**
:   Quotes have been managed so far within the context of each AIT tool, such as _cm-qotd(1)_ — the _quote of the day_ command
    line utility. These will progressively be extracted and collected here and organised into languages and topics. There are
    a lot of online websites managing quotes; we should see how to keep this category lean.

### URI schemes

All collections will be accessible through URI schemes that will be known and handled by the related AIT utilities, in particular
in Markdown documents and their various transformations to other formats (e.g. HTML, LaTeX and office formats, PDF, ...). The
general syntax for such URIs will:

-   Use the singular form of the collection name, or a recognisable abbreviation, as the protocol — `abbr` for acronyms and
    abbreviations, `list` for controlled lists, `word`^[The _dict_ protocol is already defined.] for dictionaries, and `quote`
    for quotes.

-   The domain name will represent the path to the YAML file without its extension — hence using the dot (`.`) as the path
    separator; for instance `dict://itil/SLA`, to lookup the _SLA_ term in the _itil_ dictionary. If the `itil.yaml` file
    were in the `informatics` sub-directory, then we would have `dict://informatics.itil/SLA`. As for regular domain names,
    the case is non-sensitive, and target YAML file names are expected to only be in lowercase.

-   The term to lookup is the last element of the URI — `SLA` in our previous example. The term's case responds to
    typographical conventions only and can be mixed case; however the lookup will be case insensitive and all successive
    non-alphanumeric characters will be converted to a single dash (`-`) character.

-   The bookmark syntax of URIs will be used to introduce _verbs_ either generic to all collections — `dict://itil#random`,
    or specific to a given collection `quote://english#motivational`. Such verbs should be simple and unambiguous.

-   The query syntax is reserved for subsequent introduction of a REST-like interface, if not to update, at least to perform
    extended querying of the collection's content.

### Unique identifiers

All content portions of this repository should be uniquely identified. Conventional UUIDs are overkill, and certainly too long for
our purposes. We will use a simplier algorithm to generate a 10 character id which has proved sufficient in multi user
environments for several decades — to be noted though that all users of this algorithm were aware of its limitations.

The algorithm is simple: convert the server's UNIX time at GMT+0 coordinates into a number expressed in base 36. The output
is a 10 _digit_ string. For instance  `udx81e2x5v` uniquely matches to `glossary://informatics/absorption-chillers`.

In Kornshell, the implementation of this algorithm with the AIT utility `dt-microsec` is straightforward:

``` {.sh .ksh}
typeset time=$(dt-microsec)             # Collect UNIX time in microseconds (e.g. 1596719025.096476)
integer -i36 x=${time%.*} y=${time#*.}  # Convert decimal and fraction to base 36
typeset uuid=${x#*#}${y#*#}             # Concatenate integers into a single string (e.g. qen8a922fw)
```

Alternatively to the `dt-microsec` utility, the following C language snippet builds and runs easily:

``` {.clang}
#include <stdio.h>
#include <time.h>
#include <sys/time.h>

double timevaltosecs (struct timeval *t) {
    double r;

    r = (t->tv_usec * .000001) + t->tv_sec;
    return r;

}

int main (int argc, char **argv) {
    double microsecs;
    struct timeval microtimeval;

    gettimeofday (&microtimeval, NULL);
    microsecs = timevaltosecs (&microtimeval);
    printf ("%f\n", microsecs);
    return 0;

}
```

### Filesystem layout

In some cases a same dataset could exist in multiple files or in a single file. At the time of writing we have two glossaries of
approximately the same character size which use two different approaches. The _informatics_ glossary is divided in one file per
alphabet character. The _investment_ glossary has all terms in a same file. We will support the following lookup strategies:

1.  By default a dataset named _dataset_, respectively _path/to/dataset_, will be assumed to live in the file `dataset.yaml`,
    respectively `path/to/dataset.yaml`.

2.  If the above file doesn't exist but a file named `dataset-a.yaml` exists, than the file to use will be determine from the
    lowercased first character of the term under scrutiny; for instance `dataset-s.yaml` if we are looking for _SLA_. If the
    first character is not an alphabetical character, we assume that character to be zero and we will consider the file named
    `dataset-0.yaml`.

3.  As a convenience for mid-sized datasets that are too big to fit in a single file, but too small to be managed in 26
    different files, we allow intermediate groupings of five alphabetical characters: \[a..e\], \[f..j\], \[k..o\], \[p..t\],
    and \[u..\z]; non-alphabetical characters belong to the last group. Existence of the scheme is verified by checking the
    existence of the `dataset-ae.yaml` file, in which case the _SLA_ term would be looked up in the `dataset-pt.yaml` file.

4.  For large collections, or for collections where there is a lot of data attached to each elementary content portion, it
    may be of interest to maintain one file for each such content portion. The three above schemes are then applied in
    sequence, replacing the suffixed component by a prefixed sub-directory. We add a fourth scheme which combines the last
    two themes; this would be used for very large datasets:

    a)  Search for _SLA_ in `dataset/sla.yaml`      if `đataset` is a directory
    b)  Search for _SLA_ in `dataset/s/sla.yaml`    if `dataset/a` is a directory
    c)  Search for _SLA_ in `dataset/pt/sla.yaml`   if `dataset/ae` is a directory
    d)  Search for _SLA_ in `dataset/pt/s/sla.yaml` if `dataset/ae/a` is a directory

5.  All schemes should check for alternate file extensions.

    Plain YAML files should have, by convention, the `yaml` extension. However the `yml` extension is authorised and can be
    used in lieu of the longer extension.

    The dataset can optionally be compressed, and when split into multiple files, it can also be packaged into a UNIX archive
    file. By default we assume GNU's `gzip` utility to handle compression, and the UNIX `tar` utility to manipulate archives;
    however all extensions known to the AIT `fs ar` utility are authorised -- in theory 50+ formats.

    File-based compression is simple to handle and the associated schemes are recognised without sophisticated processing:

    1. `dataset.yaml.gz`
    2. `dataset-s.yaml.gz`
    3. `dataset-pt.yaml.gz`
    4. `dataset/sla.yaml.gz`
    5. `dataset/s/sla.yaml.gz`
    6. `dataset/pt/sla.yaml.gz`
    7. `dataset/pt/s/sla.yaml.gz`

    When using UNIX archives, we assume only one archive per dataset: `dataset.tar`, possibly compressed `dataset.tar.gz`.
    Named likewise, pre-processing will be required to determine what scheme is used within the archive. To limit this we
    can suffix the dataset's name with the digit in the previous list which represents the archive's file layout:

      | Archive            |   | Associated scheme          |
      | ------------------ | - | -------------------------- |
      | `dataset-1.tar.gz` | → | `dataset.yaml`             |
      | `dataset-2.tar.gz` | → | `dataset-s.yaml`           |
      | `dataset-3.tar.gz` | → | `dataset-pt.yaml`          |
      | `dataset-4.tar.gz` | → | `dataset/sla.yaml`         |
      | `dataset-5.tar.gz` | → | `dataset/s/sla.yaml`       |
      | `dataset-6.tar.gz` | → | `dataset/pt/sla.yaml`      |
      | `dataset-7.tar.gz` | → | `dataset/pt/s/sla.yaml`    |

6.  All above schemes assume alphanumeric terms primarily starting with alphabetical characters. This is the common case we
    have experienced so far. Should we required additional schemes allowing to distribute primarily numerical data into
    multiple files, then this document should be updated accordingly.

The above schemes apply to both the source directory in this repository and the target build directories; the source and
target schemes can be different. It may be easier to maintain the dataset when it is split into multiple files, and more
performant for the target dataset to be concatenated into a single, possibly compressed, file.

<!-- vim: set digraph et nospell syn=md :-->
