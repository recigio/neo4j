# Atividade Prática – Neo4J
Atividades relacionadas Neo4J da pós de DataScience da Furb.


## :play intro-neo4j-exercises 


## Exercício 1

1.1 match(a) return a

1.2 CALL db.schema()

1.3 match(person:Person) return person

1.4 match(movies:Movie) return movies

## Exercício 2

2.1 match(movie:Movie{released:2003}) return movie

2.2 Clicar no icone da tabela do resultado

2.3 CALL db.propertyKeys

2.4 match(movie:Movie{released:2006}) return movie.title, 

match(movie:Movie{released:2006}) return movie.title, movie.released, movie.rating
   
2.5 match(a:Movie) return a.title, a.released, a.tagline

2.6 MATCH (m:Movie) RETURN m.title AS `titulo`, m.released AS `lancamento`, m.tagline AS `slogan`


