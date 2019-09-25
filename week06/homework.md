##### Week 06 Contents
- Presentation: [Everyone's Research on Station Accessibility](readme.md)
- Code: [Python Data Plotting with MatPlotLib](python-plotting.md)
- Homework Review: [Transit Availability Visualizer with Add-Ons](homework-answers.md)
- Homework: [Readings, Plotting](homework.md)

-----

### Homework for October 1

##### Readings (1 hour)


- Graham Pullin's [*Design Meets Disabilty*](https://drive.google.com/drive/folders/1lRB-g2c6-mOYRbo-Usb9As9pjDypJPDH), PDF Pages 13-45 (Book Pages 1-65) -- it's not so bad, there are lots of pictures! 

This text was critical in establishing inclusive design as an academic practice, and Pullin located interestingly in *design for provocation* and not *design in service and research* where we might have expected to find it. The text remains novel, legible, and very well-regarded. Pullin's work is located in a very similar POV as *Mismatch* but with a very different focus and example set. We will compare and contrast it with *Mismatch* next week. Try to determine at what and at whom Pullin is aiming this text. 

Relatedly, browse a few readings from my very long bookmark folder on how an emphasis on user-centrism and modernist/minimalist/inevitablist impulses have led designers astray with respect to accessibility.

- Most importantly, browse through the fantastic [Funambulist](https://thefunambulist.net). We will see readings from this website in future weeks.
- [The Bauhaus isn't Our House](https://www.goines.net/Writing/bauhaus_isn%27t_our_house.html). 
- [Modernism is the Worst Thing](https://www.businessinsider.com/why-modernism-is-the-worst-thing-that-ever-happened-to-architecture-2013-7). 
- [UCD Practices Hindered an Ecologically-Sustainable Future](https://medium.com/@eilishmcvey/a-critique-of-user-centered-design-have-ucd-practices-hindered-an-ecologically-sustainable-future-da0c2b1c2ef8)
- [HBR *Design Thinking is Fundamentally Conservative and Preserves the Status Quo*](https://hbr.org/2018/09/design-thinking-is-fundamentally-conservative-and-preserves-the-status-quo) 
- [A Critique of the Modulor](https://failedarchitecture.com/human-all-too-human-a-critique-on-the-modulor/)

Most of these are essays grounded in architectural theory, and may be using familiar language in unfamiliar ways. Record some questions, and as always, bring in your opinion and thoughts to class! I hope we will have a rich discussion on these essays. Also, browse the documentation for a few museum shows of accessible design...

- [Cooper Hewitt's *Beautiful Users Exhibit*](https://collection.cooperhewitt.org/exhibitions/51669015/) exhibitition, which strove to introduce a new definition of user-centrism and followed it up with 
- [Cooper Hewitt's *Access+Accessibility*](https://www.cooperhewitt.org/2017/11/27/cooper-hewitt-presents-accessability-featuring-more-than-70-inclusive-designs/). 

-----

##### Find Some APIs! (1 hour)

Do some research -- are there any APIs out there that we should be implementing? From the city or state? Private organizations? Research bodies? This is an ongoing exercise, with no explicit deliverable associated.

-----

##### Coding (2 hours)

Take a look at this week's code for how to [produce a plot in Python](python-plotting.md). Run the code, go through it line by line, note any questions, and manipulate it to do the following...

	- Start by creating a list of 3-5 bus routes near your home
	- Query the CTA Bus API for all of the stops for those routes
	- Draw a stroked circle for all the bus stops, with a specific color for each route
	- Draw a circle to show the current position of all the buses currently running all of the routes, color-coded by route

