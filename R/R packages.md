# R packages

A sample package developed by the group: https://github.com/zhentaoshi/fsPDA/tree/master/R_pkg_fsPDA

Summary notes of the book [R Packages](https://r-pkgs.org/index.html).

### Introduction (Ch. 1)

- Why packages?
  - Sharing your code
  - Organising code in a package makes your life easier because packages come with conventions
  - It’s even possible to use packages to structure your data analyses (Marwick, Boettiger, and Mullen [2018](https://r-pkgs.org/release.html#ref-marwick2018-peerj)[b](https://r-pkgs.org/release.html#ref-marwick2018-peerj))
- Environment and tools: Rstudio and `devtools`

### System Setup (Ch. 3)

```R
install.packages(c("devtools", "roxygen2", "testthat", "knitr"))
library(devtools)
library(testthat)
library(roxygen2)

devtools::has_devel() # Check R build toolchain
```

### Initiate the package (Ch. 2)

```R
create_package("~/path/to/package")
use_git()
use_r("file_name") # The helper use_r() creates and/or opens a script below R/.
load_all() # make functions available for experimentation
		   # It simulates the process of building, installing, and attaching the package
check()
```

- The `DESCRIPTION` file (provides metadata about your package). You may want to start with a brief description and author names.

```R
use_mit_license("your_name") # add a license 
```

- Documentation

  - We write a specially formatted comment right above `foo()`, in its source file, and then let a package called [roxygen2](https://roxygen2.r-lib.org/) handle the creation of `man/foo.Rd`.

  - Example:

  - ```R
    #' Bind two factors
    #'
    #' Create a new factor from two existing factors, where the new factor's levels
    #' are the union of the levels of the input factors.
    #'
    #' @param a factor
    #' @param b factor
    #'
    #' @return factor
    #' @export
    #' @examples
    #' fbind(iris$Species[c(1, 51, 101)], PlantGrowth$group[c(1, 11, 21)])
    ```

  - then run `document()`

- Installation

```R
install()
```

- Unit test

```R
use_testthat() #declare our intent to write unit tests and to use the testthat package
use_test("function_name") # opens and/or creates a test file
test() # Ctrl + Shift + T
```

-  Import functions from the namespace of other packages

```R
use_package("forcats")
#> ✔ Adding 'forcats' to Imports field in DESCRIPTION
#> • Refer to functions with `forcats::fun()`
```

- Github and readme

```
use_github()
use_readme_rmd()
build_readme()
```

### Package state and workflows (Ch. 4 and 5)

- 5 states an R package can be in:

  - source - a directory of files with a specific structure
  - bundled - a package that’s been compressed into a single file
  - binary - a single file for an R user who doesn’t have package development tools
    - `devtools::build(binary = TRUE)`
  - installed - a binary package that’s been decompressed into a package library
    - `devtools` exposes a family of `install_*()` functions to address some needs beyond the reach of `install.packages()` or to make existing capabilities easier to access
  - in-memory
    - `library()` for installed packages v.s. `load_all()` for developing packages

- Workflows

  - Create a package

    - survey the existing landscape
    - Name your package

    ```R
    library(available)
    available("package_name")
    ```

    - `usethis::create_package()`

  - RStudio Projects + Rstudio

  - Working directory and filepath discipline

    - We *strongly recommend* that you leave the working directory of your R process set to the top-level of the source package.

  - Test drive with `load_all()` - “lather, rinse, repeat” cycle

    - Tweak a function definition.
    - `load_all()`  (Cmd+Shift+L (macOS), Ctrl+Shift+L (Windows, Linux))
    - Try out the change by running a small example or some tests.

- `.Rbuildignore` example

  ```R
  ^.*\.Rproj$         # Designates the directory as an RStudio Project
  ^\.Rproj\.user$     # Used by RStudio for temporary files
  ^README\.Rmd$       # An Rmd file used to generate README.md
  ^LICENSE\.md$       # Full text of the license
  ^cran-comments\.md$ # Comments for CRAN submission
  ^\.travis\.yml$     # Used by Travis-CI for continuous integration testing
  ^data-raw$          # Code used to create data included in the package
  ^pkgdown$           # Resources used for the package website
  ^_pkgdown\.yml$     # Configuration info for the package website
  ^\.github$          # Contributing guidelines, CoC, issue templates, etc.
  ```

  

## Package Components

### R code (Ch. 7)

- Organize functions into files
  - don’t put all functions into one file and don’t put each function into its own separate file
  - A single `.R` file will contain multiple function definitions: such as a main function and its supporting helpers, a family of related functions, or some combination of the two.
  - `R/utils.R`
- Fast feedback via `load_all()`
- Code style
  - use the package `styler`
  - `styler::style_pkg()` restyles an entire R package.
  - `styler::style_dir()` restyles all files in a directory.
  - `usethis::use_tidy_style()` is wrapper that applies one of the above functions depending on whether the current project is an R package or not.
  - `styler::style_file()` restyles a single file.
  - `styler::style_text()` restyles a character vector.
- Understand when code is executed
  - the code in a package is run **when the package is built**.
    - This has big implications for how you write the code below `R/`: package code should only create objects, the vast majority of which will be functions.
  - Any R code outside of a function is suspicious and should be carefully reviewed
  - your functions need to be thoughtful in the way that they interact with the outside world.
- Respect the R landscape
  - R landscape, which includes not just the available functions and objects, but all the global settings.
  - **Don’t use `library()` or `require()`**.
  - **Never use `source()`**
  - Here is a non-exhaustive list of other functions that should be used with caution:
    - `options()`
    - `par()`
    - `setwd()`
    - `Sys.setenv()`
    - `Sys.setlocale()`
    - `set.seed()` (or anything that changes the state of the random number generator)
- Constant health checks
  - Here is a typical sequence of calls when using devtools for package development:
    1. Edit one or more files below `R/`.
    2. `document()` (if you’ve made any changes that impact help files or NAMESPACE)
    3. `load_all()`
    4. Run some examples interactively.
    5. `test()` (or `test_file()`)
    6. `check()`

### Metadata (Ch. 8, 9)

- `DESCRIPTION` uses a simple file format called DCF, the Debian control format

```R
Package: mypackage
Title: What The Package Does (one line, title case required)
# always use . to separate version numbers. Format: <major>.<minor>.<patch>
# An in-development package has a fourth component: the development version.
Version: 0.1
Authors@R: c(
    person("Hadley", "Wickham", email = "hadley@rstudio.com", role = c("aut", "cre")),
    person("Winston", "Chang", email = "winston@rstudio.com", role = "aut"))
# (Description) Each line must be no more than 80 characters wide. Indent subsequent lines with 4 spaces.
Description: The description of a package is usually long,
    spanning multiple lines. The second and subsequent lines
    should be indented, usually with four spaces.
# Dependency
# Recommend putting one package on each line, and keeping them in alphabetical order
# Unless there is a good reason otherwise, you should always list packages in Imports not Depends.
# (Depends) It attaches the packages
Depends: 
	R (>= 3.1.0)
# (Imports) packages listed here must be present for your package to work
# Does not mean it will be attached (it only loads the packages)
Imports:
    pkg1 (>= 0.2), # versioning the package required
    pkg2
# (Suggests) your package can use these packages, but doesn’t require them
# use requireNamespace("Pkg", quietly = TRUE) before use it
Suggests:
    pkg3,
    pkg4
URL: https://yihui.name/knitr/
BugReports: https://github.com/yihui/knitr/issues
# use_mit_license(). Details of other license: check ch. 9.
License: MIT
# If TRUE, the data won’t occupy any memory until you use them
LazyData: true
```

### Object documentation (Ch. 10)

- Make use of `roxygen2`

  - As well as generating `.Rd` files, roxygen2 can also manage your `NAMESPACE` and the `Collate` field in `DESCRIPTION`.

- The documentation workflow

  1. Add roxygen comments to your `.R` files.

     ```R
     #' (Title) Add together two numbers 
     #' 
     #' (Description) blah blah blah
     #'
     #' (Details) long blah blah
     #'
     #' @section Warning:
     #' (you can add arbitrary sections to the documentation with the @section tag.  
     #' This is a useful way of breaking a long details section into multiple chunks 
     #' with useful headings)Do not operate heavy machinery within 8 hours of using
     #' this function.
     #'
     #' @family aggregate functions
     #' @seealso \code{\link{prod}} for products, \code{\link{cumsum}} for cumulative
     #'   sums, and \code{\link{colSums}}/\code{\link{rowSums}} marginal sums over
     #'   high-dimensional arrays.
     #' @aliases alias1 alias2
     #'
     #' @param x A number.
     #' @param y A number.
     #' @return The sum of \code{x} and \code{y}.
     #' @examples
     #' add(1, 1)
     #' add(10, 1)
     #' \dontrun{
     #' sum("a")
     #' }
     add <- function(x, y) {
       x + y
     }
     ```

     - All the roxygen lines preceding a function are called a **block**.

     - Blocks are broken up into **tags**, which look like `@tagName details`. (you need to write `@@` if you want to add a literal `@`)

     - You can do that automatically in Rstudio with Ctrl/Cmd + Shift + / (or from the menu, code | re-flow comment).

     - `@seealso` allows you to point to other useful resources, either on the web, `\url{https://www.r-project.org}`, in your package `\code{\link{functioname}}`, or another package `\code{\link[packagename]{functioname}}`.

     - `@aliases alias1 alias2 ...` adds additional aliases to the topic. An alias is another name for the topic that can be used with `?`.

     - `@keywords keyword1 keyword2 ...` adds standardised keywords. Keywords are optional, but if present, must be taken from a predefined list found in `file.path(R.home("doc"), "KEYWORDS")`.

     - Documenting functions

       - `@param x,y Numeric vectors` multiple arguments in the same line
       - Example code must work without errors as it is run automatically as part of `R CMD check`.

     - Documenting packages

       ```R
       #' foo: A package for computating the notorious bar statistic
       #'
       #' The foo package provides three categories of important functions:
       #' foo, bar and baz.
       #' 
       #' @section Foo functions:
       #' The foo functions ...
       #'
       #' @docType package
       #' @name foo
       NULL
       #> NULL
       ```

       - usually put this documentation in a file called `<package-name>.R`. It’s also a good place to put the package level import statements

     - Documenting classes, generics and methods

       - S3 **generics** are regular functions, so document them as such. S3 **classes** have no formal definition, so document the constructor function. examples: `predict.lm()`

       - Document **S4 classes** by adding a roxygen block before `setClass()`.

         ```R
         #' An S4 class to represent a bank account.
         #'
         #' @slot balance A length-one numeric vector
         Account <- setClass("Account",
           slots = list(balance = "numeric")
         )
         ```

         S4 **generics** are also functions, so document them as such.
         S4 **methods** You document them like a regular function, but you probably don’t want each method to have its own documentation page. Instead, put the method documentation in one of three places (Use either `@rdname` or `@describeIn` to control where method documentation goes)

         - In the class. Most appropriate if the corresponding generic uses single dispatch and you created the class.
         - In the generic. Most appropriate if the generic uses multiple dispatch and you have written both the generic and the method.
         - In its own file. Most appropriate if the method is complex, or if you’ve written the method but not the class or generic.

         The `@include` tag gives a space separated list of file names that should be loaded before the current file

         ```R
         #' @include foo.R bar.R baz.R
         NULL
         
         setMethod("foo", c("bar", "baz"), ...)
         ```

       - Reference class

         ```R
         #' A Reference Class to represent a bank account.
         #'
         #' @field balance A length-one numeric vector.
         Account <- setRefClass("Account",
           fields = list(balance = "numeric"),
           methods = list(
             withdraw = function(x) {
               "Withdraw money from account. Allows overdrafts"
               balance <<- balance - x
             }
           )
         )
         ```

     - use `@@` for literal `@`. use `\%` for literal `%`. use `\\` for literal `\`. Same as Latex.

     - Inheriting parameters from other functions 

       ```R
       #' @param a This is the first argument
       foo <- function(a) a + 10
       
       #' @param b This is the second argument
       #' @inheritParams foo
       bar <- function(a, b) {
         foo(a) * 10
       }
       ```

     - Documenting multiple functions in the same file

       ```R
       #' Foo bar generic
       #'
       #' @param x Object to foo.
       foobar <- function(x) UseMethod("foobar")
       
       #' @describeIn foobar Difference between the mean and the median
       foobar.numeric <- function(x) abs(mean(x) - median(x))
       
       #' @describeIn foobar First and last values pasted together in a string.
       foobar.character <- function(x) paste0(x[1], "-", x[length(x)])
       ```

       `@rdname` overrides the default file name generated by roxygen and merges documentation for multiple objects into one file.

       ```R
       #' Basic arithmetic
       #'
       #' @param x,y numeric vectors.
       #' @name arith
       NULL
       #> NULL
       
       #' @rdname arith
       add <- function(x, y) x + y
       
       #' @rdname arith
       times <- function(x, y) x * y
       ```

     - Text formatting reference sheet

       - Check https://r-pkgs.org/man.html#text-formatting

  2. Run `devtools::document()` (or press Ctrl/Cmd + Shift + D in RStudio) to convert roxygen comments to `.Rd` files. (`devtools::document()` calls `roxygen2::roxygenise()` to do the hard work.)

  3. Preview documentation with `?`.

  4. Rinse and repeat until the documentation looks the way you want.

### Vignettes (Ch. 11)

To create your first vignette, run:

```R
usethis::use_vignette("my-vignette")
```

Once you have this file, the workflow is straightforward:

1. Modify the vignette.
2. Press Ctrl/Cmd + Shift + K (or click ![img](https://d33wubrfki0l68.cloudfront.net/9c8a5416a1a626e9d7cf56f366260c1ad6219633/2eeb0/images/knit.png)) to knit the vignette and preview the output.

There are three important components to an R Markdown vignette:

- The initial metadata block.
- Markdown for formatting text.
- Knitr for intermingling text, code and results.

### Testing (Ch. 12)

Add it later.

### Namespace (Ch. 13)

- Namespaces make your packages self-contained in two ways: the **imports** and the **exports**.

- Search path (the list of all the packages you have **attached**) `search()`

  - loading (`::` will also load a package automatically) v.s. attaching ( `library()` or `require()`)

  - |        | **Throws error**     | **Returns** `FALSE`                     |
    | ------ | -------------------- | --------------------------------------- |
    | Load   | `loadNamespace("x")` | `requireNamespace("x", quietly = TRUE)` |
    | Attach | `library(x)`         | `require(x, quietly = TRUE)`            |

    - Use `library(x)` in data analysis scripts. Never use `library()` in a package.
    - Use `requireNamespace("x", quietly = TRUE)` inside a package to check the availability of a package

- 8 namespace directives. Four describe exports:

  - `export()`: export functions (including S3 and S4 generics).
  - `exportPattern()`: export all functions that match a pattern.
  - `exportClasses()`, `exportMethods()`: export S4 classes and methods.
  - `S3method()`: export S3 methods.

  And four describe imports:

  - `import()`: import all functions from a package.
  - `importFrom()`: import selected functions (including S4 generics).
  - `importClassesFrom()`, `importMethodsFrom()`: import S4 classes and methods.
  - `useDynLib()`: import a function from C. This is described in more detail in [compiled code](https://r-pkgs.org/src.html#src).

- Workflow

  - Add roxygen comments to your `.R` files.
  - Run `devtools::document()` (or press Ctrl/Cmd + Shift + D in RStudio) to convert roxygen comments to `.Rd` files.
  - Look at `NAMESPACE` and run tests to check that the specification is correct.
  - Rinse and repeat until the correct functions are exported.

- The `Imports` field really has nothing to do with functions imported into the namespace: it just makes sure the package is installed when your package is. It doesn’t make functions available.

- list the package in `DESCRIPTION` so that it’s installed, then always refer to it explicitly with `pkg::fun()`. Unless there is a strong reason not to, it’s better to be explicit.

- If you are using functions repeatedly, you can avoid `::` by importing the function with `@importFrom pkg fun`.

```R
#' @importFrom methods setClass setGeneric setMethod setRefClass
NULL
```

- It doesn’t matter where they go, but if you have package docs, as described in [documenting packages](https://r-pkgs.org/man.html#man-packages), that’s a natural place to put them.

### External data (Ch. 14)

- There are three main ways to include data in your package, depending on what you want to do with it and who should be able to use it:

  - If you want to store binary data and make it available to the user, put it in `data/`. This is the best place to put example datasets.

    - Each file in this directory should be a `.RData` file created by `save()` containing a single object (with the same name as the file)

    ```R
    x <- sample(1000)
    usethis::use_data(x, mtcars)
    ```

    - It is suggested to include the data processing code in `data-raw/` and add the folder into `.Rbuildignore`. One step solution `usethis::use_data_raw()`.
    - **Documenting data**: Save the document in `R/data.R`. Example:

    ```R
    #' Prices of 50,000 round cut diamonds.
    #'
    #' A dataset containing the prices and other attributes of almost 54,000
    #' diamonds.
    #'
    #' @format A data frame with 53940 rows and 10 variables:
    #' \describe{
    #'   \item{price}{price, in US dollars}
    #'   \item{carat}{weight of the diamond, in carats}
    #'   ...
    #' }
    #' @source \url{http://www.diamondse.info/}
    "diamonds"
    ```

    - **Never `@export` a data set.**

  - If you want to store parsed data, but not make it available to the user, put it in `R/sysdata.rda`. This is the best place to put data that your functions need

    ```R
    x <- sample(1000)
    usethis::use_data(x, mtcars, internal = TRUE)
    ```

    - Objects in `R/sysdata.rda` are not exported (they shouldn’t be), so they don’t need to be documented. They’re only available inside your package.

  - If you want to store raw data, put it in `inst/extdata`.

    - To refer to files in `inst/extdata` (whether installed or not), use `system.file()`.

    ```R
    system.file("extdata", "mtcars.csv", package = "readr")
    #> [1] "/Users/runner/work/_temp/Library/readr/extdata/mtcars.csv"
    ```

### C or C++ code (Ch. 15)

Add later...

### Installed files (Ch. 16)

- When a package is installed, everything in `inst/` is copied into the top-level package directory. 

- because `inst/` is copied into the top-level directory, you should never use a subdirectory with the same name as an existing directory.

- This chapter discusses the most common files found in `inst/`:

  - `inst/AUTHOR` and `inst/COPYRIGHT`. If the copyright and authorship of a package is particularly complex, you can use plain text files, `inst/COPYRIGHTS` and `inst/AUTHORS`, to provide more information.
  - `inst/CITATION`: how to cite the package, see [package citation](https://r-pkgs.org/inst.html#inst-citation) for details.

  ```R
  citHeader("To cite lubridate in publications use:")
  
  citEntry(entry = "Article",
    title        = "Dates and Times Made Easy with {lubridate}",
    author       = personList(as.person("Garrett Grolemund"),
                     as.person("Hadley Wickham")),
    journal      = "Journal of Statistical Software",
    year         = "2011",
    volume       = "40",
    number       = "3",
    pages        = "1--25",
    url          = "https://www.jstatsoft.org/v40/i03/",
  
    textVersion  =
    paste("Garrett Grolemund, Hadley Wickham (2011).",
          "Dates and Times Made Easy with lubridate.",
          "Journal of Statistical Software, 40(3), 1-25.",
          "URL https://www.jstatsoft.org/v40/i03/.")
  )
  ```

  

  - `inst/extdata`: additional external data for examples and vignettes. See [external data](https://r-pkgs.org/data.html#data-extdata) for more detail.
  - `inst/java`, `inst/python` etc. See [other languages](https://r-pkgs.org/inst.html#inst-other-langs).

- To find a file in `inst/` from code use `system.file()`.

