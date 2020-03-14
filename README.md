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