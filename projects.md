---
layout: page
title: Projects
subtitle: Odds and Ends
---

This is a list of R packages that have been published on CRAN and/or are still being developed on Github. 

Many of the projects are part of [open tools](https://www.metrumrg.com/try-open-source-tools/) developed at Metrum Research Group. The icon [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science) denotes a Metrum project.


(Not in any logical order, just what came to mind when I wrote this...)

- *Reproduciblity*
  - **ggedit** : Interactively edit ggplot2 layer and theme aesthetics definitions. [![](img/ghicon.jpeg)](https://github.com/metrumresearchgroup/ggedit) [![](img/Ricon.png)](https://cran.r-project.org/web/packages/ggedit/index.html) [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

  - **sinew** : Create roxygen2 skeleton populated with information scraped from the within the function script. Also creates field entries for imports in the DESCRIPTION and import in the NAMESPACE files. Can be run from the R console or through the RStudio addin menu. [![](img/ghicon.jpeg)](https://github.com/metrumresearchgroup/sinew) [![](img/Ricon.png)](https://cran.r-project.org/web/packages/sinew/index.html)  [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

  - **texPreview** : Compile and preview snippets of LaTeX. Can be used directly from the R console, from RStudio, in Shiny apps and R Markdown documents. Must have pdflatex or xelatex or lualatex in PATH. [![](img/ghicon.jpeg)](https://github.com/metrumresearchgroup/texPreview) [![](img/Ricon.png)](https://cran.r-project.org/web/packages/texPreview/index.html) [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

  - **gdigest** : Wrapper for memoise that lets users cache R objects on a remote repository. Works for objects created in console and Shiny. This is meant for multiple users to share cache via a repository, and increase efficiency of shiny apps that have heavy simulations. [![](img/ghicon.jpeg)](https://github.com/yonicd/gdigest) [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

  - **vcs** : Managing, inspecting and sourcing remote repositories such as github, bitbucket and subversion. [![](img/ghicon.jpeg)](https://github.com/metrumresearchgroup/vcs) [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

- Shiny Apps, Modules and Gadgets
  - **regexSelect** : R package to enable regular expression searches within a shiny selectize object. [![](img/ghicon.jpeg)](https://github.com/yonicd/regexSelect)

  - **shinyHeatmaply** : Access functionality of the [heatmaply](https://github.com/talgalili/heatmaply) package through Shiny application and Shiny gadget. [![](img/ghicon.jpeg)](https://github.com/yonicd/shinyHeatmaply) [![](img/Ricon.png)](https://cran.r-project.org/web/packages/shinyHeatmaply/index.html)

- *Htmlwidgets*

  - **slickR** : Create and customize interactive carousels using the Slick JavaScript library and the htmlwidgets package. The carousels can contain plots produced in R, images, iframes, videos and other htmlwidgets. These carousels can be used directly from the R console, from RStudio, in Shiny apps and R Markdown documents. [![](img/ghicon.jpeg)](https://github.com/metrumresearchgroup/slickR) [![](img/Ricon.png)](https://cran.r-project.org/web/packages/slickR/index.html) [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

  - **jsTree** : Create and customize interactive trees using the jQuery jsTree plugin library and the htmlwidgets package. These trees can be used directly from the R console, from RStudio, in Shiny apps and R Markdown documents [![](img/ghicon.jpeg)](https://github.com/metrumresearchgroup/jsTree) [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

  - **d3Tree** : Create and customize interactive collapsible D3 trees using the D3 JavaScript library and the htmlwidgets package. These trees can be used directly from the R console, from RStudio, in Shiny apps and R Markdown documents. When in Shiny the tree layout is observed by the server and can be used as a reactive filter of structured data. [![](img/ghicon.jpeg)](https://github.com/metrumresearchgroup/d3Tree) [![](img/Ricon.png)](https://cran.r-project.org/web/packages/d3Tree/index.html) [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

  - **fluidSpline** : Htmlwidget that binds [Spline Editor](https://bl.ocks.org/mbostock/4342190) by Mike Bostock to create an observable object in Shiny [![](img/ghicon.jpeg)](https://github.com/metrumresearchgroup/fluidSpline) [![](img/mrgiconnew.jpg)](https://www.metrumrg.com/open-science)

- *Stats*
  - **lmmen** : R package that solves the linear mixed model elastic net. [![](img/ghicon.jpeg)](https://github.com/yonicd/lmmen) [![](img/Ricon.png)](https://cran.r-project.org/web/packages/lmmen/index.html)


*Random thoughts that weren't packaged, but are still useful.*

- **Elections** : Real time tracker and analysis of 2016 USA Elections [![](img/ghicon.jpeg)](https://github.com/yonicd/Elections)

- **CIMDO** : Shiny Dashboard for the IMF CIMDO model. [![](img/ghicon.jpeg)](https://github.com/yonicd/CIMDO)

- **supermarketprices** : Read daily suprmarket data in all israel stores. [![](img/ghicon.jpeg)](https://github.com/yonicd/supermarketprices)

- **taseR** : API to read data from Tel Aviv Stock Exchange (TASE) website to R. [![](img/ghicon.jpeg)](https://github.com/yonicd/taseR)
