

```{r setup, include=FALSE}
library(tidyverse)
#library(dsbox)
library(lubridate)
library(ggplot2)
    
knitr::opts_chunk$set(out.width = "100%")
```


## Packages

```{r load-packages}
library(tidyverse)
library(ggplot2)
```




# Real Data Example



```{r}
library(ISLR2)
car_df <- Carseats

```


------



```{r view-data, eval = FALSE}
View(car_df)

```


## Exercise 1

- State the dimension of the data set you have.  

- Which variables could be suitable for the response based on some other set of features.  

- Fit a simple linear regression model to predict Sales using Price. Provide an interpretation of the coefficient in the model and write out the model explicitly.




```{r}
library(ISLR2)

dataset_dimensions <- dim(car_df)

print("The dimensions of the car_df dataset are:")
print(paste("Number of rows:", dataset_dimensions[1]))
print(paste("Number of columns:", dataset_dimensions[2]))

variable_names <- names(car_df)
print(variable_names)

response_variable <- "Sales"
predictor_variables <- setdiff(variable_names, response_variable)

print("Suitable response variable: ")
print(response_variable)
print("Potential predictor variables: ")
print(predictor_variables)

model <- lm(Sales ~ Price, data = car_df)

model_summary <- summary(model)
print(model_summary)

```


## Exercise 2 

- Interpret the ploting results for model diagnostics (Using the `plot` command, comment on the validity of the assumption of the model 

(Hint: Note before using the `plot` command you may wish to specify a 2x2 graphics window using `par(mfrow = c(2, 2))`).)

- Using the fitted model, obtain $95\%$ confidence interval for the coefficient(s). 

```{r}

model <- lm(Sales ~ ., data = car_df)

par(mfrow = c(2, 2))

plot(model)

summary_model <- summary(model)
conf_intervals <- confint(model)
conf_intervals
```
## Exercise 3 

Fit a multiple regression model to predict Sales using **Price**, **Urban**, and **US**. Provide an interpretation of each coefficient in the model. Be careful-some of the variables in the model are qualitative!




### ABOUT REPRODUCIBILITY

```{r}
set.seed(27021)

model <- lm(Sales ~ Price + Urban + US, data = car_df)

summary(model)
```



## Exercise 4 

Now consider the same process over the splited data, not the whole sample.

- What is data splitting context and why is it important? 

- How can you split your data into training and testing sets in R using the tidymodels package or any other alternative? Consider the 80-20 split rule for training and testing, respectively. 


- Once you have split your data, refit the previously considered simple linear or multiple linear regression models on training set only. Look at the summary of the model.  

- Use the predict() function in R to generate predictions for the testing data using the regression models you fitted. Evaluate their performance on the testing data using metrics such as RMSE or R-squared?


```{r}

library(rsample)

set.seed(27021)
data_split <- initial_split(car_df, prop = 0.8, strata = NULL)

data_train <- training(data_split)
data_test <- testing(data_split)

model <- lm(Sales ~ Price + Urban + US, data = data_train)

summary(model)

predictions <- predict(model, newdata = data_test)

rmse <- sqrt(mean((data_test$Sales - predictions)^2))
cat("RMSE:", rmse, "\n")

rsquared <- cor(data_test$Sales, predictions)^2
cat("R-squared:", rsquared, "\n")
```




