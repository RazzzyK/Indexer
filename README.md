# Indexer

readme.pdf for Indexer

General Functionality
	Our Indexer utilizes a modified version of tokenizer from Asst0 and the sorted list from Asst1. Tokenizer was modified to work with a file while sorted list was implemented normally.

The Index is created using a sorted list of sorted lists. The outer list contains the token list and is a list of WordEnt_ structs. 

WordEnt_ Structs 
	WordEnt_ structs are comprised of 2 fields:
	- Word
	- RecordList
	The word field is a simple pointer to a char array that represents the word token found when 	tokenizing a file in the directory. 
	The RecordList is itself another sorted list that will maintain RecordEnt_ structs indepdently of 	where in the sorted list the WordEnt_ is found. In this way we can easily manage sorting the 	outer list by word order and the inner lists by record order

	WordEnt_ struct's are ordered in the Index (which is a sorted list) in increasing order by their 	Word field.

Each WordEnt_ struct has a RecordList that is comprised of RecordEnt_ structs and is organized in the following way.

RecordEnt_ Structs
	RecordList sorted lists are lists of RecordEnt_ structs that contain two fields: 
	- File
	- Occurences
	The file field is a simple char pointer to the relative path name of where the file was found.
	
	For example: indexing file /adir/afolder/boo.txt relative to the directory adir will return the 	store the relative path name: afolder/boo.txt in the file field of the RecordEnt_ struct.
	
	The Occurences field simply keeps track of how many times the token (represented in the 	WordEnt_ struct appeared in the file. 

	RecordEnt_ structs are sorted first by occurences (greatest to least) then (if occurences are 	equal) by their file names in alphabetical order.

	Since each WordEnt_ struct contains a RecordList, we can mange RecordEnt_ struct positions
	independently of their greater position in the Index itself.

	In this way we keep the data structure of our index (sorted list of sorted lists) simple, concise, 	and easy to navigate.




The Indexer is broken into two main parts: CreateIndex and WriteIndexToFile
CreateIndex
	- Searches the directory recursively and tokenizes each file, inserting and maintaing the 	index as it continues to search each file. 

WriteIndexToFile 
	- Takes an Index data structure, traverses it, and writes it out to a specified output file 
	while it is traversing
	
In this way, by separating these two functions, we can maintain the Index data structure in memory, without modifying it, and write it out to multiple files (if need be). 
Also: the index only needs to be created once (for each directory indexed) and can be rewritten to as many files as need be during program execution.
