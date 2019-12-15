# OzMap

### Geographically mapping meaningful statistical areas

Many R packages exist (and are even bundled with base R) for geographical data. There are plenty of options for countries and all kinds of American areas (states, counties etc.), but after substantial Googling I couldn't find the same for Australia.

A lot of my job is visualising data, and sooner or later that means 'geographically'. To further confuse the issue, I most commonly work with postcodes, while the Australian Bureau of Statistics works with other levels: they do provide very small 'mesh blocks' (down to several hundred people) but most of the interesting stats are, at their most granular, reported at the Local Government Area (i.e. council) level. Well, at least in terms of what is publicly available without either a login or a paid subscription...

So: how do we manage this?

This will be a guide to my journey through this challenge, and you can get the results - an R package you can install directly using the `devtools` package - over at [the OzMap repository](github.com/mjkerrison/OzMap).

This post will end up being a lot more structured than my actual journey was - there were prototypes which hung around for quite a while, then improvements, then refactors and trips back to square one. It's useful to see what other options are out there, though, and the variety of limitations and where I ran into them help to motivate the project.

Plus, there are many tutorials on this already out there, but none of them quite addressed every issue and an awful lot of them used the neat, tidy, pre-packaged US data...

### What data is already out there?

* https://www.matthewproctor.com/australian_postcodes
* https://stackoverflow.com/questions/48534858/geographic-data-australia-r

Interestingly Australia Post does provide this information, but they'll charge you for it:
* https://auspost.com.au/business/marketing-and-communications/access-data-and-insights/address-data/postcode-data

Commercial:
* https://all-things-spatial.blogspot.com/2016/04/australian-postcode-boundaries-2016.html
* https://psma.com.au/product/postcode-boundaries/

### Parsing geodata: it's a doozy

* Tried `geojsonio`, `rgeos`, flattening with `broom::tidy()`...

### Visualising things

* Tried `ggplot2`: great for static data
* Wanted `plotly` interactivity - enter `plotly::ggplotly()`
* Performance woes!
* At last: `leaflet`
* A brief word on `shiny`

### Back to performance...

* Lots of polygons, lots of vertices
* Hello `rmapshaper` and `ms_simplify()`
