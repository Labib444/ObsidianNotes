Changes are tracked in EF. Depending on the changes it will trigger INSERT, UPDATE or DELETE after *SaveChangesAsync* function is called.

- **Added**:  The entity doesn't yet exist in the database. The `SaveChanges` method issues an `INSERT` statement.
- **Unchanged**:  No changes need to be saved with this entity. An entity has this status when it's read from the database.
- **Modified**: Some or all of the entity's property values have been modified. The `SaveChanges` method issues an `UPDATE` statement.
- **Deleted**: The entity has been marked for deletion. The `SaveChanges` method issues a `DELETE` statement.
- **Detached**: The entity isn't being tracked by the database context.

# Ways to read one entity

The generated code uses **FirstOrDefaultAsync** to read one entity. This method returns null if nothing is found; otherwise, it returns the first row found that satisfies the query filter criteria. FirstOrDefaultAsync is generally a better choice than the following alternatives:

- **SingleOrDefaultAsync** - Throws an exception if there's more than one entity that satisfies the query filter. To determine if more than one row could be returned by the query, SingleOrDefaultAsync tries to fetch multiple rows. This extra work is unnecessary if the query can only return one entity, as when it searches on a unique key.

- **FindAsync** - Finds an entity with the primary key (PK). If an entity with the PK is being tracked by the context, it's returned without a request to the database. This method is optimized to look up a single entity, but you can't call Include with FindAsync. So if related data is needed, FirstOrDefaultAsync is the better choice.

# Avoiding Overposting

![[Pasted image 20221220060106.png]]

1. Use ***TryUpdateModelAsync*** Method
2. Use ***ViewModel*** instead of ***Model*** (Another alternative for *TryUpdateModelAsync* Method)



