If 222022-09-12

Style: #BookNotes 

Domain: #Development 

Tags:

# Chapter 2 A pragmatic approach

## The essence of good design

### Easier to change (ETC)

> So we beleive in the ETC principale: *Easier to change*

>You may need to spend a week or so deliberately asking yourselfe "did the hting I just did make the overall system easier or ahrder to cahnge?" Do it when you save a file. Do it when you write a test. Do it when you fix a bug.

>Try to make what you write replaceable.

### DRY - The evils of duplication

>Every piece of knowledge must have a single, unambiguous, authoritative representation whitin a system.

>DRY is about the duplication of knowledge, of intent.

> Here's the acid test: when some single facet of the code changes, dou you find yourself making that change in multiple places and in multiple different formats? Do you have to cahnge code and documentation or a database schema and a structure taht holds it, or..? If so, your code isn't DRY.

### Orthogonality

>In a well designed system, the database code will be orthogonal to the user interface: you can change the interface without affecting the database, and swap databases without cahnging the interface.

>When components are isolated from one another, you know that you can change one without having to worry about the rest. As long as you don't change that component's external interfaces, you can be confident that you won't cause problems that ripple through the entire system.

>An orthogonal approach reduces risks inherent in any development.
> - Diseased sections of code are isolated
> - The resulting system is less fragile
> - An orthogonal system will propably be better tested, because it will be easier to design and run tests for its components
> - You will not be tightly tied to a particular vendor, product or platform

>If I dramatically change the requirements behind a particular function, how many modules are affected?

> It may be a clumsy word, but if you use the priciple of orthogonality, combined closely with the DRY priciple, you'll find that the systems you develop are more flexible, more understandable and easier to debug, test, and maintain.


___
# References
