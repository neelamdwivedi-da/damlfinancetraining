# Daml Design Patterns

A **design pattern** is a repeatable solution to a commonly occurring problem. It is derived from designs that have been tested successfully in many different applications and contexts. Knowing these patterns inspires good design and provides a useful vocabulary to communicate design ideas.  

Daml uses several design patterns, the most common of which are listed below:

- Propose-Accept (aka Initiate-Accept) pattern
- Delegation pattern
- Multiparty agreement pattern
- Authorization pattern
- Locking pattern

The design patterns listed above are specific to Daml language, out of which Daml Finance uses **Propose-Accept** pattern extensively. 

There are other more generic design patterns that are widely used in other programming languages, one of which is **Factory pattern**. It is a creational pattern that Daml uses in creating contracts in Daml Finance library. 

Let us take a look at what these patterns are starting with [Propose-Accept](ProposeAccept.md). 
