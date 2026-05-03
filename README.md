# custom_function
This is a custom function I designed to create plots to compare different groups and include my company's logo.

## Custom Function: Utility

The main utility of the function is to categorize the data into groups that
differentiate the data (species, models, versions, countries, etc) and then 
plot the different groups into one graph. 

The function will create a plot and distinguish each group in order to better
compare the different groups in relation to another undetermined variable. 

Also, given that my organization (Council of the Americas) has to regularly 
create plots, this function will include their name, theme, and logo in the plot.

You may analyze the output included in the index.pdf.

## Replication Code
```{r}
plot_coa <- function(data, grouping, y_var){
  require(ggplot2)
  require(patchwork)
  require(dplyr)

coa_colors <- c("#E8540A", "#1A3A5C", "#F5A623", "#2E86AB", "#A8C686")

  plot <- 
    data |> 
    select({{ grouping }}, {{ y_var }}) |> 
    group_by({{ grouping }}) |> 
    mutate({{ grouping }} := as.factor({{ grouping }})) |> 
    ggplot(aes(x = {{grouping}}, y = {{ y_var }})) +
    geom_col(aes(fill = {{grouping}})) +
    scale_fill_manual(values = coa_colors) +
    scale_y_continuous(expand = expansion(mult = c(0, 0.1))) +
    labs(
      title = rlang::englue("Distribution of {{ y_var }} by {{ grouping }}"),
      subtitle = "Source: COA Analysis",
      x = rlang::englue ("Comparison between {{ grouping }}"),
      y = rlang::englue ("Distribution of {{ y_var }}"),
      tag = "COUNCIL OF THE\nAMERICAS    "
    ) +
    theme_light() +
    theme(
      plot.title = element_text(
        size = 20, 
        face = "bold",
        hjust = 0.5,
        color = "#FF6600"),
      plot.tag = element_text(
        face = "bold",
        size = 11,
        hjust = 1,
        vjust = 0,
        color = "#FF6600"),
      plot.tag.position = c(1,-0.05),
      plot.margin = margin(t = 10, r = 10, b = 100, l = 10), 
      legend.position = "none",
      plot.subtitle = element_text(size = 12, color = "gray50"),
      panel.grid.major.y = element_line(color = "gray90"),
      panel.grid.major.x = element_blank(),
      axis.line = element_line(color = "#1A3A5C"),
      axis.text = element_text(color = "#1A3A5C", size = 11),
      axis.title = element_text(
        color = "#1A3A5C",
        face = "bold",
        size = 13),
      strip.background = element_rect(fill = "#1A3A5C"),
      strip.text = element_text(
        color = "white",
        face = "bold",
        size = 11)
    )
  
  return(plot)
}
```
