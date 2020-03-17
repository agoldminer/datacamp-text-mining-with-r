# Text-Mining-DataCamp-Text Mining: Bag of Words

## 1. Jumping into Text Mining with Bag of Words

##### 1.1 What is text mining? (video)

##### 1.2 Understanding text mining

##### 1.3 Quick taste of text mining
***Instruction:***
We've created an object in your workspace called `new_text` containing several sentences.

- Load the `qdap` package.
- Print `new_text` to the console.
- Create `term_count` consisting of the 10 most frequent terms in `new_text`.
- Plot a bar chart with the results of `term_count`.

```{r}
# Load qdap
library(qdap)

# Print new_text to the console
new_text

# Find the 10 most frequent terms: term_count
term_count <- freq_terms(new_text, 10)

# Plot term_count
plot(term_count)
```

##### 1.4 Getting started (video)
##### 1.5 Load some text
The data has been loaded for you and is available in `coffee_data_file`.

- Create a new object `tweets` using `read.csv()` on the file `coffee_data_file`, which contains tweets mentioning coffee. Remember to add `stringsAsFactors = FALSE`!
- Examine the `tweets` object using `str()` to determine which column has the text you'll want to analyze.
- Make a new `coffee_tweets` object using only the text column you identified earlier. To do so, use the `$` operator and column name.


***Instruction:***

```{r}
# Import text data
tweets <- read.csv('https://assets.datacamp.com/production/course_935/datasets/coffee.csv', stringsAsFactors = F)

# View the structure of tweets
str(tweets)

# Isolate text from tweets: coffee_tweets
coffee_tweets <- tweets$text
```

##### 1.6 Make the vector a VCorpus object (1)
***Instruction:***
- Load the `tm` package.
- Create a Source object from the `coffee_tweets` vector. Call this new object `coffee_source`.

```{r}
# Load tm
library(tm)

# Make a vector source from coffee_tweets
coffee_source <- VectorSource(coffee_tweets)
```

##### 1.7 Make the vector a VCorpus object (2) 
***Instruction:***
- Call the `VCorpus()` function on the `coffee_source` object to create `coffee_corpus`.
- Verify `coffee_corpus` is a `VCorpus` object by printing it to the console.
- Print the 15th element of `coffee_corpus` to the console to verify that it's a `PlainTextDocument` that contains the content and metadata of the 15th tweet. Use double bracket subsetting.
- Print the content of the 15th tweet in `coffee_corpus`. Use double brackets to select the proper tweet, followed by single brackets to extract the content of that tweet.
- Print the `content()` of the 10th tweet within `coffee_corpus`

```{r}
## coffee_source is already in your workspace

# Make a volatile corpus: coffee_corpus
coffee_corpus <- VCorpus(coffee_source)

# Print out coffee_corpus
coffee_corpus

# Print the 15th tweet in coffee_corpus
coffee_corpus[[15]]

# Print the contents of the 15th tweet in coffee_corpus
coffee_corpus[[15]]$content

# Now use content to review plain text of the 10th tweet
content(coffee_corpus[[10]])
```

##### 1.8 Make a VCorpus from a data frame 
***Instruction:***
In your workspace, there's a simple data frame called `example_text` with the correct column names and some metadata. There is also `vec_corpus` which is a volatile corpus made with `VectorSource()`

- Create `df_source` using `DataframeSource()` with the example_text.
- Create `df_corpus` by converting `df_source` to a volatile corpus object with `VCorpus()`.
- Print out `df_corpus`. Notice how many documents it contains and the number of retained document level metadata points.
- Use `meta()` on `df_corpus` to print the document associated metadata.
- Examine the pre-loaded `vec_corpus` object. Compare the number of documents to `df_corpus`.
- Use `meta()` on `vec_corpus` to compare any metadata found between `vec_corpus` and `df_corpus`.

```{r}
# Create a DataframeSource from the example text
df_source <- DataframeSource(example_text)

# Convert df_source to a volatile corpus
df_corpus <- VCorpus(df_source)

# Examine df_corpus
df_corpus

# Examine df_corpus metadata
meta(df_corpus)

# Compare the number of documents in the vector source
vec_corpus

# Compare metadata in the vector corpus
meta(vec_corpus)
```

##### 1.9 Cleaning and preprocessing text (video)
##### 1.10 Common cleaning functions from tm 
***Instruction:***
Apply each of the following functions to `text`, simply printing results to the console:
- `tolower()`
- `removePunctuation()`
- `removeNumbers()`
- `stripWhitespace()`

```{r}
# Create the object: text
text <- "<b>She</b> woke up at       6 A.M. It\'s so early!  She was only 10% awake and began drinking coffee in front of her computer."

# Make lowercase
tolower(text)

# Remove punctuation
removePunctuation(text)

# Remove numbers
removeNumbers(text)

# Remove whitespace
stripWhitespace(text)
```

##### 1.11 Cleaning with qdap 
***Instruction:***
Apply the following functions to the `text` object from the previous exercise:

- bracketX()
- replace_number()
- replace_abbreviation()
- replace_contraction()
- replace_symbol()

```{r}
## text is still loaded in your workspace

# Remove text within brackets
bracketX(text)

# Replace numbers with words
replace_number(text)

# Replace abbreviations
replace_abbreviation(text)

# Replace contractions
replace_contraction(text)

# Replace symbols with words
replace_symbol(text)
```
##### 1.12 All about stop words
***Instruction:***

- Review standard stop words by calling `stopwords("en")`.
- Remove "en" stopwords from `text`.
- Add "coffee" and "bean" to the standard stop words, assigning to `new_stops`.
- Remove the customized stopwords, `new_stops`, from `text`.

```{r}
## text is preloaded into your workspace

# List standard English stop words
stopwords("en")

# Print text without standard stop words
removeWords(text, stopwords("en"))

# Add "coffee" and "bean" to the list: new_stops
new_stops <- c("coffee", "bean", stopwords("en"))

# Remove stop words from text
removeWords(text, new_stops)
```
##### 1.13 Intro to word stemming and stem completion
***Instruction:***
- Create a vector called `complicate` consisting of the words "complicated", "complication", and "complicatedly" in that order.
- Store the stemmed version of `complicate` to an object called `stem_doc`.
- Create comp_dict that contains one word, "complicate".
- Create `complete_text` by applying `stemCompletion()` to `stem_doc`. Re-complete the words using `comp_dict` as the reference corpus.
- Print `complete_text` to the console.

```{r}
# Create complicate
complicate <- c("complicated", "complication", "complicatedly")

# Perform word stemming: stem_doc
stem_doc <- stemDocument(complicate)

# Create the completion dictionary: comp_dict
comp_dict <- c("complicate")

# Perform stem completion: complete_text 
complete_text <- stemCompletion(stem_doc, comp_dict)

# Print complete_text
complete_text
```

##### 1.14 Word stemming and stem completion on a sentence
***Instruction:***
The document `text_data` and the completion dictionary `comp_dict` are loaded in your workspace.

- Remove the punctuation marks in `text_data` using `removePunctuation()`, assigning to `rm_punc`.
- Call `strsplit()` on `rm_punc` with the `split` argument set equal to `" "`. Nest this inside `unlist()`, assigning to `n_char_vec`.
- Use `stemDocument()` again to perform word stemming on `n_char_vec`, assigning to `stem_doc`.
- Create `complete_doc` by re-completing your stemmed document with `stemCompletion()` and using `comp_dict` as your reference corpus.

Are `stem_doc` and `complete_doc` what you expected?

```{r}
# Remove punctuation: rm_punc
rm_punc <- removePunctuation(text_data)

# Create character vector: n_char_vec
n_char_vec <- unlist(strsplit(rm_punc, split = ' '))

# Perform word stemming: stem_doc
stem_doc <- stemDocument(n_char_vec) 

# Print stem_doc
stem_doc

# Re-complete stemmed document: complete_doc
complete_doc <- stemCompletion(stem_doc, comp_dict)

# Print complete_doc
complete_doc
```
##### 1.15 Apply preprocessing steps to a corpus 
***Instruction 1:***
- Edit the custom function `clean_corpus()` in the sample code to apply (in order):
  - `tm`'s `removePunctuation()`.
  - Base R's `tolower()`.
  - Append `"mug"` to the stop words list.
  - `tm`'s `stripWhitespace()`.

```{r}
# Alter the function code to match the instructions
clean_corpus <- function(corpus) {
  # Remove punctuation
  corpus <- tm_map(corpus, removePunctuation)
  # Transform to lower case
  corpus <- tm_map(corpus, content_transformer(tolower))
  # Add more stopwords
  corpus <- tm_map(corpus, removeWords, words = c(stopwords("en"), "coffee", "mug"))
  # Strip whitespace
  corpus <- tm_map(corpus, stripWhitespace)
  return(corpus)
}
```

***Instruction 2:***

- Create `clean_corp` by applying `clean_corpus()` to the included corpus `tweet_corp`.
- Print the cleaned 227th tweet in `clean_corp` using indexing `[[227]]` and `content()`.
- Compare it to the original tweet from `tweets$text` tweet using `[227]`.

```{r}
# Alter the function code to match the instructions
clean_corpus <- function(corpus){
  corpus <- tm_map(corpus, removePunctuation)
  corpus <- tm_map(corpus, content_transformer(tolower))
  corpus <- tm_map(corpus, removeWords, words = c(stopwords("en"), "coffee", "mug"))
  corpus <- tm_map(corpus, stripWhitespace)
  return(corpus)
}

# Apply your customized function to the tweet_corp: clean_corp
clean_corp <- clean_corpus(tweet_corp)

# Print out a cleaned up tweet
clean_corp[[227]][1]

# Print out the same tweet in original form
tweet_corp[[227]][1]
```

##### 1.16 The TDM & DTM (video)
##### 1.17 Understanding TDM and DTM 
##### 1.18 Make a document-term matrix 
***Instruction:***
- Create `coffee_dtm` by applying `DocumentTermMatrix()` to `clean_corp`.
- Create `coffee_m`, a matrix version of `coffee_dtm`, using `as.matrix()`.
- Print the dimensions of `coffee_m` to the console using the `dim()` function. Note the number of rows and columns.
- Print the subset of `coffee_m` containing documents (rows) 25 through 35 and terms (columns) `"star"` and `"starbucks"`.

```{r}
# Create the document-term matrix from the corpus
coffee_dtm <- DocumentTermMatrix(clean_corp)

# Print out coffee_dtm data
coffee_dtm

# Convert coffee_dtm to a matrix: coffee_m
coffee_m <- as.matrix(coffee_dtm)

# Print the dimensions of coffee_m
dim(coffee_m)

# Review a portion of the matrix to get some Starbucks
coffee_m[25:35, c("star", "starbucks")]
```

##### 1.19 Make a term-document matrix 
***Instruction:***

- Create `coffee_tdm` by applying `TermDocumentMatrix()` to `clean_corp`.
- Create `coffee_m` by converting `coffee_tdm` to a matrix using `as.matrix()`.
- Print the dimensions of `coffee_m` to the console. Note the number of rows and columns.
- Print the subset of `coffee_m` containing terms (rows) `"star"` and `"starbucks"` and documents (columns) 25 through 35.

```{r}
# Create a term-document matrix from the corpus
coffee_tdm <- TermDocumentMatrix(clean_corp)

# Print coffee_tdm data
coffee_tdm

# Convert coffee_tdm to a matrix: coffee_m
coffee_m <- as.matrix(coffee_tdm)

# Print the dimensions of the matrix
dim(coffee_m)

# Review a portion of the matrix
coffee_m[c("star", "starbucks"), 25:35]
```

## 2. Word Clouds and More Interesting Visuals
##### 2.1 Common text mining visuals (video)
##### 2.2 Test your understanding of text mining 
##### 2.3 Frequent terms with tm
***Instruction:***

- Create `coffee_m` as a matrix using the term-document matrix `coffee_tdm` from the last chapter.
- Create `term_frequency` using the `rowSums()` function on `coffee_m`.
- Sort `term_frequency` in descending order and store the result in `term_frequency`.
- Use single square bracket subsetting, i.e. using only one `[`, to print the top 10 terms from `term_frequency`.
- Make a barplot of the top 10 terms.

```{r}
## coffee_tdm is still loaded in your workspace

# Convert coffee_tdm to a matrix
coffee_m <- as.matrix(coffee_tdm)

# Calculate the row sums of coffee_m
term_frequency <- rowSums(coffee_m)

# Sort term_frequency in decreasing order
term_frequency <- sort(term_frequency, decreasing = T)

# View the top 10 most common words
term_frequency[1:10]

# Plot a barchart of the 10 most common words
barplot(term_frequency[1:10], col = "tan", las = 2)
```

##### 2.4 Frequent terms with qdap 
***Instruction 1:***
- Create `frequency` using the `freq_terms()` function on `tweets$text`. Include arguments to accomplish the following:
  - Limit to the top 10 terms.
  - At least 3 letters per term.
  - Use `"Top200Words"` to define stop words.
- Produce a `plot()` of the `frequency` object. Compare it to the plot you produced in the previous exercise.

```{r}
# Create frequency
frequency <- freq_terms(
  tweets$text, 
  top = 10, 
  at.least = 3, 
  stopwords = "Top200Words"
)

# Make a frequency barchart
plot(frequency)
```

***Instruction 2:***

- Again, create `frequency` using the `freq_terms()` function on `tweets$text`. Include the following arguments:
  - Limit to the top 10 terms.
  - At least 3 letters per term.
  - This time use `stopwords("english")` to define stop words.
- Produce a `plot()` of `frequency`. Compare it to the plot of `frequency`. Do certain words change based on the stop words criterion?

```{r}
# Create frequency
frequency <- freq_terms(tweets$text,
  top = 10,
  at.least = 3, 
  stopwords = stopwords("english")
  )

# Make a frequency barchart
plot(frequency)
```
##### 2.5 Intro to word clouds (video)
##### 2.6 A simple word cloud
***Instruction:***

- Load the `wordcloud` package.
- Print out first 10 entries in `term_frequency`.
- Extract the terms using `names()` on `term_frequency`. Call the vector of strings `terms_vec`.
- Create a `wordcloud()` using `terms_vec` as the words, and `term_frequency` as the values. Add the parameters `max.words = 50` and `colors = "red"`.

```{r}
# Load wordcloud package
library(wordcloud)

# Print the first 10 entries in term_frequency
term_frequency[1:10]

# Vector of terms
terms_vec <- names(term_frequency)


# Create a wordcloud for the values in word_freqs
wordcloud(terms_vec, term_frequency, max.words = 50, colors = "red")
```
##### 2.7 Stop words and word clouds
***Instruction:***

- Apply `content()` to the 24th document in `chardonnay_corp`.
- Append `"chardonnay"` to the English stopwords, assigning to `stops`.
- Examine the last 6 words in `stops`.
- Create `cleaned_chardonnay_corp` with `tm_map()` by passing in the `chardonnay_corp`, the function `removeWords` and finally the stopwords, `stops`.
- Now examine the `content` of the `24` tweet again to compare results.

```{r}
# Review a "cleaned" tweet
content(chardonnay_corp[[24]])

# Add to stopwords
stops <- c(stopwords(kind = 'en'), 'chardonnay')

# Review last 6 stopwords 
tail(stops)

# Apply to a corpus
cleaned_chardonnay_corp <- tm_map(chardonnay_corp, removeWords, stops)

# Review a "cleaned" tweet again
content(cleaned_chardonnay_corp[[24]])
```

##### 2.8 Plot the better word cloud
***Instruction:***

We've loaded the `wordcloud` package for you behind the scenes and will do so for all additional exercises requiring it.

- Sort the values in `chardonnay_words` with `decreasing = TRUE`. Save as `sorted_chardonnay_words`.
- Look at the top 6 words in `sorted_chardonnay_words` and their values.
- Create `terms_vec` using `names()` on `chardonnay_words`.
- Pass `terms_vec` and `chardonnay_words` into the `wordcloud()` function. Review what other words pop out now that "chardonnay" is removed.

```{r}
# Sort the chardonnay_words in descending order
sorted_chardonnay_words <- sort(chardonnay_words, decreasing = TRUE)

# Print the 6 most frequent chardonnay terms
head(sorted_chardonnay_words)

# Get a terms vector
terms_vec <- names(chardonnay_words)

# Create a wordcloud for the values in word_freqs
wordcloud(terms_vec, chardonnay_words, max.words = 50, colors = "red")
```

##### 2.9 Improve word cloud colors
***Instruction:***
- Call the `colors()` function to list all basic colors.
- Create a `wordcloud()` using the predefined `chardonnay_freqs` with the colors "grey80", "darkgoldenrod1", and "tomato". Include the top 100 terms using `max.words`.

```{r}
# Print the list of colors
colors()

# Print the wordcloud with the specified colors
wordcloud(chardonnay_freqs$term, 
          chardonnay_freqs$num,
          max.words = 100, 
          colors = c("grey80","darkgoldenrod1", "tomato"))
```

##### 2.10 Use prebuilt color palettes
***Instruction:***
- Use `cividis()` to select `5` colors in an object called `color_pal`.
- Review the hexadecimal colors by printing `color_pal` to your console.
- Create a `wordcloud()` from the `chardonnay_freqs` `term` and `num` columns. Include the top 100 terms using `max.words`, and set the `colors` to your palette, `color_pal`.

```{r}
# Select 5 colors 
color_pal <- cividis(5)

# Examine the palette output
color_pal

# Create a wordcloud with the selected palette
wordcloud(chardonnay_freqs$term, 
          chardonnay_freqs$num,
          max.words = 100, 
          colors = color_pal)
```

##### 2.11 Other word clouds and word networks (video)
##### 2.12 Find common words
***Instruction:***
- Create `all_coffee` by using `paste()` with `collapse = " "` on `coffee_tweets$text`.
- Create `all_chardonnay` by using `paste()` with `collapse = " "` on `chardonnay_tweets$text`.
- Create `all_tweets` using `c()` to combine `all_coffee` and `all_chardonnay`. Make `all_coffee` the first term.
- Convert `all_tweets` using `VectorSource()`.
- Create `all_corpus` by using `VCorpus()` on `all_tweets`.

```{r}
# Create all_coffee
all_coffee=paste(coffee_tweets$text,collapse=" ")

# Create all_chardonnay
all_chardonnay=paste(chardonnay_tweets$text,collapse=" ")

# Create all_tweets
all_tweets=c(all_coffee,all_chardonnay)

# Convert to a vector source
all_tweets=VectorSource(all_tweets)

# Create all_corpus
all_corpus=VCorpus(all_tweets)
```

##### 2.13 Visualize common words
***Instruction:***

- Create `all_clean` by applying the predefined `clean_corpus()` function to `all_corpus`.
- Create `all_tdm`, a `TermDocumentMatrix` from all_clean.
- Create `all_m` by converting `all_tdm` to a matrix object.
- Create a `commonality.cloud()` from `all_m` with `max.words = 100` and `colors = "steelblue1"`.

```{r}
# Clean the corpus
all_clean <- clean_corpus(all_corpus)

# Create all_tdm
all_tdm <- TermDocumentMatrix(all_clean)

# Create all_m
all_m <- as.matrix(all_tdm)

# Print a commonality cloud
commonality.cloud(all_m, max.words = 100, colors = "steelblue1")
```

##### 2.14 Visualize dissimilar words
***Instruction:***

`all_corpus` is preloaded in your workspace.

- Create `all_clean` by applying the predefined `clean_corpus` function to `all_corpus`.
- Create `all_tdm`, a `TermDocumentMatrix`, from `all_clean`.
- Use `colnames()` to rename each distinct corpora within `all_tdm`. Name the first column "coffee" and the second column "chardonnay".
- Create `all_m` by converting `all_tdm` into matrix form.
- Create a `comparison.cloud()` using `all_m`, with `colors = c("orange", "blue")` and `max.words = 50`.

```{r}
# Clean the corpus
all_clean <- clean_corpus(all_corpus)

# Create all_tdm
all_tdm <- TermDocumentMatrix(all_clean)

# Give the columns distinct names
colnames(all_tdm) <- c("coffee", "chardonnay")

# Create all_m
all_m <- as.matrix(all_tdm)

# Create comparison cloud
comparison.cloud(all_m,
                 colors = c("orange", "blue"),
                 max.words = 50)
```
##### 2.15 Polarized tag cloud
***Instruction 1:***

- Convert `all_tdm_m` to a data frame. Set the rownames to a column named `"word"`.
- Filter to keep rows where all columns are greater than zero, using the syntax `. > 0`.
- Add a column named `difference`, equal to the count in the `chardonnay` column minus the count in the `coffee` column.
- Filter to keep the top `25` rows by `difference`.
- Arrange the rows by `desc()`ending order of `difference`.

```{r}
top25_df <- all_tdm_m %>%
  # Convert to data frame
  as_data_frame(rownames = "word") %>% 
  # Keep rows where word appears everywhere
  filter_all(all_vars(. > 0)) %>% 
  # Get difference in counts
  mutate(difference = chardonnay - coffee) %>% 
  # Keep rows with biggest difference
  top_n(25, wt = difference) %>% 
  # Arrange by descending difference
  arrange(desc(difference))
```

***Instruction 2:***

- Set the left count to the `chardonnay` column.
- Set the right count to the `coffee` column.
- Set the labels to the `word` column.

```{r}
top25_df <- all_tdm_m %>%
  # Convert to data frame
  as_data_frame(rownames = "word") %>% 
  # Keep rows where word appears everywhere
  filter_all(all_vars(. > 0)) %>% 
  # Get difference in counts
  mutate(difference = chardonnay - coffee) %>% 
  # Keep rows with biggest difference
  top_n(25, wt = difference) %>% 
  # Arrange by descending difference
  arrange(desc(difference))
  
pyramid.plot(
  # Chardonnay counts
  top25_df$chardonnay, 
  # Coffee counts
  top25_df$coffee, 
  # Words
  labels = top25_df$word, 
  top.labels = c("Chardonnay", "Words", "Coffee"), 
  main = "Words in Common", 
  unit = NULL,
  gap = 8,
)
```
##### 2.16 Visualize word networks
***Instruction:***

Update the `word_associate()` plotting code to work with the coffee data.

- Change the vector to `coffee_tweets$text`.
- Change the match string to `"barista"`.
- Change `"chardonnay"` to `"coffee"` in the stopwords too.
- Change the title to `"Barista Coffee Tweet Associations"` in the sample code for the plot.

```{r}
# Word association
word_associate(coffee_tweets$text, match.string = "barista", 
               stopwords = c(Top200Words, "coffee", "amp"), 
               network.plot = TRUE, cloud.colors = c("gray85", "darkred"))

# Add title
title(main = "Barista Coffee Tweet Associations")
```

##### 2.17 Teaser simple word clustering 
***Instruction:***

A hierarchical cluster object, `hc`, has been created for you from the coffee tweets.

Create a dendrogram using `plot()` on `hc`.

```{r}
# Plot a dendrogram
plot(hc)
```

## 3. Adding to Your tm Skills
##### 3.1 Simple word clustering (video)
##### 3.2 Test your understanding of text mining 
##### 3.3 Distance matrix and dendrogram 
***Instruction:***
The data frame `rain` has been preloaded in your workspace.

- Create `dist_rain` by using the `dist()` function on the values in the second column of rain.
- Print the `dist_rain` matrix to the console.
- Create `hc` by performing a cluster analysis, using `hclust()` on `dist_rain`.
- `plot()` the `hc` object with `labels = rain$city` to add the city names.

```{r}
# Create dist_rain
dist_rain <- dist(rain[, 2])

# View the distance matrix
dist_rain

# Create hc
hc <- hclust(dist_rain)

# Plot hc
plot(hc, labels = rain$city)
```

##### 3.4 Make a dendrogram friendly TDM 
***Instruction:***

`tweets_tdm` has been created using the chardonnay tweets.

- Print the dimensions of `tweets_tdm` to the console.
- Create `tdm1` using `removeSparseTerms()` with `sparse = 0.95` on `tweets_tdm`.
- Create `tdm2` using `removeSparseTerms()` with `sparse = 0.975` on `tweets_tdm`.
- Print `tdm1` to the console to see how many terms are left.
- Print `tdm2` to the console to see how many terms are left.

```{r}
# Print the dimensions of tweets_tdm
dim(tweets_tdm)

# Create tdm1
tdm1 <- removeSparseTerms(tweets_tdm, sparse = 0.95)

# Create tdm2
tdm2 <- removeSparseTerms(tweets_tdm, sparse = 0.975)

# Print tdm1
tdm1

# Print tdm2
tdm2
```

##### 3.5 Put it all together: a tea based dendrogram
***Instruction:***

- Create `tweets_tdm2` by applying `removeSparseTerms()` on `tweets_tdm`. Use `sparse = 0.975`.
- Create `tdm_m` by using `as.matrix()` on `tweets_tdm2` to convert it to matrix form.
- Create `tweets_dist` containing the distances of `tdm_m` using the `dist()` function.
- Create a hierarchical cluster object called `hc` using `hclust()` on `tweets_dist`.
Make a dendrogram with `plot()` and `hc`.

```{r}
# Create tweets_tdm2
tweets_tdm2 <- removeSparseTerms(tweets_tdm, sparse = 0.975)

# Create tdm_m
tdm_m <- as.matrix(tweets_tdm2)

# Create tweets_dist
tweets_dist <- dist(tdm_m)

# Create hc
hc <- hclust(tweets_dist)

# Plot the dendrogram
plot(hc)
```

##### 3.6 Dendrogram aesthetics 
***Instruction:***

The `dendextend` package has been loaded for you, and a hierarchical cluster object, `hc`, was created from `tweets_dist`.

- Create `hcd` as a dendrogram using `as.dendrogram()` on `hc`.
- Print the `labels` of `hcd` to the console.
- Use `branches_attr_by_labels()` to color the branches. Pass it three arguments: the `hcd` object, `c("marvin", "gaye")`, and the color `"red"`. Assign to `hcd_colored`.
- `plot()` the dendrogram `hcd_colored` with the title `"Better Dendrogram"`, added using the `main` argument.
- Add rectangles to the plot using `rect.dendrogram()`. Specify `k = 2` clusters and a `border` color of `"grey50"`.

```{r}
# Create hcd
hcd <- as.dendrogram(hc)

# Print the labels in hcd
labels(hcd)

# Change the branch color to red for "marvin" and "gaye"
hcd_colored <- branches_attr_by_labels(hcd, c("marvin", "gaye"), color = "red")

# Plot hcd
plot(hcd_colored, main = "Better Dendrogram")

# Add cluster rectangles 
rect.dendrogram(hcd_colored, k = 2, border = "grey50")
```

##### 3.7 Using word association 
***Instruction:***

- Create `associations` using `findAssocs()` on `tweets_tdm` to find terms associated with "venti", which meet a minimum threshold of `0.2`.
- View the terms associated with "venti" by printing `associations` to the console.
- Create `associations_df`, by calling `list_vect2df()`, passing `associations`, then setting `col2` to `"word"` and `col3` to `"score"`.
- Run the `ggplot2` code to make a dot plot of the association values.

```{r}
# Create associations
associations <- findAssocs(tweets_tdm, "venti", 0.2)

# View the venti associations
associations

# Create associations_df
associations_df <- list_vect2df(associations, col2 = "word", col3 = "score")

# Plot the associations_df values
ggplot(associations_df, aes(score, word)) + 
  geom_point(size = 3) + 
  theme_gdocs()
```

##### 3.8 Getting past single words (video)
##### 3.9 N-gram tokenization 
##### 3.10 Changing n-grams 
***Instruction:***

A `corpus` has been preprocessed as before using the chardonnay tweets. The resulting object `text_corp` is available in your workspace.

- Create a `tokenizer` function like the above which creates 2-word bigrams.
- Make `unigram_dtm` by calling `DocumentTermMatrix()` on `text_corp` without using the `tokenizer()` function.
- Make `bigram_dtm` using `DocumentTermMatrix()` on `text_corp` with the `tokenizer()` function you just made.
- Examine `unigram_dtm` and `bigram_dtm`. Which has more terms?


```{r}
# Make tokenizer function 
tokenizer <- function(x)
  NGramTokenizer(x, Weka_control(min = 2, max = 2))

# Create unigram_dtm
unigram_dtm <- DocumentTermMatrix(text_corp)

# Create bigram_dtm
bigram_dtm <- DocumentTermMatrix(
  text_corp,
  control = list(tokenize = tokenizer))

# Print unigram_dtm
unigram_dtm

# Print bigram_dtm
bigram_dtm
```

##### 3.11 How do bigrams affect word clouds? 
***Instruction:***
The chardonnay tweets have been cleaned and organized into a DTM called `bigram_dtm`.

- Create `bigram_dtm_m` by converting `bigram_dtm` to a matrix.
- Create an object `freq` consisting of the word frequencies by applying `colSums()` on `bigram_dtm_m`.
- Extract the character vector of word combinations with `names(freq)` and assign the result to `bi_words`.
- Pass `bi_words` to `str_subset()` with the matching pattern `"^marvin"` to review all bigrams starting with "marvin".
- Plot a simple `wordcloud()` passing `bi_words`, `freq` and `max.words = 15` into the function.

```{r}
# Create bigram_dtm_m
bigram_dtm_m <- as.matrix(bigram_dtm)

# Create freq
freq <- colSums(bigram_dtm_m)

# Create bi_words
bi_words <- names(freq)

# Examine part of bi_words
str_subset(bi_words, pattern = "^marvin")

# Plot a wordcloud
wordcloud(bi_words, freq, max.words = 15)
```

##### 3.12 Different frequency criteria (video)
##### 3.13 Changing frequency weights 
***Instruction 1:***
- Create `tdm`, a term frequency-based `TermDocumentMatrix()` using `text_corp`.
- Create `tdm_m` by converting `tdm` to matrix form.
- Examine the term frequency for "coffee", "espresso", and "latte" in a few tweets. Subset `tdm_m` to get rows `c("coffee", "espresso", "latte")` and columns 161 to 166.

```{r}
# Create a TDM
tdm <- TermDocumentMatrix(text_corp)

# Convert it to a matrix
tdm_m <- as.matrix(tdm)

# Examine part of the matrix
tdm_m[c("coffee", "espresso", "latte"), 161:166]
```

***Instruction 2:***

- Edit the `TermDocumentMatrix()` to use `TfIdf` weighting. Pass `control = list(weighting = weightTfIdf)` as an argument to the function.
- Run the code and compare the new scores to the first part of the exercise.

```{r}
# Edit the controls to use Tfidf weighting
tdm <- TermDocumentMatrix(text_corp, 
control = list(weighting = weightTfIdf))

# Convert to matrix again
tdm_m <- as.matrix(tdm)

# Examine the same part: how has it changed?
tdm_m[c("coffee", "espresso", "latte"), 161:166]
```

##### 3.14 Capturing metadata in tm 
***Instruction:***

- Rename the first column of `tweets` to "doc_id".
- Set the document schema with `DataframeSource()` on the smaller `tweets` data frame.
- Make the document collection a volatile corpus nested in the custom `clean_corpus()` function.
- Apply `content()` to the first tweet with double brackets such as `text_corpus[[1]]` to see the cleaned plain text.
- Confirm that all metadata was captured using the `meta()` function on the first document with single brackets.

Remember, when accessing part of a corpus the double or single brackets make a difference! For this exercise you will use double brackets with `content()` and single brackets with `meta()`.

```{r}
# Rename columns
names(tweets)[1] <- "doc_id"

# Set the schema: docs
docs <- DataframeSource(tweets)

# Make a clean volatile corpus: text_corpus
text_corpus <- clean_corpus( VCorpus(docs))

# Examine the first doc content
content(text_corpus[[1]])

# Access the first doc metadata
meta(text_corpus[1])
```

## 4. Battle of the Tech Giants for Talent
##### 4.1 Amazon vs. Google (video)
##### 4.2 Organizing a text mining project 
##### 4.3 Step 1: Problem definition 
##### 4.4 Step 2: Identifying the text sources 
***Instruction:***

- View the structure of `amzn` with `str()` to get its dimensions and a preview of the data.
- Create `amzn_pros` from the positive reviews column `amzn$pros`.
- Create `amzn_cons` from the negative reviews column `amzn$cons`.
- Print the structure of `goog` with `str()` to get its dimensions and a preview of the data.
- Create `goog_pros` from the positive reviews column `goog$pros`.
- Create `goog_cons` from the negative reviews column `goog$cons`.

```{r}
# Print the structure of amzn
str(amzn)

# Create amzn_pros
amzn_pros <- amzn$pros

# Create amzn_cons
amzn_cons <- amzn$cons

# Print the structure of goog
str(goog)

# Create goog_pros
goog_pros <- goog$pros

# Create goog_cons
goog_cons <- goog$cons
```
##### 4.5 Step 3:Text organization (video)
##### 4.6 Text organization 
***Instruction 1:***
- Apply `qdap_clean()` to `amzn_pros`, assigning to `qdap_cleaned_amzn_pros`.
- Create a vector source (`VectorSource()`) from `qdap_cleaned_amzn_pros`, then turn it into a volatile corpus (`VCorpus()`), assigning to `amzn_p_corp`.
- Create `amzn_pros_corp` by applying `tm_clean()` to `amzn_p_corp`.

```{r}
# qdap_clean the text
qdap_cleaned_amzn_pros <- qdap_clean(amzn_pros)

# Source and create the corpus
amzn_p_corp <- VCorpus(VectorSource(qdap_cleaned_amzn_pros))

# tm_clean the corpus
amzn_pros_corp <- tm_clean(amzn_p_corp)
```

***Instruction 2:***
- Apply `qdap_clean()` to `amzn_cons`, assigning to `qdap_cleaned_amzn_cons`.
- Create a vector source from `qdap_cleaned_amzn_cons`, then turn it into a volatile corpus, assigning to `amzn_c_corp`.
- Create `amzn_cons_corp` by applying `tm_clean()` to `amzn_c_corp`.

```{r}
# qdap_clean the text
qdap_cleaned_amzn_cons <- qdap_clean(amzn_cons)

# Source and create the corpus
amzn_c_corp <- VCorpus(VectorSource(qdap_cleaned_amzn_cons))

# tm_clean the corpus
amzn_cons_corp <- tm_clean(amzn_c_corp)
```
##### 4.7 Working with Google reviews 
***Instruction 1:***

- Apply `qdap_clean()` to `goog_pros`, assigning to `qdap_cleaned_goog_pros`.
- Create a vector source (`VectorSource()`) from `qdap_cleaned_goog_pros`, then turn it into a volatile corpus (`VCorpus()`), assigning to `goog_p_corp`.
- Create `goog_pros_corp` by applying `tm_clean()` to `goog_p_corp`.

```{r}
# qdap_clean the text
qdap_cleaned_goog_pros <- qdap_clean(goog_pros)

# Source and create the corpus
goog_p_corp <- VCorpus(VectorSource(qdap_cleaned_goog_pros))

# tm_clean the corpus
goog_pros_corp <- tm_clean(goog_p_corp)
```

***Instruction 2:***

- Apply `qdap_clean()` to `goog_cons`, assigning to `qdap_cleaned_goog_cons`.
- Create a vector source from `qdap_cleaned_goog_cons`, then turn it into a volatile corpus, assigning to `goog_c_corp`.
- Create `goog_cons_corp` by applying `tm_clean()` to `goog_c_corp`.

```{r}
# qdap clean the text
qdap_cleaned_goog_cons <- qdap_clean(goog_cons)

# Source and create the corpus
goog_c_corp <- VCorpus(VectorSource(qdap_cleaned_goog_cons))

# tm clean the corpus
goog_cons_corp <- tm_clean(goog_c_corp)
```

##### 4.8 Steps 4 & 5: Feature extraction & analysis 
##### 4.9 Feature extraction & analysis: amzn_pros 

***Instruction:***

- Create `amzn_p_tdm` as a `TermDocumentMatrix` from `amzn_pros_corp`. Make sure to add `control = list(tokenize = tokenizer)` so that the terms are bigrams.
- Create `amzn_p_tdm_m` from `amzn_p_tdm` by using the `as.matrix()` function.
- Create `amzn_p_freq` to obtain the term frequencies from `amzn_p_tdm_m`.
- Create a `wordcloud()` using `names(amzn_p_freq)` as the words, `amzn_p_freq` as their frequencies, and `max.words = 25` and `color = "blue"` for aesthetics.

```{r}
# Create amzn_p_tdm
amzn_p_tdm <- TermDocumentMatrix(
  amzn_pros_corp, 
  control = list(tokenize = tokenizer)
  )

# Create amzn_p_tdm_m
amzn_p_tdm_m <- as.matrix(amzn_p_tdm)

# Create amzn_p_freq
amzn_p_freq <- rowSums(amzn_p_tdm_m)

# Plot a wordcloud using amzn_p_freq values
wordcloud(names(amzn_p_freq),
  amzn_p_freq,
  max.words = 25, 
  color = "blue")
```
##### 4.10 Feature extraction & analysis: amzn_cons 
***Instruction:***

Create `amzn_c_tdm` by converting `amzn_cons_corp` into a `TermDocumentMatrix` and incorporating the bigram function `control = list(tokenize = tokenizer)`.
Create `amzn_c_tdm_m` as a matrix version of `amzn_c_tdm`.
Create `amzn_c_freq` by using `rowSums()` to get term frequencies from `amzn_c_tdm_m`.
Create a `wordcloud()` using `names(amzn_c_freq)` and the values `amzn_c_freq`. Use the arguments `max.words = 25` and `color = "red"` as well.

```{r}
# Create amzn_c_tdm
amzn_c_tdm <- TermDocumentMatrix(
  amzn_cons_corp,
  control = list(tokenize = tokenizer))

# Create amzn_c_tdm_m
amzn_c_tdm_m <- as.matrix(amzn_c_tdm)

# Create amzn_c_freq
amzn_c_freq <- rowSums(amzn_c_tdm_m)

# Plot a wordcloud of negative Amazon bigrams
wordcloud(names(amzn_c_freq), amzn_c_freq,
  max.words = 25, color = "red")
```
##### 4.11 amzn_cons dendrogram 
***Instruction:***

- Create `amzn_c_tdm` as a `TermDocumentMatrix` using `amzn_cons_corp` with `control = list(tokenize = tokenizer)`.
- Print `amzn_c_tdm` to the console.
- Create `amzn_c_tdm2` by applying the `removeSparseTerms()` function to `amzn_c_tdm` with the sparse argument equal to `.993`.
- Create `hc`, a hierarchical cluster object by nesting the distance matrix `dist(amzn_c_tdm2)` inside the `hclust()` function. Make sure to also pass `method = "complete"` to the `hclust()` function.
- Plot `hc` to view the clustered bigrams and see how the concepts in the Amazon cons section may lead you to a conclusion.

```{r}
# Create amzn_c_tdm
amzn_c_tdm <- TermDocumentMatrix(
  amzn_cons_corp, 
  control = list(tokenize = tokenizer))

# Print amzn_c_tdm to the console
amzn_c_tdm

# Create amzn_c_tdm2 by removing sparse terms 
amzn_c_tdm2 <- removeSparseTerms(amzn_c_tdm, sparse = .993)

# Create hc as a cluster of distance values
hc <- hclust(
  d = dist(amzn_c_tdm2), method = "complete")

# Produce a plot of hc
plot(hc)
```

##### 4.12 Word association 
***Instruction:***
The `amzn_pros_corp` corpus has been cleaned using the custom functions like before.

Construct a TDM called `amzn_p_tdm` from `amzn_pros_corp` and `control = list(tokenize = tokenizer)`.
Create `amzn_p_m` by converting `amzn_p_tdm` to a matrix.
Create `amzn_p_freq` by applying `rowSums()` to `amzn_p_m`.
Create `term_frequency` using `sort()` on `amzn_p_freq` along with the argument `decreasing = TRUE`.
Examine the first 5 bigrams using `term_frequency[1:5]`.
You may be surprised to see "fast paced" as a top term because it could be a negative term related to "long hours". Look at the terms most associated with "fast paced". Use `findAssocs()` on `amzn_p_tdm` to examine `"fast paced"` with a `0.2` cutoff.

```{r}
# Create amzn_p_tdm
amzn_p_tdm <- TermDocumentMatrix(
  amzn_pros_corp,
  control = list(tokenize = tokenizer)
)

# Create amzn_p_m
amzn_p_m <- as.matrix(amzn_p_tdm)

# Create amzn_p_freq
amzn_p_freq <- rowSums(amzn_p_m)

# Create term_frequency
term_frequency <- sort(amzn_p_freq, decreasing = T)

# Print the 5 most common terms
term_frequency[1:5]

# Find associations with fast paced
associations <- findAssocs(amzn_p_tdm, "fast paced", 0.2)
associations
```

##### 4.13 Quick review of Google reviews 
***Instruction:***
The `all_goog_corpus` object consisting of Google pro and con reviews is loaded in your workspace.

Create `all_goog_corp` by cleaning `all_goog_corpus` with the predefined `tm_clean()` function.
Create `all_tdm` by converting `all_goog_corp` to a term-document matrix.
Create `all_m` by converting `all_tdm` to a matrix.
Construct a `comparison.cloud()` from `all_m`. Set `max.words` to `100`. The `colors` argument is specified for you.

```{r}
# Create all_goog_corp
all_goog_corp <- tm_clean(all_goog_corpus)

# Create all_tdm
all_tdm <- TermDocumentMatrix(all_goog_corp)

# Create all_m
all_m <- as.matrix(all_tdm)

# Build a comparison cloud
comparison.cloud(all_m,
  colors = c("#F44336", "#2196f3"),
  max.words = 100)
```
##### 4.14 Cage match! Amazon vs. Google pro reviews 
***Instruction:***
- Create `common_words` from `all_tdm_df` using `dplyr` functions.
  - `filter()` on the `AmazonPro` column for nonzero values.
  - Likewise filter the `GooglePro` column for nonzero values.
  - Then `mutate()` a new column, `diff` which is the `abs` (absolute) difference between the term frequencies columns.
- Although we could have piped again, create `top5_df` by applying `top_n` to `common_words` to extract the top `5` values in the `diff` column. It will print to your console for review.
- Create a `pyramid.plot` passing in `top5_df$AmazonPro` then `top5_df$GooglePro` and finally add labels with `top5_df$terms`.

```{r}
# Filter to words in common and create an absolute diff column
common_words <- all_tdm_df %>% 
  filter(
    AmazonPro != 0,
    GooglePro != 0
  ) %>%
  mutate(diff = abs(AmazonPro - GooglePro))

# Extract top 5 common bigrams
(top5_df <- top_n(common_words, 5))

# Create the pyramid plot
pyramid.plot(top5_df$AmazonPro, top5_df$GooglePro, 
             labels = top5_df$terms, gap = 12, 
             top.labels = c("Amzn", "Pro Words", "Goog"), 
             main = "Words in Common", unit = NULL)
```

##### 4.15 Cage match, part 2! Negative reviews 
***Instruction:***

- Using `top_n()` on `common_words`, obtain the top `5` bigrams weighted on the `diff` column. The results of the new object will print to your console.
- Create a `pyramid.plot()`. Pass in `top5_df$AmazonNeg`, `top5_df$GoogleNeg`, and labels = top5_df$terms. For better labeling, set
  - `gap` to `12`.
  - `top.labels` to `c("Amzn", "Neg Words", "Goog")`
 
The `main` and `unit` arguments are set for you.

```{r}
# Extract top 5 common bigrams
(top5_df <- top_n(common_words, 5, wt = diff))

# Create a pyramid plot
pyramid.plot(
    # Amazon on the left
    top5_df$AmazonNeg,
    # Google on the right
    top5_df$GoogleNeg,
    # Use terms for labels
    labels = top5_df$terms,
    # Set the gap to 12
    gap = 12,
    # Set top.labels to "Amzn", "Neg Words" & "Goog"
    top.labels = c("Amzn", "Neg Words", "Goog"),
    main = "Words in Common", 
    unit = NULL
)
```

##### 4.16 Step 6: Reach a conclusion (video)
##### 4.17 Draw conclusions, insights, or recommendations 
***Instruction:***

```{r}
在这里插入代码片
```
##### 4.18 Draw another conclusion, insight, or recommendation
***Instruction:***

```{r}
在这里插入代码片
```
##### 4.19 Finished! 




