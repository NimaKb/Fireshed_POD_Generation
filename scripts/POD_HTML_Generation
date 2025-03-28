# PODs HTML Map Generation

# Load required packages
if (!requireNamespace("pacman", quietly = TRUE)) install.packages("pacman")
pacman::p_load(leaflet, geojsonio, htmlwidgets)

# Prompt the user to enter the directory path
get_user_path <- function(prompt_text) {
  readline(prompt = paste(prompt_text, ": "))
}

# Set the working directory
dir_path <- get_user_path("Enter the directory where your PODs GeoJSON file is located")
setwd(dir_path)

# Prompt the user to enter the file name
geojson_file <- get_user_path("Enter the GeoJSON file name (e.g., POD_TRY2.geojson)")

# Load the PODs GeoJSON file
pods <- geojsonio::geojson_read(geojson_file, what = "sp")

# Define color palettes using equal interval classification with 5 bins
pal_fire <- colorBin("YlOrRd", domain = pods$Fire_Threat, bins = 5)
pal_climate <- colorBin("Blues", domain = pods$Climate_Anthro, bins = 5)
pal_terrain <- colorBin("Greens", domain = pods$Terrain, bins = 5)
pal_fuel <- colorBin("Purples", domain = pods$Fuel_Distribution_Disturbance, bins = 5)

# Create the leaflet map
map <- leaflet(pods) %>%
  addProviderTiles(providers$Esri.WorldImagery, group = "Satellite") %>%
  addProviderTiles(providers$Esri.WorldTopoMap, group = "Topographic") %>%
  addProviderTiles(providers$OpenStreetMap, group = "OSM") %>%
  addProviderTiles(providers$CartoDB.DarkMatter, group = "CartoDB DarkMatter") %>%
  addProviderTiles(providers$CartoDB.Positron, group = "Positron") %>%
  addProviderTiles(providers$OpenTopoMap, group = "OpenTopoMap") %>%
  
  # Fire Threat Layer
  addPolygons(
    color = "black",
    weight = 1.5,
    fillColor = ~pal_fire(Fire_Threat),
    fillOpacity = 0.7,
    group = "Fire Threat",
    popup = ~paste0(
      "<b>BEC_ZONE: </b>", ZONE, "<br>",
      "<b>AREA_Ha: </b>", round(AREA_Ha, 2), "<br>",
      "<b>Fire Threat: </b>", round(Fire_Threat, 2), "<br>",
      "<b>Fuel Distribution: </b>", round(Fuel_Distribution_Disturbance, 2), "<br>",
      "<b>Terrain: </b>", round(Terrain, 2), "<br>",
      "<b>Climate Anthro: </b>", round(Climate_Anthro, 2)
    )
  ) %>%
  
  # Add legends for each layer
  addLegend("topright", pal = pal_fire, values = pods$Fire_Threat, title = "Fire Threat", group = "Fire Threat") %>%
  addLegend("topright", pal = pal_climate, values = pods$Climate_Anthro, title = "Climate Anthro", group = "Climate Anthro") %>%
  addLegend("topright", pal = pal_terrain, values = pods$Terrain, title = "Terrain", group = "Terrain") %>%
  addLegend("topright", pal = pal_fuel, values = pods$Fuel_Distribution_Disturbance, title = "Fuel Distribution", group = "Fuel Distribution") %>%
  
  # Layer Control
  addLayersControl(
    position = "topleft",
    baseGroups = c("Satellite", "Topographic", "OSM", "CartoDB DarkMatter", "Positron", "OpenTopoMap"),
    overlayGroups = c("Fire Threat", "Fuel Distribution", "Terrain", "Climate Anthro"),
    options = layersControlOptions(collapsed = FALSE)
  )

# Save the map as an HTML file
output_file <- get_user_path("Enter the output HTML file name (e.g., Enhanced_PODs_Map.html)")
htmlwidgets::saveWidget(map, output_file, selfcontained = TRUE)

cat("\nMap successfully created and saved as", output_file, "in the directory", dir_path, "\n")
