# README

<!-- REMEMBER: 
You can preview a formatted version of this README.md document by clicking the 'Preview' button in the RStudio toolbar.
-->

## Preparation

- Read/ Annotate: [Recipe #2](https://lin380.github.io/tadr/articles/recipe_2.html). You can refer back to this document to help you at any point during this lab activity.
- Review the relevant R Markdown documentation: 
  - [R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/)
  - [R Markdown Cookbook](https://bookdown.org/yihui/rmarkdown-cookbook/)

## Objectives

**Research:** 

  - Identify the aim(s) and main finding(s) from a primary research article.
  - Interpret a plot.

**R Markdown:** 

  - Create numbered section headers and a table of contents in R Markdown documents.
  - Add in-line citations and corresponding references to R Markdown documents.
  - Add cross-references to a table and/ or plot figure in R Markdown documents.

## Instructions

### Setup

1. Create a new R Markdown document. Title it "Lab #2" and provide add your name as the author. 
2. Delete all the material below the front matter.
3. Add your Zotero API key to this RStudio Cloud project through the Tools > Global Options... > R Markdown > Citations menu.

### Article summary

1. Open the file "Tottie - 2011 - Uh and Um as sociolinguistic markers in British English.pdf" (in your project directory).
2. Skim the article abstract (and potentially the conclusion) for: (1) the main aim(s) and (2) the main finding(s) of the study. 
3. In your own words summarize the points in step 2 for the article in the R Markdown document.
4. Add a level 1 section header above this summary titled "Article summary".

### Comparison

1. Create another level 1 header below the article summary prose you created titled "Comparison with American English".
2. Copy the following code chunk and paste it under this section header.

````

```{r swda-fillers, echo=FALSE, message=FALSE, fig.cap='Use of fillers in spoken American English'}
# Load the package used for data manipulation and plotting
library(tidyverse)
# Load the package which has the Switchboard Dialogue Act Corpus dataset
library(tadr)

# Extract count of fillers 'uh' and 'um' for each utterance
speaker_fillers <- 
  swda %>% # dataset
  mutate(uh = str_count(utterance_text, "\\{F [Uu]h")) %>% # count 'uh' per utterance
  mutate(um = str_count(utterance_text, "\\{F [Uu]m")) %>% # count 'um' per utterance
  select(speaker_id, sex, uh, um) %>% # subset
  pivot_longer(cols = c("uh", "um"), names_to = "fillers", values_to = "filler_count") %>% # wide to long format
  group_by(speaker_id, sex, fillers) %>% # group by speaker_id, sex, and fillers
  summarise(filler_count = sum(filler_count)) # count number of fillers

# Calculate the number of utterances per speaker
speaker_num_utterances <- 
  swda %>% # dataset
  group_by(speaker_id) %>% # group by speaker_id
  summarise(num_utterances = n()) # count number of utterances by

# Join speaker_fillers and speaker_num_utterances data frames
left_join(speaker_num_utterances, speaker_fillers) %>% # join speaker_fillers and speaker_num_utterances
  mutate(filler_count_per_utt = (filler_count / num_utterances) * 100) %>% # calculate fillers per 100 utterances
  ggplot(aes(x = sex, y = filler_count_per_utt, color = fillers)) + # plot variables to use
  geom_boxplot() + # produce a boxplot
  labs(title = "Switchboard Corpus of American English", 
       subtitle = "Use of fillers 'uh' and 'um'", 
       x = "Sex of speaker", 
       y = "Number of utterances (per 100)",
       color = "Filler type")
```

````

3. Knit the R Markdown document to see what you've created so far.
4. Above the code chunk, describe what this plot might tell us about the use of fillers in American English.
5. Using your summary of the Tottie (2011) article and the plot for American English, provide a description of similarities and differences that this plot might suggest exist between British and American English usage of the fillers. Add this summary below the plot.

### Document formatting

**Numbered sections and table of contents:** 

1. Either with the RStudio toolbar or manually, edit the front matter so that that document output will:
  - include numbered sections
  - include a table of contents
2. Knit the R Markdown document to verify there are numbered sections and a table of contents

**In-line citation and reference section:**

1. Identify the DOI for the Tottie (2011) article. 
  - Either by visiting [the article on the publishers website](https://www.jbe-platform.com/content/journals/10.1075/ijcl.16.2.02tot) or by inspecting the article PDF.
2. Add this reference to Zotero by copying the article DOI and entering it using the "Add by identifier" tool in Zotero.
3. Add the in-line citation with the RStudio visual editor in the "Article summary" section of your R Markdown document.
4. Add a section header "References" at the very end of the R Markdown document.
5. Knit the R Markdown document to verify the in-line citation and reference appear in the document output.

**Cross-reference a figure:**

1. In the front matter change the `output:` value `html_document:` to `bookdown::pdf_document2:`.
2. Prepend `As seen in Figure \@ref(fig:swda-fillers), ` to your description of the plot.
3. Knit the R Markdown document to verify the cross-reference appears in the document output.

## Assessment

1. Add another level 1 section which describes your learning in this lab (before the "References"" section).

Some questions to consider: 

  - What did you learn?
  - What was most/ least challenging?
  - What resources did you consult? 
  - What more would you like to know about?

## Submission

1. To prepare your lab report for submission on Canvas you will need to Knit your R Markdown document to PDF or Word. 
2. Download this file to your computer.
3. Go to the Canvas submission page for Lab #2 and submit your PDF/Word document as a 'File Upload'. Add any comments you would like to pass on to me about the lab in the 'Comments...' box in Canvas.
