<!-- vim: ft=text:fo=tcqwa
-->
So we wrote a Rails plugin to do exactly that.  The plugin consists of XXXX 
parts.

- acts_as_foreign: this is an ActiveRecord plugin that marks model objects that 
  resides in the existing database.
- acts_as_local: the complement of the above.
- field_pair: represents the relationship between fields in the two databases.  
  Also encodes the relatioonship between the fields.
- synchronization tables: we keep track of differences between the databases 
  and their results.

There was an extra requirement: all changes needed to be confirmed by an 
administrative user.  As a reault all differences between the two databases 
need to themselves be recorded in a database.  Since we'd already established 
that modifications to the legacy database are forbidden, these would need to go 
in our local database until an admin could approve them.  Honestly this 
requirement was probably a blessing in disguise, because otherwise we might not 
have separated the process of scanning for differences from resolving them as 
early as we did.  

Key to the process of synchronizing records is first scanning them for 
differences.  Actually scanning entire tables at once is feasible but 
impractical: the scan of a single table can take more than an hour, which lead 
to treating scans as if they were made of unicorn poop.  

Database scans are not made of unicorn poop.  They just aren't.  

The key to letting scans be lightweight is to keep track of where we are at any 
point so that we can pick right back up when we get interupted.
