This package is for gene ontology analysis. It has 2 main functions: 1. It receives a gene list, and give back the enrichment and merge result. 2. It can update to the newest gene ontology database.

############################Analysis############################
How to do gene ontology term analysis?

######enrichment######
create an instance for enrichment class, then call the function:

1. inputfile is genelists in a csv file: every row is a list, the first column is drivers of this gene list.  
2. outputfile_path is the directory to store the enrichment result. The number of outputfiles is same with the numbers of genelists in input file. Each output file is named by the driver of each genelist.
3. threshold is the significant p-value threshold
2. top is an optional parameter for picking up the top number of enrichment result (e.g. top 5 or top 10), by default is none. 

Example of how to use this class:
       enrichment = enrichment(host, user, password, dbname, "MLL2-MLL3.targetgenes.v9.csv", “”, 0.01)
       enrichment.enrich_csv(top=None)

######merge######
Create and instance of GoGraph class:

#Data of Go Ontology structure and gene_Goterm association
weightGographData = "weightedGoGraph.xml"
genelist = GeneList_csv_directory
output = output_directory
p_value = 0.05
subGenelistNo = 3

#Create a GoGraph object (Note: every time you use the gotermSummarization(), you need to create a new object)
gograph = GoGraph(weightGraph, genelist, output, p_value, subGeneListNo, host, user, password, dbname):
gograph.gotermSummarization()

Result is in the output directory

############################Update#############################
How to update?

#################update database##################
Before update database, user must complete the following steps: 
1. download the newest database dump: http://archive.geneontology.org/latest-lite/
2. add .sql to current database dump file, for example: change "go_20151003-assocdb-data" to "go_20151003-assocdb-data.sql"
3. log into database on server and type the following command:
	DROP DATABASE IF EXISTS assocdb
	CREATE DATABASE IF EXISTS assocdb
	quit
4. type the following command: 
        mysql -h localhost -u username -p assocdb <dbdump
   for example: 
        mysql -h localhost -u username -p assocdb <go_20151003-assocdb-data.sql
5. download 


Then update database as following: 
create an instance for updating database, then call the function:
       input_filepath = directory of NCBI gene file
       mydb = UpdateDataBase(host, user, password, dbname, input_filepath)
       mydb.updateDB()


#################update pubmeds##################

###download and parse###

example of using download and parse pubmed
pubmed_directory is the directory that user wants to store the pubmed articles 
parsedPubMed_directory is the directory that user wants to store parsed pubmed
       tool = downloadPubMed(host, user, password, dynamo, pubmed_directory, parsedPubMed_directory)
       tool.parse()

###ABNER###




#################update weights##################
This part builds a GOterm graph structure, and calculate the new weights in this structure

The input file is parsed pubmeds with ABNER
The output file is a GO term graph structure
Stopwords is a txt file contains NLP stop words

The input file path is where the parsed pubmeds are stored 
The output file path is the directory where user want to store the output GO term graph structure

input_filepath = "../taggedAbstracts/files.xml"
output_filepath = "weightedGoGraph.xml"
stopwords = "stopwords.txt"

Example of how to update weights:
g=GoStructure(host, user, password, dyname, input_filepath, output_filepath, stopwords)
g.updateWeights()

