# Creating Pins on Connect Server

- **Author:** Trishang Udhwani


## Overview

Pins lets you pin an R or Python object, or a file to a virtual board where you and others access it.  

The process is described in the official documentation at:
[Pins](https://docs.posit.co/connect/how-to/pins/). Please refer to
that link for details.

Every pin lives on a board, so your first step is to create a board object which can be called. 
This can be done using the function `board <- pins::board_connect()`.  
Note that no arguments are passed to `board_connect()`. This function is designed to just work for most cases.

However, if you are using an out-dated version of Rstudio (versions before 2024.04.1) then this function may not work.  
In that case can specify the `auth` method to inform how you authenticate to Connect. This can be done by using `auth = "envvar"` as the argument.

To do this, set environment variables with `usethis::edit_r_profile()` to open your `.Renviron` for editing, 
and then insert `Sys.setenv(CONNECT_API_KEY="paste key value")` and `Sys.setenv(CONNECT_SERVER="https://connect.posit.vetinst.no/")`.
Remember to insert a newline character (press ENTER) before saving this file.

Now you can create the board object using `board <- pins::board_connect(auth = "envvar")`


## Additional things to consider

Remember, if you’re using git, it’s a good idea to add your `.Renviron` to your `.gitignore` to ensure you’re not publishing your API key to your version control system.
