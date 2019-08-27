##### Week 02 Contents
- Presentation: [Setting Up Raspberry Pi Wifi, SSH, and VLC Access, Inclusive Design Conversation](readme.md)
- New Components: [RGB LED](circuits.md)
- Code: [Moving around with Terminal](terminal.md)
- Code: [Python Basics](python-basics.md)
- Code: [Python Lists and Dicts](python-lists.md)
- Code: [Python GPIO Control](python-gpio.md)
- Homework: [Research, Readings, API Access, Programming Practice](homework.md)

-----

### Come Back Soon for Finalized Homework!

To be settled and amended after class (depending on progress). But, it will likely be something like this:

- [Python Practice](https://www.practicepython.org)
	Complete exercises 1, 2, 3, 5, 6, and 8 for practice, and come with questions!

- Read [Kat Holmes' *Mismatch*](https://drive.google.com/drive/folders/1lRB-g2c6-mOYRbo-Usb9As9pjDypJPDH?usp=sharing), pages 84 - 123 and prepare for discussion. Please write down *on paper* three questions that this reading provoked in your mind to discuss with your peers.

- Read this short Guardian article [*What would a truly disabled-accessible city look like?*](https://www.theguardian.com/cities/2018/feb/14/what-disability-accessible-city-look-like). 

- Skim this short excerpt *Transportation Patterns and Problems of People with Disabilities* from [*The Future of Disability in America*](https://www.ncbi.nlm.nih.gov/books/NBK11420/). The whole book is available in the shared readings folder. Again, three questions on paper please!

- Request a key, and read the documentation for the [Chicago Train Tracker API](https://www.transitchicago.com/developers/traintracker/). In particular, study the *Arrivals API* as described in the [documentation](https://www.transitchicago.com/developers/ttdocs/). By next class, you should be able to ask the internet with your web browser for the next 5 predicted trains that will arrive at the 35th-Bronzeville-IIT stop (as well as your nearest home train stop?), when the trains are predicted to arrive, and which direction they are traveling. Your query will look something like this...

```
http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=#########&max=#&outputType=JSON&mapid=#####
```

We will work with this data next week on our Raspberry Pis, to create a train arrival warning system for the studio!