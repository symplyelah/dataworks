---
title: "Week 2: Data Cleaning and Wrangling"
output: learnr::tutorial
runtime: shiny_prerendered
---



# Week 2

Welcome to **Week 2** of your data science journey! In this module, you'll learn how to clean and reshape your data using `tidyverse` tools.

> 💡 Remember: Most datasets in the real world are *messy*. Your job is to tidy them for effective analysis.



### What is Tidy Data?

Tidy data has:
- Each **variable** in a column
- Each **observation** in a row
- Each **value** in a single cell

::: {.alert alert-info}
📌 This standard makes your data easier to manipulate, visualize, and model.
:::

## Reshaping Data

### Columns with Multiple Values

Use `separate()` 

<div class="tutorial-exercise" data-label="tidy-question" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Fix this dataset to be tidy.
# Try splitting Name_Age and Fever_Symptom into two separate columns
# Hint: Use separate() to fix the duration column

patient_summary <- tibble::tibble(
  Name_Age = c("John-32", "Lara-45", "Ben-28", "Zoe-40"),
  Gender = c("M", "F", "M", "F"),
  Visit_Date = c("2025-08-01", "2025-08-01", "2025-08-02", "2025-08-02"),
  Fever_Symptom = c("Yes-High", "No-", "Yes-Low", "Yes-Medium")
)


# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>


<div class="tutorial-exercise-support" data-label="tidy-question-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
patient_summary %>%
  separate(Name_Age, into = c("Name", "Age"), sep = "-") %>%
  separate(Fever_Symptom, into = c("Fever", "Symptom"), sep = "-")
```

</div>

<div class="tutorial-exercise" data-label="reshape-question" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Separate time into hour and minute

flight_schedule <- tibble::tibble(
  flight = c("AZ101", "BA202", "DL303"),
  departure = c("08:45 AM", "12:15 PM", "06:30 PM"),
  gate = c("A1", "B3", "C2")
)

# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>


<div class="tutorial-exercise-support" data-label="reshape-question-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
flight_schedule %>%
  separate(departure, into = c("hour", "minute"), sep = ":")
```

</div>


<div class="tutorial-exercise" data-label="reshape-question2" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Separate the minute into minutes and time of day

flight_schedule <- tibble::tibble(
  flight = c("AZ101", "BA202", "DL303"),
  departure = c("08:45 AM", "12:15 PM", "06:30 PM"),
  gate = c("A1", "B3", "C2")
)%>%
  separate(departure, into = c("hour", "minute"), sep = ":")

# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>

<div class="tutorial-exercise-support" data-label="reshape-question2-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
flight_schedule %>%
  separate(minute, into = c("minute", "time_of_day"), sep = " ")
```

</div>

### Separate Rows from Multi-Value Cell
Use `separate_rows()` when multiple values in a cell should each be in a row.
<div class="tutorial-exercise" data-label="multi-Value" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Separate student names into rows

school_subjects <- tibble::tibble(
  subject = c("Math", "Science", "Literature"),
  students = c("Ali, Bola, Chi", "Tola, Emma", "Ngozi, Ken, Lara")
)

# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>


<div class="tutorial-exercise-support" data-label="multi-Value-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
school_subjects %>%
  separate_rows(students, sep = ", ")
```

</div>

## Combining Columns

Use `unite()` to merge columns into one.

<div class="tutorial-exercise" data-label="combine-question" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Combine first and last name into a 'name' column separated by a space

customer_name <- tibble::tibble(
  first_name = c("Mary", "James", "Aisha"),
  last_name = c("Johnson", "Brown", "Ahmed"),
  country = c("USA", "UK", "Nigeria")
)


# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>


<div class="tutorial-exercise-support" data-label="combine-question-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
customer_name %>%
  unite("full_name", first_name, last_name, sep = " ")
```

</div>



You work for a multinational company that uses auto-dialer software to contact its customers. When new customers subscribe online, they are asked for a phone number but they often forget to add the country code needed for international calls. You were asked to fix this issue in the database. You’ve been given a data frame with national numbers and country codes named phone_nr_df. Now you want to combine the country_code and national_number columns to create valid international numbers.

<div class="tutorial-exercise" data-label="international-numbers" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
phone_nr_df <- tribble(~country, ~country_code, ~national_number,
                       "USA",            +1, 2025550117,     
                       "United Kingdom", +44, 1632960924,    
                       "Brazil",         +55, 95552452220, 
                       "Australia",      +61, 1900654321, 
                       "China",          +86, 13555953217, 
                       "India",          +91, 8555843898)

# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>

<div class="tutorial-exercise-support" data-label="international-numbers-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
phone_nr_df %>% 
  unite("international_number", country_code, national_number, sep = "")
```

</div>

The drink_df data set includes a variable that has the quantities and units of ingredients used in making each drink. Now you want to create an overview of how much of each ingredient you should buy to make these drinks.

Inspect drink_df and find the right separator in the ingredient's column.
Separate the ingredients column so that for each drink and each ingredient gets a row.

<div class="tutorial-exercise" data-label="drink" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
drink_df <- tribble(~drink, ~ingredients,                                 
                    "Chocolate milk",  "milk 0.3 L; chocolate 40 g; sugar 10 g",  
                    "Orange juice",   "oranges 3; sugar 20 g",                  
                    "Cappuccino",     "milk 0.1 L; water 0.1 L; coffee 30 g; sugar 5 g")


# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>


<div class="tutorial-exercise-support" data-label="drink-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
drink_df %>% 
  separate_rows(ingredients, sep = "; ", convert = TRUE)
```

</div>

- Inspect the output of the previous step to find the separator that splits the ingredients column into three columns: ingredient, quantity, and unit.
- Make sure to convert data types to numeric when possible.
- Group the data by ingredient and unit.
- Calculate the total quantity of each ingredient.


<div class="tutorial-exercise" data-label="drink2" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
drink_df <- tribble(~drink, ~ingredients,                                 
                    "Chocolate milk",  "milk 0.3 L; chocolate 40 g; sugar 10 g",  
                    "Orange juice",   "oranges 3; sugar 20 g",                  
                    "Cappuccino",     "milk 0.1 L; water 0.1 L; coffee 30 g; sugar 5 g")

# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>



<div class="tutorial-exercise-support" data-label="drink2-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
drink_df %>% 
  separate_rows(ingredients, sep = "; ") %>% 
  separate(ingredients, into = c("ingredients", "quantity", "unit"), sep = " ", convert = TRUE) %>% 
  group_by(ingredients, unit) %>% 
  summarise(total = sum(quantity))
```

</div>


## Missing Values

Use `is.na()`, `sum()`, `complete.cases()`, `replace_na()`, and `fill()` to identify and handle missing data.

<div class="tutorial-exercise" data-label="missing-values" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Count NAs

covid_cases <- tibble::tibble(
  state = c("Lagos", "Kano", "Abuja", "Enugu"),
  confirmed = c(2345, NA, 1500, NA),
  recovered = c(1200, 800, NA, NA)
)


# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>


<div class="tutorial-exercise-support" data-label="missing-values-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
sum(is.na(covid_cases))
```

</div>


### Impute with zeros
<div class="tutorial-exercise" data-label="missing-values2" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Replace NA with 0

covid_cases <- tibble::tibble(
  state = c("Lagos", "Kano", "Abuja", "Enugu"),
  confirmed = c(2345, NA, 1500, NA),
  recovered = c(1200, 800, NA, NA)
)

# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>



<div class="tutorial-exercise-support" data-label="missing-values2-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
covid_cases %>%
  replace_na(list(confirmed = 0, recovered = 0))
```

</div>


### 'fill'

You’ve been asked to create a report that allows management to compare sales figures per quarter for two years. The problem is that the dataset **(sales_df)** contains missing values. You’ll need to impute the values in the year column so that you can visualize the data.

- Inspect sales_df in the console, pay attention to the year column.
- Use the `fill()` function to impute the year column in the correct direction.
- Create a line plot where each year has a different color.


<div class="tutorial-exercise" data-label="fill" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
sales_df<- tribble(~year, ~quarter, ~sales,
                    NA, "Q1", 12498,
                   NA, "Q2", 20461,
                   NA, "Q3", 19737,
                   2019, "Q4", 20314,
                   NA, "Q1", 13494,
                   NA, "Q2", 19314,
                   NA, "Q3", 23640,
                   2020, "Q4", 22920)

# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>

<div class="tutorial-exercise-support" data-label="fill-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
sales_df %>%
  fill() %>%  # Impute the year column
  ggplot(aes(x = quarter, y = sales, color = year, group = year)) +
  geom_line() # Create a line plot with sales per quarter colored by year.
```

</div>

## Pivoting: From Wide to Long

Use `pivot_longer()` when column names represent variables (e.g. countries).

<div class="tutorial-exercise" data-label="pivot-question" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Convert to long format

sales_wide <- tibble::tibble(
  region = c("North", "South", "East"),
  Q1 = c(10000, 12000, 9500),
  Q2 = c(11000, 12500, 10500),
  Q3 = c(11500, 13000, 10200),
  Q4 = c(12000, 13500, 11000)
)


# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>


<div class="tutorial-exercise-support" data-label="pivot-question-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
sales_wide %>%
  pivot_longer(cols = Q1:Q4, names_to = "quarter", values_to = "sales")
```

</div>



## Integrate Data with Joins
### Perform a left join

<div class="tutorial-exercise" data-label="left-join" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
# Perform a left join

employee_info <- tibble::tibble(
  emp_id = c(101, 102, 103),
  name = c("Tunde", "Ada", "Ola"),
  department = c("Logistics", "HR", "Finance")
)

employee_salary <- tibble::tibble(
  emp_id = c(101, 102, 104),
  monthly_salary = c(250000, 200000, 300000)
)

# Your code here
```

<script type="application/json" data-ui-opts="1">{"engine":"r","has_checker":false,"caption":"<span data-i18n=\"text.enginecap\" data-i18n-opts=\"{&quot;engine&quot;:&quot;R&quot;}\">R Code<\/span>"}</script></div>



<div class="tutorial-exercise-support" data-label="left-join-solution" data-completion="1" data-diagnostics="1" data-startover="1" data-lines="0" data-pipe="|&gt;">

``` text
employee_info %>%
  left_join(employee_salary, by = "emp_id")
```

</div>


## Challenge

### 


```{=html}
<div class="panel-heading tutorial-quiz-title"><span data-i18n="text.quiz">Quiz</span></div>
```

```{=html}
<div class="panel panel-default tutorial-question-container">
<div data-label="quiz2-1" class="tutorial-question panel-body">
<div id="quiz2-1-answer_container" class="shiny-html-output"></div>
<div id="quiz2-1-message_container" class="shiny-html-output"></div>
<div id="quiz2-1-action_button_container" class="shiny-html-output"></div>
<script>if (Tutorial.triggerMathJax) Tutorial.triggerMathJax()</script>
</div>
</div>
```

```{=html}
<div class="panel panel-default tutorial-question-container">
<div data-label="quiz2-2" class="tutorial-question panel-body">
<div id="quiz2-2-answer_container" class="shiny-html-output"></div>
<div id="quiz2-2-message_container" class="shiny-html-output"></div>
<div id="quiz2-2-action_button_container" class="shiny-html-output"></div>
<script>if (Tutorial.triggerMathJax) Tutorial.triggerMathJax()</script>
</div>
</div>
```

## Summary

Today you learned how to:
- Structure your data using `tidyverse`
- Use `separate()`, `separate_rows()`, `unite()`
- Handle missing values with `replace_na()` and `fill()`
- Reshape your data using `pivot_longer()`

You’re one step closer to becoming a tidyverse pro! 🧹

Try applying these skills to your own dataset using `read_csv()` and wrangle it based on what you’ve learned!


preserve485981f7a8bf3e16
preservefb4a0c3475cca820
preserve415cb3bf04ace8ca
preserve3dc89d7a3b7680d2
preserve11e20455e3530d96
preserved213aaa52ac7aa7f
preserve92f5a3ff6b7b0f7d
preserve078e9d6b4f5f3e35
preserve2d9a67880baf8d81
preserveffc79a563aa1be6a
preserveb05288c80bbb3cdb
preserve30d60e38f9dc4ef9
preserveafa9b422200a990c
preservef00adad3ee2ff925
preservedb20c26213b8934a
preservef725d1059ed424be
preserve262c20501884bb36
preserve8a7b28c4141ab4ee
preserve03d41e73cc1fd76c
preserve5cb5af2b76a55279
preserve030a7d386d5392fa
preservefabe585fd5f62307
preserve74a62bbb75a0f5af
preserved9e785023dfd1379
preserve14a27bd9f8cb5c18
preservea50856797229bf1f
preservec810394835cb64dd
preserve66a4d29d00529b65
preserve9010a19383e06d46
preserve9ce38a1d87ff91ba
preserve69bcfebedb5151a8
preservea0b92299f3fa19eb
preserve201d852da052c2e1

<!--html_preserve-->
<script type="application/shiny-prerendered" data-context="dependencies">
{"type":"list","attributes":{},"value":[{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["header-attrs"]},{"type":"character","attributes":{},"value":["2.29"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["rmd/h/pandoc"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["header-attrs.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["rmarkdown"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["2.29"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["jquery"]},{"type":"character","attributes":{},"value":["3.6.0"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/3.6.0"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["jquery-3.6.0.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["jquerylib"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.1.4"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["bootstrap"]},{"type":"character","attributes":{},"value":["3.3.5"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["rmd/h/bootstrap"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["viewport"]}},"value":[{"type":"character","attributes":{},"value":["width=device-width, initial-scale=1"]}]},{"type":"character","attributes":{},"value":["js/bootstrap.min.js","shim/html5shiv.min.js","shim/respond.min.js"]},{"type":"character","attributes":{},"value":["css/cerulean.min.css"]},{"type":"character","attributes":{},"value":["<style>h1 {font-size: 34px;}\n       h1.title {font-size: 38px;}\n       h2 {font-size: 30px;}\n       h3 {font-size: 24px;}\n       h4 {font-size: 18px;}\n       h5 {font-size: 16px;}\n       h6 {font-size: 12px;}\n       code {color: inherit; background-color: rgba(0, 0, 0, 0.04);}\n       pre:not([class]) { background-color: white }<\/style>"]},{"type":"NULL"},{"type":"character","attributes":{},"value":["rmarkdown"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["2.29"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["pagedtable"]},{"type":"character","attributes":{},"value":["1.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["rmd/h/pagedtable-1.1"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["js/pagedtable.js"]},{"type":"character","attributes":{},"value":["css/pagedtable.css"]},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["rmarkdown"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["2.29"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["highlightjs"]},{"type":"character","attributes":{},"value":["9.12.0"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["rmd/h/highlightjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["highlight.js"]},{"type":"character","attributes":{},"value":["textmate.css"]},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["rmarkdown"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["2.29"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["tutorial"]},{"type":"character","attributes":{},"value":["0.11.5.9000"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/tutorial"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["tutorial.js"]},{"type":"character","attributes":{},"value":["tutorial.css"]},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["i18n"]},{"type":"character","attributes":{},"value":["21.6.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/i18n"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["i18next.min.js","tutorial-i18n-init.js"]},{"type":"NULL"},{"type":"character","attributes":{},"value":["<script id=\"i18n-cstm-trns\" type=\"application/json\">{\"language\":\"en\",\"resources\":{\"en\":{\"translation\":{\"button\":{\"runcode\":\"Run Code\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Hint\",\"hint_plural\":\"Hints\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Next Hint\",\"hintprev\":\"Previous Hint\",\"solution\":\"Solution\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"Copy to Clipboard\",\"startover\":\"Start Over\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Continue\",\"submitanswer\":\"Submit Answer\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Previous Topic\",\"nexttopic\":\"Next Topic\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Try Again\"},\"text\":{\"startover\":\"Start Over\",\"areyousure\":\"Are you sure you want to start over? (All exercise progress will be reset)\",\"youmustcomplete\":\"You must complete the\",\"exercise\":\"exercise\",\"exercise_plural\":\"exercises\",\"inthissection\":\"in this section before continuing.\",\"code\":\"Code\",\"enginecap\":\"{{engine}} $t(text.code)\",\"quiz\":\"Quiz\",\"blank\":\"blank\",\"blank_plural\":\"blanks\",\"exercisecontainsblank\":\"This exercise contains {{count}} $t(text.blank).\",\"pleasereplaceblank\":\"Please replace {{blank}} with valid code.\",\"unparsable\":\"It looks like this might not be valid R code. R cannot determine how to turn your text into a complete command. You may have forgotten to fill in a blank, to remove an underscore, to include a comma between arguments, or to close an opening <code>&quot;<\\/code>, <code>'<\\/code>, <code>(<\\/code> or <code>{<\\/code> with a matching <code>&quot;<\\/code>, <code>'<\\/code>, <code>)<\\/code> or <code>}<\\/code>.\\n\",\"unparsablequotes\":\"<p>It looks like your R code contains specially formatted quotation marks or &quot;curly&quot; quotes (<code>{{character}}<\\/code>) around character strings, making your code invalid. R requires character values to be contained in straight quotation marks (<code>&quot;<\\/code> or <code>'<\\/code>).<\\/p> {{code}} <p>Don't worry, this is a common source of errors when you copy code from another app that applies its own formatting to text. You can try replacing the code on that line with the following. There may be other places that need to be fixed, too.<\\/p> {{suggestion}}\\n\",\"unparsableunicode\":\"<p>It looks like your R code contains an unexpected special character (<code>{{character}}<\\/code>) that makes your code invalid.<\\/p> {{code}} <p>Sometimes your code may contain a special character that looks like a regular character, especially if you copy and paste the code from another app. Try deleting the special character from your code and retyping it manually.<\\/p>\\n\",\"unparsableunicodesuggestion\":\"<p>It looks like your R code contains an unexpected special character (<code>{{character}}<\\/code>) that makes your code invalid.<\\/p> {{code}} <p>Sometimes your code may contain a special character that looks like a regular character, especially if you copy and paste the code from another app. You can try replacing the code on that line with the following. There may be other places that need to be fixed, too.<\\/p> {{suggestion}}\\n\",\"and\":\"and\",\"or\":\"or\",\"listcomma\":\", \",\"oxfordcomma\":\",\"}}},\"fr\":{\"translation\":{\"button\":{\"runcode\":\"Lancer le Code\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Indication\",\"hint_plural\":\"Indications\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Indication Suivante\",\"hintprev\":\"Indication Précédente\",\"solution\":\"Solution\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"Copier dans le Presse-papier\",\"startover\":\"Recommencer\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Continuer\",\"submitanswer\":\"Soumettre\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Chapitre Précédent\",\"nexttopic\":\"Chapitre Suivant\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Réessayer\"},\"text\":{\"startover\":\"Recommencer\",\"areyousure\":\"Êtes-vous certains de vouloir recommencer? (La progression sera remise à zéro)\",\"youmustcomplete\":\"Vous devez d'abord compléter\",\"exercise\":\"l'exercice\",\"exercise_plural\":\"des exercices\",\"inthissection\":\"de cette section avec de continuer.\",\"code\":\"Code\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"Quiz\",\"and\":\"et\",\"or\":\"ou\",\"oxfordcomma\":\"\"}}},\"es\":{\"translation\":{\"button\":{\"runcode\":\"Ejecutar código\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Pista\",\"hint_plural\":\"Pistas\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Siguiente pista\",\"hintprev\":\"Pista anterior\",\"solution\":\"Solución\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"Copiar al portapapeles\",\"startover\":\"Reiniciar\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Continuar\",\"submitanswer\":\"Enviar respuesta\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Tema anterior\",\"nexttopic\":\"Tema siguiente\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Volver a intentar\"},\"text\":{\"startover\":\"Reiniciar\",\"areyousure\":\"¿De verdad quieres empezar de nuevo? (todo el progreso del ejercicio se perderá)\",\"youmustcomplete\":\"Debes completar\",\"exercise\":\"el ejercicio\",\"exercise_plural\":\"los ejercicios\",\"inthissection\":\"en esta sección antes de continuar.\",\"code\":\"Código\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"Cuestionario\",\"and\":\"y\",\"or\":\"o\",\"oxfordcomma\":\"\"}}},\"pt\":{\"translation\":{\"button\":{\"runcode\":\"Executar código\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Dica\",\"hint_plural\":\"Dicas\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Próxima dica\",\"hintprev\":\"Dica anterior\",\"solution\":\"Solução\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"Copiar para a área de transferência\",\"startover\":\"Reiniciar\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Continuar\",\"submitanswer\":\"Enviar resposta\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Tópico anterior\",\"nexttopic\":\"Próximo tópico\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Tentar novamente\"},\"text\":{\"startover\":\"Reiniciar\",\"areyousure\":\"Tem certeza que deseja começar novamente? (todo o progresso feito será perdido)\",\"youmustcomplete\":\"Você deve completar\",\"exercise\":\"o exercício\",\"exercise_plural\":\"os exercícios\",\"inthissection\":\"nesta seção antes de continuar.\",\"code\":\"Código\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"Quiz\",\"and\":\"e\",\"or\":\"ou\",\"oxfordcomma\":\"\"}}},\"tr\":{\"translation\":{\"button\":{\"runcode\":\"Çalıştırma Kodu\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Ipucu\",\"hint_plural\":\"İpuçları\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Sonraki İpucu\",\"hintprev\":\"Önceki İpucu\",\"solution\":\"Çözüm\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"Pano'ya Kopyala\",\"startover\":\"Baştan Başlamak\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Devam et\",\"submitanswer\":\"Cevabı onayla\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Önceki Konu\",\"nexttopic\":\"Sonraki Konu\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Tekrar Deneyin\"},\"text\":{\"startover\":\"Baştan Başlamak\",\"areyousure\":\"Baştan başlamak istediğinizden emin misiniz? (tüm egzersiz ilerlemesi kaybolacak)\",\"youmustcomplete\":\"Tamamlamalısın\",\"exercise\":\"egzersiz\",\"exercise_plural\":\"egzersizler\",\"inthissection\":\"devam etmeden önce bu bölümde\",\"code\":\"Kod\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"Sınav\",\"oxfordcomma\":\"\"}}},\"emo\":{\"translation\":{\"button\":{\"runcode\":\"🏃\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"💡\",\"hint_plural\":\"$t(button.hint)\",\"hinttitle\":\"$t(button.hint)\",\"solution\":\"🎯\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"📋\",\"startover\":\"⏮\",\"startovertitle\":\"Start Over\",\"continue\":\"✅\",\"submitanswer\":\"🆗\",\"submitanswertitle\":\"Submit Answer\",\"previoustopic\":\"⬅\",\"nexttopic\":\"➡\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"🔁\"},\"text\":{\"startover\":\"⏮\",\"areyousure\":\"🤔\",\"youmustcomplete\":\"⚠️ 👉 🧑‍💻\",\"exercise\":\"\",\"exercise_plural\":\"\",\"inthissection\":\"\",\"code\":\"💻\",\"enginecap\":\"$t(text.code) {{engine}}\",\"oxfordcomma\":\"\"}}},\"eu\":{\"translation\":{\"button\":{\"runcode\":\"Kodea egikaritu\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Laguntza\",\"hint_plural\":\"Laguntza\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Aurreko laguntza\",\"hintprev\":\"Hurrengo laguntza\",\"solution\":\"Ebazpena\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"Arbelean kopiatu\",\"startover\":\"Berrabiarazi\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Jarraitu\",\"submitanswer\":\"Erantzuna bidali\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Aurreko atala\",\"nexttopic\":\"Hurrengo atala\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Berriro saiatu\"},\"text\":{\"startover\":\"Berrabiarazi\",\"areyousure\":\"Berriro hasi nahi duzu? (egindako lana galdu egingo da)\",\"youmustcomplete\":\"Aurrera egin baino lehen atal honetako\",\"exercise\":\"ariketa egin behar duzu.\",\"exercise_plural\":\"ariketak egin behar dituzu.\",\"inthissection\":\"\",\"code\":\"Kodea\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"Galdetegia\",\"oxfordcomma\":\"\"}}},\"de\":{\"translation\":{\"button\":{\"runcode\":\"Code ausführen\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Tipp\",\"hint_plural\":\"Tipps\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Nächster Tipp\",\"hintprev\":\"Vorheriger Tipp\",\"solution\":\"Lösung\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"In die Zwischenablage kopieren\",\"startover\":\"Neustart\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Weiter\",\"submitanswer\":\"Antwort einreichen\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Vorheriges Kapitel\",\"nexttopic\":\"Nächstes Kapitel\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Nochmal versuchen\"},\"text\":{\"startover\":\"Neustart\",\"areyousure\":\"Bist du sicher, dass du neustarten willst? (der gesamte Lernfortschritt wird gelöscht)\",\"youmustcomplete\":\"Vervollstädinge\",\"exercise\":\"die Übung\",\"exercise_plural\":\"die Übungen\",\"inthissection\":\"in diesem Kapitel, bevor du fortfährst.\",\"code\":\"Code\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"Quiz\",\"blank\":\"Lücke\",\"blank_plural\":\"Lücken\",\"pleasereplaceblank\":\"Bitte ersetze {{blank}} mit gültigem Code.\",\"unparsable\":\"Dies scheint kein gültiger R Code zu sein. R kann deinen Text nicht in einen gültigen Befehl übersetzen. Du hast vielleicht vergessen, die Lücke zu füllen, einen Unterstrich zu entfernen, ein Komma zwischen Argumente zu setzen oder ein eröffnendes <code>&quot;<\\/code>, <code>'<\\/code>, <code>(<\\/code> oder <code>{<\\/code> mit einem zugehörigen <code>&quot;<\\/code>, <code>'<\\/code>, <code>)<\\/code> oder <code>}<\\/code> zu schließen.\\n\",\"and\":\"und\",\"or\":\"oder\",\"listcomma\":\", \",\"oxfordcomma\":\",\"}}},\"ko\":{\"translation\":{\"button\":{\"runcode\":\"코드 실행\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"힌트\",\"hint_plural\":\"힌트들\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"다음 힌트\",\"hintprev\":\"이전 힌트\",\"solution\":\"솔루션\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"클립보드에 복사\",\"startover\":\"재학습\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"다음 학습으로\",\"submitanswer\":\"정답 제출\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"이전 토픽\",\"nexttopic\":\"다음 토픽\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"재시도\"},\"text\":{\"startover\":\"재학습\",\"areyousure\":\"다시 시작 하시겠습니까? (모든 예제의 진행 정보가 재설정됩니다)\",\"youmustcomplete\":\"당신은 완료해야 합니다\",\"exercise\":\"연습문제\",\"exercise_plural\":\"연습문제들\",\"inthissection\":\"이 섹션을 실행하기 전에\",\"code\":\"코드\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"퀴즈\",\"blank\":\"공백\",\"blank_plural\":\"공백들\",\"exercisecontainsblank\":\"이 연습문제에는 {{count}}개의 $t(text.blank)이 포함되어 있습니다.\",\"pleasereplaceblank\":\"{{blank}}를 유효한 코드로 바꾸십시오.\",\"unparsable\":\"이것은 유효한 R 코드가 아닐 수 있습니다. R은 텍스트를 완전한 명령으로 변환하는 방법을 결정할 수 없습니다. 당신은 공백이나 밑줄을 대체하여 채우기, 인수를 컴마로 구분하기, 또는 <code>&quot;<\\/code>, <code>'<\\/code>, <code>(<\\/code> , <code>{<\\/code>로 시작하는 구문을 닫는 <code>&quot;<\\/code>, <code>'<\\/code>, <code>)<\\/code>, <code>}<\\/code>을 잊었을 수도 있습니다.\\n\",\"and\":\"그리고\",\"or\":\"혹은\",\"listcomma\":\", \",\"oxfordcomma\":\"\"}}},\"zh\":{\"translation\":{\"button\":{\"runcode\":\"运行代码\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"提示\",\"hint_plural\":\"提示\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"下一个提示\",\"hintprev\":\"上一个提示\",\"solution\":\"答案\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"复制到剪切板\",\"startover\":\"重新开始\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"继续\",\"submitanswer\":\"提交答案\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"上一专题\",\"nexttopic\":\"下一专题\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"再试一次\"},\"text\":{\"startover\":\"重置\",\"areyousure\":\"你确定要重新开始吗? (所有当前进度将被重置)\",\"youmustcomplete\":\"你必须完成\",\"exercise\":\"练习\",\"exercise_plural\":\"练习\",\"inthissection\":\"在进行本节之前\",\"code\":\"代码\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"测试\",\"blank\":\"空\",\"blank_plural\":\"空\",\"exercisecontainsblank\":\"本练习包含{{count}}个$t(text.blank)\",\"pleasereplaceblank\":\"请在{{blank}}内填写恰当的代码\",\"unparsable\":\"这似乎不是有效的R代码。 R不知道如何将您的文本转换为完整的命令。 您是否忘了填空，忘了删除下划线，忘了在参数之间包含逗号，或者是忘了用<code>&quot;<\\/code>, <code>'<\\/code>, <code>)<\\/code>,<code>}<\\/code>来封闭<code>&quot;<\\/code>, <code>'<\\/code>, <code>(<\\/code>。 or <code>{<\\/code>。\\n\",\"unparsablequotes\":\"<p>您的R代码中似乎含有特殊格式的引号，或者弯引号(<code>{{character}}<\\/code>) 在字符串前后，在R中字符串应该被直引号(<code>&quot;<\\/code> 或者 <code>'<\\/code>)包裹。<\\/p> {{code}} <p>别担心，该错误经常在复制粘贴包含格式的代码时遇到， 您可以尝试将该行中的代码替换为以下代码，也许还有其他地方需要修改。<\\/p> {{suggestion}}\\n\",\"unparsableunicode\":\"<p>您的代码中似乎包含有异常字符(<code>{{character}}<\\/code>),导致代码无效。<\\/p> {{code}} <p>有时候你的代码可能含有看似正常字符的特殊字符，特别是当你复制粘贴其他来源代码的时候。 请试着删除这些特殊字符,重新输入<\\/p>\\n\",\"unparsableunicodesuggestion\":\"<p>您的代码中似乎包含有异常字符(<code>{{character}}<\\/code>),导致代码无效。<\\/p> {{code}} <p>有时候你的代码可能含有看似正常字符的特殊字符，特别是当你复制粘贴其他来源代码的时候。 请试着删除这些特殊字符,重新输入<\\/p>\\n\",\"and\":\"且\",\"or\":\"或\",\"listcomma\":\",\",\"oxfordcomma\":\",\"}}},\"pl\":{\"translation\":{\"button\":{\"runcode\":\"Uruchom kod\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Podpowiedź\",\"hint_plural\":\"Podpowiedzi\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Następna podpowiedź\",\"hintprev\":\"Poprzednia podpowiedź\",\"solution\":\"Rozwiązanie\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"Kopiuj do schowka\",\"startover\":\"Zacznij od początku\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Kontynuuj\",\"submitanswer\":\"Wyślij\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Poprzednia sekcja\",\"nexttopic\":\"Następna sekcja\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Spróbuj ponownie\"},\"text\":{\"startover\":\"Zacznij od początku\",\"areyousure\":\"Czy na pewno chcesz zacząć od początku? (cały postęp w zadaniu zostanie utracony)\",\"youmustcomplete\":\"Musisz ukończyć\",\"exercise\":\"ćwiczenie\",\"exercise_plural\":\"ćwiczenia\",\"inthissection\":\"w tej sekcji przed kontynuowaniem\",\"code\":\"Kod\",\"enginecap\":\"$t(text.code) {{engine}}\",\"quiz\":\"Quiz\",\"blank\":\"luka\",\"blank_plural\":\"luk(i)\",\"exercisecontainsblank\":\"To ćwiczenie zawiera {{count}} $t(text.blank).\",\"pleasereplaceblank\":\"Proszę uzupełnić {{blank}} prawidłowym kodem.\",\"unparsable\":\"Wygląda na to, że może to nie być prawidłowy kod R. R nie jest w stanie przetworzyć Twojego tekstu na polecenie. Mogłeś(-aś) zapomnieć wypełnić luki, usunąć podkreślnik, umieścić przecinka między argumentami, lub zamknąć znak <code>&quot;<\\/code>, <code>'<\\/code>, <code>(<\\/code> lub <code>{<\\/code> odpowiadającym <code>&quot;<\\/code>, <code>'<\\/code>, <code>)<\\/code> lub <code>}<\\/code>.\\n\",\"unparsablequotes\":\"<p>Wygląda na to, że Twój kod zawiera szczególnie sformatowane cudzysłowy lub cudzysłowy typograficzne (<code>{{character}}<\\/code>) przy ciągach znaków, co sprawia, że kod jest niepoprawny. R wymaga cudzysłowów prostych (<code>&quot;<\\/code> albo <code>'<\\/code>).<\\/p> {{code}} <p>Nie martw się, to powszechne źródło błędów, gdy kopiuje się kod z innego programu, który sam formatuje teskt. Możesz spróbować zastąpić swój kod następującym kodem. Mogą być też inne miejsca, które wymagają poprawienia.<\\/p> {{suggestion}}\\n\",\"unparsableunicode\":\"<p>Wygląda na to, że Twój kod zawiera niespodziewany znak specjalny (<code>{{character}}<\\/code>), co sprawia, że kod jest niepoprawny.<\\/p> {{code}} <p>Czasami Twój kod może zawierać znak specjalny, który wygląda jak zwykły znak, zwłaszcza jeśli kopiujesz kod z innego programu. Spróbuj usunąć znak specjalny i wpisać do ponownie ręcznie.<\\/p>\\n\",\"unparsableunicodesuggestion\":\"<p>Wygląda na to, że Twój kod zawiera niespodziewany znak specjalny (<code>{{character}}<\\/code>), co sprawia, że kod jest niepoprawny.<\\/p> {{code}} <p>Czasami Twój kod może zawierać znak specjalny, który wygląda jak zwykły znak, zwłaszcza jeśli kopiujesz kod z innego programu. Możesz spróbować zastąpić swój kod następującym kodem. Mogą być też inne miejsca, które wymagają poprawienia.<\\/p> {{suggestion}}\\n\",\"and\":\"i\",\"or\":\"lub\",\"listcomma\":\", \",\"oxfordcomma\":\"\"}}},\"no\":{\"translation\":{\"button\":{\"runcode\":\"Kjør kode\",\"runcodetitle\":\"$t(button.runcode) ({{kbd}})\",\"hint\":\"Hint\",\"hint_plural\":\"Hint\",\"hinttitle\":\"$t(button.hint)\",\"hintnext\":\"Neste hint\",\"hintprev\":\"Forrige hint\",\"solution\":\"Løsning\",\"solutiontitle\":\"$t(button.solution)\",\"copyclipboard\":\"Kopier til utklippstavle\",\"startover\":\"Start på nytt\",\"startovertitle\":\"$t(button.startover)\",\"continue\":\"Fortsett\",\"submitanswer\":\"Send inn svar\",\"submitanswertitle\":\"$t(button.submitanswer)\",\"previoustopic\":\"Forrige tema\",\"nexttopic\":\"Neste tema\",\"questionsubmit\":\"$t(button.submitanswer)\",\"questiontryagain\":\"Prøv igjen\"},\"text\":{\"startover\":\"Start på nytt\",\"areyousure\":\"Er du sikker på at du vil starte på nytt? (All fremgang vil bli slettet)\",\"youmustcomplete\":\"Du må fullføre\",\"exercise\":\"oppgave\",\"exercise_plural\":\"oppgaver\",\"inthissection\":\"i denne seksjonen før du fortsetter.\",\"code\":\"kode\",\"enginecap\":\"{{engine}}-$t(text.code)\",\"quiz\":\"Quiz\",\"blank\":\"tomrom\",\"blank_plural\":\"tomrom\",\"exercisecontainsblank\":\"Denne oppgaven inneholder {{count}} $t(text.blank).\",\"pleasereplaceblank\":\"Erstatt {{blank}} med gyldig kode.\",\"unparsable\":\"Dette ser ikke ut til å være gyldig R-kode. Slik R ser det, utgjør ikke teksten en fullstendig kommando. Du kan ha glemt å fylle ut en luke, å fjerne en understrek, å inkludere et komma mellom argumenter, eller å lukke et åpnende <code>&quot;<\\/code>, <code>'<\\/code>, <code>(<\\/code> eller <code>{<\\/code> med et tilsvarende <code>&quot;<\\/code>, <code>'<\\/code>, <code>)<\\/code> eller <code>}<\\/code>.\\n\",\"unparsablequotes\":\"<p>Det ser ut som om R-koden din inneholder spesielt formaterte anførselstegn eller &quot;krøllete&quot; anførselstegn (<code>{{character}}<\\/code>) rundt tekststrenger, noe som gjør koden din ugyldig. R krever at tekstverdier skal være inneholdt i rette anførselstegn (<code>&quot;<\\/code> eller <code>'<\\/code>).<\\/p> {{code}} <p>Dette er et vanlig problem når du kopierer kode fra et annet program som bruker sin egen formatering på tekst.  Du kan prøve å erstatte koden i den linjen med følgende. Kanskje er det også andre steder som må fikses.<\\/p> {{suggestion}}\\n\",\"unparsableunicode\":\"<p>Det ser ut som om R-koden din inneholder et uventet spesialtegn (<code>{{character}}<\\/code>) som gjør koden din ugyldig.<\\/p> {{code}} <p>Noen ganger kan koden din inneholde et spesialtegn som ser ut som et vanlig tegn, spesielt hvis du kopierer og limer inn koden fra et annet program. Prøv å slette spesialtegnet fra koden din og skriv det inn på nytt for hånd.<\\/p>\\n\",\"unparsableunicodesuggestion\":\"<p>Det ser ut som om R-koden din inneholder et uventet spesialtegn (<code>{{character}}<\\/code>) som gjør koden din ugyldig.<\\/p> {{code}} <p>Noen ganger kan koden din inneholde et spesialtegn som ser ut som et vanlig tegn, spesielt hvis du kopierer og limer inn koden fra et annet program. Du kan prøve å erstatte koden i den linjen med følgende. Kanskje er det også andre steder som må fikses.<\\/p> {{suggestion}}\\n\",\"and\":\"og\",\"or\":\"eller\",\"listcomma\":\", \",\"oxfordcomma\":\"\"}}}}}<\/script>"]},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["tutorial-format"]},{"type":"character","attributes":{},"value":["0.11.5.9000"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["rmarkdown/templates/tutorial/resources"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["tutorial-format.js"]},{"type":"character","attributes":{},"value":["tutorial-format.css","rstudio-theme.css"]},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["jquery"]},{"type":"character","attributes":{},"value":["3.6.0"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/3.6.0"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["jquery-3.6.0.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["jquerylib"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.1.4"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["navigation"]},{"type":"character","attributes":{},"value":["1.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["rmd/h/navigation-1.1"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["tabsets.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["rmarkdown"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["2.29"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["highlightjs"]},{"type":"character","attributes":{},"value":["9.12.0"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["rmd/h/highlightjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["highlight.js"]},{"type":"character","attributes":{},"value":["default.css"]},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["rmarkdown"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["2.29"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["jquery"]},{"type":"character","attributes":{},"value":["3.6.0"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/3.6.0"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["jquery-3.6.0.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["jquerylib"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.1.4"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["font-awesome"]},{"type":"character","attributes":{},"value":["6.5.2"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["fontawesome"]}]},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["css/all.min.css","css/v4-shims.min.css"]},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["fontawesome"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.5.3"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["bootbox"]},{"type":"character","attributes":{},"value":["5.5.2"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/bootbox"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["bootbox.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["idb-keyvalue"]},{"type":"character","attributes":{},"value":["3.2.0"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/idb-keyval"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["idb-keyval-iife-compat.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[false]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["tutorial"]},{"type":"character","attributes":{},"value":["0.11.5.9000"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/tutorial"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["tutorial.js"]},{"type":"character","attributes":{},"value":["tutorial.css"]},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["ace"]},{"type":"character","attributes":{},"value":["1.10.1"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/ace"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["ace.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["name","version","src","meta","script","stylesheet","head","attachment","package","all_files","pkgVersion"]},"class":{"type":"character","attributes":{},"value":["html_dependency"]}},"value":[{"type":"character","attributes":{},"value":["clipboardjs"]},{"type":"character","attributes":{},"value":["2.0.10"]},{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["file"]}},"value":[{"type":"character","attributes":{},"value":["lib/clipboardjs"]}]},{"type":"NULL"},{"type":"character","attributes":{},"value":["clipboard.min.js"]},{"type":"NULL"},{"type":"NULL"},{"type":"NULL"},{"type":"character","attributes":{},"value":["learnr"]},{"type":"logical","attributes":{},"value":[true]},{"type":"character","attributes":{},"value":["0.11.5.9000"]}]}]}
</script>
<!--/html_preserve-->
<!--html_preserve-->
<script type="application/shiny-prerendered" data-context="execution_dependencies">
{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["packages"]}},"value":[{"type":"list","attributes":{"names":{"type":"character","attributes":{},"value":["packages","version"]},"class":{"type":"character","attributes":{},"value":["data.frame"]},"row.names":{"type":"integer","attributes":{},"value":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71]}},"value":[{"type":"character","attributes":{},"value":["backports","base","bslib","cachem","checkmate","cli","commonmark","compiler","datasets","digest","dplyr","evaluate","farver","fastmap","fontawesome","forcats","generics","ggplot2","glue","graphics","grDevices","grid","gtable","here","hms","htmltools","htmlwidgets","httpuv","jquerylib","jsonlite","knitr","later","learnr","lifecycle","litedown","lubridate","magrittr","markdown","methods","mime","pillar","pkgconfig","promises","purrr","R6","RColorBrewer","Rcpp","readr","rlang","rmarkdown","rprojroot","rstudioapi","sass","scales","shiny","stats","stringi","stringr","tibble","tidyr","tidyselect","tidyverse","timechange","tools","tzdb","utils","vctrs","withr","xfun","xtable","yaml"]},{"type":"character","attributes":{},"value":["1.5.0","4.5.1","0.9.0","1.1.0","2.3.2","3.6.5","2.0.0","4.5.1","4.5.1","0.6.37","1.1.4","1.0.4","2.1.2","1.2.0","0.5.3","1.0.0","0.1.4","3.5.2","1.8.0","4.5.1","4.5.1","4.5.1","0.3.6","1.0.1","1.1.3","0.5.8.1","1.6.4","1.6.16","0.1.4","2.0.0","1.50","1.4.2","0.11.5.9000","1.0.4","0.7","1.9.4","2.0.3","2.0","4.5.1","0.13","1.11.0","2.0.3","1.3.3","1.1.0","2.6.1","1.1-3","1.1.0","2.1.5","1.1.6","2.29","2.1.0","0.17.1","0.4.10","1.4.0","1.11.1","4.5.1","1.8.7","1.5.1","3.3.0","1.3.1","1.2.1","2.0.0","0.3.0","4.5.1","0.5.0","4.5.1","0.6.5","3.0.2","0.52","1.8-4","2.3.10"]}]}]}
</script>
<!--/html_preserve-->
