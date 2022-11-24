# TeamsAndText

The reposiitorry is designed to be run in the following order. In this application I select patents from the DISCERN US firm to patent match database, this can easily be substituted for the universe of patents, or someother sample.

This repository will do the following

  1) Collect a sample of US patents and the corresponding text from the USPTO 
      
      Notebook name: extract_descriptions
      Inputs: 
        * DISCERN patent data set - the matched US firm to patent numbers and a unique firm identifier.
        * USPTO brief descriptions per year as .tsv file, e.g. g_brf_sum_text_1990.tsv
      Outputs: 
        * descriptions.csv - a text file containing the brief summary for each of the selected patents, in the order of the list of selected patents.
  
  2) Clean this data and build a corpus to estiamte an LDA 
    
      Notebook name: build_corpus
      Inputs:
        * selected_patents.p - generated in extract_descriptions
        * descriptions.csv - generated in extract_descriptions
        * patent_inventor.tsv - patent to disambiguated inventor id taken from patentsview.com
      Outputs:
        * unique_pat_dict - a unique patent number which starts at zero (Gensim needs this instead of the USPTO patent numbers)
        * doc2inv - Matches unique_patent_number to inventor ids
        * inv2doc - Inverse of doc2inv
        * corpus - Bag of Words corpus format. Stored as a json to efficiently store a large list of lists.
        * id2word - Matches words to unique ids and columns
  
  3) Estimate an Author Topic model and save a set of results
  
      Notebook name: ATModel
      Inputs:
        * corpus - built in build_corpus
        * inv2doc - built in build_corpus
        * doc2inv - built in build_corpus
        * id2word - built in build_corpus
       Outputs:
        * inv2id - matches inventors to columns through unique numeric id
        * topic_distributions - the estimated knowledge class x word distributions
        * inventor_distributions - the estimated inventor x knowledge class distributions
      Results:
        * Convergence plot - plots the likelihood convergence
        * Log file - Relevant estimation data records
      
  
  4) Merge these results with others into an inventor x patent panel dataset

      Notebook name: 
      Inputs:
        *
       Outputs:
        * 
  

  5) Analyse these results
  
      Notebook name: 
      Inputs:
        *
       Outputs:
        * 
  
All code and results will be written in jupyter notebooks.
  
