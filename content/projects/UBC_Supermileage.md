---
title: "UBC Supermileage Vehicle Simulation"
date: "2025-02-14T16:46:16-08:00"
draft: false
tags: ["MATLAB", "UBC", "Database"]
categories: ["Projects"]
github: "https://github.com/AP010307/UBC-Supermileage-Vehicle-Simulation.git"
weight: 1
---

# Overview
A MATLAB project looking for the best racing strategy for the UBC Supermileage team.

# Introduction
In the motorsport world, the more time a driver spends on the track, the better their throttle control, braking point and racing line become. They also develop their own racing style that not only suits the car but also their racing philosophy. Two great examples are Max Verstappen late braking and preference for oversteering and late apex, and Lewis Hamilton's smooth driving style and preference for early apex. Nevertheless, the driver's racing style is not the only factor that affects the car's performance. The car's setup, the track's condition, and the weather also play a significant role in determining the car's performance and most importantly, the racing line.

# Objective
This project focuses on studying the racing line and the car's performance in the UBC Supermileage team. The goal is to find the best racing strategy for the team to achieve the best fuel efficiency in the Shell Eco Marathonn. There are three cars in the team, the Urban Concept, Gas Prototype and Fuel Cell Prototype. The project will focus on the Urban Concept at the moment, with the Gas Prototype and Fuel Cell Prototype to be added in the future.

# Methodology
The project is inspired by the strategy developed by many F1 teams, including the Oracle Red Bull Racing team. It is also inspired by student racing teams across the world as it used Simulink and Simscape to simulate the car's performance. The project will use the following steps to achieve the goal:

## Track information
Given that Shell Eco Marathon is held in the Indy 500 track, I will use MATLAB to create the track's map and the track's information, such as the track's length, the track's corners, the track's elevation, and the track's surface. Weather conditions is also saved into a database using SQL and I will implement a function to randomly select a weather condition for the simulation. The use of weather database is also used to predict the weather, giving the team an insight on how to prepare for the marathon

## Simulink Model
The project uses exisiting Simulink models, such as glider model combined with powertrain blockset, to simulate the car's performance. With the provided parameters of Urban, the project will record car's performance using Monte Carlo simulation provided from UBC Supermileage's former data. 

## Racing Line



[View the project on GitHub](https://github.com/AP010307/UBC-Supermileage-Vehicle-Simulation.git)