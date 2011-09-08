<!-- vim: ft=markdown:fo=:lbr 
Consider :so showoff.vim 
--> 
!SLIDE 
# My Presentation #

!SLIDE bullets incremental
# Bullet Points #

* first point
* second point
* third point

!SLIDE 
# The project #
We used to use this case as an interview question.  If you're looking for a 
solid contract job, you might want to listen carefully.

We were hired to write a social networking website for alumni of a large 
university.  The big challenge was that the reference for user data was an 
atavistic Oracle database.  An Oracle database application.  Which meant that 
we couldn't make any changes to the schema.  So what could we do?  

We needed to be able to have our own data to keep track of features our client 
wanted.  So, we needed our own separate database.

We considered synchronizing the two databases.  But that's insane, right?

At first, we considered using two completely different databases.  And there's 
some sense to that.  Two nice, clean separate resources - ideal.  Consider  
doing joins though: suddenly the query takes seconds to return.

But the alternative is to do synchronization.  And that's insane, right?  
That's what we thought.  Ultimately, we needed to do exactly that.

There was an extra requirement: all changes needed to be confirmed by an 
administrative user.
