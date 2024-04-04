# Azure AI/Fundamentals of Knowledge Mining and Azure AI Search

**Azure AI Search** provides the infrastructure and tools to create search solutions that extract data from various structured, semi-structured, and non-structured documents.

Feature set:
* **Data from any source**: accepts data from any source provided in JSON format, with auto crawling support for selected data sources in Azure.
* **Full text search and analysis**: offers full text search capabilities supporting both simple query and full Lucene query syntax.
* **AI powered search**: has Azure AI capabilities built in for image and text analysis from raw content.
* **Multi-lingual** offers linguistic analysis for 56 languages to intelligently handle phonetic matching or language-specific linguistics. Natural language processors available in Azure AI Search are also used by Bing and Office.
* **Geo-enabled**: supports geo-search filtering based on proximity to a physical location.
* **Configurable user experience**: has several features to improve the user experience including autocomplete, autosuggest, pagination, and hit highlighting.

Data source (files in Azue Storage, text in a DB, anything represented as JSON)
Indexer - consumes data source, can have a Skillset attached to enrich data making it more searchable.

**Built in Skills**
* Natural Language Processing Skills  
  * Key Phrase Extraction
  * Text Translation Skill
* Image Processing Skills
  * Image Analysis Skill
  * Optical Character Recognition Skill

Azure AI Search Indexer
: can be thought of as a container of searchable documents. Conceptually similar to a table with a row representing a document. columns as fields in a document.


#### Use an indexer to build an index
Loading documents:
* Push Method: JSON data is pused to a search index via REST API or .NET SDK. Most flexible, no restrictions on data source type, location or frequency.
* Pull Method: Search service indexers can pull data from populat data sources, and export data into JSON if necessary.

**Making changes to an index**
You have to drop and recreate indexes if you need to make changes to field definitions. Adding new fields is supported, with all existing documents having null values. You'll find it faster using a code-based approach to iterate your designs, as working in the portal requires the index to be deleted, recreated, and the schema details to be manually filled out.

An approach to updating an index without affecting your users is to create a new index under a different name. You can use the same indexer and data source. After importing data, you can switch your app to use the new index.


Knowledge Store
: persistent storage of enriched content. i.e. save the results of an AI Skillset that generates captions from images.

A knowledge store can contain one or more of three types of projection of the extracted data:

* Table projections are used to structure the extracted data in a relational schema for querying and visualization
* Object projections are JSON documents that represent each data entity
* File projections are used to store extracted images in JPG format

Creating an index in the portal requires data to be in a supported data source:
* Cosmos DB - SQL API
* Azure SQL
* Azure Storage (Blob, table, ADLS Gen2)

#### Query data in an Azure AI Search index

Azure AI Search queries can be submitted as an HTTP or REST API request, with the response coming back as JSON.


Query syntax: https://learn.microsoft.com/en-us/azure/search/query-odata-filter-orderby-syntax
