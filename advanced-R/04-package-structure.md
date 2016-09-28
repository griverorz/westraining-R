Package structure
================
September 28, 2016

The minimal package structure requires:

1.  An `R/` directory in which the R code is stored.
2.  A `DESCRIPTION` file that contains the package metadata
3.  A `NAMESPACE` file with the namespaces.

The `DESCRIPTION` file looks like:

    Package: mypackage
    Title: What The Package Does (one line, title case required)
    Version: 0.1
    Authors@R: person("First", "Last", email = "first.last@example.com",
                      role = c("aut", "cre"))
    Description: What the package does (one paragraph)
    Depends: R (>= 3.1.0)
    License: What license is it under?
    LazyData: true

It also includes the list of packages your package needs to work:

1.  `Imports` lists packages that must be present and will be installed when your package is installed. However these packages will not be attached along with your package. `Depends` does the same thing but only *loads* the package.

2.  `Suggests` lists packages that are not required and they are not automatically installed.

You can specify the version of the package through the usual versioning system:

    Imports:
        ggvis (>= 0.2),
        dplyr (>= 0.3.0.1)
    Suggests:
        MASS (>= 7.3.0)

Packages are expected to have a license with them, commonly MIT or GPL-2 or GP3. In our case, we may consider using a proprietary licence in a separate file:

    License: file LICENSE

The `NAMESPACE` file is useful to deal with disambiguation of function names and to avoid name conflicts. Think about how many packages provide similarly named functions. The `NAMESPACE` defines `imports` and `exports`, but it is better to delegate the creation of the `NAMESPACE` file to `roxygen2` (the documentation tool).

To call a function, R first has to find it. R does this by first looking in the global environment. If R doesn’t find it there, it looks in the search path, the list of all the packages you have attached.

If a package is installed,

-   Loading will load code, data and any DLLs; register S3 and S4 methods; and run the `.onLoad()` function. After loading, the package is available in memory, but it's not in the search path and won’t be able to access its components without using `::`.

-   Attaching puts the package in the search path. You can’t attach a package without first loading it.

Never use `library` or `require` in a package. Use `Imports` in `DESCRIPTION`.

The directives that are used in `NAMESPACE` are:

1.  For exporting:

    -   `export()`: export functions (including S3 and S4 generics).

    -   `exportClasses()`, `exportMethods()`: export S4 classes and methods.

2.  For importing:

    -   `import()`: import all functions from a package.

    -   `importFrom()`: import selected functions (including S4 generics).

    -   `importClassesFrom()`, `importMethodsFrom()`: import S4 classes and methods.
