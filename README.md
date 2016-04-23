#### psmysqlqueryoptperm
#####optimizing Data access
######best practice
it is better to use order by than sort in app
#####understanding mysql query
######execution path of a query
client->query cache->parser->preprocessor->query optimizer->query execution engine->(storagee engines->data)->client
######query cache
resuletset will store into query cache,next time if execute a query not in cache, it will run parser
######parser
take a single query and divide into multiple tokens or operators and build a parse tree from them.  
Now this tree is validated againest mysql's language grammar.  
next step is propressor.
######proprecessor
check various privileges,semantic like aliases, column name
######optimizer
convert from parse tree created by parser or preprocessor to query plan.
######query optimizer responsibilities
convert sub-optimal join type to efficient logic  
reorder join tables  
reducing constant expressions  
optimizing alg rules  
logic short circuit  
optimal index usage  
sort optimization  
optimizing aggregate functions
######query optimizer limitation
No parallel query execution  
No cinsideration to parallel running query  
dependency on storage engine statistics  
fastest executing query vs most resource optimized query
######query execution engine
executes the plan, which is byte code  
however in case of mysql execution plan
