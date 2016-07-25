

## The main idea behind the API


   Once, we talked about the importance about the standard of a NoSQL database API, the next step is discuss, in more details, about API. However, to make an easy explanation, first gonna talk about both layer and tier. These structures level make the communication, maintenance, split the responsibility clearer. The new API proposal gonna be responsible to be bridge between the logic tier and data tier, to do this, we need to create two APIs one to communication to a database and another one to be a high abstraction to Java application.




### Tier, a physical structure in software

![Tier, a physical structure in software
](../01.png)



  In a software world is really common that application has structures: tier, a physical structure, and layer, logic one. The multi-tier application has basically three tiers:

* **Presentation** 	tier: That has as main duty translate the result, from below tiers, to user can understand.	
* **Logic tier:** The tier where has all business rules, process, conditions, save 	informations, etc. This tier moves and processes an information between other tiers.
* **Data tier:** Storage and retrieve information either database or a system file.



### Layer, the logic structure in a software


![Layer, the logic structure in a software](../02.png)
 
 
   
   Talking precisely in a logic tier, to split responsibility, there are layers. These layer have an overpass through presentation and data tier besides the business rules. Wherever the architecture structure chosen (MVC, HMVC, PAC, MVA, MVP, MVVM) they have basically four layers:

* **Application layer:** Bridge between presentation tier and logic tier, this layer, for example, transforms an object to JSON.
* **Service layer:** The functional layer that make business layers available. Grouping services into functional layers reduces the impact of change. Most 	changes affect only the layer in which they're made, with few side-effects that impact other layers. 	
* **Business layer:** Where that has business rules and models.
Persistence layer: Layer that access a data, the data tier.


###	Persistence layer in relational database



