# Atividade Prática – Neo4J
Atividades relacionadas Neo4J da pós de DataScience da Furb.


## :play intro-neo4j-exercises 


## Exercício 1

```
1.1 match(a) return a
```
```
1.2 CALL db.schema()
```
```
1.3 match(person:Person) return person
```
```
1.4 match(movies:Movie) return movies
```
## Exercício 2

```
2.1 match(movie:Movie{released:2003}) return movie
```
```
2.2 Clicar no icone da tabela do resultado
```
```
2.3 CALL db.propertyKeys
```
```
2.4 match(movie:Movie{released:2006}) return movie.title, 
```
```
match(movie:Movie{released:2006}) return movie.title, movie.released, movie.rating
```
```
2.5 match(a:Movie) return a.title, a.released, a.tagline
```
```
2.6 MATCH (m:Movie) RETURN m.title AS `titulo`, m.released AS `lancamento`, m.tagline AS `slogan`
```
## Exercício 3

```
3.1 CALL db.schema()
```
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
3.3 
```
match(movie:Movie)<--(p:Person{name:'Tom Hanks'}) return movie

match(movie:Movie)<--(p:Person{name:'Kevin Pollak'}) return movie

match(movie:Movie{title:'Cloud Atlas'})<--(p:Person) return p
```

3.4
```
match(movie:Movie)<-[rel]-(p:Person{name:'Tom Hanks'}) return movie.title,type(rel)

match(movie:Movie)<-[rel]-(p:Person{name:'Keanu Reeves'}) return movie.title,type(rel)

```

3.5
```
match(movie:Movie)<-[rel]-(p:Person{name:'Tom Hanks'}) return movie.title,rel.roles

match(movie:Movie)<-[rel]-(p:Person{name:'Keanu Reeves'}) return movie.title,rel.roles

match(movie:Movie{title:'Top Gun'})<-[rel:ACTED_IN]-(p:Person) return movie.title,rel.roles
```

## Exercício 4

4.1
```
match(p:Person)-[:ACTED_IN]->(m:Movie)
where p.name = 'Tom Cruise'
return m.title as Filme
```

4.2
```
match(p:Person)
where p.born > 1969 and p.born < 1980
return p.name, p.born
```

4.3
```
match(p:Person)-[:ACTED_IN]->(m:Movie)
where p.born > 1960 and m.title='The Matrix'
return p.name, p.born
```

4.4
```
match(m)
where m:Movie and m.released=2000
return m.title
```

4.5
```
match(p)-[rel]->(m)
where m:Movie and p:Person and type(rel)='ACTED_IN'
return p.name,m.title
```

4.6
```
match(p)
where p:Person and not(exists(p.born))
return p.name
```

4.7
```
match(p:Person)-[rel]->(m:Movie)
where exists(rel.rating)
return p.name,m.title,rel.rating
```
4.8
```
match(p:Person)-[:ACTED_IN]->(m:Movie)
where p.name STARTS WITH 'James'
return p.name
```

4.9
```
MATCH (:Person)-[r:REVIEWED]->(m:Movie)
WHERE toLower(r.summary) CONTAINS 'fun'
RETURN  m.title as Filme, r.summary as Opiniao, r.rating as Nota

MATCH (m:Movie)
WHERE m.tagline CONTAINS 'love'
RETURN  m.title
```

4.10
```
MATCH (a:Person)-[:PRODUCED]->(m:Movie)
WHERE NOT ((a)-[:DIRECTED]->(:Movie))
RETURN a.name, m.title
```

4.11
```
MATCH (a1:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(a2:Person)
WHERE exists( (a2)-[:DIRECTED]->(m) )
RETURN  a1.name as Ator, a2.name as `Ator e Diretor`, m.title as Filme
```

4.12
```
MATCH (m:Movie)
WHERE m.released in [2000, 2004, 2008]
RETURN m.title, m.released
```

4.13
```
MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE m.title in r.roles
RETURN m.title, p.name, r.roles
```

## Exercicio 5

5.1
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person),
      (a2:Person)-[:ACTED_IN]->(m)
WHERE a.name = 'Gene Hackman'
RETURN m.title as filme, d.name AS diretor , a2.name AS `co autores`
```

5.2
```
MATCH (p1:Person)-[:FOLLOWS]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2
```

5.3
```
MATCH (p1:Person)-[:FOLLOWS*3]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2
```

5.4
```
MATCH (p1:Person)-[:FOLLOWS*1..2]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2
```

5.5
```
MATCH (p1:Person)-[:FOLLOWS*]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2
```

5.6
```

``` 