# Text-Mining-DataCamp-String Manipulation with stringr in R
## 1. String Basics
##### 1.1 Welcome! (video)
##### 1.2 Quotes 
***Instruction:***

Following the guidelines for using quotes, define the three strings, `line1`, `line2` and `line3`:

- Line 1: `The table was a large one, but the three were all crowded together at one corner of it:`
- Line 2: `"No room! No room!" they cried out when they saw Alice coming.`
- Line 3: `"There's plenty of room!" said Alice indignantly, and she sat down in a large arm-chair at one end of the table.`

```{r}
# Define line1
line1 <- "The table was a large one, but the three were all crowded together at one corner of it:"

# Define line2
line2 <- '"No room! No room!" they cried out when they saw Alice coming.'

# Define line3
line3 <- '"There\'s plenty of room!" said Alice indignantly, and she sat down in a large arm-chair at one end of the table.'
```

##### 1.3 What you see isn't always what you have 
***Instruction:***
We've put your lines from Alice's Adventures in Wonderland in a vector called `lines`.

- Take a look at `lines` to see R's representation of the strings.
- Pass `lines` to `writeLines()` to see the content of strings you've created.
- By default `writeLines()` separates the strings with a newline, which you can change using the `sep` argument. Write `lines` to the screen again, but this time set the `sep` argument to a space, " ".
- Finally, try using `writeLines()` on the string `"hello\n\U1F30D"`. You'll learn about what's going on here in the next exercise.

```{r}
# Putting lines in a vector
lines <- c(line1, line2, line3)

# Print lines
lines

# Use writeLines() on lines
writeLines(lines)

# Write lines with a space separator
writeLines(lines, sep = " ")

# Use writeLines() on the string "hello\n\U1F30D"
writeLines("hello\n\U1F30D")
```

##### 1.4 Escape sequences 

***Instruction:***
- Edit the string inside `writeLines()` so that it correctly displays (all on one line):

```{r}
To have a \ you need \\
```

- Edit the string inside `writeLines()` so that it correctly displays (with the line breaks in these positions)

```{r}
This is a really 
really really 
long string
```

- Try `writeLines()` with the string containing Unicode characters: 

```{r}
"\u0928\u092e\u0938\u094d\u0924\u0947 \u0926\u0941\u0928\u093f\u092f\u093e". You just said "Hello World" in Hindi!
```

```{r}
# Should display: To have a \ you need \\
writeLines("To have a \\ you need \\\\")

# Should display: 
# This is a really 
# really really 
# long string
writeLines("This is a really \nreally \nreally \nlong string")

# Use writeLines() with 
# "\u0928\u092e\u0938\u094d\u0924\u0947 \u0926\u0941\u0928\u093f\u092f\u093e"
writeLines("\u0928\u092e\u0938\u094d\u0924\u0947\u0926\u0941\u0928\u093f\u092f\u093e")
```
##### 1.5 Turning numbers into strings (video)
##### 1.6 Using format() with numbers 

***Instruction:***

- Format `c(0.0011, 0.011, 1)` with `digits = 1`. This is like the example described above.
- Now, format `c(1.0011, 2.011, 1)` with `digits = 1`. Try to predict what you might get before you try it.
- Format `percent_change` by choosing the `digits` argument so that the values are presented with one place after the decimal point.
- Format `income` by choosing the `digits` argument so that the values are presented as whole numbers (i.e. no places after the decimal point).
- Format `p_values` using a fixed representation.

```{r}
# Some vectors of numbers
percent_change  <- c(4, -1.91, 3.00, -5.002)
income <-  c(72.19, 1030.18, 10291.93, 1189192.18)
p_values <- c(0.12, 0.98, 0.0000191, 0.00000000002)

# Format c(0.0011, 0.011, 1) with digits = 1
format(c(0.0011, 0.011, 1), digits = 1)

# Format c(1.0011, 2.011, 1) with digits = 1
format(c(1.0011, 2.011, 1), digits = 1)

# Format percent_change to one place after the decimal point
format(percent_change, digits = 2)

# Format income to whole numbers
format(income, digits = 2)

# Format p_values in fixed format
format(p_values, scientific = FALSE)
```

##### 1.7 Controlling other aspects of the string formatC() 

***Instruction:***
We've assigned your formatted `income` from the previous exercise to `formatted_income`.

- Print `formatted_income` Notice the spaces at the start of the strings.
- Call `writeLines()` on the formatted `income`. Notice how the numbers line up on the decimal point.
- Define `trimmed_income` by using `format()` on `income` with `digits = 2` and `trim = TRUE`.
- Call `writeLines()` on `trimmed_income`. Notice how this removes the spaces at the start of the strings and the values line up on left.
- Define `pretty_income` by using `format()` on `income` with `digits = 2` and `big.mark = ","`.
- Call `writeLines()` on pretty_income.

```{r}
formatted_income <- format(income, digits = 2)

# Print formatted_income
formatted_income

# Call writeLines() on the formatted income
writeLines(formatted_income)

# Define trimmed_income
trimmed_income <- format(income, digits = 2, trim = TRUE)

# Call writeLines() on the trimmed_income
writeLines(trimmed_income)

# Define pretty_income
pretty_income <- format(income, digits = 2, big.mark = ",")

# Call writeLines() on the pretty_income
writeLines(pretty_income)
```
##### 1.8 formatC()
***Instruction:***

The vectors `income`, `percent_change`, and `p_values` are available in your workspace.

- First, compare the behavior of `formatC()` to `format()` by calling `formatC()` on `x` with `format = "f"` and `digits = 1`. This is the same vector you used with format(), do you see the difference?
- Call `formatC()` on `y` with `format = "f"` and `digits = 1`. Notice how `digits` has consistent behavior regardless of the vector you format.
- Format `percent_change` to one decimal place after the decimal point.
- Format `percent_change` to one decimal place after the decimal point and add `flag = "+"`. This forces the display of the sign.
- Format `p_values` using `format = "g"` and `digits = 2`. This can be useful, since if there are any p-values in scientific notation, they must be < 0.0001.

```{r}
# From the format() exercise
x <- c(0.0011, 0.011, 1)
y <- c(1.0011, 2.011, 1)

# formatC() on x with format = "f", digits = 1
formatC(x, format = "f", digits = 1)

# formatC() on y with format = "f", digits = 1
formatC(y, format = "f", digits = 1)

# Format percent_change to one place after the decimal point
formatC(percent_change, format = "f", digits = 1)

# percent_change with flag = "+"
formatC(percent_change, format = "f", digits = 1, flag = "+")

# Format p_values using format = "g" and digits = 2
formatC(p_values, format = "g", digits = 2)
```

##### 1.9 Putting strings together (video)
##### 1.10 Annotation of numbers
***Instruction:***
We've put the formatted vectors `pretty_income` and `pretty_percent` in your workspace along with `years`.

- Paste a `$` to the front of each value in `pretty_income`, use `sep = ""`, so there is no space between the `$` and value.
- Paste a `%` to the end of each value in `pretty_percent`, use `sep = ""`, so there is no space between the value and the `%`.
- `years` contains the year each `pretty_percent` corresponds to. Use `paste()` to produce a vector with elements like `2010: +4.0%` and assign it to `year_percent`.
- Use `paste()` with `year_percent` to create single string that collapses all the years: `2010: +4.0%, 2011: -1.9%, 2012: +3.0%, 2013: -5.0%`


```{r}
# Add % to pretty_percent
paste("$", pretty_income, sep = "")
 
# Add % to pretty_percent
paste(pretty_percent, "%", sep = "")

# Create vector with elements like 2010: +4.0%`
year_percent <- paste(years, ": ", pretty_percent, "%", sep = "")

# Collapse all years into single string
paste(year_percent, collapse = ",")
```

##### 1.11 A very simple table 
The `income` vector is loaded in your workspace.

- Create `pretty_income` by using `format()` with `digits = 2` and `big.mark = ","`.
- Create `dollar_income` by pasting `$` to `pretty_income` (don't forget to set the `sep` argument).
- Create `formatted_names` by using `format()` on `income_names` with `justify = "right"`.
- Create `rows` by pasting together `formatted_names` and `dollar_income`. Use three spaces as a separator to give some room between your columns. Be sure to surround your separator in `"`.
- Call `writeLines()` on `rows` to see your table.

***Instruction:***

```{r}
# Define the names vector
income_names <- c("Year 0", "Year 1", "Year 2", "Project Lifetime")

# Create pretty_income
pretty_income <- format(income, digits = 2, big.mark = ",")

# Create dollar_income
dollar_income <- paste("$", pretty_income, sep = "")

# Create formatted_names
formatted_names <- format(income_names, justify = "right")

# Create rows
rows <- paste(formatted_names, dollar_income, sep = "   ")

# Write rows
writeLines(rows)
```

##### 1.12 Let's order pizza! 

***Instruction:***
- Print `my_toppings` to see your random toppings.
- Add `"and "` to the start of the third element by using `paste()` with `my_toppings` and a vector you define.
- Create a vector `these_toppings` by using `paste()` to collapse `my_toppings_and` with a comma and space between each element.
- Create `my_order` by pasting `"I want to order a pizza with "` to `these_toppings` and ending with a period, `"."`.
- Order your pizza by calling `writeLines()` on `my_order`.
- Try re-running all your code (including the sampling of toppings). You should get a brand new pizza order!

```{r}
# Randomly sample 3 toppings
my_toppings <- sample(toppings, size = 3)

# Print my_toppings
my_toppings

# Paste "and " to last element: my_toppings_and
my_toppings_and <- paste(c("", "", "and "), my_toppings, sep = "")

# Collapse with comma space: these_toppings
these_toppings <- paste(my_toppings_and, collapse = ", ")

# Add rest of sentence: my_order
my_order <- paste("I want to order a pizza with ", these_toppings, ".", sep = "")

# Order pizza with writeLines()
writeLines(my_order)
```

## 2. Introduction to Stringr 
##### 2.1 Introducing stringr (video)
##### 2.2 Putting strings together with stringr 
***Instruction:***

```{r}
library(stringr)

my_toppings <- c("cheese", NA, NA)
my_toppings_and <- paste(c("", "", "and "), my_toppings, sep = "")

# Print my_toppings_and
my_toppings_and

# Use str_c() instead of paste(): my_toppings_str
my_toppings_str <- str_c(c("", "", "and "), my_toppings, sep = "")

# Print my_toppings_str
my_toppings_str

# paste() my_toppings_and with collapse = ", "
paste(my_toppings_and, collapse = ", ")

# str_c() my_toppings_str with collapse = ", "
str_c(my_toppings_str, collapse = ", ")
```
##### 2.3 String length 
***Instruction:***

```{r}
library(stringr)
library(babynames)
library(dplyr)

# Extracting vectors for boys' and girls' names
babynames_2014 <- filter(babynames, year == 2014)
boy_names <- filter(babynames_2014, sex == "M")$name
girl_names <- filter(babynames_2014, sex == "F")$name

# Take a look at a few boy_names
head(boy_names)

# Find the length of all boy_names
boy_length <- str_length(boy_names)

# Take a look at a few lengths
head(boy_length)

# Find the length of all girl_names
girl_length <- str_length(girl_names)

# Find the difference in mean length
mean(girl_length) - mean(boy_length)

# Confirm str_length() works with factors
head(str_length(factor(boy_names)))
```
##### 2.4 Extracting substrings 
***Instruction:***

```{r}
# Extract first letter from boy_names
boy_first_letter <- str_sub(boy_names, 1, 1)

# Tabulate occurrences of boy_first_letter
table(boy_first_letter)
  
# Extract the last letter in boy_names, then tabulate
boy_last_letter <- str_sub(boy_names, -1,-1)
table(boy_last_letter)

# Extract the first letter in girl_names, then tabulate
girl_first_letter <- str_sub(girl_names, 1, 1)
table(girl_first_letter)

# Extract the last letter in girl_names, then tabulate
girl_last_letter <- str_sub(girl_names, -1, -1)
table(girl_last_letter)
```

##### 2.5 Hunting for matches (video)
##### 2.6 Detecting matches 
***Instruction:***

```{r}
# Look for pattern "zz" in boy_names
contains_zz <- str_detect(boy_names, pattern = fixed("zz"))

# Examine str() of contains_zz
str(contains_zz)

# How many names contain "zz"?
sum(contains_zz)

# Which names contain "zz"?
boy_names[contains_zz]


# Which rows in boy_df have names that contain "zz"?
boy_df <- filter(babynames_2014, sex == "M")
boy_df[contains_zz,]
```

##### 2.7 Subsetting strings based on match 
***Instruction:***

```{r}
# Find boy_names that contain "zz"
str_subset(boy_names, pattern = fixed("zz"))

# Find girl_names that contain "zz"
str_subset(girl_names, pattern = fixed("zz"))

# Find girl_names that contain "U"
starts_U <- str_subset(girl_names, pattern = fixed("U"))
starts_U

# Find girl_names that contain "U" and "z"
str_subset(starts_U, pattern = "z")
```
##### 2.8 Counting matches 

***Instruction:***

```{r}
# Count occurrences of "a" in girl_names
number_as <- str_count(girl_names, pattern = fixed("a"))

# Count occurrences of "A" in girl_names
number_As <- str_count(girl_names, pattern = fixed("A"))

# Histograms of number_as and number_As
hist(number_as)
hist(number_As) 

# Find total "a" + "A"
total_as <- number_As + number_as

# girl_names with more than 4 a's
girl_names[total_as > 4]
```

##### 2.9 Splitting strings (video)

##### 2.10 Parsing strings into variables 
***Instruction 1:***

```{r}
# Some date data
date_ranges <- c("23.01.2017 - 29.01.2017", "30.01.2017 - 06.02.2017")

# Split dates using " - "
split_dates <- str_split(date_ranges, pattern = fixed(" - "))
split_dates
```

***Instruction 2:***

```{r}
# Some date data
date_ranges <- c("23.01.2017 - 29.01.2017", "30.01.2017 - 06.02.2017")

# Split dates with n and simplify specified
split_dates_n <- str_split(date_ranges, pattern = fixed(" - "), simplify = TRUE, n = 2)
split_dates_n
```

***Instruction 3:***

```{r}
# From previous step
date_ranges <- c("23.01.2017 - 29.01.2017", "30.01.2017 - 06.02.2017")
split_dates_n <- str_split(date_ranges, fixed(" - "), n = 2, simplify = TRUE)

# Subset split_dates_n into start_dates and end_dates
start_dates <- split_dates_n[, 1]
end_dates <- split_dates_n[, 2]

# Split start_dates into day, month and year pieces
str_split(start_dates, pattern = fixed("."), simplify = TRUE)
```

***Instruction 4:***

```{r}
both_names <- c("Box, George", "Cox, David")

# Split both_names into first_names and last_names
both_names_split <- str_split(both_names, pattern = fixed(", "), simplify = TRUE)

# Get first names
first_names <- both_names_split[, 2]

# Get last names
last_names <- both_names_split[, 1]
```

##### 2.11 Some simple text statistics
***Instruction:***

```{r}
# Split lines into words
words <- str_split(lines, pattern = fixed(" "))

# Number of words per line
lapply(words, length)
  
# Number of characters in each word
word_lengths <- lapply(words, str_length)
  
# Average word length per line
lapply(word_lengths, mean)
```

##### 2.12 Replacing matches in strings (video)
##### 2.13 Replacing to tidy strings 
***Instruction 1:***

```{r}
# Some IDs
ids <- c("ID#: 192", "ID#: 118", "ID#: 001")

# Replace "ID#: " with ""
id_nums <- str_replace(ids, "ID#: ", "")

# Turn id_nums into numbers
id_ints <- as.numeric(id_nums)
```

***Instruction 2:***

```{r}
# Some (fake) phone numbers
phone_numbers <- c("510-555-0123", "541-555-0167")

# Use str_replace() to replace "-" with " "
str_replace(phone_numbers, "-", " ")

# Use str_replace_all() to replace "-" with " "
str_replace_all(phone_numbers, "-", " ")

# Turn phone numbers into the format xxx.xxx.xxxx
str_replace_all(phone_numbers, "-", ".")
```

##### 2.14 Review 

***Instruction:***

```{r}
# Find the number of nucleotides in each sequence
str_length(genes)

# Find the number of A's occur in each sequence
str_count(genes, pattern = fixed("A"))

# Return the sequences that contain "TTTTTT"
str_subset(genes, pattern = fixed("TTTTTT"))

# Replace all the "A"s in the sequences with a "_"
str_replace_all(genes, pattern = fixed("A"), replacement = "_")
```

##### 2.15 Final challenges 

***Instruction 1:***

```{r}
# Define some full names
names <- c("Diana Prince", "Clark Kent")

# Split into first and last names
names_split <- str_split(names, pattern = fixed(" "), simplify = TRUE)

# Extract the first letter in the first name
abb_first <- str_sub(names_split[, 1], 1, 1)

# Combine the first letter ". " and last name
str_c(abb_first,". ", names_split[,2])
```

***Instruction 2:***

```{r}
# Use all names in babynames_2014
all_names <- babynames_2014$name

# Get the last two letters of all_names
last_two_letters <- str_sub(all_names, -2, -1)

# Does the name end in "ee"?
ends_in_ee <- str_detect(last_two_letters, pattern = fixed("ee"))

# Extract rows and "sex" column
sex <- babynames_2014$sex[ends_in_ee]

# Display result as a table
table(sex)
```

## 3. Pattern Matching with Regular Expressions
##### 3.1 Regular expressions (video)
##### 3.2 Matching the start or end of the string 

***Instruction 1:***

```{r}
# Some strings to practice with
x <- c("cat", "coat", "scotland", "tic toc")

# Print END
END

# Run me
str_view(x, pattern = START %R% "c")
```

***Instruction 2:***

```{r}
# Match the strings that start with "co" 
str_view(x, pattern = START %R% "co")
```

***Instruction 3:***

```{r}
# Match the strings that end with "at"
str_view(x, pattern = 
"at" %R% END)
```

***Instruction 4:***

```{r}
# Match the strings that is exactly "cat"
str_view(x, pattern = START %R% "cat" %R% END)
```

##### 3.3 Matching any character 
***Instruction 1:***

```{r}
# Match two characters, where the second is a "t"
str_view(x, pattern = ANY_CHAR %R% "t")
```

***Instruction 2:***

```{r}
# Match a "t" followed by any character
str_view(x, pattern = "t" %R% ANY_CHAR)
```

***Instruction 3:***

```{r}
# Match two characters
str_view(x, pattern = ANY_CHAR %R% ANY_CHAR)
```

***Instruction 4:***

```{r}
# Match a string with exactly three characters
str_view(x, pattern = START %R% ANY_CHAR %R% ANY_CHAR %R% ANY_CHAR %R% END)
```

##### 3.4 Combining with stringr functions 
***Instruction 1:***

```{r}
pattern <- "q" %R% ANY_CHAR

# Find names that have the pattern
names_with_q <- str_subset(boy_names, pattern)

# How many names were there?
length(names_with_q)
```

***Instruction 2:***

```{r}
# Find part of name that matches pattern
part_with_q <- str_extract(boy_names, pattern)

# Get a table of counts
table(part_with_q)
```

***Instruction 3:***

```{r}
# Did any names have the pattern more than once?
count_of_q <- str_count(boy_names, pattern)

# Get a table of counts
table(count_of_q)
```

***Instruction 4:***

```{r}
# Which babies got these names? (get logical vector back)
with_q <- str_detect(boy_names, pattern)

# What fraction of babies got these names? (get mean)
mean(with_q)
```

##### 3.5 More regular expressions (video)
##### 3.6 Alternation 
***Instruction 1:***

```{r}
# Match Jeffrey or Geoffrey
whole_names <- or("Jeffrey", "Geoffrey")
str_view(boy_names, pattern = whole_names, 
  match = TRUE) 
```

***Instruction 2:***

```{r}
# Match Jeffrey or Geoffrey, another way
common_ending <- or("Je", "Geo") %R% "ffrey"
str_view(boy_names, pattern = common_ending, 
  match = TRUE)
```

***Instruction 3:***

```{r}
# Match with alternate endings
by_parts <- or("Je", "Geo") %R% "ff" %R% or("ry", "ery", "rey", "erey")
str_view(boy_names, 
  pattern = by_parts, 
  match = TRUE)
```

***Instruction 4:***

```{r}
# Match names that start with Cath or Kath
ckath <- or("C", "K") %R% "ath"
str_view(girl_names, pattern = ckath, match = TRUE)
```

##### 3.7 Character classes 
***Instruction 1:***

```{r}
# Create character class containing vowels
vowels <- char_class("aeiouAEIOU")

# Print vowels
vowels

# See vowels in x with str_view()
str_view(x, vowels)
```

***Instruction 2:***

```{r}
# See vowels in x with str_view_all()
str_view_all(x, vowels)
```

***Instruction 3:***

```{r}
# Number of vowels in boy_names
num_vowels <-  str_count(boy_names, vowels)

# Number of characters in boy_names
name_length <- str_length(boy_names)
```

***Instruction 4:***

```{r}
# Calc mean number of vowels
mean(num_vowels)

# Calc mean fraction of vowels per name
mean(num_vowels/name_length)
```

##### 3.8 Repetition 
***Instruction 1:***

```{r}
# Vowels from last exercise
vowels <- char_class("aeiouAEIOU")

# See names with only vowels
str_view(boy_names, 
  pattern = exactly(one_or_more(vowels)), 
  match = TRUE)
```

***Instruction 2:***

```{r}
# Use `negated_char_class()` for everything but vowels
not_vowels <- negated_char_class("aeiouAEIOU")

# See names with no vowels
str_view(boy_names, 
  pattern = exactly(one_or_more(not_vowels)), 
  match = TRUE)
```

##### 3.9 Shortcuts (video)
##### 3.10 Hunting for phone numbers 
***Instruction 1:***

```{r}
# Create a three digit pattern and test
three_digits <- DGT %R% DGT %R% DGT

# Test it
str_view_all(contact, pattern = three_digits)
```

***Instruction 2:***

```{r}
# Create a separator pattern and test
separator <-  char_class("-.() ")

# Test it
str_view_all(contact, pattern = separator)
```

***Instruction 3:***

```{r}
# Use these components
three_digits <- DGT %R% DGT %R% DGT
four_digits <- three_digits %R% DGT
separator <- char_class("-.() ")

# Create phone pattern
phone_pattern <- optional(OPEN_PAREN) %R%
  three_digits %R%
  zero_or_more(separator) %R%
  three_digits %R% 
  zero_or_more(separator) %R%
  four_digits
      
# Test pattern           
str_view_all(contact, phone_pattern)
```

***Instruction 4:***

```{r}
# Use this pattern
three_digits <- DGT %R% DGT %R% DGT
four_digits <- three_digits %R% DGT
separator <- char_class("-.() ")
phone_pattern <- optional(OPEN_PAREN) %R% 
  three_digits %R% 
  zero_or_more(separator) %R% 
  three_digits %R% 
  zero_or_more(separator) %R%
  four_digits
  
# Extract phone numbers
str_extract(contact, phone_pattern)

# Extract ALL phone numbers
str_extract_all(contact, phone_pattern)
```

##### 3.11 Extracting age and gender from accident narratives
***Instruction 1:***

```{r}
# Pattern to match one or two digits
age <- DGT %R% optional(DGT)

# Test it
str_view(narratives, pattern = age)
```

***Instruction 2:***

```{r}
# Use this pattern
age <- DGT %R% optional(DGT)

# Pattern to match units 
unit <- optional(SPC) %R% or("YO", "YR", "MO")

# Test pattern with age then units
str_view(narratives, 
         pattern = age %R% unit)
```

***Instruction 3:***

```{r}
# Use these patterns
age <- DGT %R% optional(DGT)
unit <- optional(SPC) %R% or("YO", "YR", "MO")

# Pattern to match gender
gender <- optional(SPC) %R% char_class("MF")

# Test pattern with age then units then gender
str_view(narratives, 
         pattern = age %R% unit %R% gender)
```

***Instruction 4:***

```{r}
# Use these patterns
age <- DGT %R% optional(DGT)
unit <- optional(SPC) %R% or("YO", "YR", "MO")
gender <- optional(SPC) %R% or("M", "F")

# Extract age_gender, take a look
age_gender <- str_extract(narratives, pattern = age %R% unit %R% gender)
age_gender
```
##### 3.12 Parsing age and gender into pieces 
***Instruction 1:***

```{r}
# age_gender, age, gender, unit are pre-defined
ls.str()

# Extract age and make numeric
as.numeric(str_extract(age_gender, age))
```

***Instruction 2:***

```{r}
# Replace age and units with ""
genders <- str_remove(age_gender, pattern = age %R% unit)

# Replace extra spaces
str_remove_all(genders, pattern = one_or_more(SPC))
```

***Instruction 3:***

```{r}
# Numeric ages, from previous step
ages_numeric <- as.numeric(str_extract(age_gender, age))

# Extract units 
time_units <- str_extract(age_gender, unit)

# Extract first word character
time_units_clean <- str_extract(time_units, WRD)

# Turn ages in months to years
ifelse(time_units_clean == "Y", ages_numeric, ages_numeric / 12)
```
## 4. More Advanced Matching and Manipulation
##### 4.1 Capturing (video)
##### 4.2 Capturing parts of a pattern 
***Instruction 1:***

```{r}
# Capture part between @ and . and after .
email <- capture(one_or_more(WRD)) %R% 
  "@" %R% capture(one_or_more(WRD)) %R% 
  DOT %R% capture(one_or_more(WRD))

# Check match hasn't changed
str_view(hero_contacts, pattern = email)
```

***Instruction 2:***

```{r}
# Pattern from previous step
email <- capture(one_or_more(WRD)) %R% 
  "@" %R% capture(one_or_more(WRD)) %R% 
  DOT %R% capture(one_or_more(WRD))
  
# Pull out match and captures
email_parts <- str_match(hero_contacts, pattern = email)
email_parts


# Save host
host <- email_parts[, 3]
host
```

##### 4.3 Pulling out parts of a phone number 
***Instruction 1:***

```{r}
# View text containing phone numbers
contact

# Add capture() to get digit parts
phone_pattern <- capture(three_digits) %R% zero_or_more(separator) %R% 
           capture(three_digits) %R% zero_or_more(separator) %R%
           capture(four_digits)
           
# Pull out the parts with str_match()
phone_numbers <- str_match(contact, phone_pattern)

# Put them back together
str_c(
  "(",
  phone_numbers[, 2],
  ") ",
  phone_numbers[, 3],
  "-",
  phone_numbers[, 4])
```

##### 4.4 Extracting age and gender again 
***Instruction 1:***

```{r}
# narratives has been pre-defined
narratives

# Add capture() to get age, unit and sex
pattern <- capture(optional(DGT) %R% DGT) %R%  
  optional(SPC) %R% capture(or("YO", "YR", "MO")) %R%
  optional(SPC) %R% capture(or("M", "F"))

# Pull out from narratives
str_match(narratives, pattern = pattern)
```

***Instruction 2:***

```{r}
# Edit to capture just Y and M in units
pattern2 <- capture(optional(DGT) %R% DGT) %R%  
  optional(SPC) %R% capture(or("Y", "M")) %R% optional(or("O","R")) %R%
  optional(SPC) %R% capture(or("M", "F"))

# Check pattern
str_view(narratives, pattern = pattern2)

# Pull out pieces
str_match(narratives, pattern = pattern2)
```
##### 4.5 Backreferences (video)
##### 4.6 Using backreferences in patterns 
***Instruction 1:***

```{r}
# See names with three repeated letters
repeated_three_times <- capture(LOWER) %R% REF1 %R% REF1

# Test it
str_view(boy_names, pattern = repeated_three_times, match = TRUE)
```

***Instruction 2:***

```{r}
# See names with a pair of repeated letters, egeg. abab
pair_of_repeated <- capture(LOWER %R% LOWER) %R% REF1

# Test it
str_view(boy_names, pattern = pair_of_repeated, match = TRUE)
```

***Instruction 3:***

```{r}
# See names with a pair that reverses, e.g. abba
pair_that_reverses <- capture(LOWER) %R% capture(LOWER) %R% REF2 %R% REF1

# Test it
str_view(boy_names, pattern = pair_that_reverses, match = TRUE)
```

***Instruction 4:***

```{r}
# Four letter palindrome names
four_letter_palindrome <- exactly(
  capture(LOWER) %R% capture(LOWER) %R% REF2 %R% REF1
)

# Test it
str_view(boy_names, pattern = four_letter_palindrome, match = TRUE)
```

##### 4.7 Replacing with regular expressions 
***Instruction:***

```{r}
# View text containing phone numbers
contact

# Replace digits with "X"
str_replace(contact, pattern = DGT, replacement = "X")

# Replace all digits with "X"
str_replace_all(contact, pattern = DGT, replacement = "X")

# Replace all digits with different symbol
str_replace_all(contact, pattern = DGT, 
  replacement = c("X", ".", "*", "_"))
```

##### 4.8 Replacing with backreferences 
***Instruction:***

```{r}
# Build pattern to match words ending in "ING"
pattern <- one_or_more(WRD) %R% "ING"
str_view(narratives, pattern)

# Test replacement
str_replace(narratives, capture(pattern), 
  str_c("CARELESSLY", REF1, sep = " "))

# One adverb per narrative
adverbs_10 <- sample(adverbs, 10)
```

##### 4.9 Unicode and pattern matching (video)
##### 4.10 Matching a specific code point or code groups 
***Instruction:***

```{r}
# Names with builtin accents
(tay_son_builtin <- c(
  "Nguy\u1ec5n Nh\u1ea1c", 
  "Nguy\u1ec5n Hu\u1ec7",
  "Nguy\u1ec5n Quang To\u1ea3n"
))

# Convert to separate accents
tay_son_separate <- stri_trans_nfd(tay_son_builtin)

# Verify that the string prints the same
tay_son_separate

# Match all accents
str_view_all(tay_son_separate, pattern = UP_DIACRITIC)
```

##### 4.11 Matching a single grapheme 

***Instruction 1:***

```{r}
# tay_son_separate has been pre-defined
tay_son_separate

# View all the characters in tay_son_separate
str_view_all(tay_son_separate, pattern = ANY_CHAR)
```

***Instruction 2:***

```{r}
# View all the graphemes in tay_son_separate
str_view_all(tay_son_separate, pattern =  GRAPHEME)
```

***Instruction 3:***

```{r}
# Combine the diacritics with their letters
tay_son_builtin <- stri_trans_nfc(tay_son_separate)

# View all the graphemes in tay_son_builtin
str_view_all(tay_son_builtin, pattern = GRAPHEME)
```
## 5. Case Studies
##### 5.1 A case study, reading a play (video)
##### 5.2 Getting the play into R 
***Instruction 1:***

```{r}
# Read play in using stri_read_lines()
earnest <- stri_read_lines(earnest_file)
```

***Instruction 2:***

```{r}
# Read play in using stri_read_lines()
earnest <- stri_read_lines(earnest_file)


# Detect start and end lines
start <- which(str_detect(earnest, fixed("START OF THE PROJECT")))
end <- which(str_detect(earnest, fixed("END OF THE PROJECT")))

# Get rid of gutenberg intro text
earnest_sub  <- earnest[(start + 1):(end - 1)]
```

***Instruction 3:***

```{r}
# Read play in using stri_read_lines()
earnest <- stri_read_lines(earnest_file)

# Detect start and end lines
start <- str_which(earnest, fixed("START OF THE PROJECT"))
end <- str_which(earnest, fixed("END OF THE PROJECT"))

# Get rid of gutenberg intro text
earnest_sub  <- earnest[(start + 1):(end - 1)]

# Detect first act

lines_start <- which(str_detect(earnest_sub, fixed("FIRST ACT")))

# Set up index
intro_line_index <- 1:(lines_start - 1)

# Split play into intro and play
intro_text <- earnest_sub[intro_line_index]
play_text <- earnest_sub[-intro_line_index
```

***Instruction 4:***

```{r}
# Read play in using stri_read_lines()
earnest <- stri_read_lines(earnest_file)

# Detect start and end lines
start <- str_which(earnest, fixed("START OF THE PROJECT"))
end <- str_which(earnest, fixed("END OF THE PROJECT"))

# Get rid of gutenberg intro text
earnest_sub  <- earnest[(start + 1):(end - 1)]

# Detect first act
lines_start <- str_which(earnest_sub, fixed("FIRST ACT"))

# Set up index
intro_line_index <- 1:(lines_start - 1)

# Split play into intro and play
intro_text <- earnest_sub[intro_line_index]
play_text <- earnest_sub[-intro_line_index]

# Take a look at the first 20 lines
writeLines(play_text[1:20])
```
##### 5.3 Identifying the lines, take 1 

***Instruction 1:***

```{r}
# Pattern for start word then .
pattern_1 <- START %R% one_or_more(WRD) %R% DOT

# Test pattern_1
str_view(play_lines, pattern = pattern_1, 
  match = TRUE) #to see matched lines
str_view(play_lines, pattern = pattern_1, 
  match = FALSE) 
```

***Instruction 2:***

```{r}
# Pattern for start, capital, word then .
pattern_2 <- START %R% ascii_upper() %R% one_or_more(WRD) %R% DOT

# Test pattern_2
str_view(play_lines, pattern_2, match = TRUE)
str_view(play_lines, pattern_2, match = FALSE)
```

***Instruction 3:***

```{r}
# Pattern from last step
pattern_2 <- START %R% ascii_upper() %R% one_or_more(WRD) %R% DOT

# Get subset of lines that match
lines <- str_subset(play_lines, pattern = pattern_2)

# Extract match from lines
who <- str_extract(lines, pattern = pattern_2)

# Let's see what we have
unique(who)
```

##### 5.4 Identifying the lines, take 2 
***Instruction 1:***

```{r}
# Create vector of characters
characters <- c("Algernon", "Jack", "Lane", "Cecily", "Gwendolen", "Chasuble", 
  "Merriman", "Lady Bracknell", "Miss Prism")

# Match start, then character name, then .
pattern_3 <- START %R% or1(characters) %R% DOT

# View matches of pattern_3
str_view(play_lines, pattern = pattern_3, match = TRUE)
  
# View non-matches of pattern_3
str_view(play_lines, pattern = pattern_3, match = FALSE)
```

***Instruction 2:***

```{r}
# Variables from previous step
characters <- c("Algernon", "Jack", "Lane", "Cecily", "Gwendolen", "Chasuble", 
  "Merriman", "Lady Bracknell", "Miss Prism")
pattern_3 <- START %R% or1(characters) %R% DOT

# Pull out matches
lines <- str_subset(play_lines, pattern = pattern_3)

# Extract match from lines
who <- str_extract(lines, pattern = pattern_3)

# Let's see what we have
unique(who)

# Count lines per character
table(who)
```

##### 5.5 A case study on case (video)
##### 5.6 Changing case to ease matching 
***Instruction 1:***

```{r}
# catcidents has been pre-defined
head(catcidents)

# Construct pattern of DOG in boundaries
whole_dog_pattern <- whole_word("DOG")

# View matches to word "DOG"
str_view(catcidents, pattern = whole_dog_pattern, match = TRUE)
```

***Instruction 2:***

```{r}
# From previous step
whole_dog_pattern <- whole_word("DOG")

# Transform catcidents to upper case
catcidents_upper <- str_to_upper(catcidents)

# View matches to word "DOG" again
str_view(catcidents_upper, pattern = whole_dog_pattern, match = TRUE)
```

***Instruction 3:***

```{r}
# From previous steps
whole_dog_pattern <- whole_word("DOG")
catcidents_upper <- str_to_upper(catcidents)

# Which strings match?
has_dog <- str_detect(catcidents_upper, pattern = whole_dog_pattern)

# Pull out matching strings in original 
catcidents[has_dog]
```
##### 5.7 Ignoring case when matching 
***Instruction 1:***

```{r}
# View matches to "TRIP"
str_view(catcidents, pattern = "TRIP", match = TRUE)

# Construct case insensitive pattern
trip_pattern <- regex("TRIP", ignore_case = TRUE)

# View case insensitive matches to "TRIP"
str_view(catcidents, pattern = trip_pattern, match = TRUE)
```

***Instruction 2:***

```{r}
# From previous step
trip_pattern <- regex("TRIP", ignore_case = TRUE)

# Get subset of matches
trip <- str_subset(catcidents, pattern = trip_pattern)

# Extract matches
str_extract(trip, pattern = trip_pattern)
```

##### 5.8 Fixing case problems 
***Instruction:***

```{r}
library(stringi)

# Get first five catcidents
cat5 <- catcidents[1:5]

# Take a look at original
writeLines(cat5)

# Transform to title case
writeLines(str_to_title(cat5))

# Transform to title case with stringi
writeLines(stri_trans_totitle(cat5)) #same

# Transform to sentence case with stringi
writeLines(stri_trans_totitle(cat5, type = "sentence"))
```


##### 5.9 Wrapping up 
##### 5.10 An interview with Hadley Wickham








