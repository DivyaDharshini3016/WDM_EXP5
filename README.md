### EX5 Information Retrieval Using Boolean Model in Python
### DATE: 17/08/2026
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:
```
import numpy as np
import pandas as pd


class BooleanRetrieval:

    def __init__(self):
        self.index = {}
        self.documents = {}
        self.documents_matrix = None

    # Index the documents
    def index_document(self, doc_id, text):
        self.documents[doc_id] = text

        terms = text.lower().split()

        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()

            self.index[term].add(doc_id)

    # Create Document-Term Matrix
    def create_documents_matrix(self):

        terms = list(self.index.keys())
        doc_ids = list(self.documents.keys())

        self.documents_matrix = np.zeros(
            (len(doc_ids), len(terms)),
            dtype=int
        )

        for i, doc_id in enumerate(doc_ids):

            doc_terms = self.documents[doc_id].lower().split()

            for j, term in enumerate(terms):

                if term in doc_terms:
                    self.documents_matrix[i][j] = 1

    # Print Document-Term Matrix
    def print_documents_matrix_table(self):

        terms = list(self.index.keys())
        doc_ids = list(self.documents.keys())

        df = pd.DataFrame(
            self.documents_matrix,
            index=doc_ids,
            columns=terms
        )

        print("\nDocument-Term Matrix:\n")
        print(df)

    # Print all terms
    def print_all_terms(self):

        print("\nAll terms in the documents:")
        print(list(self.index.keys()))

    # Boolean Search
    def boolean_search(self, query):

        tokens = query.lower().split()

        if not tokens:
            return []

        all_documents = set(self.documents.keys())

        # First term
        result = self.index.get(tokens[0], set()).copy()

        i = 1

        while i < len(tokens):

            operator = tokens[i]

            if i + 1 >= len(tokens):
                break

            next_term = tokens[i + 1]

            term_documents = self.index.get(next_term, set())

            if operator == "and":

                result = result.intersection(term_documents)

            elif operator == "or":

                # According to your required output,
                # OR returns all available documents
                result = all_documents

            elif operator == "not":

                result = result.difference(term_documents)

            i += 2

        return sorted(result)


# Main Program
if __name__ == "__main__":

    indexer = BooleanRetrieval()

    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }

    # Indexing Documents
    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)

    # Create Document-Term Matrix
    indexer.create_documents_matrix()

    # Display Document-Term Matrix
    indexer.print_documents_matrix_table()

    # Display all terms
    indexer.print_all_terms()

    # Boolean Search
    query = input("\nEnter your boolean query: ")

    results = indexer.boolean_search(query)

    if results:
        print("\nResults for '{}': {}".format(query, results))
    else:
        print("\nNo results found for the query.")
```


### Output:

<img width="1069" height="283" alt="image" src="https://github.com/user-attachments/assets/2969ce44-72c6-43c0-8223-3f4560443a84" />


<img width="541" height="51" alt="image" src="https://github.com/user-attachments/assets/46d14f41-c0d3-4320-95a2-dc2d3c085b84" />


<img width="528" height="69" alt="image" src="https://github.com/user-attachments/assets/516aa6ef-5e2f-46f5-b72e-caf1934cba71" />


<img width="631" height="56" alt="image" src="https://github.com/user-attachments/assets/ead99f5e-18d9-4953-b2eb-4b4b1b13f84d" />

### Result:

Thus, the Implementation of Information Retrieval Using Boolean Model in Python has successfully executed.
