# Anatomy of an R Package
**Author:** Håkon Kaspersen

## Overview
This tutorial will describe the basics on R packages. How they work, what they are used for, and how to share them.


## Tutorial
### What is an R Package?
An R package is a convenient way to organize your code, making it easy to share with others. In the R universe, a package is the fundamental unit of shareable code. It bundles together code, data, documentation, and tests. Most packages are available on the Comprehensive R Archive Network (CRAN), and as of date, they host 18308 packages. This is partly why R has become so successful - among these 18000+ packages, chances are someone already solved a problem you are working on, and it is easy to download their package and use it.

On your computer, most packages live in the `library` directory inside the directory where R is installed (typically `C:/Program Files/R-<version>`). Here, a package is simply a directory that follows a specific structure convention, where functions are stored in the `R` directory, and their respective help pages are stored in the `man` or `help` directory. Other files, such as `DESCRIPTION` and `NAMESPACE` are located in the package directory. The `DESCRIPTION` file lists information about what the package does, who the author is, contact information, versions, license information, package dependencies, and more. The `NAMESPACE` file makes it possible for packages to communicate to each other. Here, the functions that are defined in the package are exported, which is practice means that they will be available when you load the package. The `NAMESPACE` file also imports functions from other packages, managing dependencies. 
> Note: Packages are usually only handled in R directly, and not in your own file system! Do not make changes to packages directly in your library!

Other files may or may not exist inside the directory of a package. Sometimes a `data` folder hold some test data, a `html` directory with a html-file that lists information about the functions inside the package, and others. CRAN packages are usually in a binary format, and the CRAN packages in your default library is uncompressed binary packages. The difference in the directory structure of the package is:
- No `.R` files in the R directory
- A `Meta` directory holding the package metadata (similar to `DESCRIPTION` above)
- Help content appears in `help` and `html`
- A `libs` directory that holds the results of compiling the code

Package states and structures are complex, and this will not be covered in detail here. You can read more about it [here](https://r-pkgs.org/package-structure-state.html). 


### Where can you find R packages?
Most packages are available through repositories, where the biggest is [CRAN](https://cran.r-project.org/). CRAN is a network of FTP and web servers located around the world, and is coordinated by the R foundation. Packages hosted by CRAN are trustworthy due to extensive documentation criteria and testing to make sure it follows CRAN [policies](https://cran.r-project.org/web/packages/policies.html). 

Another repository for R packages is [Bioconductor](https://www.bioconductor.org/). This is a topic-specific repository, intended for open source software for bioinformatics. It has its own [submission and review process](https://www.bioconductor.org/developers/package-submission/), much like CRAN. The community contributing to Bioconductor is very active hand have several conferences and meetings each year.

Lastly, probably the most popular repository for open source packages is [GitHub](https://github.com/). This is popular because it makes collaborations easier and it integrates with git. However, no review process is done for packages hosted on GitHub, so use with care!


### How to install, update, and remove R packages

#### Install
##### CRAN
Packages hosted by CRAN can be installed by the function `install.packages("package")`. With default arguments, this will install the package in question at the default library location. To change the library location (if you have a separate library), define the path in the `lib` argument: `install.packages("package", lib = "path/to/library")`. Specify which mirror you want to use with `repo`:  `install.packages("package", repo = "https://lib.ugent.be/CRAN/")`. You can list available mirrors with `getCRANmirrors()`. Several packages can be installed with one `install.packages` call, simply supply a character vector with the package names as the first argument in the function. 

##### Bioconductor
Packages hosted by Bioconductor cannot be installed the regular way, unless hosted on both platforms. Bioconductor have their own manager package called `BiocManager` which you need to install first, as described above (hosted by CRAN). Then, to install specific packages hosted by Bioconductor, use `Biocmanager::install("package")`. 

##### GitHub
Packages hosted on GitHub cannot be installed the regular way, unless they are hosted on CRAN as well (this is the case for several packages). The function `devtools::install_github("github_user/package_name")` is often used to install packages from GitHub, which originate from the `devtools` package. The `devtools` package also has other useful functions, such as `install_bioc()` and `install_cran()`, which will install packages from Bioconductor and CRAN, respectively (I am however unsure about the difference between `install.packages` and `Biocmanager::install` and these two). 

The `remotes` package also has a similar function to install packages from GitHub: `remotes::install_github(github_user_name/package_name)`

#### Update and remove
To check which packages have updates available, use `old.packages()`. To update all packages, use `update.packages()`. If you want to update just one package, simply run the installation function you used to install it again. To remove packages, use `remove.packages("package")`. This will uninstall the package from the default library, or in the location where R detects the package has been installed.



### How to use packages
#### General usage
After installing a package, a package can be activated from your library with the `library(package)` function. This will load the package into memory, making all the functions defined inside it directly available for use. Sometimes, however, you may only need a single function from a package. The function in question may be accessed without loading the package to memory: `package_name::function_name()`. By using the `package::function` notation, you avoid potential problems with overlapping function names between packages, and it is easier to identify where the specific function originated. 

On a side note, the `library()` function can be used with or without quotation marks, like this: `library("package")` or `library(package)`. This is, according to Yihui Xie, the original author's wish to be lazy. The editors of Journal of Statistical Software apparently tries to promote the use of quotations. Or as Yihui put it:

> *"Apparently, the editors of JSS (Journal of Statistical Software) have been trying to promote the form `library("foo")` and discourage `library(foo)`, but I do not think it makes much sense now or it will change anything. If it were in the 90’s, I’d wholeheartedly support it. It is simply way too late now. Yes, two extra quotation marks will kill many kittens on this planet."*
> -- *Yihui Xie*

To unload a package, the function `detach(package)` can be used.  What is the difference between a library and a package? Think of it this way: A package is a book inside a library. The `library` function checks a package out of the library, and loads it into memory. 

Some people like to use `require()` instead of `library()` to load a package. The difference here is that `require()` *tries* to load a package using `library()`, then returns a logical indicating success or failure, while `library()` actually load a package. One consequence of using `require()` is that it does not throw an error if a package you are trying to load is not installed. Therefore, when you try to use a function from the package, it will throw an error, since the package is not installed. In this case, `library()` would already have thrown an error since the package was not installed. However, sometimes you need to use `require()` to use a package conditionally, i.e. if the package is not crucial to the script, but optional. If that is the case, the following is useful for detecting that the package is not installed:

```r
if (require('foo')) {
	awesome_foo_function() 
} else { 
	warning('You missed an awesome function') 
}

if (!require('foo')) {
	stop('The package foo was not installed') 
}

```

Basically, it sums up to this:

> *"Try not! Do, or do not. There is no try."
> -- Jacob Westfall*

`require()` breaks one of the fundamental rules of robust software systems: [**fail early**](https://stackoverflow.com/a/2807375/1968). This is why `library()` is generally recommended.

#### Help pages and vignettes
Help pages for functions in packages may be accessed like this: `?function_name`, `?package::function`, `?package`, or by the `help` function: `help(function, package = "package")`. The help pages will describe the general use of a function, give detailed information on what each function argument does, and some simple examples. For a more detailed documentation, many packages have vignettes available. Vignettes are documents that presents package functionality in a more detailed way, often with many examples and plots. The vignettes can be accessed by `browseVignettes(package = "package")` or `vignette(package = "package")`.


### How to find the correct R package
So how do you find an R package addressing a specific problem, when there are likely over 20.000 packages to choose from? The answer is not simple. Many packages pop up in tutorials that are made for a specific topic, or from talks and workshops around the world. There is also the [CRAN Task Views](https://cran.r-project.org/web/views/) that lists all CRAN packages by topic, making it a little bit easier to sift through. However, by using R and doing tutorials, you will get familiar with many useful packages related to many different problems. Also, asking other R users is one of the quickest ways of finding packages.

## Take-Home messages
- An R package is a convenient way of packing and sharing code
- Install packages with `install.packages()`, `Biocmanager::install()`, or `devtools::install_github()`
- Load packages or call functions directly from a package with `library()` and `package::function`, avoid using `require()` unless it is absolutely necessary
- Unload a package with `detach()`
- Access help pages with `?package::function` or `?function` (if package is loaded) or `help()`
- Ask about packages, or find them in the CRAN Task Views list
- Use tutorials online!

## Resources
- [DataCamp - R Packages: A Beginner's Guide](https://www.datacamp.com/community/tutorials/r-packages-guide)
- [Wikipedia - R Package](https://en.wikipedia.org/wiki/R_package)
- [Comprehensive R Archive Network](https://cran.r-project.org/)
- [List of all R packages hosted by CRAN](https://cran.r-project.org/web/packages/available_packages_by_date.html)
- [Yihui Xie - library() vs. require() in R](https://yihui.org/en/2014/07/library-vs-require/)
- [Hadley Wickham & Jenny Bryan - R Packages](https://r-pkgs.org/index.html)
