# WCKBQuery
## Introduction
This script enables you to perform batch searching across your institution's WorldCat Knowledge Base or limited to a particular collection. It pulls important metadata from the search results and writes them to a file as a .tsv.

The script was developed to help assess subscriptions and availability of items in your knowledge base. If you're looking at subscription changes, you can run a batch search to see if that title is available in other collections. For example, if you're canceling subscriptions to particular titles, you can use this script to find if aggregator databases you subscribe to provide access to that title and what the coverage is.

In addition to search across your whole knowledge base, you can limit to a particular collection. This functionality was developed to help with evaluating two similar collections. For my library had a local collection of ebooks, and we could use this script to compare the titles in that local collection against a global KB colleciton to see if we could select that collection.
## Getting Started

Please note, this script is for using Python 3; I will be create a Python 2 version in the near future.

If you haven't already, you will need to install the [Requests library](http://docs.python-requests.org/en/master/). Otherwise, other dependecies should be part of the standard Python 3 package.

This script uses the [WorldCat knowledge base API](https://www.oclc.org/developer/develop/web-services/worldcat-knowledge-base-api.en.html) from OCLC. so you will need a wskey to use the script. If you are at an OCLC member institution, you can request a wskey [here](https://platform.worldcat.org/wskey/wayf). Once you recieve your key, you'll need to add it to the code, assigning it to the "key" variable which should appear like:
```
#Add your API key below, assigned to the "key" variable

key = ''
```
You just need to copy and paste the wskey between the single quotes.

## Running the script
The script is written to take a text file and perform searches as it iterates through the list. It is not coded to read a complex file, such as a KBART file, so if you are working with a list of titles which includes a series of metadata, it is best, for this script, to select the a single field (ex. issn, isbn, title, etc) to search on and copy that to a seperate text file. Save this text file to the same directory as the script. Now you're set to actually run the script.

Open your terminal/command line and navigate to the folder with the script. Call the script:
```
python3 kbquery3.py
```
The script will now start. First, the script will ask you to list the file you're running the search with:

>Input File:

Simply list the name of the text file you just created:

>Input File: issnsToBeSearched.txt

Next the script will ask you the name the file you want the results to be saved to. You do not need to create a file yourself before running the script; it will create it for you in the directory:

>Name of file to save results: myOutput.tsv

Now it will ask what you're searching on:

>What are you searching on? ISSN, ISBN, OCN, or Title?:

Input which you want to search on. This performs the kind of search the script runs. If what you enter isn't an exact match (for example you enter issn or something else entirely), the script will use the full-text query searcher rather than one of the above, specific searchers.

Finally, the script will ask if you want to search within a particular KB collection or if you want to search across everything you have selected in the KB:

>To search in a particular collection, enter its ID otherwise type 'no':

You can find the collection IDs under the properties tab for a collection within WorldShare Collection Manager.

Once all that is completed, the script will begin running. The terminal will display each URL it creates for each query and display the results. While it does that, it is also writing the results as a .tsv to the output file.

## Understanding the output
Once the script finishes running, you can go to the folder containing the script and open the output file (which you named).

The file should be formatted as a table seperated values (tsv) file (whether or not you assign it the .tsv extension), so you should be able to open the file in Excel, Open Refine, a text editor, or other spreadsheet software.

The file contains 8 columns:

1. number
   - This column tells you where this search results falls within the search. For example number 1.1 is the first search term and its first search result; 1.2 is the first search term and the second search result; and 2.1 is the second search term and its first search result.
   - The number column helps you understand how many results were returned for each search term.
2. title
   - This column contains the title of the resource in the KB collection
3. ISBN or ISSN
   - This column records the ISBN or ISSN found for the title. If none is found, the field will say that list that no standard number was found.
4. ocn
   - The records the primary OCLC number for the KB record. This is the OCLC number listed as "OCN" and "override" in the Collection Manager interface. This is note a grouped OCN.
5. Collection Name
   - Name of the collection the particular item record is in.
6. KB ID
   - This field is for the title ID for the item in the KB, if one is found.
7. Coverage
   - This field lists the coverage information for the item. For example, if you have full text coverage from 2000-2017 it will read "fulltext@2000~2017".
8. Search Term
   - Lastly, the output includes the search term. This can help you better group and understand the results, but it can also be helpful for identify a problem if odd search results are turned.
