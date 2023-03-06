# Finding duplicate combinations of values

Recently, I was trying to add a constraint to an existing table to enforce that certain combinations of values were unique.

Let’s assume that users can bookmark recipes. The bookmarks are organized in collections, like “vegetarian”, “one pot” or “simple & fast”.
Each recipe should only be bookmarked once in any given collection.
The following SQL finds conflicting rows:

```sql
SELECT t1.*
FROM recipe_collection_entry t1
INNER JOIN
(
    SELECT collection_id, recipe_id
    FROM recipe_collection_entry
    GROUP BY collection_id, recipe_id
    HAVING COUNT(*) > 1
) t2
    ON t1.collection_id = t2.collection_id AND t1.recipe_id = t2.recipe_id;
```
