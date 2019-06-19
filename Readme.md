
<!-- README.md is generated from README.Rmd. Please edit that file -->

# sortable

<!-- badges: start -->

[![Travis build
status](https://travis-ci.org/rstudio/sortable.svg?branch=master)](https://travis-ci.org/rstudio/sortable)
[![CRAN
version](http://www.r-pkg.org/badges/version/sortable)](https://cran.r-project.org/package=sortable)
[![sortable downloads per
month](http://cranlogs.r-pkg.org/badges/sortable)](http://www.rpackages.io/package/sortable)
[![Codecov test
coverage](https://codecov.io/gh/rstudio/sortable/branch/master/graph/badge.svg)](https://codecov.io/gh/rstudio/sortable?branch=master)
<!-- badges: end -->

The goal of `sortable` is to expose the functionality of
[`sortable.js`](https://sortablejs.github.io/Sortable/) as an
[htmlwidget](https://htmlwidgets.org) in R, so you can use this in Shiny
apps and widgets, as well as R Markdown.

## Installation

~~You can install the released version of sortable from
[CRAN](https://CRAN.R-project.org) with:~~

``` r
~~install.packages("sortable")~~
```

And the development version from
[GitHub](https://github.com/rstudio/sortable) with:

``` r
# install.packages("remotes")
remotes::install_github("rstudio/sortable")
```

## Examples

### Simple example of a sortable list

``` r
library(sortable)
```

It is easy to use `sortable` with `htmltools::tags`. You can essentially
make any HTML element sortable. The key idea is that the CSS id of the
HTML element must match the `selector` argument of the `sortable`
object:

``` r
library(htmltools)

html_print(tagList(
  tags$p("This is a sortable list:"),
  tags$ul(
    id = "uniqueId01",
    tags$li("Move"),
    tags$li("Or drag"),
    tags$li("Each of the items"),
    tags$li("To different positions")
  ),
  sortable("uniqueId01") # use the id as the selector
))
```

![](man/figures/simple_sortable_list.gif)

### Sortable widgets

You can also use `sortable()` to drag and drop other widgets:

``` r
library(DiagrammeR)
library(htmltools)

html_print(tagList(
  tags$p("You can drag and drop the diagrams to switch order:"),
  tags$div(
    id = "aUniqueId",
    tags$div(
      style = "border: solid 0.2em gray; float:left;",
      mermaid("graph LR; S[Sortable.js] -->|sortable| R ", height = 300, width = 300)
    ),
    tags$div(
      style = "border: solid 0.2em gray; float:left;",
      mermaid("graph TD; JavaScript -->|htmlwidgets| R ", height = 300, width = 200)
    )
  ),
  sortable("aUniqueId") # again, the CSS id must match the selector
))
```

### Using sortable in Shiny

It only works as an output right now, but of course I want it to be an
input (*I’ll try*) also. Let’s see it as an output.

``` r
library(shiny)

ui <- shinyUI(fluidPage(
  fluidRow(
    column(
      width = 4,
      tags$h4("sortable in Shiny + Bootstrap"),
      tags$div(
        id = "veryUniqueId", class = "list-group",
        tags$div(class = "list-group-item", "Item 1"),
        tags$div(class = "list-group-item", "Item 2"),
        tags$div(class = "list-group-item", "Item 3")
      )
    )
  ),
  sortable("veryUniqueId")
))

server <- function(input, output) {}

shinyApp(ui = ui, server = server)
```

![](man/figures/simple_sortable_shiny.gif)

### Capturing input with `sortable_list()` inside a Shiny app

Now, let’s see if we can get an idea what it might look like as an input
or integral piece of Shiny.

``` r
library(shiny)
library(sortable)

ui <- shinyUI(fluidPage(
  fluidRow(
    column(
      width = 12,
      tags$h2("Using `sortable_list()` in Shiny"),
      tags$p("Once you move an item, the new order will appear in results"),
      sortable_list(
        output_id = "mySort",
        labels = paste("Item", 1:3)
      ),
      tags$h4("Current sorting order"),
      verbatimTextOutput("results")
    )
  )
))

server <- function(input, output) {
  output$results <- renderPrint({
    input$mySort
  })
}
shinyApp(ui=ui,server=server)
```

![](man/figures/sortable_list_shiny.gif)

### Sortable tabsets

Or, check out these reorderable tabs by
`runGist("2dbe45f77b65e28acab9")`. All we had to do was add an `id` and
add one line of code to the [Tabset
example](https://github.com/rstudio/shiny-examples/tree/master/006-tabsets)
from [RStudio](https://rstudio.com). (You can find the [source code in
this
gist](https://gist.github.com/timelyportfolio/2dbe45f77b65e28acab9)).

![](man/figures/sortable_tabs.gif)
