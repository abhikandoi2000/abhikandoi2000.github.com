---
title:  "Harley - a daily activity center for autistic children"
date:   2015-10-03 10:18:00
description: Building a daily activity center for autistic children
---

Back in February, some of the folks from [SDSLabs](https://sdslabs.co/ "Portfolio of SDSLabs") participated in Microsoft's Code.Fun.Do hackathon organized in IIT Roorkee. Code.Fun.Do is a nation wide hackathon conducted by Microsoft, more on which can be found [here](https://www.acadaccelerator.com/ "Code.Fun.Do is a part of Microsoft's acad accelerator program") (there really is no point repeating the same information here).

I participated along with Dhaval, Sopan and Parag. Together, we worked on an application called **Harley**, a daily activity center for autistic children.

We **ranked 2nd** among a total of 78 teams who made a submission. After this we participated in the Nationalists Forum (India Level), where we competed with teams from other IITs and colleges. We stood **second in the design milestone** and **ninth in the coding milestone**.

Source code of our application is hosted [here](https://github.com/abhikandoi2000/harley "Source code  - Harley").

![Homescreen](http://i.imgur.com/deuLDjg.png "Homescreen")
Homescreen of **harley**


##Brainstorming

When the event kickstarted we were still brain storming on what to make. Our aim was to create something that comes naturally to people (we used Kinect for natural gestures), and after a few hours into the discussion, an idea stuck our mind. **The idea of creating something to help people, and specially children, with autism**.

This led us to explore the different challenges faced by autistic people. And we started reading different articles, blog posts and research papers to identify this core set of challenges.


##Challenges faced by Autistic people

Soon after, we had enough ground covered around the issues faced by autistic people. Our next motive was to think of solutions in order to tackle these problems. Autistic people have a hard time coordinating their motor and neural abilities. And years of research has shown, that constant practice of coordinating their muscles and neurons, helps them improve significatly.

Significant amount of research in the academia has identified these major challenges

* **motor difficulties**: including movements of hand in a specific shape, standing upright
* **cognitive challenges**: including lack of concentration and attention
* **communication difficulties**: they find it hard to start a conversation or repeat a given sentence  
  
  
##Design of our solution

In order to help them tackle these challenges, we started designing the following fun activities in our application

* **Star jump**: helps increase concentration and motor skills
* **Shape drawing**: helps tackle motor and neural difficulties
* **Facial expression**: improves muscle-neuron coordination
* **Hurdle Jump**: gives a boost to concentration and motor skills
* **Karaoke Style Poem Reading**: helps improve speaking abilities

![The Facial expression game](http://i.imgur.com/AZu5xjA.png "The Facial expression game")
The **Facial expression game** in play

Engagement plays a governing role in the results. The activities we designed are interactive, and they guide the child after every action in a playful manner. Computer generated voice, voice input and onscreen instructions are some of the interactions that enable a high level of engagement.

We used the Kinect device to take skeleton level inputs from the user. Dynamic Measurement of angles and distances between the different body parts of the child allows us to determine whether a activity has been performed properly. Speech is prepared accordingly and is delivered using the Microsoft Speech SDK. Harley also leverages our Wrapper written over the Speech Recognition SDK to take voice inputs for changing from one activity to another.

##Winding up

One crucial thing that I realized during the competition is trusting your team members. There were instances when we were debugging a single bug for more than 2 hours and were about to lose hope of ever completing the application. However, it was the collective spirit of the team that enabled us to move forward and complete the application in record time.

##Technical details

* The application is developed using the Kinect SDK v1.8
* Kinect Measurement Toolkit is used for angle, distance and other vector based calculations
* Kinect Speech SDK is used for speech synthesis and speech recognition
* We used Kinect Toolbox for identification of Shape gestures (Circle, Square and Triangle)
* Adobe Illustrator was used to design the interface and the logo

More details in the [README](https://github.com/abhikandoi2000/harley "Source code of Harley")
