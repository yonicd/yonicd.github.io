---
layout: page
title: Projects
subtitle: Odds and Ends
---

This is a list of R packages that have been published on CRAN and/or are still being developed on Github.

(Not in any logical order, just what came to mind when I wrote this...)

- Reproduciblity
  - [ggedit](https://github.com/metrumresearchgroup/ggedit) : Interactively edit ggplot2 layer and theme aesthetics definitions.

  - [sinew](https://github.com/metrumresearchgroup/sinew) : Create roxygen2 skeleton populated with information scraped from the within the function script. Also creates field entries for imports in the DESCRIPTION and import in the NAMESPACE files. Can be run from the R console or through the RStudio addin menu.

  - [texPreview](https://github.com/metrumresearchgroup/texPreview) : Compile and preview snippets of LaTeX. Can be used directly from the R console, from RStudio, in Shiny apps and R Markdown documents. Must have pdflatex or xelatex or lualatex in PATH.

  - [gdigest](https://github.com/yonicd/gdigest) : Wrapper for memoise that lets users cache R objects on a remote repository. Works for objects created in console and Shiny. This is meant for multiple users to share cache via a repository, and increase efficiency of shiny apps that have heavy simulations. 

  - [vcs](https://github.com/metrumresearchgroup/vcs) : Managing, inspecting and sourcing remote repositories such as github, bitbucket and subversion.

- Shiny Modules
  - [regexSelect](https://github.com/yonicd/regexSelect) : R package to enable regular expression searches within a shiny selectize object.

  - [shinyHeatmaply](https://github.com/yonicd/shinyHeatmaply) : Access functionality of the [heatmaply](https://github.com/talgalili/heatmaply) package through Shiny application and Shiny gadget.

- htmlwidgets

  - [slickR](https://github.com/metrumresearchgroup/slickR) : Create and customize interactive carousels using the Slick JavaScript library and the htmlwidgets package. The carousels can contain plots produced in R, images, iframes, videos and other htmlwidgets. These carousels can be used directly from the R console, from RStudio, in Shiny apps and R Markdown documents.

  - [jsTree](https://github.com/metrumresearchgroup/jsTree) : Create and customize interactive trees using the jQuery jsTree plugin library and the htmlwidgets package. These trees can be used directly from the R console, from RStudio, in Shiny apps and R Markdown documents

  - [d3Tree](https://github.com/metrumresearchgroup/d3Tree) : Create and customize interactive collapsible D3 trees using the D3 JavaScript library and the htmlwidgets package. These trees can be used directly from the R console, from RStudio, in Shiny apps and R Markdown documents. When in Shiny the tree layout is observed by the server and can be used as a reactive filter of structured data.

  - [fluidSpline](https://github.com/metrumresearchgroup/fluidSpline): Htmlwidget that binds [Spline Editor](https://bl.ocks.org/mbostock/4342190) by Mike Bostock to create an observable object in Shiny

- Stats
  - [lmmen](https://github.com/yonicd/lmmen) : R package that solves the linear mixed model elastic net


Random thoughts that weren't packaged, but are still useful.

- [Elections](https://github.com/yonicd/Elections) : Real time tracker and analysis of 2016 USA Elections

- [CIMDO](https://github.com/yonicd/CIMDO) : Shiny Dashboard for the IMF CIMDO model.

- [supermarketprices](https://github.com/yonicd/supermarketprices) : Read daily suprmarket data in all israel stores

- [taseR](https://github.com/yonicd/taseR) : API to read data from Tel Aviv Stock Exchange (TASE) website to R 
