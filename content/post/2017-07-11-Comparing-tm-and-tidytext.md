---
title: "A Comparision of tidytext and tm"
description: "Part 1: Data Structures"
author: "Sean Warlick"
date: 2017-07-11
---

The default package for many working with text and Natural Language Processing in R has been **tm**.  This past fall, a new package **tidytext** entered the ring and offers new ways to work with text in R.  Having done all of my text analysis using the **tm** package, I thought it was time to take a look at **tidytext** and compare the two libraries.  
  
We'll start the comparison by looking at the underlying data structures of the two packages.  In a future post we will compare the text cleaning process.  Finally, we'll take a look at how the **tm** and **tidytext** can be utilized together

## Practice Data  
For this comparison, our data set is a sample of 19 documents from the [Gutenburg Collection](http://www.gutenberg.org).  The Gutenberg collection can be accessed in R using the **gutenbergr** package.  

The data for this example is a data frame with four columns and nineteen rows. Each row represents a document.  The text of the document is in the *full_text* variable.  The remaining columns - *id*, *author*, and *title* - provide metadata for the given document.  



```r
dim(text_data)
```

```
## [1] 19  4
```

```r
head(text_data)
```

```
## # A tibble: 6 x 4
##       id      author
##   <fctr>      <fctr>
## 1   1892 Twain, Mark
## 2   2875 Twain, Mark
## 3   3185 Twain, Mark
## 4   3193 Twain, Mark
## 5   5784 Twain, Mark
## 6   5785 Twain, Mark
## # ... with 2 more variables: title <fctr>, full_text <fctr>
```

## Data Structure  
### tm
In the **tm** package, the main type of object is the corpus object.  A corpus is a collection of documents, where a document can be a book, website, email or even a sentence. We'll start our comparison by creating a corpus with our practice data.  In this case we will use `VectorSource()` to create the corpus. We will also assign metadata to the corpus.  



```r
# Create A Corpus
corpus <- VCorpus(VectorSource(text_data$full_text))

# Assign Metadata
meta(corpus, type = 'indexed', 'tag' = 'author') <- text_data$author
meta(corpus, type = 'indexed', 'tag' = 'title') <- text_data$title

# Explore The Corpus
corpus
```

```
## <<VCorpus>>
## Metadata:  corpus specific: 0, document level (indexed): 2
## Content:  documents: 19
```

If we look at the structure of the corpus, we see that the corpus is really just a list.  Each item in the list is a document and each document is a list of two items.  The first item in the document is the contents.  The second is the metadata for the given document.  
  
Given that we are working with a list, we can access the elements by position or name.  While we can use functions like `lapply()` to manipulate the corpus, **tm** contains functions specific to the type of manipulation associated with text mining.  


```r
# Examine Data Structure of Corpus and Document in the corpus
str(corpus[1:2])
```

```
## List of 2
##  $ 1:List of 2
##   ..$ content: chr "EXTRACTS FROM ADAM'S DIARY  Translated from the original MS.  by Mark Twain     [NOTE.--I translated a portion "| __truncated__
##   ..$ meta   :List of 7
##   .. ..$ author       : chr(0) 
##   .. ..$ datetimestamp: POSIXlt[1:1], format: "2017-07-12 03:45:19"
##   .. ..$ description  : chr(0) 
##   .. ..$ heading      : chr(0) 
##   .. ..$ id           : chr "1"
##   .. ..$ language     : chr "en"
##   .. ..$ origin       : chr(0) 
##   .. ..- attr(*, "class")= chr "TextDocumentMeta"
##   ..- attr(*, "class")= chr [1:2] "PlainTextDocument" "TextDocument"
##  $ 2:List of 2
##   ..$ content: chr " This eBook is for the use of anyone anywhere at no cost and with  almost no restrictions whatsoever.  You may "| __truncated__
##   ..$ meta   :List of 7
##   .. ..$ author       : chr(0) 
##   .. ..$ datetimestamp: POSIXlt[1:1], format: "2017-07-12 03:45:19"
##   .. ..$ description  : chr(0) 
##   .. ..$ heading      : chr(0) 
##   .. ..$ id           : chr "2"
##   .. ..$ language     : chr "en"
##   .. ..$ origin       : chr(0) 
##   .. ..- attr(*, "class")= chr "TextDocumentMeta"
##   ..- attr(*, "class")= chr [1:2] "PlainTextDocument" "TextDocument"
##  - attr(*, "class")= chr [1:2] "VCorpus" "Corpus"
```

```r
str(corpus[[1]])
```

```
## List of 2
##  $ content: chr "EXTRACTS FROM ADAM'S DIARY  Translated from the original MS.  by Mark Twain     [NOTE.--I translated a portion "| __truncated__
##  $ meta   :List of 7
##   ..$ author       : chr(0) 
##   ..$ datetimestamp: POSIXlt[1:1], format: "2017-07-12 03:45:19"
##   ..$ description  : chr(0) 
##   ..$ heading      : chr(0) 
##   ..$ id           : chr "1"
##   ..$ language     : chr "en"
##   ..$ origin       : chr(0) 
##   ..- attr(*, "class")= chr "TextDocumentMeta"
##  - attr(*, "class")= chr [1:2] "PlainTextDocument" "TextDocument"
```

```r
# Access content and document metadata
substring(corpus[[1]]$content, 1, 250)
```

```
## [1] "EXTRACTS FROM ADAM'S DIARY  Translated from the original MS.  by Mark Twain     [NOTE.--I translated a portion of this diary some years ago, and a friend of mine printed a few copies in an incomplete form, but the public never got them.  Since then I"
```

```r
corpus[[1]]$meta
```

```
##   author       : character(0)
##   datetimestamp: 2017-07-12 03:45:19
##   description  : character(0)
##   heading      : character(0)
##   id           : 1
##   language     : en
##   origin       : character(0)
```
Looking closely at the metadata, there is no metadata stored with the document, despite having created author and title data. Because we used `type = 'indexed'` to create the metadata it is stored in a data frame at the corpus level.  I find that this is generally easier to work with and take the metadata across the analysis.  We access the metadata as shown below.  

```r
# Access document metadata stored at the corpus level. 
head(meta(corpus, type = 'indexed'))
```

```
##        author                                                        title
## 1 Twain, Mark Extracts from Adam's Diary, translated from the original ms.
## 2 Twain, Mark             Personal Recollections of Joan of Arc — Volume 2
## 3 Twain, Mark                                    Those Extraordinary Twins
## 4 Twain, Mark                  Mark Twain's Letters — Volume 1 (1853-1866)
## 5 Twain, Mark                                   A Tramp Abroad — Volume 03
## 6 Twain, Mark                                   A Tramp Abroad — Volume 04
```

### tidytext
In **tidytext** we have the advantage of working with a data frame as the primary data object.  This means that we can start working with our data right away and use the familiar **dplyr** tools.  Using this method our document text and our metadata are stored in the same row.
  
Accessing the meta data and the document content is straightforward in this format.  The document author is in one column and the document title is another column. The document content is in the final column.    


```r
tidy_text <- text_data %>% select(-id)
head(tidy_text)
```

```
## # A tibble: 6 x 3
##        author                                                        title
##        <fctr>                                                       <fctr>
## 1 Twain, Mark Extracts from Adam's Diary, translated from the original ms.
## 2 Twain, Mark             Personal Recollections of Joan of Arc — Volume 2
## 3 Twain, Mark                                    Those Extraordinary Twins
## 4 Twain, Mark                  Mark Twain's Letters — Volume 1 (1853-1866)
## 5 Twain, Mark                                   A Tramp Abroad — Volume 03
## 6 Twain, Mark                                   A Tramp Abroad — Volume 04
## # ... with 1 more variables: full_text <fctr>
```

## Conclusion
Until recently **tm** was the main package for working with text. Designed around the corpus data structure.  The corpus structure organizes text as a collection of documents and can be worked with like a list.  **tidytext** on the other hand utilizes a data frame as the data structure.  This is particularly helpful for users who are more familiar with the **tidyverse** than **base** R.  
  
In the next post we'll take a deeper look at the two packages.   We'll look at how to perform common text cleaning and normalization tasks is R.  
