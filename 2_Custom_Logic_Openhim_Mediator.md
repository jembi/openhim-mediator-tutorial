# ![Open Health Information Exchange Mediator Logo](images/openhimLogoGreen.svg)

## **OpenHIM Mediator With Custom Logic Tutorial**

**TLDR; Watch Linux Tutorial Setup on [YouTube](https://www.youtube.com/watch?v=)**

## Useful Links

[OpenHIM Resources](http://openhim.org/)

[OpenHIM Orchestrator Bootstrap Mediator](https://github.com/jembi/openhim-mediator-bootstrap-orchestrator)

[Jembi Health Systems NPC](https://www.jembi.org/)

## Introduction

> Tutorial Purpose: Create an Orchestrator OpenHIM Mediator registered with your local OpenHIM instance that illustrates how to implement your own custom mediator logic.

In this tutorial we will be designing an orchestrator mediator that will accept a request from the client for a list of Health Facilities. In our example, the client wants the facility list in JSON format. [DHIS2](https://docs.dhis2.org/2.28/en/user/html/ch02s02.html) is the source of our facility data and in this example only provides the data in XML. Therefore, our mediator will have to translate the data from XML into JSON before sending the response back to the client.

![Mediator Diagram](images/mediatorDiagram.jpg)

## Prerequisites

This tutorial assumes you've setup the OpenHIM and understand the basics of an openHIM Mediator.
