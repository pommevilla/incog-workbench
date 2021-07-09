# Quarto Docs Revision

This is a preview site for the Quarto revision of the Workbench documentation ([#2689](https://github.com/rstudio/rstudio-pro/pull/2689)). 
Here are some instructions to get you started working on the the docs with Quarto.

1. [Install Quarto](https://quarto.org/docs/getting-started/installation.html). 
    * Download the CLI [here](https://github.com/quarto-dev/quarto-cli/releases/latest).
    * Install the Quarto R Package with `install.packages("quarto")`.
2. Clone [rstudio-pro](https://github.com/rstudio/rstudio-pro)
3. Open up `rstudio-pro/docs/server` in your favorite IDE. 
    * There is an Rproj file in `rstudio-pro/docs/server`. Also, the daily build of RStudio has nice support for developing in Quarto.
4. Serve the project. 
    * From RStudio, enter `quarto_serve()`. 
    * From the command line, `quarto serve` from within the `rstudio-pro/docs/server` directory.

This will serve the site to your web browser. Quarto will detect when you make changes to any of the `qmd` files and rerender that page. 
An exception to this is if the page contains code - Quarto will not rerender this during `quarto serve`. 
You will need to stop the server and render it manually with `quarto render file_name.qmd` (on the command line) or `quarto_render("file_name.qmd") (in R). 

Additionally, since `freeze` is set to `true` in `quarto.yml`, Quarto will not automatically re-execute any code in any of the documents.
This means that if you make edit the code in any of the documents, you will have to manually rerender that page in order for the final site to reflect those changes.
Pages with code are:

* [rstudio_ide_commands/rstudio_ide_commands.qmd](https://github.com/rstudio/rstudio-pro/blob/main/docs/server/rstudio_ide_commands/rstudio_ide_commands.qmd)
* [session_user_settings/session_user_settings.qmd](https://github.com/rstudio/rstudio-pro/blob/feature/quarto-docs-revision/docs/server/session_user_settings/session_user_settings.qmd)
* [getting_started/getting_started/qmd](https://github.com/rstudio/rstudio-pro/blob/main/docs/server/getting_started/getting_started.qmd)
