!SLIDE subsection
# We wrote a Rails plugin

!SLIDE bullets
# That is

* Dave Ladelfa started it
* I finished it
* Evan did the UI

!SLIDE  smbullets incremental
# 4 Parts #

* Acts\_as\_\* ActiveRecord modules
* A data mapping DSL
* Synchronization tracking tables
* Synchronization manager

!SLIDE 
# An Aside #
### Welcome Millennium-vania

!SLIDE bullets
# Synchronization Management #

* Some Rake tasks
* Class to collect methods

!SLIDE bullets
# ActiveRecord

* acts\_as\_millennium
* acts\_as\_millsyncable
* mark AR objects as pairs

!SLIDE 
# acts\_as\_millennium #

    @@@ Ruby
    class Millennium::Payment < Millennium::Base
      acts_as_millennium 
        :associated_object => :person_payment
      belongs_to_corebio_via :payid
    end

!SLIDE 
# acts\_as\_millsyncable #

    @@@ Ruby
    class PersonPayment < ActiveRecord::Base
      acts_as_millsyncable 
        :millennium_class_name => "Payment"         
    end

!SLIDE 
# Data Mapping #
## **Problem:**
### Data stored in one database isn't properly formatted for the other

!SLIDE bullets incremental
# Example #

* Millennium pads strings to fixed length
* Millennium stores timestamps without a time zone


!SLIDE small
# Field pairs #

    @@@ Ruby
    class Millennium::Payment < Millennium::Base
      set_field_pair "Amount", :amount, :paydueamt
    end


!SLIDE small
# Field pairs #

    @@@ Ruby
    set_field_pair "Amount", :amount, :paydueamt
    #              pretty name

!SLIDE small
# Field pairs #

    @@@ Ruby
    set_field_pair "Amount", :amount, :paydueamt
    #                        local column

!SLIDE small
# Field pairs #

    @@@ Ruby
    set_field_pair "Amount", :amount, :paydueamt
    #                                 millennium column


!SLIDE smaller
# Field pairs #

    @@@ Ruby
    set_field_pair "Due Date", :due_on, :payduedate do |field_pair|
      field_pair.millennium_to_app do |record|
        dt = millennium_value(record)
        return nil if dt.nil?
        return Time.utc(dt.year, dt.month, dt.day, dt.hour, dt.min, dt.sec).in_time_zone
      end
  
      field_pair.app_to_millennium do |record|
        dt = app_value(record)
        return nil if dt.nil?
        return dt.utc
      end
    end

!SLIDE small
# Field pairs #

    @@@ Ruby
    set_field_pair "Due Date", :due_on, :payduedate do |field_pair|
      field_pair.standard_conversion :TimeWithZone
    end

!SLIDE  full-page
![Image Description](image/MismatchedEvan.png)

!SLIDE 
# Synchronization tracking #

!SLIDE bullets
# Scan for missing rows #

* Insert on one side
* Delete on the other

!SLIDE bullets
# Scan for mismatched fields #

* Data translation in both directions

!SLIDE bullets
# Both a recorded in the local DB #

* SynchronizationAdd
* SynchronizationMismatch

!SLIDE 
# We resolve what we can #

!SLIDE bullets incremental
# The rest goes into a DBA interface #

* Yes: a human being approves a lot of the updates
* Required by the client

!SLIDE  bullets incremental
# New Problem#

* Scans of the busy database are long
* Full scan is about 6 hours
* And there's memory leaks

!SLIDE 
# Database scans are long running #

!SLIDE 
# And valuable... #

!SLIDE  full-page
# ... like unicorn poop #

![Image Description](image/unicorn_poop.jpg)

!SLIDE
# Database scans are not made of unicorn poop.  
# They just aren't.

!SLIDE bullets incremental
# The key is letting scans be lightweight 

* Keep track of where they are
* Can pick right back up when interupted
* (When, not if)

!SLIDE  full-page
![Image Description](image/scan-z.png)

!SLIDE  full-page
![Image Description](image/scan-9.png)

!SLIDE  full-page
![Image Description](image/scan-8.png)

!SLIDE  full-page
![Image Description](image/scan-7.png)

!SLIDE  full-page
![Image Description](image/scan-6.png)

!SLIDE  full-page
![Image Description](image/scan-5.png)

!SLIDE  full-page
![Image Description](image/scan-4.png)

!SLIDE  full-page
![Image Description](image/scan-3.png)

!SLIDE  full-page
![Image Description](image/scan-2.png)

!SLIDE  full-page
![Image Description](image/scan-1.png)


<!-- vim: ft=markdown:fo=:lbr 
Consider :so showoff.vim 
--> 
