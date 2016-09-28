GitHub and Travis
================
September 27, 2016

GitHub
------

The standard way in the `R` community to distribute is through CRAN, the Comprehensive R Archive Network.

More recently, other solutions are being used by the community, especially for either code in development or for packages that violate some of the guidelines imposed by CRAN. GitHub is the most popular alternative. GitHub is a web-based Git repository hosting service. It is oriented towards `git` and a number of collaboration features such as bug tracking, feature requests, task management, and wikis for every project. It is *very* popular in the open source community to host both code and documentation.

### Travis

Travis CI is "a hosted, distributed continuous integration service used to build and test software projects hosted at GitHub." So, what is Continuous Integration anyway? Continuous Integration is a development practice that relies heavily on tests and test-driven development to integrate new code to the production codebase. A full CI is not something we will need in our day-to-day operations. However, the CI tools can be very useful for projects that are not pushed to CRAN (because they are not ready to be share or because they cannot be shared). However, CI systems can help us. Every time we make a new submission to CRAN, it runs a `R CMD check` on our package on a fresh environment. A tool like Travis CI allows us to emulate that behavior. Of course, we can do it manually in our local machine, but it can be very time consuming. Travis CI automatizes that process for us by running `R CMD check` automatically ever time we make a new commit. Note that `R` is not officially supported.

Travis CI provides a default build environment and a default set of steps for each programming language. The steps in the build process are customized in the `.travis.yml` (a file in the [YAML](https://en.wikipedia.org/wiki/YAML) format). Travis CI uses `.travis.yml` in the root of the repository. With every commit, Travis will try to:

1.  Install any dependencies in a fresh environment. As of today, Travis only build on Linux and macOS.
2.  Build the package.

For instance, we can specify a `.travis.yml` as simple as this

    language: r
    warnings_are_errors: false

It will notify you with the outcome of the build.
