```
library(tidyverse)
library(plotly)
library(shiny)

requireNamespace("OzMap") # See mjkerrison.github.io/projects/OzMap.html

set.seed(42)


# Data prep --------------------------------------------------------------------

VIC <- OzMap::SimpleLGA

VIC <- VIC[which(VIC$STE_NAME16 == "Victoria"), ]

# A ggplot and plotly like we'll use require tabular data, so unfortunately the
# SpatialPolygonsDataFrame is too complex; broom::tidy() is our friend here:
VIC.flat <- broom::tidy(VIC)

# This converts each (long, lat) coordinate pair into a row of a data frame,
# which plot_ly will know to connect because they have the same ID:
head(VIC.flat)

# A tibble: 6 x 7
#    long   lat order hole  piece group id   
#   <dbl> <dbl> <int> <lgl> <chr> <chr> <chr>
# 1  147. -36.5     1 FALSE 1     131.1 131  
# 2  147. -36.5     2 FALSE 1     131.1 131  
# 3  147. -36.5     3 FALSE 1     131.1 131  
# 4  147. -36.4     4 FALSE 1     131.1 131  
# 5  147. -36.4     5 FALSE 1     131.1 131  
# 6  147. -36.4     6 FALSE 1     131.1 131  

# Unfortunately, in that process, we lose some data of interest - the name of
# each local government area. Let's re-collect and re-attach that:

VIC.flat <- inner_join(

  broom::tidy(VIC),

  tibble(

    # First we grab the IDs so we can use it to join the two tables.
    id    = map_chr(VIC@polygons, function(x){slot(x, "ID")}),

    # (The 'slot' function acts like `$` or `[` to extract information from a
    # formal class - in our case, a SPDF)

    # Then the data of interest - the names of the local government areas,
    name  = as.character(VIC@data$LGA_NAME16),

    # And then we'll just generate some data to colour by (imagine: population)
    count = rnorm(n = length(id))

  )

)

head(VIC.flat)

# A tibble: 6 x 8
#    long   lat order hole  piece group id    name       count
#   <dbl> <dbl> <int> <lgl> <chr> <chr> <chr> <chr>      <dbl>
# 1  147. -36.5     1 FALSE 1     131.1 131   Alpine (S)  1.37
# 2  147. -36.5     2 FALSE 1     131.1 131   Alpine (S)  1.37
# 3  147. -36.5     3 FALSE 1     131.1 131   Alpine (S)  1.37
# 4  147. -36.4     4 FALSE 1     131.1 131   Alpine (S)  1.37
# 5  147. -36.4     5 FALSE 1     131.1 131   Alpine (S)  1.37
# 6  147. -36.4     6 FALSE 1     131.1 131   Alpine (S)  1.37


# Shiny ------------------------------------------------------------------------

# Barebones:
ui <- fluidPage(

  # a plotly output to display our map-graph,
  plotlyOutput("outputVIC"),

  # and text to show what we clicked.
  verbatimTextOutput("outputClick"),
  verbatimTextOutput("outputLookup")

)


server <- function(input, output, session){

  output$outputVIC <- renderPlotly({

    VIC.flat %>%

      plot_ly(x = ~long, y = ~lat, text = ~name, source = "Victoria") %>%

      add_polygons(split = ~group, color = ~count)

  })

  output$outputClick <- renderPrint({

    event_data(event = "plotly_click", source = "Victoria")

  })

  # clickLookup <- reactive({
  #
  #   to.return <- VIC.flat %>% dplyr::select(id, name) %>% distinct()
  #
  #   event.data <- event_data(event = "plotly_click", source = "Victoria")
  #   
  #   index <- event.data[[1]]
  #
  #   return(to.return[index, ])
  #
  # })

  output$outputLookup <- renderPrint({

    VIC.flat %>% dplyr::select(id, name) %>% distinct() %>%

      `[`(event_data(event = "plotly_click", source = "Victoria")[[1]], )

  })

}


shinyApp(ui, server)
```
