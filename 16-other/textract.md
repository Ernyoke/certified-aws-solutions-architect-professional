# Amazon Textract

- Is a ML product used to detect and analyse text contained in input documents such as JPEG, PNG, PDF or TIFF
- The output is extracted text, structure of that text and any analysis that can be performed on that text
- For most documents the Textract is capable to operate in a synchronous way (real time)
- For large documents it will operate in asynchronous way
- It is pay per usage, custom pricing being available for large volume of documents
- Use cases of Textract:
    - Detection of text and the relationship between the text, example receipt - dates, items, prices. It also offers metadata about the text: where the ext occurs
    - Document analysis:
        - For generic documents might detect names, addresses, birth date, etc.
        - For receipts: prices, vendors, line items, dates, etc.
        - Identity documents: abstraction of certain fields to being able to store them in a table of a database
- It can be integrated with other AWS services