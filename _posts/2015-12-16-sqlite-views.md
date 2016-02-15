---
title: Views in SQLite
date: 2015-12-16
---

SQLite has a pretty nifty feature called a [View][2] which allows you to compose a
read-only virtual table from a stored query. If you join quite a lot among your
tables Views can prove to be very advantageous. It can also make your contract 
classes clean as a whistle.

To give you an idea of the benefits of using views in a real Android application
check out my [SimpleNoteTaker][1] application. The schema consists of three tables: 
Notes, Tags, and NoteTags. When the user views a note, a list of tags is displayed
through the use of a join between the `NoteTags` and `Tags` tables. When selecting
a tag in the navigation drawer a join occurs between `Notes` and `NoteTags` to
display the appropriate notes.

The View-less version of this project consisted of contract classes specifically
for the three tables. SQL statements consisting of (not so complex) joins were
built when they were requested, multiple fields in contracts were requested, it
just looked and felt horrible.

With Views I could begin to treat the joins like ordinary tables. I built contract
classes for each view which made referencing columns a hell of a lot easier. SQL
statements asking for joins were no longer built on the fly. All I had to do issue
a simple select statement, this made the [querying portion][3] of my Content Provider
code so much cleaner.

If you ever face a project where you codebase is littered with SQL statements 
consisting of joins built on the fly, look into Views, it might just make your
code a hell of a lot easier to maintain and might even make your day brighter.
Also, if you happen to be querying often, this can actually have a positive
performance impact as the raw SQL is parsed once and then reused the rest of
the time.

[1]: https://github.com/asadmshah/SimpleNoteTaker/blob/master/app/src/main/java/com/asadmshah/simplenotetaker/database/
[2]: https://www.sqlite.org/lang_createview.html
[3]: https://github.com/asadmshah/SimpleNoteTaker/blob/master/app/src/main/java/com/asadmshah/simplenotetaker/database/DatabaseProvider.java#L92
