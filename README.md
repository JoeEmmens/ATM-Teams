# TeamsAndText

The reposiitorry is designed to be run in the following order. In this application I select patents from the DISCERN US firm to patent match database, this can easily be substituted for the universe of patents, or someother sample.

This repository will do the following

  1) Collect a sample of US patents and the corresponding text from the USPTO 
    Notebook name: extract_descriptions
      * Inputs: 
        * DISCERN patent data set - the matched US firm to patent numbers and a unique firm identifier.
        * USPTO brief descriptions per year as .tsv file, e.g. g_brf_sum_text_1990.tsv
      * Outputs: 
        * descriptions.csv - a text file containing the brief summary for each of the selected patents, in the order of the list of selected patents.
      
    Comments: Data taken from DISCERN and available at [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4320782.svg)](https://doi.org/10.5281/zenodo.4320782)


  
  2) Clean this data and build a corpus to estiamte an LDA 
    
      Notebook name: build_corpus
      * Inputs:
        * selected_patents.p - generated in extract_descriptions
        * descriptions.csv - generated in extract_descriptions
        * patent_inventor.tsv - patent to disambiguated inventor id taken from patentsview.com
      * Outputs:
        * unique_pat_dict - a unique patent number which starts at zero (Gensim needs this instead of the USPTO patent numbers)
        * doc2inv - Matches unique_patent_number to inventor ids
        * inv2doc - Inverse of doc2inv
        * corpus - Bag of Words corpus format. Stored as a json to efficiently store a large list of lists.
        * id2word - Matches words to unique ids and columns
        
      Comments: This stage requires significant memory. The use of ijson to save the corpus improves efficiency but this needs improving.
  
  3) Estimate an Author Topic model and save a set of results
  
      Notebook name: ATModel
      * Inputs:
        * corpus - built in build_corpus
        * inv2doc - built in build_corpus
        * doc2inv - built in build_corpus
        * id2word - built in build_corpus
      * Outputs:
        * inv2id - matches inventors to columns through unique numeric id
        * topic_distributions - the estimated knowledge class x word distributions
        * inventor_distributions - the estimated inventor x knowledge class distributions
        * model
          * expElogbeta
          * gamma
      * Results:
        * Convergence plot - plots the likelihood convergence
        * Log file - Relevant estimation data records
  
  4) Calculate contribution weights and patent x knowledge class distribution 

      Notebook name: CalculateParameterStatistics
        * Inputs:
          * doc2inv 
          * inv2id
          * expElogbeta
          * gamma
          * unique_pat_dict
       * Outputs:
          * document_data - .csv file mapping UPSTO patent number, unique_pat_no, inventor_id and alpha_i
          * theta_ds - list of tuples which match USPTO patent numbers to patent x knowledge class distributions
  
      Comments: This could eaisly be parralelised and sped up. You need to extract the corpus.json file in the same path as the zipped version before running.
   
   
  5) Build inventor x patent panel
      
      Notebook name: BuildPanel
        * Inputs
          * document_data
          Then a collection of external data sources
            * https://patentsview.org/download/data-download-tables 
              * Inventor gender, assignment date, citations etc.
            * Market value - KDSS paper
        * Outputs
          * inventor_patent_panel
        
  6) Analyse these results
  
        Notebook name: AnalysePanel
        * Inputs:
          * inventor_patent_panel
        * Outputs:
          * Results: Iteration.html - A notebook file recording a set of simple descriptive statitics
           
           
           
Replication Instructions

 0) For now keep the baseline corpus constant - with the same cleaning. Check this out later.
 1) Run the AT model and create statistics : TODO change concat to faster method!
 2) Replicate notebook results and save
  * HTML file of notebook output
  * Convergence plot
  * Log file
  * Wordclouds in seperate folder
           
           
           
           
           
           
           
           
  
All code and results will be written in jupyter notebooks.
  
