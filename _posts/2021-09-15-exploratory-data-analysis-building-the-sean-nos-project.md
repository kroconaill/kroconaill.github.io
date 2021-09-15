---
layout: post
title: "Exploratory Data Analysis: Building the Sean-Nós NLTK Project"
---
[View Jupyter Notebook on GitHub](https://github.com/kroconaill/gaeilge-topic)

I really love Irish folk music, and one thing I have learned over the years is that, as an American, what I consider to be “Irish folk” music actually represents multiple categories of music. For this project, I wanted to know more about sean-nós music.  Although there are sean-nós songs in English, I wanted to focus on the ones in Gaeilge (Irish) and explore what the songs talk about.

Disclaimer: I am not a native Irish speaker, so it is entirely possible that some of my translations and stop words are incorrect.  I used [teanglann.ie](https://www.teanglann.ie/) to translate individual words and made educated guesses about the most likely meaning of words.  For example, in my initial analysis, the word *raibh* appeared often.  Teanglann.ie gives the first meaning as “floating seaweed”, but another meaning is a form of the verb “to be”.  Since *raibh* appeared more than 150 times, I chose to exclude it from analysis as it is more likely to be a form of “to be” rather than 150+ songs about seaweed.

## Data Extraction
The most complete corpus I could find of specifically Gaeilge songs is [SongsInIrish.com](https://songsinirish.com/tags/sean-nos), where songs are specifically tagged as “sean-nós”.  I noticed that the column for Gaeilge text on each song page has an id of “teacs-gaeilge”, which made it easy to extract the text using BeautifulSoup.

I did not need to use a separate web crawler because I was extracting a known number of links from a known page and pulling out a known field from each link.

## Data Cleaning
I wanted to analyze the text using NLTK, and although I was able to use certain features, Gaeilge is not a supported language.  This means that I had to build my own stop words, and using part-of-speech tagging and lemmatization would not be possible without significant custom coding on my part.  Originally, I had planned to use gensim to create a topic model, but without lemmatization, my results were likely to be skewed.

Another complication that arose is the use of fadas.  There is a significant difference between *Órla* (a personal name), and *orla* (vomit).  However, as I analyzed the data, it became clear that fadas were not consistently used in the Gaeilge text.  This introduced an element of ambiguity to my results.  Although it would theoretically be possible to infer fadas from context or check each song against known versions, this would require a significant amount of manual work and a level of Gaeilge expertise that I do not currently have.

For cleaning, I did my best to filter out pronouns, common verbs such as “to be”, and other common grammatical constructions that are not useful for determining topics.  This was somewhat difficult because Gaeilge uses prepositional pronouns, which we do not have in English.  Instead of simply filtering out “is”, “am”, “are”, there is a separate construction for each pronoun (see [Bitesize Irish](https://www.bitesize.irish/blog/prepositions-in-irish/)).  Furthermore, regional variations in dialect mean that there are multiple forms of interrogatives such as “what”, and because the songs are not labelled with their region of origin, it is important to include each known variation.

## Data Analysis
Since I was not able to build a topic model, I decided to start with word frequency analysis.  I created a frequency distribution using NLTK.  This was complicated by grammar.  In Gaeilge, words with the same root meaning can have different letters at the beginning depending on their grammatical case as well as which words precede them.  For example, *baile* (home) shows up as *abhaile*, *bhaile*, and *mbaile* in the text.  I attempted to normalize nouns into their nominative form by creating a list of common mutations and replacing the mutation with the nominative form.  However, I was not able to account for vowel changes mid-word that indicate different grammatical cases, such as *dóiteán* (nominative) to *dóiteáin* (genitive).

For my analysis, I chose three different metrics: frequent words across songs, shared words between songs, and word repetition within songs.  To filter out common words that I may not have caught in my stop word list, I limited the results to words with 4 letters or more unless otherwise indicated.  This is problematic because there are some words that showed up frequently, such as *rí* (king) and *lá* (day), that add meaning to the text but were not captured in my analysis.

## Data Visualization
![Frequent Words Graph](/assets/freqwords.png)

For this visualization, I limited results to words that appear more than 150 times throughout the text.  The most common words were *croí* (heart), *baile* (home), and *bean* (woman).  While more data is needed for true topic modeling, it is likely that many of these songs are love songs or songs about place (exiles missing home or coming home, for example).  *Oíche* (night) and *maidin* (day) also appear frequently, which makes sense as many folk songs orient the listener with an introduction such as, “As I roved out on a May morning”.

![Shared Words Graph](/assets/sharedwords.png)

For shared words, I limited results to words that appear in 100 songs or more.  For this one, I had to increase the word length limit to 5 letters or more rather than 4, as many grammatical words initially appeared in my results.

To clarify, “shared words” specifically counts whether a word appears in a song or not, rather than counting how often a word appears in a song.  The top 3 shared words were *teach* (house), *oíche* (night), and *neach* (being or person).  This type of analysis does not seem as helpful in determining topic as there are many types of songs that could include a person and a house at night.

![Repeated Words Graph](/assets/repeatwords.png)

I wanted to see if particular words were repeated often within a song, as this may indicate the song’s topic.  This was probably my favorite analysis and the one that I felt yielded the most clarity.

The most repeated words were *deabhas* (deuce or devil), *faoitín* (whiting, a type of fish), and *bhuileabó*.  I had difficulty finding a translation of the last word, but it appears in a song called [“‘S Ambo Éara”](https://mudcat.org/thread.cfm?threadid=47853), a spinning song.  It could be a dialect word that does not appear in a standard dictionary.  *Faoitín* could indicate an occupational song about fishing.  Other words of interest were *seoithín*, a whispering sound used in lullabies, and *óchon* (actually spelled *ochón* according to Teanglann.ie), a cry that translates approximately to “Alas!” or “Woe is me!”, used in laments.

## Data Problems
I noticed that some words appeared in my results that could be English or Gaeilge (i.e. “each”).  It is possible that the formatting is not consistent across the song pages and therefore my scraper picked up some of the English translation text.  In addition, I noticed that SongsInIrish.com had several songs listed twice: *Aird Uí Chuain* also appeared as *Aird Tí Chuain*, and *Mo Ghile Mear* appeared twice with that title and once as *Mo ghille mear*.  This duplication of songs likely skewed the results.

## Resources
- [https://www.teanglann.ie/](https://www.teanglann.ie/)
- [https://songsinirish.com/tags/sean-nos](https://songsinirish.com/tags/sean-nos)
- [https://www.bitesize.irish/blog/prepositions-in-irish/](https://www.bitesize.irish/blog/prepositions-in-irish/)


