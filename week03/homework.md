##### Week 03 Contents
- Presentation: [Inclusive Design Small Group Chats, Electrical Signalling](readme.md)
- Components: [RGB LED Color Controller](circuits.md)
- Code: [Python Lists and Dictionaries](python-lists.md)
- Code: [Python GPIO Control](python-gpio.md)
- Application: [Train Arrival Warning Alarm](application.md)
- Homework: [Readings, Other APIs, Programming Practice](homework.md)
	
-----

### Homework for September 10

##### Programming Practice (2 hours)

Complete exercises 7, 10, 13, and 14 at [Practice Python](https://www.practicepython.org), and come with questions! Answers are on the same page. You will need to do some things we did not cover in class, so give these exercises a try, and we will talk about them next week.

-----

##### Readings (2 hours)

- Read this short CityLab article first [*Why Can't Uber and Lyft Be More Wheelchair-Friendly?*](https://www.citylab.com/transportation/2018/12/ride-hailing-users-disabilitiies-wheelchair-access-uber/577855/). Additionally scan [Uber](https://accessibility.uber.com) and [Lyft's](https://blog.lyft.com/posts/lyfts-commitment-to-accessibility) accessiblity claims. Before continuing to the other readings this week, try to identify exactly where Lyft and Uber identify the limits of their responsibilities with respect to impaired customers and employees. 

- Read [Kat Holmes' *Mismatch*](https://drive.google.com/drive/folders/1lRB-g2c6-mOYRbo-Usb9As9pjDypJPDH?usp=sharing), pages 138 - 151 and prepare for discussion (we're skipping some chapters full of examples — worth reading if you have time!). Notably, the final paragraph on page 142 offers a lovely summation of why inclusive design is natively so hard to learn, teach, and implement. As designers, we often pride ourselves on our ability to engage *uncertainty*, and yet Holmes blames an over-dependency on certainty as a prime motivator for exclusionary thinking and doing. How does this feel? How might our design process adapt to accomodate less certainty?

- We will start class with small group discussion on *teaching inclusive design praxis*, so please come prepared to prompt one another.

-----

##### Data Exploration (1.5 hours)

Take a peak at the available [Divvy bikeshare APIs](https://gbfs.divvybikes.com/gbfs/gbfs.json). Of particular note is the `station_status` api available at...

```
https://gbfs.divvybikes.com/gbfs/es/station_status.json
```

No key is needed to access this API, as it is an example of an open API standard called the [Global Bikeshare Feed System (GBFS)](https://github.com/NABSA/gbfs) designed for ingestion by other public system computational tools.

Using Python knowledge built up over the last two weeks, try to write individual Python programs that print answers to the following questions.

1. How many bikes are available in the entire Divvy system?
2. How many broken ("disabled") bikes there are in the entire Divvy system?
3. What station has the most available docks total?
4. Which stations have 0 bikes available?
5. Which stations are full of bikes?

-----

##### Train Arrival Warning Alarm (1.5 hours)

For homework, you will improve your Raspberry Pi alarm, but that will depend on how far we get in class! Come back soon for more info! 

