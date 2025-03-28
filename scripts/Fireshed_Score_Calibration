# Fireshed Cumulative Score Calibration

# Load required packages
if (!require("pacman")) install.packages("pacman")
pacman::p_load(foreign, dplyr, tibble, ggplot2, ggthemes, scales, patchwork)

# Get user-defined directory path
get_user_path <- function(prompt_text) {
  readline(prompt = paste(prompt_text, ": "))
}

# Prompt user to select the area (Williams Lake, Quesnel, Yunesit'in)
cat("Select the area for calibration (Williams Lake, Quesnel, Yunesit'in): \n")
area <- get_user_path("Area")

# Predefine file paths and burn count groupings based on area
if (area == "Williams Lake") {
  dbf_path <- get_user_path("Enter the path to Williams Lake DBF file")
  burn_count_groups <- c("0-60", "60-120", "120-180")
  cumulative_column <- "Cumulative"
  burn_count_column <- "Burn_Count"
  plot_title <- "Williams Lake Calibration Graph"
} else if (area == "Quesnel") {
  dbf_path <- get_user_path("Enter the path to Quesnel DBF file")
  burn_count_groups <- c("0-27", "27-54", "54-81", "81-108")
  cumulative_column <- "Fireshedsc"
  burn_count_column <- "Burncount"
  plot_title <- "Quesnel Calibration Graph"
} else if (area == "Yunesit'in") {
  dbf_path <- get_user_path("Enter the path to Yunesit'in DBF file")
  burn_count_groups <- c("0-100", "100-200", "200-300", "300-400")
  cumulative_column <- "Fireshedsc"
  burn_count_column <- "Burnedcoun"
  plot_title <- "Yunesit'in Calibration Graph"
} else {
  stop("Invalid area selected. Please choose from Williams Lake, Quesnel, or Yunesit'in.")
}

# Read the DBF file and convert it to a tibble
dbf_tibble <- as_tibble(read.dbf(dbf_path, as.is = TRUE))

# Define Burn Count Groups dynamically
dbf_tibble <- dbf_tibble %>%
  mutate(Burn_Count_Group = case_when(
    !!sym(burn_count_column) >= 0 & !!sym(burn_count_column) < as.numeric(strsplit(burn_count_groups[1], "-")[[1]][2]) ~ burn_count_groups[1],
    !!sym(burn_count_column) >= as.numeric(strsplit(burn_count_groups[2], "-")[[1]][1]) & !!sym(burn_count_column) < as.numeric(strsplit(burn_count_groups[2], "-")[[1]][2]) ~ burn_count_groups[2],
    !!sym(burn_count_column) >= as.numeric(strsplit(burn_count_groups[3], "-")[[1]][1]) & !!sym(burn_count_column) <= as.numeric(strsplit(burn_count_groups[3], "-")[[1]][2]) ~ burn_count_groups[3],
    !!sym(burn_count_column) >= as.numeric(strsplit(burn_count_groups[4], "-")[[1]][1]) & !!sym(burn_count_column) <= as.numeric(strsplit(burn_count_groups[4], "-")[[1]][2]) ~ burn_count_groups[4]
  ))

# Convert to factor for ordered plotting
dbf_tibble$Burn_Count_Group <- factor(dbf_tibble$Burn_Count_Group, levels = burn_count_groups)

# Function to find the mode (peak density)
get_mode <- function(x) {
  density_x <- density(x, na.rm = TRUE)
  mode_index <- which.max(density_x$y)
  return(density_x$x[mode_index])
}

# Function to create a density plot for each burn count group
density_plot <- function(group, show_x_axis_label = FALSE) {
  df_filtered <- dbf_tibble %>% filter(Burn_Count_Group == group)
  mode_value <- get_mode(df_filtered[[cumulative_column]])
  p <- ggplot(df_filtered, aes(x = !!sym(cumulative_column), fill = Burn_Count_Group)) +
    geom_density(alpha = 0.7, color = "black") +
    geom_vline(xintercept = mode_value, linetype = "dashed", color = "black", size = 1) +
    theme_minimal() +
    theme(text = element_text(size = 14), legend.position = "none") +
    labs(title = paste("Burn Count:", group), y = "Density")
  if (show_x_axis_label) {
    p <- p + labs(x = "Cumulative Score") + theme(axis.title.x = element_text(size = 16))
  }
  return(p)
}

# Generate density plots for each group
plots <- lapply(burn_count_groups, function(group) density_plot(group, show_x_axis_label = (group == tail(burn_count_groups, 1))))

# Combine plots into a single layout
final_plot <- Reduce("+", plots) + plot_layout(ncol = 1) + plot_annotation(title = plot_title)

# Display the final plot
print(final_plot)

