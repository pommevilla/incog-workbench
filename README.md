Here's how to work with the docs in this folder:

1. [Install Quarto](https://quarto.org/docs/getting-started/installation.html). 
    * Download the CLI [here](https://github.com/quarto-dev/quarto-cli/releases/latest).
    * Install the Quarto R Package with `install.packages("quarto")`.
2. Clone [rstudio-pro](https://github.com/rstudio/rstudio-pro)
3. Open up `rstudio-pro/docs/server` in your favorite IDE. 
    * There is an Rproj file in `rstudio-pro/docs/server`. Also, the daily build of RStudio has nice support for developing in Quarto.
4. Make a copy of `_quarto.yml.in` so Quarto knows how to build the project: `cp _quarto.yml.in _quarto.yml`
    * `_quarto.yml` is the project-level file that tells Quarto that the current directory is a Quarto. It is created at build time from `_quarto.yml.in` after being passed appropriate version number. 
5. Serve the project. 
    * From RStudio, enter `quarto_serve()`. 
    * From the command line, `quarto serve` from within the `rstudio-pro/docs/server` directory.

This will serve the site to your web browser. Quarto will detect when you make changes to any of the `qmd` files and rerender that page. 
An exception to this is if the page contains code - Quarto will not rerender this during `quarto serve`. 
You need to render it manually with `quarto render file_name.qmd` (on the command line) or `quarto_render("file_name.qmd")` (in R). 
Note that you don't need to stop the server while doing this as Quarto will pick up the changes to the file.

Additionally, since `freeze` is set to `true` in `quarto.yml`, Quarto will not automatically re-execute any code in any of the documents.
This means that if you make edit the code in any of the documents, you will have to manually rerender that page in order for the final site to reflect those changes.
Pages with code are:

* [rstudio_ide_commands/rstudio_ide_commands.qmd](https://github.com/rstudio/rstudio-pro/blob/main/docs/server/rstudio_ide_commands/rstudio_ide_commands.qmd)
* [session_user_settings/session_user_settings.qmd](https://github.com/rstudio/rstudio-pro/blob/feature/quarto-docs-revision/docs/server/session_user_settings/session_user_settings.qmd)
* [getting_started/getting_started/qmd](https://github.com/rstudio/rstudio-pro/blob/main/docs/server/getting_started/getting_started.qmd)


## About `_quarto.yml`

Note that `_quarto.yml` is an ignored file, so any changes you make to `_quarto.yml` need to be transferred over to `_quarto.yml.in`. When you do so, *make sure* that the following lines still have `${CPACK_PACKAGE_VERSION}` in them: https://github.com/rstudio/rstudio-pro/blob/747b989a271903b03c1f2ea54d49de71e80f1963/docs/server/_quarto.yml.in#L5-L6. This is where the build version goes and it won't populate if you've overwritten them.

## Testing the build locally

In order to test that the Jenkinsfile build works properly, do the following.

```
cd docs/server
docker build --tag rstudio/docs .
docker run -it --mount type=bind,source="$(pwd)",target=/app rstudio/docs bash

cmake -E env RSTUDIO_VERSION_MAJOR=7 RSTUDIO_VERSION_MINOR=14 RSTUDIO_VERSION_PATCH=2021 RSTUDIO_VERSION_SUFFIX=1809 cmake .
```

This command populates the version number that appears in the final docs. 
This will show up in `_variables.yml` and `_quarto.yml`.

Note that the above `cmake` command needs to be run in the `docs/server` folder.
Afterwards, exit the container, reenter the container from the project root directory, and `cd` back to the Quarto docs directory before running `make` to mimic the build pipeline on Jenkins: 

```
## Exit container & cd to root directory in order to test build in proper directory
cd ../..
docker run -it --mount type=bind,source="$(pwd)",target=/app rstudio/docs bash
cd app/docs/server
make
```

## Testing against the build server

You can run `pro-docs-pipeline` against your branch by going to the [Jenkins Dashboard](https://build.rstudioservices.com/job/IDE/job/pro-docs-pipeline/build?delay=0sec) and putting the name of your branch in the `branch_name` field (and the `git_revision` build to be safe)
