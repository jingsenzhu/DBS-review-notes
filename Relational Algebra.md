### Chapter 1: Introduction

* Data Manipulation Language (DML)
* Data Definition Language (DDL)



### Chapter 2: Intro to Relational Model

**Attribute:** domain (The set of allowed values) , atomic (Attribute values are
 required to be) , null (special value)

**Relation schema:** $A_1,A_2,\ldots,A_n$ attributes, then $R=(A_1,A_2,\ldots,A_n)$ is a
*relation schema*. Represented by a *row* in a table

**Keys:** $K\subseteq R$, *K* is a **superkey** of *R* if values for *K* are sufficient to identify a unique tuple of each possible relation *r(R)* . One of the candidate keys is selected to be the **primary key**

#### Relational Query Languages

**Selection of tuples: **$\sigma_{A=B \text{ and } D>5}(r)$

**Selection of columns: ** $\Pi_{A,C}(r)$

**Cartesian Product:** $\times$

**Union**: $\cup$

**Set difference:** $-$

**Intersection:** $\cap$

**Natural Join:** $\Join$



### Chapter 6: Formal Relational Query Languages 

#### Relational Algebra

**Six basic operators**

* select $\sigma$: $\sigma_p(r)=\{t|t\in r\text{ and }p(t)\}$
* project $\Pi$: $\Pi_{A_1,\ldots,A_k}(r)$
* union $\cup$: $r\cup s=\{t|t\in r\text{ or }t\in s\}$
* set difference $-$: $r - s=\{t|t\in r\text{ and }t\notin s\}$
* Cartesian product $\times$: $r \times s=\{t\ q|t\in r\text{ and }q\in s,r\cap s=\varnothing\}$
* rename $\rho$: $\rho_{x(A_1,\ldots,A_n)}(E)$: attributes renamed to $A_1,\ldots,A_n$
  * e.g. Find the largest salary in the university
    * Find instructor salaries that are less than some other instructor salary (i.e. not maximum)
      $\Pi_{instructor.salary}(\sigma_{instructor.salary < d.salary}(instructor\times \rho_d(instructor))$
    * Find the largest salary
      $\Pi_{salary}(instructor)-\Pi_{instructor.salary}(\sigma_{instructor.salary < d.salary}(instructor\times \rho_d(instructor))$

**Additional Operations**

* intersection $\cap$: $r\cap s=\{t|t\in r\text{ and }t\in s\}$

* Natural Join $\Join$

* Theta Join $\Join_\theta$: $r\Join_\theta s=\sigma_{\theta}(r\times s)$

* Assignment $\leftarrow$

* Outer Join: 

  * Left outer join $\sqsupset\Join$: $r\sqsupset\Join s=(r\Join s)\cup ((r-\Pi_R(r\Join s))\times\{(null,\ldots,null)\})$
  * Right outer join $\Join\sqsubset$
  * Full outer join $\sqsupset\Join\sqsubset$

* Null values: nComparisons
  with null values return the special truth value: *unknown*
  (unknown or true) = true, (unknown or false) = unknown, (unknown or unknown) = unknown
  (unknown and true) = unknown, (unknown and false) = false, (unknown and unknown) = unknown
  (not unknown) = unknown

* Division $\div$: Given relations $r(R),s(S)$, assume $S\subset R$, the largest relation $t(R-S)$ such that $t\times s\subseteq r$
  $$
  temp1\leftarrow \Pi_{R-S}(r)\\
  temp2\leftarrow \Pi_{R-S}((temp1\times s)-\Pi_{R-S,S}(r))\\
  result = temp1 - temp2
  $$
  e.g:
  $$
  r(ID,cource\_id)=\Pi_{ID,course\_ID}(takes)\\
  s(cource\_id)=\Pi_{cource\_id}(\sigma_{dept\_name=''Biology''}(course))\\
  r\div s=\text{students who have taken all courses in the Biology department}
  $$

* Generalized Projection

* Aggregate Functions and Operations $\mathcal{G}$: $_{G_1,\ldots,G_n}\mathcal{G}_{F_1(A_1),\ldots,F_n(A_n)}(E)$

  * $G_1,\ldots,G_n$: A list of attributes on which to group (can be empty)

  * $F_i$: an aggregate function:

    * **avg**
    * **min**
    * **max**
    * **sum**
    * **count**

  * $A_i$: an attribute name

  * e.g. Find the average salary in each department: $_{dept\_name}\mathcal{G}_{\mathbf{avg}(salary)}(instructor)$

  * Rename: $_{dept\_name}\mathcal{G}_{\mathbf{avg}(salary)\ \mathbf{as}\ avg\_sal}(instructor)$

    





