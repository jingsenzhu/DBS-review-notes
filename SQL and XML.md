### Chapter 3:  Introduction to SQL

#### Domain Types:

* **char(n)**
* **varchar(n)**
* **int**
* **smallint**
* **numeric(p,d)**: p digits with d decimals
* **real, double precision**
* **float(n)**: n precisions

#### Constructs

- **create table** *r* (*A1 D1*, ..., *An Dn*, (integrity-constraint1), ..., (integrity-constraintk))
- **insert into** *r* **values**(...)
- **Integrity Constraints **:
  - **not null**
  - **primary key**(*A*1, ..., *An*) : automatically **not null**
  - **foreign key** (*A*m, ..., *An* ) **references** *r*
- **drop table** *r*
- **delete from** *r*
- **alter table** *r* **add** *A D*
- **alter table** *r* **drop** *A* 

#### **Basic Query Structure** 

SQL **data-manipulation language (DML)**

* **select** *A*1, ..., *An* **from** *r*1, r*2*, ..., *rm* **where** *P*
  * **select distinct**: force the elimination of duplicates
  * **select all**: duplicates not be removed
  * **select \***: all attributes
* **Natural Join**
  * **select** *name*, *course_id* **from** *instructor, teaches* **where** *instructor.ID* = *teaches.ID*;
    **select** *name*, *course_id* **from** *instructor* **natural join** *teaches*;
  * Danger in natural join: beware of unrelated attributes with same name which get equated incorrectly
  * *A* **join** *B* **using**(*attribute*)
* **Rename: as**
  * e.g. Find the names of all instructors who have a higher salary than some instructor in ‘Comp. Sci’.
    **select distinct** *T.name* **from** *instructor* **as** *T,instructor* **as** *S* 
    **where** *T.salary* *>* *S.salary* **and** *S.dept_name* *= ‘Comp. Sci.’*
  * **as** can be omitted
* **String operations**
  * percent (%).  The % character matches any substring.
  * underscore (_).  The _ character matches any character.
  * e.g.
    * ‘Intro%’ matches any string beginning with “Intro”.
    * ‘%Comp%’ matches any string containing “Comp” as a substring.
    * ‘_ _ _’ matches any string of exactly three characters.
    * ‘_ _ _ %’ matches any string of at least three characters.
* **desc, asc**: descending, ascending order for the display (asc default)
* **between x and y** : $x\leq \text{and} \leq y$
* Tuple comparison: **where** (instructor.ID, dept_name) = (teaches.ID, ’Biology’);
* **Set operarions**
  * **union**
  * **intersect**
  * **except**
* **is null** (**not **= null)
* **Aggregate Functions**
  * **group by**
  * **having**
    * predicates in the **having** clause are applied after the formation of groups whereas predicates in the **where** clause are applied before forming groups
    * e.g. 
      **select** dept_name, avg (salary) **from** instructor **group by** dept_name **having** avg (salary) > 42000;
  * All aggregate operations except **count(*)** ignore tuples with null values on the aggregated attributes
* **Nested Subqueries**
  * **(not) in**
* **Set comparison**
  * **some**
  * **all**
* **Test for Empty Relations**
  * **exists** r $\equiv r\neq\varnothing$
  * **not exists** r $\equiv r=\varnothing$
    * $X-Y=\varnothing\equiv X\subseteq Y$
    * i.e. **not exists**(X **except** Y)
* **unique** construct tests whether a subquery has any duplicate tuples 
* **with** ... **as** *A*

#### Modification of the Database 

* **delete**
* **insert**
  * **insert into** *r* **values**(...)
  * **insert into** *r* **select** ... ...
* **update** *r* **set** *A*=... **where**
  * **set** *A* = **case**
    			 **when** ... **then** *x*
    			 **else** *y*
    		 	**end**



### Chapter 4: Intermediate SQL

#### Join operations

**natural left outer join, natural right outer join, natural full outer join**

#### Views

**create view** *v* **as** <...>

#### Integrity Constraints

* **check clause**: **check**(*P*)
  e.g: create table section (
         	course_id varchar (8),
         	sec_id varchar (8),
         	semester varchar (6),
         	year numeric (4,0),
         	building varchar (15),
         	room_number varchar (7),
         	time slot id varchar (4), 
         	primary key (course_id, sec_id, semester, year),
         	**check** (semester in (’Fall’, ’Winter’, ’Spring’, ’Summer’))
         );
  * Complex check clauses: **check**(xxx **in** (**select** ......))

* **Cascading**: **on update/delete cascade/set null/set default**

* **Built-in Data Types in SQL **: **data, time, timestamp, interval**

#### Index

**create** index *I* **on** *r*(*A*)

#### Authorization

**grant** \<privilege list>
		**on** \<relation name or view name> **to** \<user list>

**revoke** \<privilege list>
		**on** \<relation name or view name> **from** \<user list>

**privileges**

* select
* insert
* update
* delete
* all privileges



### Chapter 5:  Advanced SQL

**JDBC, ODBC**

#### Procedural Constructs in SQL

* **SQL Functions**:
  * **create function** *func_name* (*parameters*)
    **returns ...(type)**
    **begin** ... **end**
  * **Table functions**
* **Procedures**

### Triggers

* **create trigger** *\<name\>* **on** *\<table>* **before/after** ***\<insert/delete/update>* of** *\<tablename>* 
  * **referencing old/new row as** *\<name>*
  * **for each row**
  * e.g 
    **create trigger** credits_earned **after update of** takes **on** (grade)
    **referencing new row as** nrow
    **referencing old row as** orow
    **for each row**
    **when** nrow.grade <> ’F’ **and** nrow.grade **is not null**
        **and** (orow.grade = ’F’ **or** orow.grade **is null**)
    **begin atomic
         update** student
         **set** tot_cred= tot_cred + 
               (**select** credits
                **from** course
                **where** course.course_id= nrow.course_id)
         **where** student.id = nrow.id;
    **end**;



### Chapter 23:  XML

* **Structure of XML Data**: *\<tagname\>* element *\</tagname>*
  Elements must be properly ***nested***

* **Attributes**
  ```xml
  <course  course_id = “CS-101”  credits=“4”>
  ```

* **Namespaces**

  ```xml
   <university xmlns:yale=“http://www.yale.edu”>
   ...
       <yale:course>
           <yale:course_id> CS-101 </yale:course_id>
           <yale:title> Intro. to Computer Science</yale:title>
           <yale:dept_name> Comp. Sci. </yale:dept_name>
           <yale:credits> 4 </yale:credits>
       </yale:course>
   ...
  </university>
  ```

* **Document Type Definition (DTD)**

  ```xml-dtd
  <!ELEMENT element (subelements-specification) >
  <!ATTLIST element attributes-specification  >
  ```

  * **Element Specification**

    * name of elements

    * #PCDATA (parsed character data)

    * EMPTY / ANY

      ```xml-dtd
      <!ELEMENT department (dept_name,  building, budget)>
      <!ELEMENT dept_name (#PCDATA)>
      <!ELEMENT budget (#PCDATA)>
      ```

    * Reg-Exp

      ```xml-dtd
      <!ELEMENT university ( ( department | course | instructor | teaches )+)>
      ```

    * e.g

        ```xml-dtd
        <!DOCTYPE  university [
            <!ELEMENT university ( (department|course|instructor|teaches)+)>
            <!ELEMENT department ( dept name, building, budget)>
            <!ELEMENT course ( course id, title, dept name, credits)>
            <!ELEMENT instructor (IID, name, dept name, salary)>
            <!ELEMENT teaches (IID, course id)>
            <!ELEMENT dept name( #PCDATA )>
            <!ELEMENT building( #PCDATA )>
            <!ELEMENT budget( #PCDATA )>
            <!ELEMENT course id ( #PCDATA )>
            <!ELEMENT title ( #PCDATA )>
            <!ELEMENT credits( #PCDATA )>
            <!ELEMENT IID( #PCDATA )>
            <!ELEMENT name( #PCDATA )>
            <!ELEMENT salary( #PCDATA )>
        ]>
        ```

  * **Attribute Specification**
  
    * name + type + *\<options>*
  
    * Type: CDATA, ID, IDREF, IDREFS
  
    * Options:
  
      * mandatory (#REQUIRED)
      * has a default value (value)
      * neither (#IMPLIED)
  
    * e.g.
  
      ```xml-dtd
      <!ATTLIST course
      	course_id   ID     #REQUIRED
          dept_name   IDREF  #REQUIRED
      	instructors IDREFS #IMPLIED   >
      ```
  
* e.g. University DTD with Attributes

  ```xml-dtd
  <!DOCTYPE university-3 [
          <!ELEMENT university ( (department|course|instructor)+)>
          <!ELEMENT department ( building, budget )>
          <!ATTLIST department
              dept_name ID #REQUIRED >
          <!ELEMENT course (title, credits )>
          <!ATTLIST course
              course_id ID #REQUIRED
              dept_name IDREF #REQUIRED
              instructors IDREFS #IMPLIED >
          <!ELEMENT instructor ( name, salary )>
          <!ATTLIST instructor
              IID ID #REQUIRED 
              dept_name IDREF #REQUIRED >
          · · · declarations for title, credits, building,
          budget, name and salary · · ·
      ]>
  ```

* e.g. XML Data

  ```xml-dtd
  <university-3>
         <department dept name=“Comp. Sci.”>
                 <building> Taylor </building>
                 <budget> 100000 </budget>
         </department>
         <department dept name=“Biology”>
                 <building> Watson </building>
                 <budget> 90000 </budget>
         </department>
         <course course id=“CS-101” dept name=“Comp. Sci”
                           instructors=“10101 83821”>
                  <title> Intro. to Computer Science </title>
                  <credits> 4 </credits>
         </course>
         ….
         <instructor IID=“10101” dept name=“Comp. Sci.”>
                  <name> Srinivasan </name>
                  <salary> 65000 </salary>
         </instructor>
         ….
  </university-3>
  ```

#### Querying and Transforming XML Data

* **XPath**
  * `/university-3/instructor/name` :
        \<name>Srinivasan\</name>
        \<name>Brandt\</name>
  * `/university-3/instructor/name/text( ) `: 
        returns the same names, but without the enclosing tags
  * Selection predicates in []:
    `/university-3/course[credits >= 4] `
  * Attributes are accessed using “@”
    `/university-3/course[credits >= 4]/@course_id`
  * Functions
    * count()
      `/university-2/instructor[count(./teaches/course)> 2]`
    * id() : IDREF
      `/university-3/course/id(@dept_name)`
    * "|" for union, "//" to skip multiple levels, etc.
  
* **XQuery**

  * ```xquery
    for ... let ... where ... order by ... result ...
    ```

    * `for` $\equiv$ **SQL** `from`
    * `where`
    * `order by`
    * `result` $\equiv$ **SQL** `select`
    * `let` : temporary variables

  * **FLWOR**

    ```xquery
    for  $x in /university-3/course
    let   $courseId := $x/@course_id
    where $x/credits > 3
    return <course_id> { $courseId } </course id>
    ```

    selection used in XPath

    ```xquery
    for $x in /university-3/course[credits > 3]
    return <course_id> { $x/@course_id } </course_id>
    ```

  * **JOIN**

    ```xquery
    for $c in /university/course,
    	$i in /university/instructor,
    	$t in /university/teaches
    where $c/course_id= $t/course_id and $t/IID = $i/IID
    return <course_instructor> { $c $i } </course_instructor>
    ```

  * **Nested Queries**

    ```xquery
    for $d in /university/department
    return <department>
    		{ $d/* }
        	{ for $c in /university/course[dept name = $d/dept name]
        	  return $c }
           </department>
    ```

  * **Grouping and Aggregation**

    ```xquery
    for $d in /university/department
    return
           <department-total-salary>
                 { $d/dept name } 
                 <total_salary> { fn:sum(
                      for $i in /university/instructor[dept_name = $d/dept_name]
                      return $i/salary
                    ) } 
                  </total_salary>
            </department-total-salary> 
    ```

  * **Sorting**: `order by`

    ```xquery
    for $i in /university/instructor
    order by $i/name
    return <instructor> { $i/* } </instructor>
    ```

    

  

  

