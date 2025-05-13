## Ontology Design

Our ontology is designed based on standard upper-level ontologies such as the Semanticscience Integrated Ontology \cite{dumontier2014semanticscience}, to ensure compatability. 
From the upper level ontology, the classes defined in COCO are based on the objects, roles and processes identified in the knowledge acquisition from the domain. 
As a result, COCO has three main classes:
1. **Object**: These are entities that do not change over time, and can be identified by a set characteristics that remain fixed. Subclasses included, Person, Organization, etc.  
   Objects are dichotomized into **SpatialObject** (e.g. a person, a particle or hospital) and **InformationObject** (e.g. bank account, evidence).
2. **Role**: This describes the role that an entity assumes to take part in a process. For example, in the act of being scammed, an entity that is impacted takes the role of a victim and the entity that commits the fraud is the offender. 
3. **Process**: This captures a dynamic entity, such as an action or an activity, in which objects can participate. For example, calling or messaging.  

![coco_main drawio](https://github.com/user-attachments/assets/6ae8ff4e-01df-405d-8cad-465c0147527e)

To increase the applicability of our ontology across various scenarios, we have adopted two well-known Ontology Design Patterns (ODPs). 
1. The first pattern we employed is the **Process Role Object** Ontology Design Pattern. This pattern offers a structured approach to representing how entities participate in processes through specific roles.
   **Process - hasParticipant -> Role <-  hasRole - Objects** can represent scenarios such as a person participating in a criminal activity in the role of a criminal.
   **Process Role Object** defines the roles that are both necessary and specific to each entity and identifies them as direct participants in a given process. 
2. The other pattern used in the ontology is  **PartOf ODP**, which models part-whole (meronymic) relationships (e.g., a wheel is part of a car). Since the **partOf** relation is transitive, we have defined three sub-properties tailored to specific class types: **PPIsPartOf** for processes, **OOIsPartOf** for objects, and **RRIsPartOf** for roles.
