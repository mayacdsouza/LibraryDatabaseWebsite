# Library Database Website

Project Title: Abibliophobia Anonymous

Team Member Names: Maya D’Souza, Nathan Huffman

Team Number: 35

<h2>Executive Summary</h2>

From our initial database idea, to our final portfolio, we made a series of changes to our database in order to improve the design. We began by creating an outline for our database that included a general description of the database, an ER Diagram, and a list of entities and foreign keys. One of the changes we made on these sections was to make sure our naming conventions were consistent in capitalization and pluralization across entities and attributes. Additionally, we fixed our foreign key relationships on this step to make sure the “many” entity in one-to-many relationships had a foreign key of the “one” entity instead of the other way around.

We then created a schema for our database and continued to improve our design. We simplified our system by changing it from having multiple library branches represented by a Libraries entity to getting rid of the Libraries entity and changing our system to have 1 branch. We also modified the way we designed our library items over time. We began by having one entity for each type of item (i.e. Books, Movies, etc.). This entity was linked to our Library_Items entity. We changed this to have an Item_Types entity that stored info about the type like the allowed checkout time, fines per day, and name of the type. Each row in Library_Items now has a foreign key link to Item_Types. This helped to normalize our database and reduce the number of entities needed to store the same information.

We then created SQL code to create our entities and insert data for each entity. As we continued to design our database, we improved this code by adding additional comments to improve readability, verifying our cascading rules for the Holds entity so that a hold gets deleted if the associate Library_Item or patron does, and verifying the type for all attributes (VARCHAR, INT, etc.) was set correctly.

Next we worked on creating a database website that allowed users to perform the relevant create/read/update/delete operations. We began by creating a static html page to represent the layout . We then used NodeJS/Handlebars to convert this into an interactive database website. Some improvements we made in this process were adding styling to the pages for each entity, making sure the layout of each entity page was consistent, and having the titles of the foreign key entity displayed in all tables/dropdowns instead of the foreign key ID. Another was to think through which CRUD operations made sense for each entity and to reduce some operations that did not make sense. The last change we made was to work through bugs that we had in our code that prevented the CRUD operations from functioning properly. A few examples of this were tables not displaying correctly when adding data unless you manually refreshed the page and not being able to update the patron_id foreign key for Library_Items to NULL.

In working through all these changes, we progressed from a high-level concept for a Library database to a fully-functioning interactive database website.

<h2>Project Outline</h2>

A small town of 3500 people is looking to build a new library for its residents.  The library will be accessible to anyone who registers for an account. It currently holds 3 media formats (Books,  Movies, and CD’s) and has the capacity to handle additional formats. It can also hold up to 10,000 titles. The library will need to keep track of patrons and patron data, such as their name and the amount of fines they have. The library will also need the ability to catalog inventory and keep track of location of items to know when they are checked in and out and who currently has each item. Additionally, the library will allow patrons to place holds on items and will track their queue position on an item.

<h2>Database Outline</h2>

Entity1: Library_Items (a list of all the items in the library system i.e. 2 of the same book are considered separate items)

    - item_id: int, auto_increment, unique, not NULL, PK
    - title: varchar(200), not NULL
    - genre: varchar(50), not NULL
    - author: varchar(100), not NULL
    - year: int, not NULL
    - patron_id: int, FK
    - item_type_id: int, not NULL
    - Relationships:
      - many: 1 between Library_Items and Patrons using patron_id as FK (this will represent item location)
      - many:many between Library_Items and Patrons to represent holds. To create this relationship we have a 
        holds entity that has M:1 relationships with Library_Items and Patrons

Entity2: Patrons (a list of all the patrons registered in the library system)

    - patron_id: int, auto_increment, unique, not NULL, PK
    - first_name: varchar(100), not NULL
    - last_name: varchar(100), not NULL
    - fine: int, not NULL
    - Relationships: 
      - many:many between Library_Items and Patrons to represent holds. To create this relationship we have a 
        holds entity that has M:1 relationships with Library_Items and Patrons
      - 1:M between Patrons and Library_Items
	
Entity3: Holds (a list of holds for items in the library)

    - hold_id: int, auto_increment, not NULL, PK
    - hold_date: date, not NULL
    - queue_position: int, not NULL (some summary value we calculate to issue book)
    - item_id: int, not NULL FK
    - patron_id: int, not NULL, FK
    - Relationships:
      - M:1 with Library_Items (many holds can be for 1 item), item_id from Library_Items as a FK
      - M:1 with Patrons (many holds can be for 1 patron), patron_id as a FK

Entity4: Item_Types

    - item_type_id: int, auto_increment, not NULL, PK
    - type: varchar(20), not NULL
    - check_out_length: int, not NULL
    - fine_per_day: int, not NULL

<h2>Entity Relationship Diagram (ERD)</h2>

![image](https://user-images.githubusercontent.com/97061012/233409965-8167422a-c200-4ff6-bcbc-21154fbf7137.png)


<h2>Schema</h2>

![image](https://user-images.githubusercontent.com/97061012/233410509-0ecb36e5-d4ca-4406-acea-25f3c160a210.png)

<h2>Sample Data</h2>

![image](https://user-images.githubusercontent.com/97061012/233410709-1bbdbe7d-055d-4163-b9a5-1bfae13fb5d1.png)

<h2>Screen Captures</h2>



Figure 1: Homepage with Links to each Entity

![image](https://user-images.githubusercontent.com/97061012/233410894-ea79dac0-77d9-4350-9d14-fd2eb1e5d509.png)


Figure 2: Library Items Page with Create/Read/Update/Delete Functionality
Note: Our NULLable relationship is the FK patron_id in Library_Items

![image](https://user-images.githubusercontent.com/97061012/233410972-68eed1e4-9111-46af-beeb-1722bf2a774f.png)


Figure 3: Patrons Page with Create/Read/Update Functionality

![image](https://user-images.githubusercontent.com/97061012/233411073-3ba0f7dd-1f4a-41c8-a488-32143d5d0cc5.png)


Figure 4: Holds Page with Create/Read/Update/Delete Functionality
Note: The Holds entity is the intersection table between Library_Items and Patrons to facilitate the M:M relationship. If a Library Item is deleted, the associated Holds will also be deleted.

![image](https://user-images.githubusercontent.com/97061012/233411169-b2f3e9a0-ad7f-48d5-8b5a-b3ad3ad9d0a6.png)


Figure 5: Item Types Page with Create/Read Functionality

![image](https://user-images.githubusercontent.com/97061012/233411261-cf2797ff-49fc-4df1-b3e7-d700721e73d2.png)



