# Plotly

(This post is a draft! Mainly I'm just getting this out there on the off chance that it'll help at least one sorry Googler.)

### Trace Order (Out of Order!)

Something that I ran into particularly when making maps / choropleths was the nuance of trace order.

### Data

Using a flat data frame or tibble, such as the output of `broom::tidy()` when called on a `SpatialPolygonsDataFrame`, that looks something like:

| long | lat | order | hole | piece | group | id | Name | State | Count |
|------|-----|-------|------|-------|-------|----|------|-------|-------|
| 147  | -36 | 1     | F    | 1     | 1.1   | 1  | Albury | NSW | 30 |
| 147  | -36 | 2     | F    | 1     | 1.1   | 1  | Albury | NSW | 30 |

(i.e. each vertex of the local council boundary is its own row)

### Problem

I was making a Shiny app that provided local government areas (councils) that could be clicked on to drill down to the postcodes that comprise them.

The structure was a council map, which when clicked updated the postcode data data frame, which prompted the postcode map to re-render (using the newly-filtered data).

To make the council map, I was using:

```
output$mainPlot <- renderPlotly({

    ggplotobject <- ggplot(data = df.lga(),

      aes(
        x = long, y = lat,
        text = paste0(
          "Council: ", Name, "\n", "Count: ", round(Count, 0)
        )
      )

    ) +

      geom_polygon(aes(group = group, fill = Count)) +

      scale_fill_gradient(low = "#e3f5a4", high = "#13513e") +

      theme(line = element_blank(),
            axis.text = element_blank(),
            axis.title = element_blank(),
            panel.background = element_blank(),
            plot.margin = margin(0,0,0,0, "mm")) +

      coord_quickmap()


    ggplotly(
      ggplotobject,
      tooltip = "text",
      source = "LGAmap"
    ) %>%

    config(scrollZoom = T, displayModeBar = F) %>%

    layout(dragmode = "pan") %>%

    return()

  })
```

The issue was that when the `plotlyOutput` returned a click event, it would _not_ return the council I thought I had clicked on - I was originally anticipating that the polygons - ordered by `ID` in the data frame - would be rendered in the same order, so that when the click event returned `42` I could look up ~~the polygon with `ID == 42`~~ the 42nd polygon. No such luck: I'd be clicking on a council in New South Wales and wind up with the name/outline/data for a council in South Australia.

### The Solution

The answer lies under the hood of `plotly`. Amongst the dark margic it weaves to turn R code into Javascript and HTML, for whatever reason it (nearly always?) re-orders traces, and this doesn't seem to be very well documented (at least not that I could find).

In this particular use-case, after inspecting the HTML output, it was re-ordering the polygons __first__ based on their fill value (i.e. `Count` in the table above), __then__ on their `ID`.

The workaround I developed was to create a lookup dictionary.

It would take the unique councils,
```
df <- df.lga() %>% distinct(Name, .keep_all = T)
```
order them by `Count` and then by `ID`,
```
df <- df[with(df, order(Count, id)), ]
```
calculate the resulting render order,
```
df$plotorder <- seq(0, nrow(df) - 1, by = 1)
```
and then the reactive that this was embedded in would return the (unique) name of the council for use in other lookups elsewhere.
