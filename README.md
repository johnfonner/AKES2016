# Clouds, Clusters, and Containers: Tools for responsible, collaborative computing

## Overview

Better tools enable new discoveries; it is as true for computation as it is for experiment. Computing has been a pervasive part of scientific research across disciplines for a while, but as computational requirements increasingly exceed the capabilities of single workstations, the pressure is mounting for researchers to develop a new skillset and leverage a new toolset.

This workshop addresses the challenges and requirements for working effectively on cloud computing and high performance computing resources, discusses the key principles that should guide responsible scientific computation and collaboration, and using hands-on sessions presents practical solutions using emergent software tools that are becoming widely adopted in the global scientific community. Specifically, we will look at using “containers” to bundle software applications and their full execution environment in a portable way. We will look at managing and sharing data across distributed resources. And finally, we will tackle how to orchestrate job execution across systems and capture metadata on the results (and the process) so that parameters and methodologies are not lost. And perhaps the most important part, we will not be using the command line to achieve this.

## Pre-workshop Preparation

To participate in the workshop activities, you will need to bring your own computer and have a Cyverse account.  Cyverse accounts may be freely created here: [https://user.cyverse.org/](https://user.cyverse.org/)

## Schedule

|Time           | Topic                                                                                    |
|---------------|------------------------------------------------------------------------------------------|
|  8:30 -  9:00 | [Overview and introductions (Presentation and Discussion)](https://docs.google.com/presentation/d/1RioNnjvL2qyRPQHSQ2-MG04m_LwNzf9gGqHwsSpCguM/edit?usp=sharing)      |
|  9:00 -  9:20 | [Connect laptops to online resources (Hands-on)](connect.md)                             |
|  9:20 - 10:15 | [Containerization tutorial (Hands-on)](docker.md)                                        |
| 10:15 - 10:45 | Coffee break                                                                             |
| 10:45 - 11:05 | [Cyverse Science APIs (Presentation)](Science_APIs.pptx)                                 |
| 11:05 - 12:00 | [Hacking session 1: Systems and Data movement (Hands-on)](systems.md)                    |
| 12:00 - 13:00 | Lunch                                                                                    |
| 13:00 - 13:50 | [Hacking session 2: Scalable Analyses (Hands-on)](apps.md)                               |
| 13:50 - 14:10 | [Metadata and Reproducibility (Presentation and Discussion)](reproducibility.pptx)       |
| 14:10 - 14:50 | [Hacking session 3: Metadata, Reproducibility, and Collaboration (Hands-on)](sharing.md) |
| 14:50 - 15:30 | [Workflow example (Hands-on)](workflow.md)                                               |
| 15:30 - 16:00 | Coffee break                                                                             |
| 16:00 - 16:30 | Web portals (Presentation)                                                               |
| 16:30 - 17:15 | Real world applications (Q&A Session and Discussion)                                     |
| 17:15 - 17:30 | Closing thoughts and participation survey                                                | 

## Learning Objectives

* Build Containers – Like gift wrapping for code, it makes any scrappy workflow more socially acceptable.  Containers let you (and everyone else) run your workflows almost anywhere and get the same answers.
* Use Science APIs – Once your workflows are containerized, Science APIs are the key to bending all computers to your will.  It is perhaps the most powerful way to collaborate, capture metadata, orchestrate workflows, share data, and scale compute power.
* Publish responsibly – Capturing computational workflows only in the methods section of a paper or by posting source code is woefully insufficient now.  Why not let your reviewers repeat your calculations with a few clicks?
