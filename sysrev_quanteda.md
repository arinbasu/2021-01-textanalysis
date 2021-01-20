## Using computational text analysis to filter first pass title and abstracts in systematic reviews
(Draft)
## Abstract

Systematic reviews and meta-analyses are time consuming but information intensive activities at evidence appraisal. At the heart of constructing systematic reviews is the process of information retrieval, where a body of evidence in the form of primary studies are first identified using information retrieval tools and search of literature databases. Following this first pass, researchers spend time in sifting through possibly a large body of titles and abstracts with a list of selction and rejection criteria to identify which studies should be retained for full text retrieval and final analysis. This process is erro-prone and time consuming.
In this paper, we argue that use of computational text analysis using quanteda software can speed up the process. We provide an example and codes to facilitate this process using a case study of retrieval of studies on mindfulness meditation for anxiety relief. We initially identified a total of 95 titles (and abstract) by searching Pubmed/Medline and narrowed down to a few studies. The process can be near-automated once the inclusion and exclusion terms are specified and a dictionary is provided to a corpus of text which is subsequently tokenised and converted to a document-term-matrix. We propose to extend this scope to full text analyses for extraction of meaning. 

keywords: systematic review, meta-analysis, methods

## Background

Systematic reviews and meta analyses follow a stepwise approach to synthesise information on primary studies to arrive at summary estimates and conclusions about the effectiveness and efficacies of different interventions and outcomes. However, while the approach is systematic and reproducible, several components of the process can be time consuming, and can benefit from the use of computational approaches that can not only reduce the time, but also improve accuracy. Two such processes are the initial first pass culling of irrelevant research sources based on title and abstracts and at a later stage, identification of themes from a study of the full text of the relevant primary sources. The first of these two steps are time consuming and error prone. 

In all cases, systematic reviews and meta-analyses follow a stepwise approach, initially outlined in the protocol of such studies and followed up as actual procedures with some modifications. The process begins with framing of a study question using a framework of population under study (referred to as "P"), interventions ("I" and in cases where the objective is to study the association between an exposure and a defined outcome, this is also referred to as "E" or exposure), a comparison group ("C" often referred to as comparator), and outcomes or "O"; hence the acronym PICO question. Following the PICO question, the analysts conduct a systematic search of the literature datbases and this usually involves searching more than a single source of literature data. In case of health sciences literature, such search involves searching [Medline/Pubmed](https://www.pubmed.gov), CINAHL, and other sources of literature. Other search sources include Google Scholar and generic search tools (cite??). Following the search, the analysts go through the titles and abstracts of each search result and based on a set of inclusion and exclusion criteria, identify studies for further retrieval of full texts or not. In the next step, after retrieval of full texts, the analysts obtain relevant data from the full texts of the studies identified in the previous steps and critically read each of the studies. At this time, they also obtain additional studies and run additional searches that also undergo the previous steps to match with inclusion and exclusion criteria. At the point of obtaining full texts of the research reports, they commence on the next actual analytical steps of systematic reviews and meta-analyses integrating the themes and points covered in the full texts of the primary studies.

A systematic review or a meta-analysis is time consuming (cite??) and is often error prone and can be subjective. In the first step of the process, where a large number of study titles and abstracts can be retrieved, the process can be error prone and subjective. A standard practice is to use two analysts or researchers who usually go through each list of retrieved titles and abstracts and after reading the titles and abstracts looking for keywords and expressions based on the inclusion and exclusion criteria, they either select or reject the studies. A standard practice is to consult or agree/disagree on conflicted conclusions. Overall, this process is time consuming when confronted with a large list of titles and abstracts. Such processes might benefit from computational approaches where a corpus of text can be subjected to further analyses using computational approaches and can be summarised (cite??).

Computational quantitative text analyses start with a corpus of text that can be broken down into tokens. Such tokens can be bag of words (where no positional information is relevant), or can be positional where words in specific contexts and positions within the entire text is meaningful. Using such approaches it is possible to identify specific positions within the texts and identify presence of specific types of studies, interventions, and outcomes to enable rapid classification of documents whether such documents are relevant for the systematic review and meta-analyses. 

In the following sections, we will explore use of computational quantitative text analysis programme, "quanteda" to conduct a first-pass analysis of a body of search results retrieved from a search of a sample systematic review and demonstrate the key steps used in the process. 

## Methods

We will use quanteda, a free open source R package for this purpose (cite??). Our study question for the systematic review is as follows: "What is the effectiveness of mindfulness meditation for treatment of anxiety in adults?". Based on this study question, we have identified the following PICOs: adults of all sex and ethnicity (P), mindfulness meditation as I, all other approaches that do not include mindfulness meditation as C, and relief of anxiety as O. We will also include all studies conducted since 2010 in the review. 

Step 1: based on the above conditions, we will first run a search on Pubmed/Medline with the following keywords: "mindfulness meditation", "anxiety" and "randomised controlled trial OR randomized controlled trial" and select all studies that are based on adults, in the past 10 years, and published in English language. This resulted in a set of 95 studies (Figure 1)

![Figure 1](https://github.com/arinbasu/2021-01-textanalysis/blob/main/step1_pubmed.JPG)

Step 2: These studies are then downloaded and made available as a CSV file. The sample CSV file can be downloaded from the following site [sample csv file](https://raw.githubusercontent.com/arinbasu/2021-01-textanalysis/main/csv-mindfulnes-set.csv)

Step 3: The CSV file downloaded from the pubmed can now be analysed using quanteda to identify and separate studies based on study design, intervention and outcomes using computational text approaches. The first step is to read the file and subsequent steps are annotated in the codes. The data object is then converted to a corpus; the corpus can be used to create new document level variables for further analyses and exported to CSV for further processing. 

The corpus is converted to tokens by a process referred to as "tokenisation". Here the main text part of the corpus, that is the titles are converted to individual words and common english stopwords such as "a", "an", and "the" are removed. At the end of this process, this bag of words can be subjected to dictionary-based methods by constructing dictionaries to identify which terms are present in the tokenised version of the corpus. For use in systematic reviews, we used the pattern "random*" to indicate whether the words randomised or randomized were present; we also used mindful* to indicate where in the doucment the words mindfulness or its derivatives were present, "medit*" to indicate where in the document the words meditations were present, and "anxiety" to indicate that the outcome words were present. The tokens were then converted to document feature matrix and further analysed to identify the frequency of different terms to further narrow down and reduce the volume of data. Finally, the dataset was exported as CSV to be able to truncate and retrieve only those specific primary studies that would be useful for final analysis. 

## Results

A preliminary search of the Pubmed database yielded 95 papers that we included for consideration in this paper for further reduction as not all papers were about testing effectiveness and efficacy of mindfulness meditation for reduction of anxiety symptoms using randomised controlled trial study design. The number of studies that had randomised controlled trial as study design was 46.

After converting the tokenised corpus to a document feature matrix, when we used a dictionary to identify which words corresponded to our selection criteria, it turned out that out of the 46 studies that were randomised controlled trials, 40 were labelled as randomized controlled trials, and six were labelled as randomised controlled trials, indicating the lexical variations of the same terms that were captured by the pattern recognition algorithm in quanteda (Table 1)

Table 1. List of the most frequent terms in the corpus of the search results titles

|feature           | frequency| rank| docfreq|group |
|:-----------------|---------:|----:|-------:|:-----|
|mindfulness       |        55|    1|      53|all   |
|randomized        |        40|    2|      40|all   |
|meditation        |        39|    3|      39|all   |
|anxiety           |        25|    4|      23|all   |
|mindfulness-based |        22|    5|      21|all   |
|randomised        |         6|    6|       6|all   |

It was also seen that the term "anxiety" appeared in 22 studies out of 95 and 77 instances of the term mindfulness or mindfulness-based were found. The term "meditation" occurred in 39 out of 95 studies. These suggested considerable scope of narrowing the scope of the search using quanteda.

We further narrowed the scope of the search by exporting the features by counting which features occurred in each of the 95 documents that were initially identified (Figure 2).

|document                 | design| intervention| outcome|
|:------------------------|------:|------------:|-------:|
|csv-mindfulnes-set.csv.1 |      1|            1|       2|
|csv-mindfulnes-set.csv.2 |      0|            0|       0|
|csv-mindfulnes-set.csv.3 |      1|            1|       0|
|csv-mindfulnes-set.csv.4 |      0|            0|       0|
|csv-mindfulnes-set.csv.5 |      0|            1|       1|
|csv-mindfulnes-set.csv.6 |      0|            1|       0|

As can be seen in Figure 2, out of the first six studies, only the first study contained instances of all the three inclusive terms, and none of the remaining five studies in the mix contained _all_ of the three terms to be included in the final analysis for retrieval of the full text and further analyses. The resultant output would then be used to identify and retrieve further studies for the next steps in systematic review. 

## Discussion

The goal of this paper was to describe a process using computational text analysis where complex textual material that demand high cognitive processes and time can be significantly reduced. Computational text analysis uses corpus of textual data and tokenisation to reduce large body of text to their component parts, and in this instance, we used a dictionary based approach with only bag-of-words approach to classify and reduce the complexity of words to only a few documents that would be further processed for systematic review. The process is efficient and time-saving compared with a manual process to retrieve and read each and every title of a retrieved search result and process them. We have here demonstrated using titles only, but this process can be extended to abstracts of studies as well. 

As an illustration, only a few lines of code would allow an analyst to identify that about 46 randomised controlled trials and roughly 25 studies would be considered to be included from the 95 initial results from the search. It may be argued that While this would be obvious to a trained observer studying titles alone for 95 studies and would not warrant the use of a software algorithm. However, the complexity might scale with hundreds of retrieved references. At that point, the task of correctly identifying a complex array of inclusion criteria from titles and abstracts can be overwhelming and hence a rapid identification of even a set of conservative inclusion criteria (where presence of a few terms in the title would be sufficient to be included in the final analyses) would help to reduce the cognitive load and processing time for systematic reviews.

Use of computational text analysis can be extended to abstracts and full texts of the article, beyond culling large reference lists. We will extend the scope of this work in future by using text collocations, and sentiment analyses and use of dictionaries to capture themes in complex documents following an initial pass. This exercise was meant to be a first pass to demonstrate the capability of a computational text analysis algorithm to simplify and speed up the process of systematic review and information retrieval. 

## References
to be added

## Code blocks

```R
library(quanteda)
library(readtext)
library(tidyverse)
library(knitr)
```


```R
# read the CSV file first
basefile <- "https://raw.githubusercontent.com/arinbasu/2021-01-textanalysis/main/csv-mindfulnes-set.csv"
pubmed <- readtext(basefile, text_field = "Title")
# use readtext() function to read the csv file
# need to specify which variable is going to be treated as text for processing
# in our case it is the title field, rest of the variables will be treated
# as docvars

docvars(pubmed) # 10 document level variables
```


```R
docnames(pubmed) # names of the documents

```


```R
## corpus
## First create a corpus using corpus()
pubcorpus <- corpus(pubmed)
summary(pubcorpus) # gives you details about the document type variables 
# note that it will state the title as a sentence
# see what it looks like
```


```R
document_level_vars <- head(docvars(pubcorpus)) # document level variables
# which years?
years <- docvars(pubcorpus, field = "Publication.Year")
head(years)
# Create a new document level variable named after 2010 or post_2010
docvars(pubcorpus, field = "post_2010") <- docvars(pubcorpus, 
                                                field = "Publication.Year") >= 2010
# check it has worked
docvars(pubcorpus)


```


```R
# create a separate corpus for only those years after 2010
post_2010 <- corpus_subset(pubcorpus, Publication.Year >= 2010)
ndoc(post_2010) # how many studies published after 2010? Ans. 88
# this will enable you to separately analyse these corpus
# you can also identify which documents were published before 2010
# 
```


```R
# find out how many randomised controlled trials
# segment this corpus on the basis of RCTs

rct_segment <- corpus_segment(pubcorpus, pattern = "random*")
ndoc(rct_segment) # how many controlled trials? Ans. 46
```


46



```R
## tokens
# break down the titles into their component words
# take corpus and break down
# pubtok <- tokens(pubmed) # will not work! because it is raw text data
pubtok <- tokens(pubcorpus,
                remove_punct = T) # will work as this is a corpus
# remove common stopwords
pubtok2 <- tokens_remove(pubtok, pattern = stopwords("en"))
# head(pubtok)
# see where you can find random* in the documents

kwic_rand <- kwic(pubtok2, pattern = "random*")
# print(head(kwic_rand))

# you can now include more than study design
# you can include selection criteria in it, 
# create a dictionary

mydict <- dictionary(list(design = "random*",
                         intervention = "mindful*",
                         outcome = "anxiety"))
# then look up

pubfit <- tokens_lookup(pubtok2, mydict)

# head(pubfit) # may not always be accurate

# We can also create n-gram tokens, for our case
# n = 3
pubgram <- tokens_ngrams(pubtok2, n = 3)
# head(pubgram)
# that was all possible combinations, we can specifically search for controlled trials
pubct <- tokens_compound(pubtok2, pattern = phrase("controlled trial"))
head(pubct)
```


    tokens from 6 documents.
    csv-mindfulnes-set.csv.1 :
     [1] "Randomized"       "controlled_trial" "mindfulness"      "meditation"      
     [5] "generalized"      "anxiety"          "disorder"         "effects"         
     [9] "anxiety"          "stress"           "reactivity"      
    
    csv-mindfulnes-set.csv.2 :
    [1] "Meditation"    "programs"      "psychological" "stress"       
    [5] "well-being"    "systematic"    "review"        "meta-analysis"
    
    csv-mindfulnes-set.csv.3 :
     [1] "Physical"         "activity"         "mindfulness"      "meditation"      
     [5] "heart"            "rate"             "variability"      "biofeedback"     
     [9] "stress"           "reduction"        "randomized"       "controlled_trial"
    
    csv-mindfulnes-set.csv.4 :
    [1] "Integrative" "Medicine"    "Therapies"   "Pain"        "Management" 
    [6] "Cancer"      "Patients"   
    
    csv-mindfulnes-set.csv.5 :
     [1] "Brief"        "Mindfulness"  "Meditation"   "Depression"   "Anxiety"     
     [6] "Symptoms"     "Patients"     "Undergoing"   "Hemodialysis" "Pilot"       
    [11] "Feasibility"  "Study"       
    
    csv-mindfulnes-set.csv.6 :
    [1] "mindfulness" "meditation"  "improve"     "chronic"     "pain"       
    [6] "systematic"  "review"     




```R
## document feature matrix
# create a document feature matrix from token
pubdfm2 <- dfm(pubtok2)
# what are the names of these features?
feature_names <- featnames(pubdfm2) # gives the  names of the features
# which features are present most of the times?
top_features <- topfeatures(pubdfm2) # gives you an idea that mindfulness occurs in 55/95 studies
# We want to see what %s, so
txfrq <- textstat_frequency(pubdfm2)

## Alternatively, create a document feature matrix from pubcorpus
pubdfm3 <- dfm(pubcorpus,
              remove_punct = T,
              tolower = T,
              remove = stopwords("en"))

keep_rand <- dfm_keep(pubdfm3, pattern = c("random*", "mindful*", "medit*", "anx*"))
tfq <- textstat_frequency(keep_rand)
# use the lookup dictionary we defined earlier to test
publkup <- dfm_lookup(keep_rand, mydict)
head(publkup)
publkup2 <- as_tibble(publkup)
```


    Document-feature matrix of: 6 documents, 3 features (55.6% sparse).
    6 x 3 sparse Matrix of class "dfm"
                              features
    docs                       design intervention outcome
      csv-mindfulnes-set.csv.1      1            1       2
      csv-mindfulnes-set.csv.2      0            0       0
      csv-mindfulnes-set.csv.3      1            1       0
      csv-mindfulnes-set.csv.4      0            0       0
      csv-mindfulnes-set.csv.5      0            1       1
      csv-mindfulnes-set.csv.6      0            1       0


    Warning message:
    “'as.data.frame.dfm' is deprecated.
    Use 'convert(x, to = "data.frame")' instead.
    See help("Deprecated")”


```R
publkup2 %>% 
  head() %>%
  kable()
```


    
    
    |document                 | design| intervention| outcome|
    |:------------------------|------:|------------:|-------:|
    |csv-mindfulnes-set.csv.1 |      1|            1|       2|
    |csv-mindfulnes-set.csv.2 |      0|            0|       0|
    |csv-mindfulnes-set.csv.3 |      1|            1|       0|
    |csv-mindfulnes-set.csv.4 |      0|            0|       0|
    |csv-mindfulnes-set.csv.5 |      0|            1|       1|
    |csv-mindfulnes-set.csv.6 |      0|            1|       0|



```R
kable(head(tfq))
```


    
    
    |feature           | frequency| rank| docfreq|group |
    |:-----------------|---------:|----:|-------:|:-----|
    |mindfulness       |        55|    1|      53|all   |
    |randomized        |        40|    2|      40|all   |
    |meditation        |        39|    3|      39|all   |
    |anxiety           |        25|    4|      23|all   |
    |mindfulness-based |        22|    5|      21|all   |
    |randomised        |         6|    6|       6|all   |

