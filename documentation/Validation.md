## Competency Questions and SPARQL

<details>

<summary> Which technique does a given criminal group use for establishing
contact with a victim? </summary>

 ```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX coco: <http://www.semanticweb.org/coco#>

SELECT   ?contact  ?contacter ?group
WHERE {

?contact rdf:type ?temp .
?temp rdfs:subClassOf coco:InitialContact .

?contact coco:hasAgent ?contacter .
?contacter a coco:Contacter .

?contacter coco:RRisPartOf ?group .
?group a coco:CriminalGroup .

}
```
</details>

<details>

<summary> What kind of attack types is a given criminal group using? </summary>

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX coco: <http://www.semanticweb.org/coco#>


SELECT   ?attacktype ?contacter ?group
WHERE {

?contact rdf:type ?temp .
?temp rdfs:subClassOf coco:InitialContact .

?contact coco:hasParticipant ?story .
?story coco:OOisPartOf  ?attacktype.
?attacktype rdf:type coco:StoryType.

?contact coco:hasAgent ?contacter .
?contacter a coco:Contacter .

?contacter coco:RRisPartOf ?group .
?group a coco:CriminalGroup .


}

```
</details>

<details>
  
<summary> What are the characteristics of the victims targeted by a given criminal group? </summary>
  
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX coco: <http://www.semanticweb.org/coco#>

SELECT   ?person (STR(?agenum) AS ?age)  (STR(?city) AS ?cityname)  ?group 
WHERE {

?contact rdf:type ?temp .
?temp rdfs:subClassOf coco:InitialContact .

?contact coco:hasAgent ?contacter .
?contacter a coco:Contacter .
?contact coco:hasAgent ?victim.
?victim a coco:Victim .
?person coco:hasRole ?victim.
?person rdf:type coco:Person.

OPTIONAL { ?person coco:age ?agenum. }
OPTIONAL { ?person coco:hasFeature ?home. }
?home rdf:type coco:ResidentialInformation. 
OPTIONAL { ?home coco:city_name ?city. }

?contacter coco:RRisPartOf ?group .
?group a coco:CriminalGroup .


}
```

</details>


<details>

<summary> List the offenders with the same bank account used. </summary>

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX coco: <http://www.semanticweb.org/coco#>

SELECT  ?person1 ?role1 ?nick_name1 (STR(?bank_account1) AS ?bank1) ?person2 ?role2 ?nick_name2 (STR(?bank_account2) AS ?bank2)
 WHERE {
  
# Find persons who have a role with bank accounts
?person1 rdf:type coco:Person .
?person1 coco:hasRole ?role1 .

?person2 rdf:type coco:Person .
?person2 coco:hasRole ?role2 .
# ?role rdf:type ?type .
# ?type rdfs:subClassOf* coco:CriminalGroup.

#Retrieve the bank account associated with the person
?person1 coco:hasFeature ?type1 .
?type1 rdf:type coco:BankAccountInformation .
OPTIONAL{?type1 coco:IBAN ?bank_account1 .}

?person2 coco:hasFeature ?type2 .
?type2 rdf:type coco:BankAccountInformation .
OPTIONAL{?type2 coco:IBAN ?bank_account2.}

# Return the necessary information
OPTIONAL {?person1 coco:nick_name ?nick_name1 .}
OPTIONAL {?person2 coco:nick_name ?nick_name2 .}

#Filter
FILTER (STR(?bank_account1) = STR(?bank_account2) && ?person1 != ?person2)
}


```

</details>

<details>

<summary> List the offenders with the same last names. </summary>

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX coco: <http://www.semanticweb.org/coco#>

SELECT  ?person1 ?last_name1 ?person2 ?last_name2 
 WHERE {
  
# Find persons who have a role with bank accounts
?person1 rdf:type coco:Person .
?person2 rdf:type coco:Person .

# Return the necessary information
OPTIONAL {?person1 coco:last_name ?last_name1 .}
OPTIONAL {?person2 coco:last_name ?last_name2 .}

#Filter
FILTER (STR(?last_name1) = STR(?last_name2) && ?person1 != ?person2)
}

```

</details>

