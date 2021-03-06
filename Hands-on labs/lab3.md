# Optimize NoSQL design

## Context
The team will begin evaluating query patterns as stated within the guide and as observed within the sample web application and 
related artifacts.  
At a high level, the following is a set of specific query patterns that Contoso has explained they want to make sure continue to work in 
the new design, are performant, and are cost-optimized for the following anticipated query volumes:  
  
  - Retrieve list of top 10 movies by biggest popularity and lowest price ~500 requests/second  
	  SELECT top(10) FROM c order by c.Popularity DESC, c.UnitPrice DESC  
  
  - Retrieve the list of categories: ~500 requests/second  
	  SELECT DISTINCT(c.CategoryName)  
  	
  - Filter movies (items) by category description: ~200 requests/second  
	  SELECT c.CategoryId FROM c where c.CategoryName = "Fantasy"  
	  SELECT * FROM c where c.CategoryId = 14 (on category for the details=  
    
  - Retrieve orders with details showing products with quantities: ~10 requests/second  
	  SELECT * FROM c where c.OrderId = 50 (on orders)  
	  SELECT * FROM c where c.OrderId =50 (on orderdetails)  
	  SELECT * FROM c where c.ItemId  BETWEEN 397519 AND 397524  
  
Contoso currently has 85,000 concurrent users during peak time, but would like to plan for 5 million concurrent users.  
Perform sizing exercise to determine anticipated scale requirements. Optimize the schema design to reduce scale requirements if possible.  

## Tasks
1. Open the Data Explorer  

2. ...   


>### Tips  
>   - Compare the partition strategy developed in the previous challenge to what you think it should be now. Has your plan evolved? Are there any instances where having more than one partition to support different read-heavy workloads make sense?  
>   - When optimizing your schema, consider the following:  
>     - The optimal partition key has been applied at the collection level for each collection.  
>     - Related documents/entities are collocated under the same partition. These should be entities that are most often retrieved together.  
>     - If appropriate, some related data is embedded. For example, if you return a product and want to include some information about the category (which may be stored in a different partition) without retrieving the entire category document. In other words, denormalization.  
>     - As needed, virtual partition key values are used.  
>     - Adjust indexing appropriate for read or write-optimized workloads.  
>     - The most appropriate consistency model is selected based on usage patterns within the web application, and through testing.  
>   - Take care to consider the differences in modeling data for different data stores.  
>     - Avoid treating collections like tables and understand how multiple entity types can possibly be stored within the same collection.   
  
## Resources  
  - [Choosing a Data Model](https://docs.microsoft.com/azure/architecture/data-guide/big-data/non-relational-data)  
  - [Data Modeling for NoSQL](https://docs.microsoft.com/azure/cosmos-db/modeling-data)  
  - [Partitioning in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview)  
  - [Modeling and partitioning a real-world example on Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/how-to-model-partition-example)  
