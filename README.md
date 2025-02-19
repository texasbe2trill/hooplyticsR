# hooplyticsR: Basketball Metrics Analysis Using kNN

## Overview

**hooplyticsR** is a basketball analytics project focused on predicting various player performance metrics using machine learning techniques. The project utilizes the `caret` package in R, applying k-Nearest Neighbors (kNN) regression models to forecast key basketball statistics such as points, rebounds, assists, and fantasy scores.

## Key Features

-   **Multiple Metric Predictions**: Predicts points, rebounds, assists, total PRA (points, rebounds, assists), 3-pointers made, steals + blocks, turnovers, and fantasy scores.
-   **Machine Learning with kNN**: Utilizes kNN regression models for accurate performance prediction.
-   **Cross-Validation**: Employs 5-fold repeated cross-validation to ensure robust model evaluation.
-   **Data Pre-processing**: Filters and transforms player data for consistent training and evaluation.

## Dataset

The project uses the nbastatR library containing player-level basketball statistics with columns for points (`pts`), rebounds (`treb`), assists (`ast`), steals (`stl`), blocks (`blk`), turnovers (`tov`), 3-pointers made (`fg3m`), and fantasy scores (`fpts`).

## Model Training and Evaluation

### Model Training

-   Models are trained for each metric using kNN from the `caret` package.
-   Training data is split into 80% training and 20% testing sets.
-   Features include points, rebounds, assists, steals, blocks, and turnovers.

### Model Evaluation

-   Predictions are generated for each model using the test data.

## Usage

1.  Install `renv` (if not already installed)

Before activating the lock file, ensure you have the `renv` package installed:

``` r
install.packages("renv")
library(renv) # Loads the renv package
renv::restore() # Install exact packages used during development
```

2.  **Data Input**: nbastatR data is loaded and filtered to include player data for training and evaluation.

3.  **Training**: Run the training functions to generate models for each basketball metric.

4.  **Prediction**: Use the evaluation function to generate predictions for new player data.

## Example Usage

``` r
# Define the NBA players you want to analyze 

players <- c("Anthony Edwards", "Austin Reaves", "Klay Thompson")

# Retrieve player data from the past 10 seasons

player_data <- game_logs(seasons = 2015:2025, result_types = "player",
Sys.setenv("VROOM_CONNECTION_SIZE" = 131072 \* 2))

# Train KNN models for the selected players

model_data <- train_knn_models(player_data, players)
models <- model_data$models
train_data <- model_data$train_data
test_data <- model_data$test_data

# Evaluate the trained models

predictions <- evaluate_knn_models(models, test_data)

# Define player-specific projections (e.g., points per game)

player_projections <- list(

"Austin Reaves" = list(points_model = 19.5), 
"Anthony Edwards" = list(points_model = 30.5), 
"Klay Thompson" = list(points_model = 15)

)

# Define player 5-game averages for the model input

player_5_game_averages <- list(

"Austin Reaves" = list(points_model = 27), 
"Anthony Edwards" = list(points_model = 32),
"Klay Thompson" = list(points_model = 20)

)

# Make fantasy decisions based on predictions, test data, projections, and 5-game averages

results <- make_fantasy_decisions_by_model(predictions, test_data, 
player_projections, player_5_game_averages)
```
