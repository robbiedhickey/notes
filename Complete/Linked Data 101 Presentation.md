# Linked Data 101 Presentation

> Tech Know Presentation

Presented by Sam Lawrence, an architect in the core platform technology group. Looking for practical implementation of linked data for large companies

## Introduction

* Project BOLD (Big Open Linked Data)
	* Big Data
	* Open Data
	* Linked Data
* Linked Data
	* A set of standards and principles used to connect data through semantic relationships

## Building Blocks of Linked Data

The Triple!

A triple is an object that is made up of, each represented as a URI (although an Object can be a primitive):

* Subject - the thing 
* Predicate - the relationship
* Object - the fact

Usually a subject will have many predicates and associated objects. 

These representations manifest themselves in file format(s) (RDF xml, JSON) or a graph

```xml
<?xml version="1.0">
<rdf:RDF>
	<rdf:description:rdf:about="http://tr.com/1234">
		<tr:name>sam</tr:name>
	</rdf:description>
</rdf:RDF>
```

```json
{
	"@id":"http://tr.com/1234",
	"@type":"rdf namespace url",
	"name":"sam"
}
```
You look up things by the Subject URI (http://tr.com/1234). 

A great advantage of this approach is how easy it is to incorporate new facts and data. It is not bound by any specific type of schema. You can also use the predicate URI to indicate similar relationships from different entities. 

## Querying Linked Data

Use a language called SPARQL.
```sparql
select ?subject ?predicate ?object
where {
	?subject ?predicate ?object.
	FILTER(?subject = <http://tr.com/1234>)
}
```

It is a very flexible and expressive langauge. 


## Types in Linked Data

You can use a predicate that represents a type (w3.org/...ns#type) and the Object to dictate what class of type (tr.com/ont/Person). 

Ontologies are a way to understand how you can interact with that data. 

Ontologies are also described in the same triples form. You can have multiple types associated to a subject (A Person and a Judge). 

An ontology allows you to have some expectations about the data, and allows you to make inferences. You can also add rules to your Ontologies. 

### Example

If you start to combine data from different sources, other organizations will inevitably describe their data differently. Two orgs may have defined two different predicates that both represent the name. 

You can use the equivalentProperty to define a rule that these ontologies are the same. 

## Q&A

### Pros/Cons
PRO: You do not need to know the schema up front. Very flexible for a changing world where all of the specs are not known. You can focus on what you currently know and just model that, and the triples provide an intuitive way to extend those facts.

PRO: It is a set of standards that aren't proprietary. Data is not silo'd any longer, and can be aggregated among parties. There is not a huge ETL process to translate one set of data to another. Makes seemingly disparate sets of data compatible. 

### Does this support Real Time Data?

Probably referring to a lot of facts that are coming in very quickly. He does not think it would be mature enough to do this quickly enough. 

# Part 2 - Practical guide to Ontologies

Ontologies empower machines to derive meaning from triples. By meaning we mean a network of related concepts. The language we use to write ontologies is OWL. We can use a tool called Protege to run a reasoner and derive inferences from our triples. 

```owl
@prefix: <http://welldodgy/#> .
@prefix owl: <http://222.23.org/2002/07/owl#> .
@prefix rds: <http://www.w3.org/2000/01/rdf-schema#> .

:Person a owl:Class .
:isMarriedTo a owl:ObjectProperty, owl:SymmetricProperty ;
	rdfs:domain :Person ;
	rdfs:range :Person .
```
The properties like URIs to other URIs. SymmetricProperty indicates that the relationship works both ways. 

## Why ontologies?

You don't need ontologies to write RDF, however, they provide:

* Documentation - machine-readable contract of intent, "intellisense"
* The rudiments of data integrity - however, the open world assumption will monkey with your brain if you are used to relational models
* Inferencing - reasoners allow you to calculate implied triples
	* :isMarriedTo a owl:SymmetricProperty .
	* :Wilma :isMarriedTo:Fred
	* Due to SymmetricProperty infers that - :Fred :isMarriedTo :Wilma

## So what?

* The real power is the ability to combine heterogeneous and incomplete data sets
* If an ontology is a language describing meaning, different data sets can be written in different languages

## Enter SWRL

* OWL reasoning has limits, it can only reason about sets - classes; sets of individual swith certain properties, not about individuals themselves
* Semantic web rule language extends OWL to allow you to create rules about individuals using a subset of predicate logic

```swrl
hasInterestIn(?x ?y), isAdversary(?y ?x)
```

## In Summary

Allows new facts to be deduced from data without it being explicitly stated. Creating mappings between ontologies allows two or more data sets to be combined. Once the data sets are mapped, it provides and easy framework for extracting new information from the combined data. 

## Q&A

Q: What books/resources do you recommend?
A: Tutorial written for protege called the "Owl Pizza tutorial". http://130.88.198.11/tutorials/protegeowltutorial/resources/ProtegeOWLTutorialP4_v1_3.pdf 


Q: What tools do we use?
A: Still looking for production ready triplestore. Use protege for data modeling. 