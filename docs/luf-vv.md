# LUF-VV: Framework for Validation and Verification of Lua Implementations

A couple of notes to the reader. This document is an early draft, so please consider following:

* Predicated *must*, *should* and *may* are not strictly defined in the text, please interpret them in a typical RFC’ish sense.
* Terms *implementation-defined behavior*, *unspecified behavior* and *undefined behavior* are also used throughout the text without exact definition, please interpret them close to the C Standard usage.
* Search the document for **FIXME:** to find and fix gaps in the text.
* Search the document for **NB!** to find emphasized paragraphs.

# Introduction and Target Audience

This document describes a framework and accompanying infrastructure for validation and verification of implementations of the Lua language (codename LUF-VV). The document per se outlines the requirements for building LUF-VV. The target audience of LUF-VV are:

* Developers (both individuals and organizations) who build software using Lua and feel need in providing validation and verification of their underlying platform.
* Developers (both individuals and organizations) who build implementations of the Lua language

## Rationale for Building LUF-VV

**FIXME:** Elaborate on this.

# LUF-VV Core Requirements

## Development Model

LUF-VV is an open-source project licensed under MIT license. All project sources must be kept in a public repository with a publicly available issue tracker for submitting bugs and requests. Documentation for the project should be licensed under the CC BY-SA license.

## Supported Lua Versions

LUF-VV must support following versions of the language:

* 5.1
* 5.2
* 5.3

**FIXME:** process to change this list (and other musts)

**NB!** Although Lua 5.1 was released more than 10 years ago, it is still quite popular; besides, LuaJIT 2.0, widely used implementation of Lua, is primarily compliant with Lua 5.1.

## Supported Run Modes

LUF-VV must support following modes:

* CLI mode: Tested Lua implementation is built as a stand-alone binary which is used for running tests.
* Library mode: Tested Lua implementation is built as a library which is embedded into tests for subsequent running.

There is absolutely no requirement that each test in LUF-VV supports all modes listed above, but once tested Lua implementation formally specifies its build configuration via some protocol (**FIXME:** elaborate), all parts of the LUF-VV infrastructure should act accordingly building and using for testing appropriate binary artifacts provided by the tested implementation.

## Supported Hardware Platforms and Operating Systems

The final goal for LUF-VV is to support not less hardware platforms and operating system than supported by the reference implementation, targets that must be supported from the start are:

* Linux, x86-64

# LUF-VV Features

## Annotated Reference Manual (ARefMan)

Current official Reference Manual fully and adequately describes the language. However, for providing more help in building and ensuring compilance of custom Lua implementations, LUF-VV should provide annotated Reference Manual for each supported version of the language. The Annotated Reference Manual is the vanilla Reference Manual plus a set of annotations which correct and clarify the original text with a special emphasis on:

* Typos
* Semantic errors
* Outlining implementation-defined / unspecified / undefined behavior, where the original text is not concrete enough

### ARefMan Maintenance

* Each version of the ARefMan must be annotated with the respective version of the language and the timestamp of the last changes.
* All maintained versions of the ARefMan must be regularly published automatically from their sources.
* The published versions of the ARefMan must be accessible on-line at some convenient place.

The ARefMan must be kept in sync with the original text. Thus there must be established a regular review procedure during which all changes from the original text are transferred to the ARefMan. This procedure must be conducted by the ARefMan maintainer on regular basis and should preferably be semiautomatic consisting of following steps:

* Automatic syncing with the original text (think about merging the upstream into your local repository in terms of git)
* Manual review of the result and committing the changes to the ARefMan repository

### Bugs in the ARefMan

Bugs in the ARefMan should be submitted through the LUF-VV repository. All bug reports covering the original text are out of scope of ARefMan responsibility, but should be reported to the upstream by the ARefMan maintainer.

### Pushing Annotations from the ARefMan to the Original Text

**FIXME:** This is a nice feature, but a process should be described here.

## Test Suites

LUF-VV offers tests of several types:

* Compliance testing
* Testing using real-world applications

Each type of testing is represented by its own independent suite which are described in details.

**NB!** 1:1 mapping `type of testing` ⇔ `suite` is not a final decision and has to be thoroughly discussed. The main problem here is that the existing Lua ecosystem already provides some sources for testing, but these sources are highly heterogeneous: There is no unified runner, no unified output protocol etc. That said, there is a temptation to bundle several suites, equip them with a thin integration layer and ship together as a single “meta-site”. This approach allows to increase the number of tests rather quickly. On the other hand, though, this approach adds less value in long term: Developing a coherent suite from the start requires some additional efforts (plus, in future, there will be efforts required for adopting tests from the other suites), but gives an ability to build things consistently and provide better integration facilities.

### Compliance Suite

The goal of this suite is to ensure compliance with the Reference Manual.

**Status quo.** The suite shipped with the [lua-TestMore](https://fperrad.github.io/lua-TestMore/) module can be used as a basis for building the compliance suite for LUF-VV. However, following issues should be mitigated:

* The code should be checked to clean out implementation-dependent tests.
* Currently tests from the suite create temporary files in unportable manner, which makes it impossible to run the suite in parallel.

On top of that, there are other suites available:

* [The official test suite for PUC-Rio interpreter](https://www.lua.org/tests/)
* [Mike Pall's suite for LuaJIT](https://github.com/LuaJIT/LuaJIT-test-cleanup)
* [Test suite for CERN's MAD project](https://github.com/MethodicalAcceleratorDesign/MAD/tree/dev/tests), yet this one contains lots of code with non-standard syntax extensions

These suites are built for concrete implementations, but their portions may be adopted in the future to become a part of the LUF-VV compliance suite.

## Suite of Real-World Applications

**FIXME:** Elaborate on this section.

The idea behind the suite is to assemble as many real-world applications / libraries as possible and run them with some data using the tested Lua implementation.

**Status quo.** This is one of the most challenging parts of the project. Following issues should be addressed:

* Establish a “proof of concept” set of applications and libraries. Initially it will most likely be some open source projects, but they must be equipped with adequate data sets to ensure that running this code gives any reasonable outcome.
* We should take into account and mitigate any differences in the build chains of the applications. Looks like a Nix ecosystem is the solution here, but any research for alternatives is welcome, if deemed necessary.
* Beyond coding, there should be established some process to constantly extend this suite. This process should include solving legal questions, even for open-source projects (to avoid GPL-poisoning, for example).
* Something should be done about closed-source projects. There is obvious that some code will never be donated to an open project like the one described. And the truth is, the most part of this closed-source code would be important for testing. One of the ideas here would be to build a protocol / API so that a closed-source project could be plugged into a privately-hosted LUF-VV infrastructure together with a tested Lua implementation. After that, the whole machinery could be built and run, and the result reported to all relevant parties without disclosing any sensitive information.

### Managing and Extending Suites

Following process should be adopted:

* All code related to test suites must be a part of the LUF-VV source tree. However, closed-source application (see above) used for real-world testing is an exception and might be treated separately.
* If any suite is extended with a 3rd-party code, there should be no legal issues preventing from including the code into the source tree.
* All newly added code must undergo adoption procedure:
  * It must be reviewed
  * For compliance tests, non-standard / non-portable / implementation-dependent behavior must be cleaned. **NB!** It is not clear how we should treat real-world code that relies on custom extensions. Should this code be rejected? Should we shape test data in such a manner that will allow us to actually skip executing non-standard code?

### Reporting Bugs in the Suites

Bugs are reported via LUF-VV repository.

# Exploitation

**FIXME:** Elaborate on this section.

LUF-VV must be usable in both public and private infrastructure. Exploitation in public infrastructure means that there must be an option to book computational resources in the infrastructure operated by LUF-VV and run all available tests there. Exploitation in private infrastructure means that there must be an option to deploy LUF-VV sources in some internal environment and all tests inside it. Regardless of the infrastructure type, prior to running, following configuration information must be submitted to LUF-VV in a machine-readable way (**FIXME:** Protocol to be developed / borrowed from existing CI solutions):

* How to build the tested implementation
* Location of deliverables (stand-alone executables, libraries for embedding, etc.)
* Tested hardware platforms and OSes
* Tested versions of the language

There must be following features available (heavily inspired by Perl’s [prove](https://perldoc.perl.org/prove.html) utility and [TAP](https://en.wikipedia.org/wiki/Test_Anything_Protocol) protocol):

* Reproducible runs
* Re-running of failed tests only
* Running certain tests inside suites
* (nice to have) Shuffled run order
* Parallel test run
