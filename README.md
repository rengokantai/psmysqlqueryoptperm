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
however in case of mysql execution plan mysql follows the instruction given in a query execution plan to get desired result set

######show full processlist
```
show full processlist
```


#####performance optimization by practical
######Index used for select
```
use db;
select * from tb;
```
######table order in join clause

``
explain extended select * ...
```

######subquery vs exists vs joins 1
subquery:
```
select * from tba a where a.id in (select b.id from tbb b where tbbid<100)   // queryplan: 1000, 1, 100
```
exists:
```
select * from tba a where exists (select b.id from tbb b where tbbid<100 and b.id=a.id)   //better. query plan: 1000,2
```
join:
```
select distinct a.* from tba a inner join tbb b on b.id=a.id where tbbid<100   // queryplan: 100,1
```
######tuning aggregate function
these two queries:
```
explain select min(id) from tb where a=100;  //avoid using this
```
```
explain select id from tb where a=100 order by id limit 1   //faster.
```
if we already created index on id column.(if id is not indexed, the result might not better)

######optimizeing group by clause
compare:
```
select a,b from tb where b <10 group by a,b;
```
```
select a,b from tb where b <10 group by id; //faster,set pk
```
These two return same result, but second one is faster
######optimizing paging with limit
retrieve record from 396-400
```
select * from tb order by x limit 395,5;
```

######impact on performace of union
using union instead of union all

