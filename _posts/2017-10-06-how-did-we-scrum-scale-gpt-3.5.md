---
layout: post
date: 2017-06-10
title:  How we scaled Scrum (gpt-3.5 translation)
description: |
    Hello, everyone! My name is Lesha. I work in the Alpha Bank division that focuses on the development of electronic channels. Internet and mobile banking are what we're all about.<br><br>
    
    When we talk about Scrum, we often imply a single team working on a single product with no more than nine people. However, sometimes a product can be so complex that nine people may not be enough to implement it by the assigned deadline. So, what should we do?<br><br>
    
    Today, I want to share our experience of scaling Scrum when multiple teams worked on a single product. How did we reach this point, and what came out of it? I invite all interested parties to continue reading.
author: Alexey Lobzovv
place: Moscow, Russia
source: https://habr.com/ru/companies/alfa/articles/339510/
---

Currently, we are developing a variety of products. Here, you can find both large monolithic legacy systems, inherited from a large bank, and innovative solutions based on modern technologies such as [blockchain](https://habr.com/ru/companies/alfa/articles/323070/) or [contactless payment technology](https://habr.com/ru/companies/alfa/articles/332596/). However, most of our products are web or mobile applications that allow customers to use the bank's services without physically visiting its branches.

The core fighting unit of our division is a team working on the creation and development of a specific product. Most teams are cross-functional and follow Scrum, enabling them to independently organize and carry out their activities. This autonomy was maintained until we realized that our products are interconnected, and we are not just building separate applications but complex solutions that provide customers with a wide range of services.

## Problem Statement

The bank set a deadline to develop a new internet banking platform for customers in one of the business segments. The final product should provide customers with various services such as single payments, group payment operations, payment statement generation, and more. The time constraints necessitated parallel work, so a separate team was formed for each service.

In Scrum, teams are independent and self-organize the sprint length, its goal, and the list of stories to be completed to achieve an increment. However, this independence could pose a number of risks.

### Low-priority functionality was implemented

Each team has a product owner who has a vision for the development of their product, a separate service within the complex product. This vision is often limited to a specific service. Therefore, the functionality implemented by the time of launching the new internet banking platform might be significant within an individual application but insignificant within the context of the complex final product.

### Functionality duplication occurred

This applies not only to the front-end applications but also to the middle services called from the front end. Sending a payment for execution in the bank should follow a unified algorithm, regardless of whether it is done from a specific payment page or from the customer's payment list. Otherwise, the final product would be too complex to maintain and develop.

### Only a part of the complex product was handed over for maintenance

One of the requirements for handing over the product for maintenance is the availability of documentation for the front end and microservices. The front-end description should be provided on the project page in Confluence. Documentation for microservices should be located alongside the code in Git.

Five teams worked on the new internet banking platform. Two teams documented according to the functional maintenance requirements, while the rest decided to document everything in Git, including the front-end documentation. This decision was reasonable, but it could have caused difficulties when transferring parts of the applications for maintenance.

### One product, different designs

Each team has a designer responsible for the visual appearance of the application. Therefore, even with certain agreements between the designers, discrepancies in the components used, fonts, spacing, etc., can be discovered when assembling the final product from individual applications.

The teams have been working in a coordinated manner for several months. However, discrepancies are still identified during the design review process, which are promptly addressed. It is difficult to imagine the burden that would fall on the teams if we discovered all these discrepancies at the very end when assembling the new internet banking platform.

## Our Solution

To mitigate risks, we began exploring approaches to scaling Scrum. That's when we came across LeSS (Large-Scale Scrum). According to LeSS, multiple teams are assigned to a single product owner with a unified backlog. The teams' activities are synchronized through sprints, and regular meetings involving representatives from all teams working on the complex product are held to coordinate their efforts.

Of course, every implementation has its peculiarities, and our version of LeSS differs somewhat from the official framework described on the website. However, we did not aim for perfect alignment. Our primary focus was to establish coordinated collaboration among multiple teams working on a unified product. Here's what we achieved.

### Sprints

The teams aligned themselves with sprints. Now, the sprints start simultaneously and last exactly one week. This allowed us to jointly plan and monitor the work on the complex product.

One of the tasks for all teams was to transition the applications to a new design. Previously, the main menu was located at the top, and the application background was gray. In the target state, the menu is positioned on the left side of the screen, and the background is white.

The teams took on this task in a sprint, and on the penultimate day, the new design was deployed across all applications simultaneously. This way, we managed to avoid a situation where some sections of the internet banking system were styled in the new design while others remained in the old one.

### Sprint Planning

Product owners merged their backlogs and performed cross-prioritization of user stories. The unified backlog is reviewed, and the priorities are updated weekly before sprint planning. This reduces the risk of teams working on low-priority stories within the context of creating a complex product.

After the priorities are updated, the product owners disperse to their respective teams, where they collectively define the goal and plan the upcoming sprint. Each team only takes on user stories for their specific applications, allowing them to maintain and accumulate expertise in their designated service within the final product.

The process concludes with a joint planning meeting, where representatives from all teams identify the set of user stories they plan to tackle in the sprint. The overall sprint goal is formulated, and the teams begin their work.

### Work Execution

Cross-functionality in Scrum means that a team possesses all the necessary competencies for product development. It doesn't necessarily mean that every team member possesses all those competencies. For example, there might be only one Java developer in the team who understands the code of Java-based services. In such a situation, who will conduct code reviews?

Previously, the code was often reviewed by developers not involved in the project. Now, code reviews involve developers from the teams working on the same complex product. They are familiar with its peculiarities and nuances, which leads to improved code quality, early error detection, and prevention of function duplication.

Interaction occurs across all functions. Automation engineers review automated tests. Analysts agree on a unified documentation format for projects. Designers collaborate to align the visual appearance of their applications. All these efforts are aimed at enhancing the quality of the final product.

### Sprint Review

During the sprint review, teams showcase their contributions to the creation of the complex product. Each team demonstrates what they have accomplished during the sprint.

As it is known, comparison helps us gain insights. If a team, working independently, fails to complete some of the user stories committed to in the sprint, it can be attributed to taking on too much. However, if other teams have taken on a similar number of user stories of comparable complexity and closed all of them, it's an opportunity to reflect on their own productivity and dedication to the work.

Every two weeks, real users are invited to the joint sprint review. The purpose of this activity is to refine requirements, showcase prototypes, and gather feedback. All of this is aimed at making the final product more valuable to the Bank's customers.

### Retrospective

Inter-team collaboration matters are discussed during the overall retrospective, which takes place after the individual team retrospectives. Many useful decisions have been made during these sessions. They include setting the sprint planning order, adopting a Definition of Done (DoD) common to all teams, and developing an approach to obtain feedback from end users of the new internet banking system.

## In Summary

Can synergy be achieved through coordinated work of multiple Scrum teams? Practice shows that it is possible. We prevented risks that could have arisen when assembling the final product from multiple web applications. We started sharing knowledge and working on improving not only our own product but also the products of other teams.

Certainly, there are downsides. The teams lost some of their independence, and the number of meetings distracting participants from product development increased. However, overall, the benefits of collaborative work outweighed the drawbacks. Therefore, if you have multiple Scrum teams working on related products, don't hesitate to coordinate their activities. Coordinated work may bring additional benefits to the teams and provide added value to your customers.

---

[Source]({{ page.source }})