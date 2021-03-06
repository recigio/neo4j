# Atividade Prática – Neo4J
Atividades relacionadas Neo4J da pós de DataScience da Furb.


# :play intro-neo4j-exercises 

## Exercício 1

1.1: Retrieve all nodes from the database
```
1.1 match(a) return a
```
1.2: Examine the data model for the graph
```
1.2 CALL db.schema()
```
1.3: Retrieve all Person nodes
```
1.3 match(person:Person) return person
```
 1.4: Retrieve all Movie nodes.
```
1.4 match(movies:Movie) return movies
```
## Exercício 2

2.1: Retrieve all movies that were released in a specific year.
```
2.1 match(movie:Movie{released:2003}) return movie
```
2.2: View the retrieved results as a table
```
2.2 Clicar no icone da tabela do resultado
```
2.3: Query the database for all property keys
```
2.3 CALL db.propertyKeys
```
2.4: Retrieve all Movies released in a specific year, returning their titles
```
2.4 match(movie:Movie{released:2006}) return movie.title, 

match(movie:Movie{released:2006}) return movie.title, movie.released, movie.rating
```
2.5: Display title, released, and tagline values for every Movie node in the graph.
```
2.5 match(a:Movie) return a.title, a.released, a.tagline
```
2.6: Display more user-friendly headers in the table
```
2.6 MATCH (m:Movie) RETURN m.title AS `titulo`, m.released AS `lancamento`, m.tagline AS `slogan`
```
## Exercício 3

3.1: Display the schema of the database
```
3.1 CALL db.schema()
```
3.2: Retrieve all people who wrote the movie Speed Racer
```
3.2 
match(person:Person)-[:WROTE]->(movie:Movie{title:'Speed Racer'}) return person

match(person:Person)-[:WROTE]->(movie:Movie) 
where not(movie.title='Speed Racer')
return person;

match(person:Person)-[:ACTED_IN]->(movie:Movie{title:'Speed Racer'}) 
 return person;
 
 match(person:Person)-[:DIRECTED]->(movie:Movie{title:'Speed Racer'}) 
 return person;
```  
3.3: Retrieve all movies that are connected to the person, Tom Hanks.•Exercise

```
match(movie:Movie)<--(p:Person{name:'Tom Hanks'}) return movie

match(movie:Movie)<--(p:Person{name:'Kevin Pollak'}) return movie

match(movie:Movie{title:'Cloud Atlas'})<--(p:Person) return p
```
3.4: Retrieve information about the relationships Tom Hanks had with the set of movies retrieved earlier
```
match(movie:Movie)<-[rel]-(p:Person{name:'Tom Hanks'}) return movie.title,type(rel)

match(movie:Movie)<-[rel]-(p:Person{name:'Keanu Reeves'}) return movie.title,type(rel)

```
3.5: Retrieve information about the roles that Tom Hanks acted in.
```
match(movie:Movie)<-[rel]-(p:Person{name:'Tom Hanks'}) return movie.title,rel.roles

match(movie:Movie)<-[rel]-(p:Person{name:'Keanu Reeves'}) return movie.title,rel.roles

match(movie:Movie{title:'Top Gun'})<-[rel:ACTED_IN]-(p:Person) return movie.title,rel.roles
```

## Exercício 4

4.1: Retrieve all movies that Tom Cruise acted in
```
match(p:Person)-[:ACTED_IN]->(m:Movie)
where p.name = 'Tom Cruise'
return m.title as Filme
```

4.2: Retrieve all people that were born in the 70’s
```
match(p:Person)
where p.born > 1969 and p.born < 1980
return p.name, p.born
```

Exercise 4.3: Retrieve the actors who acted in the movie The Matrix who were born after 1960
```
match(p:Person)-[:ACTED_IN]->(m:Movie)
where p.born > 1960 and m.title='The Matrix'
return p.name, p.born
```

Exercise 4.4: Retrieve all movies by testing thenode label and a property.
```
match(m)
where m:Movie and m.released=2000
return m.title
```

4.5: Retrieve all people that wrote movies by testing the relationship between two nodes
```
match(p)-[rel]->(m)
where m:Movie and p:Person and type(rel)='ACTED_IN'
return p.name,m.title
```

4.6: Retrieve all people in the graph that do not have a property.
```
match(p)
where p:Person and not(exists(p.born))
return p.name
```

4.7: Retrieve all people related to movies where the relationship has a property.
```
match(p:Person)-[rel]->(m:Movie)
where exists(rel.rating)
return p.name,m.title,rel.rating
```
4.8: Retrieve all actors whose name begins with James.
```
match(p:Person)-[:ACTED_IN]->(m:Movie)
where p.name STARTS WITH 'James'
return p.name
```

4.9: Retrieve all all REVIEW relationships from the graph with filtered results.
```
MATCH (:Person)-[r:REVIEWED]->(m:Movie)
WHERE toLower(r.summary) CONTAINS 'fun'
RETURN  m.title as Filme, r.summary as Opiniao, r.rating as Nota

MATCH (m:Movie)
WHERE m.tagline CONTAINS 'love'
RETURN  m.title
```

Exercise 4.10: Retrieve all people who have produceda movie, but have not directed a movie.
```
MATCH (a:Person)-[:PRODUCED]->(m:Movie)
WHERE NOT ((a)-[:DIRECTED]->(:Movie))
RETURN a.name, m.title
```

Exercise 4.11: Retrieve the movies and their actors where one of the actors also directed the movie
```
MATCH (a1:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(a2:Person)
WHERE exists( (a2)-[:DIRECTED]->(m) )
RETURN  a1.name as Ator, a2.name as `Ator e Diretor`, m.title as Filme
```

4.12: Retrieve all movies that were released in a set of years
```
MATCH (m:Movie)
WHERE m.released in [2000, 2004, 2008]
RETURN m.title, m.released
```

4.13: Retrieve the movies that have an actor’s role that is the name of the movie.
```
MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE m.title in r.roles
RETURN m.title, p.name, r.roles
```

## Exercicio 5

5.1: Retrieve data using multiple MATCH patterns
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person),
      (a2:Person)-[:ACTED_IN]->(m)
WHERE a.name = 'Gene Hackman'
RETURN m.title as filme, d.name AS diretor , a2.name AS `co autores`
```

5.2: Retrieve particular nodes that have a relationship
```
MATCH (p1:Person)-[:FOLLOWS]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2
```

 5.3: Modify the query to retrieve nodes that are exactly three hops away.•
```
MATCH (p1:Person)-[:FOLLOWS*3]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2
```

5.4: Modify the query to retrieve nodes that are one and two hops away.
```
MATCH (p1:Person)-[:FOLLOWS*1..2]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2
```

5.5: Modify the query to retrieveparticular nodes that are connected no matter how many hops are required.
```
MATCH (p1:Person)-[:FOLLOWS*]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2
```

5.6: Specify optional data to be retrieved during the query
```
MATCH (p1:Person)
WHERE toLower(p1.name) STARTS WITH 'tom'
RETURN p1

MATCH (p:Person)
WHERE p.name STARTS WITH 'Tom'
OPTIONAL MATCH (p)-[:DIRECTED]->(m:Movie)
RETURN p.name, m.title
``` 

5.7: Retrieve nodes by collecting a list
```
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
RETURN p.name as ator, collect(m.title) AS `lista de filmes`
```

5.8
```
MATCH (p:Person{name:'Tom Cruise'})-[r:ACTED_IN]->(m:Movie)<-[r2:ACTED_IN]-(p2:Person)
RETURN m.title,p.name, collect(p2.name) AS `quadjuvantes`
```

Exercise 5.9: Retrieve nodes as lists and return data associated with the corresponding lists
```
MATCH (p:Person)-[r:REVIEWED]->(m:Movie)
RETURN m.title, count(p) AS `total de criticas`, collect(p.name) AS `criticos`
```

5.10: Retrieve nodes and their relationships as lists.
```
MATCH (p:Person)-[r:DIRECTED]->(m:Movie)<-[r2:ACTED_IN]-(p2:Person)
RETURN m.title,p.name, count(p2) AS `total de atores`, collect(p2.name) AS `atores`
```

5.11: Retrieve the actors who have acted in exactly five movies.
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WITH  a, count(a) AS numeroFiles, collect(m.title) AS filmes
WHERE numeroFiles = 5
RETURN a.name, filmes
```

5.12: Retrieve the movies that have at least 2 directors with other optional data
```
MATCH (a:Person)-[:DIRECTED]->(m:Movie)<-[r2:ACTED_IN]-(p2)
WITH  collect(a.name) as diretores,m, count(a) as numeroDiretores, collect(p2.name) AS atores
WHERE numeroDiretores = 2 
RETURN diretores,m.title, atores
```

5.13
```
MATCH (m:Movie)
WITH m, size((:Person)-[:DIRECTED]->(m)) AS diretores
WHERE diretores >= 2
OPTIONAL MATCH (p:Person)-[:REVIEWED]->(m)
RETURN  m.title, p.name
```

##Exercício 6

6.1: Execute a query that returns duplicate records
```
MATCH (p:Person)-[r:DIRECTED]->(m:Movie)<-[r2:ACTED_IN]-(p2:Person)
RETURN m.title,p.name, count(p2) AS `total de atores`, collect(p2.name) AS `atores`
```
Modify the query to eliminate duplication
```
match(p:Person)-[r:ACTED_IN]->(m:Movie)
where m.released > 1990 and m.released < 2000
return m.released, collect(m.title), collect(p.name)
```
6.3: Modify the query to eliminate more duplication.
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released > 1990 AND m.released < 2000
RETURN  m.released, collect(DISTINCT m.title), collect(a.name)
```

6.4: Sort results returned.
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released > 1990 AND m.released < 2000
RETURN  m.released, collect(DISTINCT m.title), collect(a.name)
ORDER BY m.released DESC
```

6.5: Retrieve the top 5 ratings and their associated movies.
```
match(p:Person)-[r:REVIEWED]->(m:Movie)
return m.title, r.rating
ORDER BY r.rating DESC
LIMIT 5
```

6.6: Retrieve all actors that have not appeared in more than 3 movies

```
match(p:Person)-[r:ACTED_IN]->(m:Movie)
with count(p.name) as `atuacoes`,p,collect(m.title) as `filmes`
where atuacoes < 4
return p.name, filmes
```

##Exercício 7

7.1: Collect and use lists
```
match(p:Person)-[r:ACTED_IN]->(m:Movie)<-[r2:PRODUCED]-(p2)
with m.title as `titulo`, collect(p.name) as `atores`, collect(p2.name) as `produtores`, count(p.name) as `total`
return DISTINCT titulo,atores,total,produtores
order by total
```

 7.2: Collect a list
```
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WITH p, collect(m) AS filmes
WHERE size(movies)  > 5
RETURN p.name, filmes
```

7.3: Unwind a list
```
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WITH p, collect(m) AS filmes
WHERE size(filmes)  > 5
WITH p, filmes UNWIND filmes AS movie
RETURN p.name, movie.title
```

7.4: Perform a calculation with thedate type.
```
MATCH (a:Person{name:'Tom Hanks'})-[:ACTED_IN]->(m:Movie)
RETURN  m.title, m.released, date().year-m.released, m.released-a.born
```

##Exercício 8

8.1 Create a Movie node
```
CREATE (:Movie {title: 'Cidade de Deus'})
Added 1 label, created 1 node, set 1 property, completed after 3 ms.
```

8.2 Retrieve the newly-created node
```
Match(m:Movie{title:'Cidade de Deus'})
return m
```
8.3 Create a Person node 
```
CREATE (:Person {name: 'Robin Wright'})
```

8.4 Retrieve the Person node you just created by its name
```
MATCH (p:Person{name:'Robin Wright'})
RETURN p
```

8.5: Add a label to a node
```
MATCH (m:Movie)
WHERE m.released < 2010
SET m:OlderMovie
RETURN m.title
```
8.6 Retrieve the node using the new labe
```
MATCH (m:OlderMovie)
RETURN m.title
```

8.7 Add the Female label to selected nodes
```
MATCH (p:Person)
where p.name STARTS WITH 'Robin'
SET p:FEMALE
```

Exercise 8.8: Retrieve all Female nodes
```
MATCH (p:Female)
RETURN p
```

Exercise 8.9: Remove the Female label from the nodes that have this labels
```
MATCH (p:Female)
REMOVE p:Female
```

Exercise 8.10: View the current schema of the graph
```
CALL db.schema
```

Exercise 8.11: Add properties to a movie 
```
MATCH (m:Movie)
WHERE m.title = 'Cidade de Deus'
SET m:OlderMovie,
    m.released = 1994,
    m.tagline = "Life is like a box of chocolates...you never know what you're gonna get.",
    m.lengthInMinutes = 142

OBS:Alterado por que foi o filme cadastrado
```

Exercise 8.12: Retrieve an OlderMovie node to confirm the label and properties
```
MATCH (m:OlderMovie)
WHERE m.title = 'Cidade de Deus'
RETURN m
```

Exercise 8.13: Retrieve an OlderMovie node to confirm the label and properties
```
MATCH (p:Person)
WHERE p.name = 'Robin Wright'
SET p.born = 1966, p.birthPlace = 'Dallas'
```

Exercise 8.14: Retrieve an updated Person 
```
MATCH (p:Person)
WHERE p.name = 'Robin Wright'
return p
```

Exercise 8.15: Remove a property from a Movie node
```
MATCH (m:Movie)
WHERE m.title = 'Cidade de Deus'
SET m.lengthInMinutes = null
```

Exercise 8.16: Retrieve the node to confirm that the property has been removed
```
MATCH (m:Movie)
WHERE m.title = 'Cidade de Deus'
RETURN m
```

Exercise 8.17: Retrieve the node to confirm that the property has been removed
```
MATCH (p:Person)
WHERE p.name = 'Robin Wright'
REMOVE  p.birthPlace
```

Exercise 8.18: Retrieve the node to confirm that the property has been removed
```
MATCH (p:Person)
WHERE p.name = 'Robin Wright'
RETURN p
```

##Exercício 9

Exercise 9.1: Create ACTED_IN relationships.
```
MATCH (m:Movie)
WHERE m.title = 'Cidade de Deus'
MATCH (p:Person)
WHERE p.name = 'Tom Hanks' OR p.name = 'Robin Wright' OR p.name = 'Gary Sinise'
CREATE (p)-[:ACTED_IN]->(m)
```

Exercise 9.2: Create DIRECTED relationships.
```
MATCH (m:Movie)
WHERE m.title = 'Cidade de Deus'
MATCH (p:Person)
WHERE p.name = 'Robert Zemeckis'
CREATE (p)-[:DIRECTED]->(m)
```

Exercise 9.3: Create a HELPED relationship.
```
MATCH (p1:Person)
WHERE p1.name = 'Tom Hanks'
MATCH (p2:Person)
WHERE p2.name = 'Gary Sinise'
CREATE (p1)-[:HELPED]->(p2)
```

Exercise 9.4: Query nodes and new relationships.
```
MATCH (m:Movie{title:"Cidade de Deus"})-[r]-(p:Person)
return m,r,p
```

Exercise 9.5: Add properties to relationships.
```
MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie)
WHERE m.title = 'Cidade de Deus'
SET rel.roles =
CASE p.name
  WHEN 'Tom Hanks' THEN ['Forrest Gump']
  WHEN 'Robin Wright' THEN ['Jenny Curran']
  WHEN 'Gary Sinise' THEN ['Lieutenant Dan Taylor']
END
```

Exercise 9.6: Add a property to the HELPED relationship.
```
MATCH (p1:Person)-[rel:HELPED]->(p2:Person)
WHERE p1.name = 'Tom Hanks' AND p2.name = 'Gary Sinise'
SET rel.research = 'war history'
```

Exercise 9.7: View the current list of property keys in the graph.
```
call db.propertyKeys
```

Exercise 9.8: View the current schema of the graph.
```
call db.schema
```

Exercise 9.9: Retrieve the names and roles for actors.
```
MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie)
WHERE m.title = 'Cidade de Deus'
return p.name, rel.roles
```

Exercise 9.10: Retrieve information about any specific relationships.
```
MATCH (p1:Person)-[rel:HELPED]-(p2:Person)
RETURN p1.name, rel, p2.name
```

Exercise 9.11: Modify a property of a relationship.
```
MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie)
WHERE m.title = 'Cidade de Deus' AND p.name = 'Gary Sinise'
SET rel.roles =['Lt. Dan Taylor']
```

Exercise 9.12: Remove a property from a relationship.
```
MATCH (p1:Person)-[rel:HELPED]->(p2:Person)
WHERE p1.name = 'Tom Hanks' AND p2.name = 'Gary Sinise'
REMOVE rel.research
```

Exercise 9.13: Confirm that your modifications were made to the graph.
```
MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie)
WHERE m.title = 'Cidade de Deus'
return p, rel, m
```

##Exercício 10

Exercise 10.1: Delete a relationship.
```
MATCH (:Person)-[rel:HELPED]-(:Person)
DELETE rel
```

Exercise 10.2: Confirm that the relationship has been deleted.
```
MATCH (p1:Person)-[rel:HELPED]-(p2:Person)
return p1, rel, p2
```

Exercise 10.3: Retrieve a movie and all of its relationships.
```
MATCH (m:Movie{title:"Cidade de Deus"})-[rel]-(p:Person)
return m, rel, p
```

Exercise 10.4: Try deleting a node without detaching its relationships.
```
MATCH (m:Movie)
WHERE m.title = 'Cidade de Deus'
DELETE m

Cannot delete node<180>, because it still has relationships. To delete this node, you must first delete its relationships.
```

Exercise 10.5: Delete a Movie node, along with its relationships.
```
MATCH (m:Movie)
WHERE m.title = 'Cidade de Deus'
DETACH DELETE m
```

Exercise 10.6: Confirm that the Movie node has been deleted.
```
MATCH (p:Person)-[rel]-(m:Movie)
WHERE m.title = 'Cidade de Deus'
RETURN p, rel, m
```