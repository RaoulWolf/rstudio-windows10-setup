
# Installing R and RStudio on Windows 10 with Chocolatey

> An Incomplete and Unofficial Guide

# Introduction

This is a very rough and unofficial guide for setting up
[R](https://www.r-project.org/) and [RStudio](https://www.rstudio.com/)
on a [Windows 10](https://www.microsoft.com/en-us/windows) machine using
the [Chocolatey](https://chocolatey.org/) package manager. The aim is to
ensure as much functionality as possible, e.g., being able to install
packages (and even R itself) from source and using version control.

All software, version numbers and links were last referenced on
2021-05-19. The below combination of software and version numbers works
reasonably well. Update the software versions at your own risk!

# Installation Procedure

First, the Chocolatey package manager needs to be installed. Detailed
instructions can be found [here](https://chocolatey.org/install). In
short, you need to open Windows PowerShell (`powershell.exe`) <span
style="color:red">with administrative privileges</span> and execute the
following chunk of code:

    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

If you are unsure about any of this, [read the official
documentation](https://chocolatey.org/install). Chocolatey is used from
Windows PowerShell <span style="color:red">with administrative
privileges</span>.

The following is a list of the neccessary software components for the
installation. This is based on the official [R project
recommendations](https://cloud.r-project.org/bin/windows/Rtools/Rtools.txt).
The Chocolatey installation command is given as `code`:

-   [Git 2.31.1](https://git-scm.com/): `choco install git`

-   [MiKTeX 21.3](https://miktex.org/): `choco install miktex`

-   [Inno Setup 6.1.2](http://www.jrsoftware.org/isinfo.php):
    `choco install innosetup`

-   [QPDF 10.3.2](http://qpdf.sourceforge.net/): `choco install qpdf`

-   [Strawberry Perl 5.32.1.1](http://strawberryperl.com/):
    `choco install strawberryperl`

-   [R 4.1.0](https://www.r-project.org/): `choco install r.project`

-   [Rtools 40](https://cloud.r-project.org/bin/windows/Rtools/):
    `choco install rtools`

-   [RStudio 1.4.1106](https://www.rstudio.com/):
    `choco install r.studio`

You can list installed packages with `choco list -l` and update
installed packages with `choco upgrade all`.

# Putting RTools on the PATH

After installation is complete, you need to perform **one more step** to
be able to compile R packages: you need to put the location of the
Rtools *make utilities* (`bash`, `make`, etc.) on the `PATH`. The
easiest way to do so is create a text file `.Renviron` in your Documents
folder which contains the following line:

`PATH="${RTOOLS40_HOME}\usr\bin;${PATH}"`

# Adjust Your User Account’s Environment Variables

Per default, R’s System Library is installed in
`C:\Program Files\R\R-4.1.0\library`. Here, all essential base packages
are located. For additional packages, it is advisable to setup a User
Library, e.g., at
`C:\Users\Documents\[your user name]\R\win-library\4.1`. However,
because \[your employer\] outsources your user account’s
`C:\Users\Documents` folder to another server (for backup reasons), we
need to create and specify a new location for the user package library;
that’s where `R_LIBS_USER` comes into play.

## `R_LIBS_USER`: R’s (and Your) User Library for Packages

As stated above, we have to manually create the folder in which the User
Library should be stored. Using the Windows Explorer app, navigate to
`C:\Users\[user name]`, where you create a new folder called `R`. Inside
`R`, place a folder named `win-library`, which contains another folder
called `4.1`, indicating the major R version in use. You should end up
with the following path: `C:\Users\[your user name]\R\win-library\4.1`.
The next step is to create the `R_LIBS_USER` environment variable.

You can create your `R_LIBS_USER` environment variable in graphically.
Open the Windows Settings application and start typing “environment” in
the “Find a setting” search box. Out of the suggestions that appear,
choose “Edit environment variables for your account”, which causes the
“Environment Variables” window to pop up.

Under “User Variables for \[user name\]”, click “New…” and enter the
following information: “Variable name:” `R_LIBS_USER` and “Variable
value:” `C:\Users\[user name]\R\win-library\4.1`.
