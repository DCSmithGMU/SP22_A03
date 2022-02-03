# Assignment 3: Flights of New York

- [Assignment 3: Flights of New York](#assignment-2-flights-of-new-york)
  - [Instructions](#instructions)
  - [About the dataset](#about-the-dataset)
  - [Exercises](#exercises)
  - [How to submit](#how-to-submit)
  - [Cheatsheets](#cheatsheets)

## Instructions

Obtain the GitHub repository you will use to complete the assignment, which contains a starter file named `flights_of_new_york.Rmd`.
This assignment will help you become more familiar with manipulating datasets using R by guiding you through some examples.
Read the [About the dataset](#about-the-dataset) section to get some background information on the dataset that you'll be working with.
Each of the below [exercises](#exercises) are to be completed in the provided spaces within your starter file `flights_of_new_york.Rmd`.
Then, when you're ready to submit, follow the directions in the **[How to submit](#how-to-submit)** section below.

## About the dataset

The dataset we are working with in this assignment is the `flights` dataset that is loaded via the `nycflights13` R package.
This dataset contains on-time data for all flights that departed from a New York City airport (airport codes: JFK, LGA, EWR) in 2013.

## Exercises

Start working on these exercises **after** you have successfully cloned your repository into RStudio Server. Make sure that you are working on the Assignment 3 project (the current project is shown in the top left of RStudio), otherwise you will not be able to commit your changes.

1. As always, you should inspect a new dataset to become more familiar with what it contains.
    
    First, run the setup code block in the `flights_of_new_york.Rmd` file.
    This will install and load the required packages.

    Then type the following command **in your *Console* window** (not in the R Markdown file):

    ```{r}
    View(flights)
    ```

    Since the `flights` dataset is loaded as an R package via the `library(nycflights13)` in your setup code block, additional documentation is available.
    To read more about the dataset, type the following command **in your _Console_ window**:

    ```{r}
    ?flights
    ```
    
    A description of the dataset will appear in the *Help* pane.

    Use these two resources to answer these initial questions about the dataset in your RMarkdown file (you can use `i.`, `ii.`, etc. to create a Latin numeral list for your answers - just make sure each item is on its own line with a blank line between each).

    i.  How many rows and columns does this dataset have?

    ii.  What does a single row in this dataset represent?

    iii.  What is the difference between the information contained in the `arr_time` and `sched_arr_time` columns? (Take a look at the column descriptions)

    iv.  Airplanes are reused across many different flights.
        Which columns would be helpful to use in identifying individual airplanes?
    
    Once you have written your answers, save the `flights_of_new_york.Rmd` file, and commit it (in the Git pane in the top left of RStudio), with an appropriate commit message (e.g. "Finished exercise 1").

2. Let's start with the `select()` function.
    The `select()` function *selects* columns from a dataset, which is useful when you're working with a dataset that contains dozens of variables.
    
    Put the following code in a code chunk in your RMarkdown file and run it:

    ```{r}
    flights %>%
      select(year, month)
    ```

    Based on the output, explain what happens when you run this command.
    
    Commit your answer for Exercise 2.

    > #### What does `%>%` mean?
    >
    > The strange looking symbol `%>%` is called the **pipe**, and it is a handy way to pass a dataset through a chain of commands.
    > All examples going forward will use the pipe whenever possible.
    

3. There are multiple ways to specify columns with the `select()` command.
    To demonstrate this, copy the code you wrote in **Exercise 2** into a new code chunk, and replace `select(year, month)` with `select(year:day)`.
    What does the colon `:` do?
    
    Commit your work.

4. The sort operation is a common and indispensible operation for organizing data, and the function from `dplyr` that allows us to sort is called `arrange()`.
    `arrange()` sorts columns with textual data (`chr` data type) into alphabetical order and sorts numerical data into numerical order.

    Try running the following code in a new code chunk:

    ```{r}
    flights %>%
      arrange(air_time, distance)
    ```

    Based on the output, answer the following questions. Does it look like both the `air_time` and `distance` columns were sorted?
    Which column was sorted first?
    What happens if you reverse the order you specify the columns in `arrange()`?
    
    Commit your work.

    > #### Important
    >
    > Students often find that there is a _base R_ function named `sort()` as well, leading to confusion later in the course.
    > Using `sort()` the same way you use `arrange()` will give you errors.
    > For the duration of course **we will always prefer to use the `tidyverse` version of a function**, so always use `arrange()` and pretend `sort()` doesn’t exist.

5.  By default, `arrange()` will sort data in ascending order.
    The function `desc()` can be used to sort in descending order.
    For example, to sort by months in reverse order, we would use `arrange(desc(month))`
    
    Let's use it to answer a simple question about the dataset: *what flight experienced the longest departure delay?*
    Identify the column that gives information about flight departure delays, and then use `arrange()` in combination with `desc()` to sort the `flights` dataset to find the flight with the longest departure delay.
    
    Commit your work.

6.  Let's now try an example that uses the `mutate()` function, which is a little more complex.
    `mutate()` lets us **transform** a dataset by applying the same operation to each row in the dataset and appending the results as a *new* column.
    More simply, this is how you can add, subtract, multiply, and divide different two or more columns against each other.
    As an example, run the following command to calculate the average speed for each flight in miles per hour:

    ```{r}
    flights %>%
      mutate(
        average_speed = distance / (air_time / 60)
      )
    ```

    Where does the new column you just computed show up in the dataset and what is the name of this new column?
    What part of the code is controlling the name of the new column?
    
    Commit your work.

7.  We are not limited to only one input at a time in `mutate()`.
    As long as we separate each input by a comma, we can put as many inputs as we want in the `mutate()` function!
    As an example, let's take the `dep_time` column, which gives the clock time as an integer (for example, 11:00am is 1100) and separate it into an hours column and minutes column:

    ```{r}
    flights %>%
      mutate(
        dep_time_hour = dep_time %/% 100,
        dep_time_minute = dep_time %% 100
      )
    ```

    Next, copy and paste this code into a new code block, add a second comma, and then add a third line.
    **Have this third line use the `dep_time_hour` and `dep_time_minute` columns to compute the number of minutes since midnight.**
    Name this third column `dep_time_minutes_midnight`.
    
    As an example, if a flight departed at 1:30am, dep_time_minutes_midnight should be 90 (since 1:30am is 90 minutes after midnight).
    
    Once you have answered this question, commit your work.

    > #### What do `%/%` and `%%` mean?
    >
    > The operators `%/%` and `%%` let you do **modular arithmetic.**
    > The symbol `%/%` means *integer division* and `%%` means *remainder*.
    > As a quick example, you might remember learning about proper and improper fractions in math class.
    9/4 is an example of an improper fraction.
    > To convert it into a proper fraction, first we need to figure out how many times 4 goes into 9.
    > We can easily compute this with integer division:
    >
    > ```{r integer-division-example, comment = "#"}
    > 9 %/% 4
    > ```
    >
    > We also need to know the remainder, or how much is left  over after dividing 4 into 9.
    >
    > ```{r remainder-example, comment = "#"}
    > 9 %% 4
    > ```
    >
    > So the proper fraction representation of 9/4 is 2 1/4.
    

8. Next we consider the `filter()` function, which provides a ruled-based way to keep a subset of rows and remove the rest.
    Here we just consider rules that are simple comparisons, which involve the symbols:

    *   `>`: greater than
    *   `>=`: greater than or equal to
    *   `<`: less than
    *   `<=`: less than or equal to
    *   `!=`: not equal
    *   `==`: equal

    Give `filter()` a try by running the following two code blocks:

    ```{r}
    flights %>%
      filter(
        arr_delay < 0
      )
    ```

    ```{r}
    flights %>%
      filter(
        carrier == "AA"
      )
    ```

    After running the above code, figure out how to combine the two examples in one `filter()` function (**hint:** it resembles what you did with `mutate()`).
    This will tell you all the flights operated by American Airlines (airline code: AA) that arrived early.
    
    When finished, commit your work.

9. It is common to want to summarize the information contained within a dataset, such as computing sums and averages, or counting how many data points belong to different groups.
    This is called data aggregation, as it **aggregates** many data points together and uses them to compute a cetain quantity.
    We perform data aggregation in R by using the commands `group_by()` and `summarize()`, which frequently show up as a pair.

    The `group_by()` command is applied to one or more columns, and allows you create groups that share common values in a column of categorical data.
    The `summarize()` command can then be used to compute quantities like averages **within** those groups.
    It will help to see an example.
    
    i. Run the following code, which calculates the mean (average) arrival delay for each airline carrier:

      ```{r}
      flights %>%
        group_by(carrier) %>%
        summarize(
          average_arr_delay = mean(arr_delay, na.rm = TRUE)
        )
      ```
      
      (You can ignore any warnings about `'summarise()' ungrouping output`.)
 
      Which airline carrier had the longest arrival delays on average?
      Which airline carrier had the shortest arrival delays on average?
     
    ii. Copy the previous code chunk and add a line of code within the `summarize` function to also calculate the average departure delay (i.e. the output of the summarize function should display the average departure and arrival delays for all carriers).
    
    Commit your work when you have finished this question.

10. Sometimes we need to save the output of a series of piped functions to a new variable.

    As a reminder, we use the assignment operator `<-` to assign a value (on the right of the arrow) to a variable (on the left):
    
    ```r
    x <- 2
    ```
    
    However, the thing on the right can also be an expression, which will be evaluated (to a value) before it is stored in the variable:
    
    ```r
    x <- 2 + 2
    ```
    
    We can also store the output of piped functions in a variable:
    
    ```r
    flights_to_miami <- flights %>%
      filter(dest == "MIA")
    ```
    
    This previous code chunk filters just flights that were going to Miami International Airport (which has the 3-letter airport code MIA), and stores that reduced dataframe in a new variable `flights_to_miami`.
    
    Modify the code so that the `filter()` function filters rows corresponding to flights traveling to Miami *which also* arrived late. Then pipe this output to the `select()` function to get just the `arr_delay` and `carrier` columns. Change the name of the variable that the output dataframe is stored in to a new variable called `late_flights_to_miami`.
    
    When you have done this successfully, you should see this new variable appear in the Environment tab in the top right pane of RStudio. If your code is correct, it should be listed as a dataframe containing 3855 observations of 2 variables. (There should be no output from this code chunk itself, as you are storing the dataframe in a variable rather than printing it.)
    
    Commit your work when you have finished this question.

11. The `flights` dataset is already in a *tidy* format.

    Let's create a new *un*-tidy dataset so that you can practice *tidy*-ing that instead.
    
    Copy-and-paste this code chunk into your Rmd file, and then run it to create a new dataframe called `monthly_delays`:
    
      ```{r}
      monthly_delays <- flights %>%
        group_by(month, carrier) %>%
        summarize(
          arrival_delay = mean(arr_delay, na.rm = TRUE), 
          .groups = "drop"
        ) %>%
        spread(carrier, arrival_delay) %>%
        select(-`9E`)
      ```
    
    You can take a look at the dataframe by clicking it's name in the Environment tab of the upper-right pane in RStudio (or use the `View(...)` function in your Console as you did in Exercise 1).
    
    In this new dataframe, each row is a month, and each column holds the average arrival delay for a different airline (for that month).

    In this format, the data is not particularly useful. If we wanted to visualizae the delays of every airline, we would have need to create a seperate graph for each airline, e.g.
    
    ```r
    qplot(x = month, y = UA, geom="line", data=monthly_delays)
    ```

    Note that we specify that we want a line graph with the `geom="line"` argument.
    
    That would be very slow and repetitive, but fortunately we can achieve the same effect much more easily by reshaping our dataframe into a *tidy* format with the `pivot_longer` function.
    
    i. First, `pivot_longer` all 15 airline columns in the `monthly_delays` dataframe into two columns (a *name* column and a *value* column) by replacing the ellipses in the example code below. Exactly what you name the new name and value columns is up to you, but the name column will hold the airline IDs (e.g. `AA`, `UA`, etc.), and the value column will hold the delays:
    
        ```{r}
        monthly_delays %>%
          pivot_longer(cols = ..., names_to = ..., values_to = ...)
        ```
      
      If your code is correct, you should have created a dataframe with 3 columns and 180 rows.
      
      <!-- You can ignore the warnings that appear about gather and ungrouping. -->
    
    ii. Next, we will create a graph by repeating our code from part (i) and storing it in a new variable, and then using that new variable as the dataset for the `qplot()` function. To do this, create a new code chunk and modify this example code by replacing the ellipses (`...`) to create a line graph of the monthly delays of all 15 airlines:
    
        ```{r}
        pivoted_monthly_delays <- monthly_delays %>%
          pivot_longer(cols = ..., names_to = ..., values_to = ...)
        
        qplot(x = month, 
          y = ..., 
          color = ...,
          geom ="line", 
          data = pivoted_monthly_delays
          )
        ```
    
    **Hints:** 
    
    * Use the same values in the `pivot_longer(...)` function that you used in part (i).
    * Remember that the output of `pivot_longer()` (i.e. part (i)) is the dataframe that `qplot()` will use to create the graph.
    * You need to fill in the ellipses in `qplot()` with the two columns that you created with the `pivot_longer` function. Hint: the y-axis of a scatter plot will need to be continuous, whereas we wish to color the lines by a categorical variable.
    * You can ignore the warning about "missing rows". By default, R will warn you if it can't plot rows because they have missing data (just in case you, the data scientist, hadn't realized that fact and thought you were plotting every row).
    
    When you are finished, commit your work.


## How to submit

To submit your assignment, follow the two steps below.
Your homework will be graded for credit **after** you've completed both steps!

1.  Save, commit, and push your completed R Markdown file (include any last corrections you made to your answers) so that everything is synchronized to GitHub.
    If you do this right, then you should be able to see your most recent commit on the GitHub website.

2.  Knit your R Markdown document to the PDF format, export (download) the PDF file from RStudio Server, and then upload it to *Assignment 3* posting on Blackboard.

## Cheatsheets

You are encouraged to review and keep the following cheatsheets handy while working on this assignment:

*   [dplyr cheatsheet][dplyr-cheatsheet]

*   [ggplot2 cheatsheet][ggplot2-cheatsheet]

*   [RStudio cheatsheet][rstudio-cheatsheet]

*   [RMarkdown cheatsheet][rmarkdown-cheatsheet]

*   [RMarkdown reference][rmarkdown-reference]

[dplyr-cheatsheet]:     https://github.com/rstudio/cheatsheets/raw/master/data-transformation.pdf
[ggplot2-cheatsheet]:   https://github.com/rstudio/cheatsheets/raw/master/data-visualization-2.1.pdf
[rstudio-cheatsheet]:   https://github.com/rstudio/cheatsheets/raw/master/rstudio-ide.pdf
[rmarkdown-reference]:  https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf
[rmarkdown-cheatsheet]: https://github.com/rstudio/cheatsheets/raw/master/rmarkdown-2.0.pdf
