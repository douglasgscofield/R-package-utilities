R-package-utilities
===================

Makefile.R-code-only
--------------------

A general Makefile for R packages that include R code only.

In its current form it assumes you have only R code in your package, use
Roxygen2 for documentation, knitr and markdown for vignettes, are using
the inst/ directory to contain e.g., a CITATION file, and have a NEWS.md file
that you want converted to plaintext for package release.  As I get more
sophisticated with package contents, its utility will expand.

Tools and packages required:

* R (via the command line), pandoc (same)
* R packages `devtools`, `rmarkdown`, `knitr` (>= 0.5 for vignettes via R markdown)

As for the package environment, it requires a preliminary version to be set
up, with a DESCRIPTION file in place containing Package: and Version: lines.
It uses these to determine the package name and version.

### Targets

**make all** or simply **make** builds package pieces but not the package tarball.  It echoes package variables, builds the `NEWS` file, documentation (creating the `man/*.Rd` files with Roxygen2), and vignettes.

make build creates a package tarball in the parent directory, `..` by default (set in PARENTDIR).

**make check** and **make check-cran** create a directory in the package directory called CHECKDIR (`check_tmp` by default) and unpack the tarball inside it when checking.

**make clean** removes CHECKDIR and the package tarball

**make vars** echoes various variables.

**make doc** builds roxygen2 documentation using `devtools::document()`.

**make vignettes** builds vignettes using `devtools::build_vignettes()`, working around an apparent bug where R cant find pandoc.

**make NEWS** builds plaintext `NEWS` from `NEWS.md` using pandoc.

Targets **build**, **check** and **check-cran** use a few other utility targets.

Thanks to [Karl Broman](http://kbroman.org/pkg_primer/pages/docs.html)
for the should-have-been-obvious idea to use a Makefile.

Makefile.R-code-data-into-RData-hardcoded
-----------------------------------------

Identical to the above, but with a couple of package-specific hard-coded rules
for building `data/*.RData` files from files in `inst/extdata/*.txt`, from my
package `readGenalex`.  Nonstandard data formats should be kept in
`inst/extdata/`, and input for `readGenalex` falls under this category.  The
`data` rule creates separate `.RData` files in `data/` for each file.

I would like to generalise these rules.

For data in this category, I have adopted some rules of practice.  First, the
data is kept in a file named after the object holding the data when loaded into
R with the `data()` command, with appropriate extension.  Second, the data is
loaded into that named object and then saved as the only object within an
`.RData` file named after the object.  Thus for a data set providing an object
named `foo`, the file `inst/extdata/foo.txt` contains the data that will be
found within `foo` and the file `data/foo.RData` contains `foo` once the data
has been loaded into it.  Each `.RData` provides a single object, and each
object/`.RData` combination gets its own documentation.
