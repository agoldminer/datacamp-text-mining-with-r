# Text Analytics-DataCamp-Introduction to Text Analysis in R
## 1. Wrangling Text
##### 1.1 Text as data (video)
##### 1.2 Airline tweets data
***Instruction :***

```{r}
# Load the tidyverse packages
library(tidyverse)

# Print twitter_data
twitter_data

# Print just the complaints in twitter_data
twitter_data %>% 
  filter(complaint_label == 'Complaint')
```

##### 1.3 Grouped summaries

***Instruction :***

```{r}
# Start with the data frame
twitter_data %>% 
  # Group the data by whether or not the tweet is a complaint
  group_by(complaint_label) %>% 
  # Compute the mean, min, and max follower counts
  summarize(
    avg_followers = mean(usr_followers_count),
    min_followers = min(usr_followers_count),
    max_followers = max(usr_followers_count)
  )
```
##### 1.4 Counting categorical data (video)
##### 1.5 Counting user types 

***Instruction :***

```{r}
# Load the tidyverse package
library(tidyverse)

twitter_data %>% 
  # Filter for just the complaints
  filter(complaint_label == 'Complaint') %>% 
  # Count the number of verified and non-verified users
  count(usr_verified)
```
##### 1.6 Summarizing user types 

***Instruction :***

```{r}
library(tidyverse)

twitter_data %>% 
  # Group by whether or not a user is verified
  group_by(usr_verified) %>% 
  summarize(
    # Compute the average number of followers
    avg_followers = mean(usr_followers_count),
    # Count the number of users in each category
    n = n()
  )
```
##### 1.7 Tokenizing and cleaning (video)
##### 1.8 Tokenizing and counting 

***Instruction :***
```{r}
# Load the tidyverse and tidytext packages
library(tidyverse)
library(tidytext)

tidy_twitter <- twitter_data %>% 
  # Tokenize the twitter data
  unnest_tokens(word, tweet_text) 

tidy_twitter %>% 
  # Compute word counts
  count(word) %>% 
  # Arrange the counts in descending order
  arrange(desc(n))
```
##### 1.9 Cleaning and counting

***Instruction :***

```{r}
tidy_twitter <- twitter_data %>% 
  # Tokenize the twitter data
  unnest_tokens(word, tweet_text) %>% 
  # Remove stop words
  anti_join(stop_words)

tidy_twitter %>% 
  # Filter to keep complaints only
  filter(complaint_label == 'Complaint') %>% 
  # Compute word counts and arrange in descending order
  count(word) %>% 
  arrange(desc(n))
```
## 2. Visualizing Text
##### 2.1 Plotting word counts (video)
##### 2.2 Visualizing complaints 

***Instruction :***

```{r}
word_counts <- tidy_twitter %>% 
  filter(complaint_label == "Complaint") %>% 
  count(word) %>% 
  # Keep words with count greater than 100
  filter(n > 100)

# Create a bar plot using word_counts with x = word
ggplot(word_counts, aes(x = word, y = n)) +
  geom_col() +
  # Flip the plot coordinates
  coord_flip()
```
##### 2.3 Visualizing non-complaints 

***Instruction :***
```{r}
word_counts <- tidy_twitter %>% 
  # Only keep the non-complaints
  filter(complaint_label == 'Non-Complaint') %>% 
  count(word) %>% 
  filter(n > 150)

# Create a bar plot using the new word_counts
ggplot(word_counts, aes(x = word, y = n)) +
  geom_col() +
  coord_flip() +
  # Title the plot "Non-Complaint Word Counts"
  ggtitle("Non-Complaint Word Counts")
```
##### 2.4 Improving word count plots (video)
##### 2.5 Adding custom stop words

***Instruction :***
```{r}
custom_stop_words <- tribble(
  # Column names should match stop_words
  ~word, ~lexicon,
  # Add http, win, and t.co as custom stop words
  "http", "CUSTOM",
  "win", "CUSTOM",
  "t.co", "CUSTOM"
)

# Bind the custom stop words to stop_words
stop_words2 <- stop_words %>% 
  bind_rows(custom_stop_words)
```
##### 2.6 Visualizing word counts using factors 

***Instruction :***
```{r}
word_counts <- tidy_twitter %>% 
  filter(complaint_label == "Non-Complaint") %>% 
  count(word) %>% 
  # Keep terms that occur more than 100 times
  filter(n > 100) %>% 
  # Reorder word as an ordered factor by word counts
  mutate(word2 = fct_reorder(word, n))

# Plot the new word column with type factor
ggplot(word_counts, aes(x = word2, y = n)) +
  geom_col() +
  coord_flip() +
  ggtitle("Non-Complaint Word Counts")
```
##### 2.7 Faceting word count plots (video)
##### 2.8 Counting by product and reordering 

***Instruction :***
```{r}
word_counts <- tidy_twitter %>%
  # Count words by whether or not its a complaint
  count(word, complaint_label) %>%
  # Group by whether or not its a complaint
  group_by(complaint_label) %>%
  # Keep the top 20 words
  top_n(20, n) %>%
  # Ungroup before reordering word as a factor by the count
  ungroup() %>%
  mutate(word2 = fct_reorder(word, n))
```
##### 2.9 Visualizing word counts with facets

***Instruction :***
```{r}
# Include a color aesthetic tied to whether or not its a complaint
ggplot(word_counts, aes(x = word2, y = n, fill = complaint_label)) +
  # Don't include the lengend for the column plot
  geom_col(show.legend = FALSE) +
  # Facet by whether or not its a complaint and make the y-axis free
  facet_wrap(~complaint_label, scales = "free_y") +
  # Flip the coordinates and add a title: "Twitter Word Counts"
  coord_flip() +
  ggtitle("Twitter Word Counts")
```
##### 2.10 Plotting word clouds (video)
##### 2.11 Creating a word cloud
***Instruction :***
```{r}
# Load the wordcloud package
library(wordcloud)

# Compute word counts and assign to word_counts
word_counts <- tidy_twitter %>% 
  count(word)

wordcloud(
  # Assign the word column to words
  words = word_counts$word, 
  # Assign the count column to freq
  freq = word_counts$n,
  max.words = 30
)
```
##### 2.12 Adding a splash of color

***Instruction :***

```{r}
# Compute complaint word counts and assign to word_counts
word_counts <- tidy_twitter %>% 
  filter(complaint_label == "Complaint") %>% 
  count(word)

# Create a complaint word cloud of the top 50 terms, colored red
wordcloud(
words = word_counts$word,
freq = word_counts$n,
max.words = 50,
colors = "red"
)
```
## 3. Sentiment Analysis
##### 3.1 Sentiment dictionaries (video)
##### 3.2 Counting the NRC sentiments 

***Instruction :***
```{r}
# Load the tidyverse and tidytext packages
library(tidyverse)
library(tidytext)

# Count the number of words associated with each sentiment in nrc
get_sentiments("nrc") %>% 
  count(sentiment) %>% 
  # Arrange the counts in descending order
  arrange(desc(n))
```
##### 3.3 Visualizing the NRC sentiments 
***Instruction :***

```{r}
# Pull in the nrc dictionary, count the sentiments and reorder them by count
sentiment_counts <- get_sentiments("nrc") %>% 
  count(sentiment) %>% 
  mutate(sentiment2 = fct_reorder(sentiment, n))

# Visualize sentiment_counts using the new sentiment factor column
ggplot(sentiment_counts, aes(x = sentiment2, y = n)) +
  geom_col() +
  coord_flip() +
  # Change the title to "Sentiment Counts in NRC", x-axis to "Sentiment", and y-axis to "Counts"
  labs(
    title = "Sentiment Counts in NRC",
    x = "Sentiment",
    y = "Counts"
  )
```
##### 3.4 Appending dictionaries (video)
##### 3.5 Counting sentiment 
***Instruction :***

```{r}
# Join tidy_twitter and the NRC sentiment dictionary
sentiment_twitter <- tidy_twitter %>% 
  inner_join(get_sentiments("nrc"))

# Count the sentiments in sentiment_twitter
sentiment_twitter %>% 
  count(sentiment) %>% 
  # Arrange the sentiment counts in descending order
  arrange(desc(n))
```
##### 3.6 Visualizing sentiment
***Instruction 1:***
```{r}
  inner_join(get_sentiments("nrc")) %>% 
  filter(sentiment %in% c("positive", "fear", "trust")) %>%
  # Count by word and sentiment and keep the top 10 of each
  count(word, sentiment) %>% 
  group_by(sentiment) %>% 
  top_n(10, n) %>% 
  ungroup() %>% 
  # Create a factor called word2 that has each word ordered by the count
  mutate(word2 = fct_reorder(word, n))
```
***Instruction 2:***

```{r}
# Create a bar plot out of the word counts colored by sentiment
ggplot(word_counts, aes(x = word2, y = n, fill = sentiment)) +
  geom_col(show.legend = FALSE) +
  # Create a separate facet for each sentiment with free axes
  facet_wrap(~ sentiment, scales ="free") +
  coord_flip() +
  # Title the plot "Sentiment Word Counts" with "Words" for the x-axis
  labs(
    title = "Sentiment Word Counts",
    x = "Words"
  )
```

##### 3.7 Improving sentiment analysis (video)
##### 3.8 Practicing reshaping data 
***Instruction :***
```{r}
tidy_twitter %>% 
  # Append the NRC sentiment dictionary
  inner_join(get_sentiments("nrc")) %>% 
  # Count by complaint label and sentiment
  count(complaint_label, sentiment) %>% 
  # Spread the sentiment and count columns
  spread(sentiment, n)
```
##### 3.9 Practicing with grouped summaries 
***Instruction :***

```{r}
tidy_twitter %>% 
  # Append the afinn sentiment dictionary
  inner_join(get_sentiments("afinn")) %>% 
  # Group by both complaint label and whether or not the user is verified
  group_by(complaint_label, usr_verified) %>% 
  # Summarize the data with an aggregate_value = sum(value)
  summarise(aggregate_value = sum(value)) %>% 
  # Spread the complaint_label and aggregate_value columns
  spread(complaint_label, aggregate_value) %>% 
  mutate(overall_sentiment = Complaint + `Non-Complaint`)
```
##### 3.10 Visualizing sentiment by complaint type
***Instruction 1:***
```{r}
 sentiment_twitter <- tidy_twitter %>% 
 # Append the bing sentiment dictionary
  inner_join(get_sentiments("bing")) %>% 
# Count by complaint label and sentiment
  count(complaint_label, sentiment) %>% 
# Spread the sentiment and count columns
  spread(sentiment, n) %>% 
# Compute overall_sentiment = positive - negative
  mutate(overall_sentiment = positive - negative)
```
***Instruction 2:***

```{r}
# Create a bar plot out of overall sentiment by complaint level, colored by a complaint label factor
ggplot(
  sentiment_twitter, 
  aes(x = complaint_label, y = overall_sentiment, fill = as.factor(complaint_label))
) +
  geom_col(show.legend = FALSE) +
  coord_flip() + 
  # Title the plot "Overall Sentiment by Complaint Type," with an "Airline Twitter Data" subtitle
  labs(
    title = "Overall Sentiment by Complaint Type",
    subtitle = "Airline Twitter Data"
  )
```

## 4. Topic Modeling
##### 4.1 Latent Dirichlet allocation (video)
##### 4.2 Topics as word probabilities 

***Instruction 1:***

- Print the output from an LDA run on the Twitter data. It is stored in lda_topics.

```{r}
# Print the output from LDA run
lda_topics
```

***Instruction 2:***

- Arrange the topics by word probabilities in descending order.

```{r}
# Start with the topics output from the LDA run
lda_topics %>% 
  # Arrange the topics by word probabilities in descending order
  arrange(desc(beta))
```

##### 4.3 Summarizing topics 
***Instruction :***


##### 4.4 Visualizing topics 
***Instruction :***

```{r}
word_probs <- lda_topics %>%
  # Keep the top 10 highest word probabilities by topic
  group_by(topic) %>% 
  top_n(10, beta) %>% 
  ungroup() %>%
  # Create term2, a factor ordered by word probability
  mutate(term2 = fct_reorder(term, beta))

# Plot term2 and the word probabilities
ggplot(word_probs, aes(x = term2, y = beta)) +
  geom_col() +
  # Facet the bar plot by topic
  facet_wrap(~ topic, scales = "free") +
  coord_flip()
```
##### 4.5 Document term matrices (video)
##### 4.6 Creating a DTM

***Instruction :***

```{r}
# Start with the tidied Twitter data
tidy_twitter %>% 
  # Count each word used in each tweet
  count(word, tweet_id) %>% 
  # Use the word counts by tweet to create a DTM
  cast_dtm(tweet_id, word, n)
```
##### 4.7 Evaluating a DTM as a matrix 
***Instruction :***

```{r}
# Assign the DTM to dtm_twitter
dtm_twitter <- tidy_twitter_subset %>% 
  count(word, tweet_id) %>% 
  # Cast the word counts by tweet into a DTM
  cast_dtm(tweet_id, word, n)

# Coerce dtm_twitter into a matrix called matrix_twitter
matrix_twitter <- as.matrix(dtm_twitter)

# Print rows 1 through 5 and columns 90 through 95
matrix_twitter[1:5, 90:95]
```
##### 4.8 Running topic models (video)
##### 4.9 Fitting an LDA 
***Instruction :***

```{r}
# Load the topicmodels package
library(topicmodels)

# Cast the word counts by tweet into a DTM
dtm_twitter <- tidy_twitter %>% 
  count(word, tweet_id) %>% 
  cast_dtm(tweet_id, word, n)

# Run an LDA with 2 topics and a Gibbs sampler
lda_out <- LDA(
  dtm_twitter,
  k = 2,
  method = "Gibbs",
  control = list(seed = 42)
)
```
##### 4.10 Tidying LDA output 
***Instruction :***

```{r}
# Glimpse the topic model output
glimpse(lda_out)

# Tidy the matrix of word probabilities
lda_topics <- lda_out %>% 
  tidy(matrix = "beta")

# Arrange the topics by word probabilities in descending order
lda_topics %>% 
  arrange(desc(beta))
```
##### 4.11 Comparing LDA output 
***Instruction :***

```{r}
# Run an LDA with 3 topics and a Gibbs sampler
lda_out2 <- LDA(
  dtm_twitter,
  k = 3,
  method = "Gibbs",
  control = list(seed = 42)
)

# Tidy the matrix of word probabilities
lda_topics2 <- lda_out2 %>% 
  tidy(matrix = "beta")

# Arrange the topics by word probabilities in descending order
lda_topics2 %>% 
  arrange(desc(beta))
```

##### 4.12 Interpreting topics (video)
##### 4.13 Naming three topics
***Instruction :***

```{r}
# Select the top 15 terms by topic and reorder term
word_probs2 <- lda_topics2 %>% 
  group_by(topic) %>% 
  top_n(15, beta) %>% 
  ungroup() %>%
  mutate(term2 = fct_reorder(term, beta))

# Plot word_probs2, color and facet based on topic
ggplot(
  word_probs2, 
  aes(term2, beta, fill = as.factor(topic))
) +
  geom_col(show.legend = FALSE) +
  facet_wrap(~topic, scales = "free") +
  coord_flip()
```
##### 4.14 Naming four topics
***Instruction :***
```{r}
# Select the top 15 terms by topic and reorder term
word_probs3 <- lda_topics3 %>% 
  group_by(topic) %>% 
  top_n(15, beta) %>% 
  ungroup() %>%
  mutate(term2 = fct_reorder(term, beta))

# Plot word_probs3, color and facet based on topic
ggplot(
  word_probs3, 
  aes(term2, beta, fill = as.factor(topic))
) +
  geom_col(show.legend = FALSE) +
  facet_wrap(~topic, scales = "free") +
  coord_flip()
```
##### 4.15 Wrap-up



