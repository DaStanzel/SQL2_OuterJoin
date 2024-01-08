# SQL2_OuterJoin

# Inner Join vs Outer Join

![Inner vs Outer Joins](https://wiki.streampy.at/index.php?title=File:InnerOuter.png)


## Outer Join

Inner Join: Falls eine Zeile einer Tabelle die Join-Bedingung nicht erfüllt, so wird sie auch nicht ausgegeben. Z.B. hat die Abteilung 40 keine Mitarbeiter in der Tabelle `EMP`. Daher wird die Abteilung 40 auch nicht ausgegeben.

Will man trotzdem die Abteilungen, die keine Mitarbeiter beschäftigen, ausgeben, so verwendet man den outer join Operator:

```sql
...WHERE DEPT.DEPTNO = EMP.DEPTNO (+)
```

*Erzeugt als Ergebnis mindestens alle Tupel einer der beiden Relationen.*

Durch das Outer Join Symbol wird SQL veranlasst, die Tabelle `EMP` so zu behandeln, als würde sie eine weitere Zeile mit einem Null-Wert in jeder Spalte enthalten. Diese Zeile wird dann mit denjenigen Zeilen der Tabelle `DEPT` verbunden, die sonst mit keiner Zeile aus `EMP` verbunden werden können.

### Beispieltabellen

```sql
CREATE TABLE e1 AS SELECT * FROM emp WHERE deptno IN (10,20);
CREATE TABLE e2 AS SELECT * FROM emp WHERE deptno IN (20,30);
```

Ergebnis:

![OuterJoinTables](OuterJoinTables.png)

### Left Outer Join

Ein `LEFT OUTER JOIN` enthält immer alle Records der linken Tabelle, auch wenn die Join Condition keine Entsprechung in der rechten Tabelle liefert.

Oracle Notation:

```sql
SELECT e1.ename, e1.deptno, e2.ename, e2.deptno
FROM   e1, e2
WHERE  e1.empno = e2.empno (+);
```

ANSI Notation:

```sql
SELECT e1.ename, e1.deptno, e2.ename, e2.deptno
FROM   e1 LEFT OUTER JOIN e2
USING (empno);
```

Ergebnis:

```
ENAME      DEPTNO   ENAME      DEPTNO    
------------------------------------- 
JONES      20       JONES      20       
FORD       20       FORD       20        
SMITH      20       SMITH      20        
SCOTT      20       SCOTT      20        
ADAMS      20       ADAMS      20        
KING       10                           
CLARK      10                            
MILLER     10    
```

### Right Outer Join

In diesem Fall wird der Join von rechts nach links interpretiert. Fehlende ‚Join Partner‘ auf der linken Seite werden durch `NULL` ergänzt.

Oracle Join:

```sql
SELECT e1.ename, e1.deptno, e2.ename, e2.deptno
FROM   e1, e2
WHERE  e1.empno (+) = e2.empno;
```

ANSI Join:

```sql
SELECT e1.ename, e1.deptno, e2.ename, e2.deptno
FROM   e1 RIGHT OUTER JOIN e2
USING (empno);
```

Ergebnis:

```
ENAME      DEPTNO            ENAME      DEPTNO   

JONES      20                JONES      20       
FORD       20                FORD       20       
SMITH      20                SMITH      20 
SCOTT      20                SCOTT      20       
ADAMS      20                ADAMS      20       
                             TURNER     30       
                             BLAKE      30       
                             ALLEN      30       
                             WARD       30       
                             MARTIN     30                                									        
                             JAMES      30  
```

### Full Outer Join

#### ANSI Full Outer Join

Dabei wird das Ergebnis eines `LEFT` und eines `RIGHT OUTER JOINs` mit einander kombiniert:

```sql
SELECT e1.ename, e1.deptno, e2.ename, e2.deptno
FROM e1 FULL OUTER JOIN e2
USING (empno);
```

Ergebnis:

```
ENAME      DEPTNO    ENAME      DEPTNO 
---------- ---------------------- ---- 
ADAMS      20        ADAMS      20     
CLARK      10                          
FORD       20        FORD       20     
JONES      20        JONES      20     
KING       10                          
MILLER     10                          
SCOTT      20        SCOTT      20     
SMITH      20        SMITH      20     
                     ALLEN      30     
                     BLAKE      30     


                     JAMES      30     
                     MARTIN     30     
                     TURNER     30     
                     WARD       30     
```

#### Oracle Full Outer Join

```sql
SELECT e1.ename, e1.deptno, e2.ename, e2.deptno
FROM   e1, e2
WHERE  e1.empno (+) = e2.empno
UNION
SELECT e1.ename, e1.deptno, e2.ename, e2.deptno
FROM   e1, e2
WHERE  e1.empno = e2.empno (+)
```

#### ANSI Full Outer Join mit Prädikat

```sql
SELECT d.dname, d.loc, e.ename, e.job
FROM dept d LEFT OUTER JOIN emp e
ON (d.deptno = e.deptno)
WHERE  d.loc = 'DALLAS';
```

#### Beispiel

Gesucht sind alle Abteilungen, die keinen Mitarbeiter beschäftigen.

```sql
SELECT DISTINCT DEPT.DEPTNO, DNAME, LOC
FROM DEPT,EMP
WHERE DEPT.DEPTNO = EMP.DEPTNO (+)
  AND EMPNO IS NULL;
```
