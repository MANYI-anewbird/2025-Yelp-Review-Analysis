# Team 10 working log

Week-by-week notes from the BA820 project (kept as-is).

- Read in json files ✅
- preprocess/cleaning ✅
- EDA ✅
- one method/analysis ✅
- updated report ✅
- text mining ✅
- LDA/Topic Modeling ✅
- sentiment analysis ✅
- similarity summary function ✅
- final report



** February 12th - Barrett to Shreya ** 
Hi Shreya! Here is what I did today for the project:

- Created a virtual instance with a beefier RAM to process our larger files.
    * This should make it easier to review the reviews and users files.
- Converted .json to .parquet files.
    * .parquet files are a more efficient way of storing data. We got the review file size cut in half(!!!). From now on, use pd.read_parquet("filepath")
      to read user and review files into dataframes.
    * business file is still a .json since it small enough to process as a .json. For business.json, use pd.read_json("filepath", lines = True).
- Removed smaller virtual machine to reduce confusion.

Here is what can be done next:

- Checking null values
- Determining if subsetting is needed or if we can proceed with entire contents of files
- EDA things like average length of text in review per business type

Our work is contained in 820-process.ipynb on the VM. Once you finish your contributions, download the file and push the file to this repo. 
Don't forget to stop the VM once you're done!

** February 13th - Shreya to Aryan **

Hey Aryan, here's the things i did so far!

- checked for null values. only the business file had null values in two columns (attributes, hours). 
   * i am still unsure about how we should proceed with dealing with these because business is a secondary file for our analysis. 
- subsetting doesn't seem to be necessary. all the files are running properly and quickly so far.
- merged review and users files on user_id
- EDA:
   * checked the average length of the review texts
   * created a couple plots of different relevant relationships
   * attempted to find the most common words used in the reviews, which crashed the kernel multiple times (please see if you can figure it out)
   * found the top 10 users (number can be changed) by number of reviews.
   * distribution of the frequency of stars
   * scatterplot about the r/s b/w review length and voted useful.

I think going forward you could do the following: 
- trying to see how to deal with the null values in business that makes sense for our project. we can discuss this too!
- figuring out how to get the code running for the most common words used in reviews.
- any further EDA you think is necessary
- if you're really zooming through all of this and feel like you have a lot of time, maybe starting with topic modeling on the reviews.

** February 14th - Aryan to Manyi **
Hi Manyi, here's what all I did today - 
- null values exist in business data frame, in three columns - attributes, categories and hours. Hours are not provided not just for businesess that 
are closed but also for ones that are still open
- I believe we can leave the null values as it is as these are all categorical columns and hence we cannot use any statstical measure to replace it with,
also I think that replacing the nulls without "Unknown" would affect our techniques like one hot encoding
- merged_df also has a few null values but I dont think we really need those columns 
- Created a graph for the distribution of star ratings amongst the reviews dataset to get a better understanding of most dished out ratings by users
- Added a table for the 10 most reviewed businesses 
- Added graph for distribution of business ratings in top 10 cities 
- Added a heatmap for only numeric values in the merged_df 
- I got the code running for the most common words used but I think its not really usefull, here are the top 5 results:

Word, Frequency
0, the, 36711872
1, and, 26130927
2, i, 18989680
3, a, 18805999
4, to, 17718274
5, was, 13446562



Things that can be done next:
1. Choose which EDA graphs to keep which are most relevant to our project, not sure if we need this many 
2. See if the most common word code can be improved in any way to make it more relevant or simply delete it
3. Start with non-dimensionality reduction and clustering algorithms 


** February 15th - Manyi to Shreya **
Hi Shreya, here's what I did today -
- About the EDA, I'm not sure whether our future step will cover ratings distribution, I checked all the graphs and I think they still can provide some insight
so I kept all of them in this stage.
- Use NLP methods to figure out the most common word, (a method the professor will cover in the next class, but he mentioned the principle in the last class, 
and I use the code in his hands-on notebook), though I used some packages like Stopword and Punkt to improve the performance of the result, I don't think the result is 
meaningful enough to support our analysis.
- Use Kmeans to do the initial clustering without dimensionality reduction
- Try PCA to do dimensionality reduction, and identify 6 main components that can explain 85%
- Use K-means to do the clustering based on PCA 
- Try to use UMAP, but it seems the machine cannot handle it, and crash repeatedly.


Things that can be done next:
1. Try to improve the performance of the clustering though it has already improved With PCA
2. To see if we can use the UMAP method to perform better.
3. If possible, try to think about how to relate our works to the proposal 

THANK YOU! AND IF YOU HAVE ANY PROBLEM, FEEL FREE TO REACH OUT ANY TIME!!!

** February 21th - Manyi to Barrett **
Hi, here's what I did today - (Text mining file)
- Clean the text with tokenize, stopword, and punctuation methods, then merge the data back
- Try BoW methods to identify the frequency of words

Things that can be done next:
1. Try to more methods

** February 22nd - Barrett to Aryan **
Hi Aryan, here's what I did:
- Reduced .parquet files to be read in to ~relevant~ columns only, for computational efficiency
- filtered text mining to only reviews regarding cafe businesses, for more targeted sampling.
- got more targeted tokenization through TweetTokenizer and cafe filtered reviews

To do next:
1. Applying TF-IDF Vectorization to cafe subset and test clustering effectiveness.

Please don't forget to shut down the VM when finished! Shreya's GCP budget is all out and we switched to a new budget

** February 23rd - Aryan to Shreya **
Hi Shreya, here's what I did:
- Added the code for checking cosine similarity 
- Added code to see for euclidian similarity
- Added codes for TF-IDF
- Had to sample the dataset to get it work with these three codes as kernel crashed multiple times despite trying alternative methods 

To do next:
1. All three of these codes keep crashing the kernel, I have tried running them multiple times and some alternative methods on GPT
but I could not come up with any such method to handle such a large dataframe so see if you could find a way to run them 
2. Delete sample rows code 
3. Filter further in cafes business and move codes to seperate python file with just cafes parquet

** February 24th - Barrett to Shreya **
Hi Shreya, here's what I did:
- Adjusted text mining code for clarity and computational efficiency, honed down cafes further
- began topic modeling process. Perplexity was the measure I found that judges how many components LDA should have, but it seems difficult to interpret. May need further review of LDA documentation.
- created separate text_mining file for efficiency sake.

To do next:
Find the optimal number 

** February 25th - Shreya to Arryan **
Hey Aryan! Here's what I did today:
- Found the optimal number and trained the LDA model
- Found the top words per topic
- Assigned the top topic to each review
- plotted the topic distribution to see their frequency
- started topic similarity

To do next: 
- maybe consider looking at some topic distributions (positive/negative)
- topic similarity

** February 26th - Aryan to Manyi **
Hi Manyi! Here's what I did today:
- Filtered for most popular business - Starbucks and re ran codes in text mining file 
- ran the similarity matrices
- changed comments according to further filtered dataframe
- added the codes to check topic similarity between any two reviews
- switched to NMF instead of LDA and got better results in terms of distribution between the topics and re ran codes after that

To do next: 
- sentiment analysis including the positive / negative topic distribution plot 
- see if LDA codes need to be kept or delete otherwise 

** March 1st **

Synthesized code into functions to help produce a relevant review for a user based on inputted preferences.

Last part is the report and adjustments to organization of our code!