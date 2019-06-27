# ![Open Health Information Exchange Mediator Logo](./startUpImages/openhimLogoGreen.svg)

## **Scaffold OpenHIM Mediator Tutorial**

**TLDR; Watch Tutorial Setup on [YouTube](https://www.youtube.com/watch?v={vid-ID})**

## Useful Links

http://openhim.org/

https://github.com/jembi/openhim-mediator-bootstrap-scaffold

https://www.jembi.org/

## Introduction

> Tutorial Purpose: Create a Scaffold OpenHIM Mediator and Register it with your local OpenHIM instance.

This mediator is purely for demonstration purposes and is in no way production ready. However, the mechanisms used in this mediator can easily be used in your own OpenHIM mediator projects. The repository is also useful to read through as there are detailed comments describing important aspects of an OpenHIM Mediator.

The advantage of using the OpenHIM mediator framework over another stand alone service is that OpenHIM mediators are registered and tracked by your OpenHIM instance. This allows administrators to **view the health status** of the mediator, to easily setup **routing** and **logging** to the registered mediator and to **provide new configuration** settings to the mediator all from the OpenHIM Console.

## Prerequisites

* Node and NPM

This tutorial assumes you have already successfully setup the OpenHIM.

Having a basic understanding of ExpressJS and Javascript ES6 syntax is advised.

---

## OpenHIM Mediator Setup

### Step 1 - Creating project skeleton

In a terminal, create a directory for your project. Move into that directory and and run `npm init` and fill in the details you want:

```sh
mkdir openhim-mediator-bootstrap-scaffold
cd openhim-mediator-bootstrap-scaffold
npm init
```
