managed document database, not a relational database service.




-   Built **on top of [[BigTable]]**.

-   non-consistent for every row
-   document DB for **non-relational** data

-   Suitable:

	-   **Atomic transactions**: can execute a set of operations where either all succeed, or none occur.
	-   Supports **ACID transactions, SQL-like queries**.
	-   for structured data.
	-   for **hierarchical** document storage such as HTML

-   Query

	-   can search **by keys or properties** (if indexed)
	-   Key lookups somewhat like Amazon DynamoDB
	-   Allows for **SQL-like querying** down to property level 
	-   does **not support complex joins** with multiple inequality filters
	
	-   Performance:
	
	-   Fast to **Terabyte** scale, Low latency
	-   Quick read, **slow write** as it relies on indexing every property (by default) and must **update indexes as updates/writes occur**

  
48. An online gaming company is building a prototype data store for a player information system using [[Cloud Datastore]]. Developers have created a database with 10,000 fictitious player records. The attributes include a player identifier, a list of possessions, a health status, and a team identifier.  Queries that return player identifier and list of possessions filtered by health status return results correctly, however, queries that return player identifier and team identifier filtered by health status and team identifier do not return any results even when there are entities in the database that satisfy the filter. What would you first check when troubleshooting this problem?

The correct answer is to **verify a single composite index** exists on the player identifier and the team identifier. **Cloud Datastore only retrieves results using indexes**, it does not scan entities checking filter conditions. Single attribute indexes are created automatically so any query that references a single attribute will return correct values with no additional indexes.  **Composite conditions that include two or more attributes require composite indexes which must be created manually. Indexed values do not need to be of integer type.  There is no SCAN_ENABLED database parameter in Cloud Datastore.**

  

**Errors and Error Handling**

-   UNAVAILABLE, DEADLINE_EXCEEDED

-   Retry using **exponential backoff**.

-   INTERNAL

-   Do not retry this request more than **once**. 

-   Other

-   Do not retry without **fixing the problem**