---
layout: page
title: Skeeball Trivia
description: Engaging take on the game of skeeball that includes trivia questions.
img: assets/img/4_gameplay_0.jpg
importance: 3
category: school
toc:
    sidebar: left
---



## Project Overview
This project was a four-player skee-ball game that incorporated a trivia system, built as part of a team project. At the start of each game, the screen prompts the user to select the number of players (1 to 4). Each player then chooses a trivia category from a set that includes Social Studies, Language Arts, Math, Computer Science, Nature & Wildlife, and Science.

The trivia questions were based on the Michigan K–12 educational standards for grades 6 through 8.

## Role & Contributions
Although this was a collaborative effort, I was primarily responsible for the electrical and software systems, which included:

- Circuit design and prototyping
- Component selection and hardware integration
- Graphical user interface (GUI) design
- Programming the game logic, trivia system, and I/O handling

Suggested Media:

 Block diagram of system architecture

 Circuit schematic or breadboard layout

 GUI screenshot

 <div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/4_people_playing.jpg" title="Gameplay" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/4_gameplay_1.jpg" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

## Software & Programming

This was my first major project using Python and my introduction to object-oriented programming (OOP). While my early code style had room for improvement, I'm proud of how the program came together and functioned reliably in real-time.

The game was built around a Raspberry Pi 4B, which served as the central controller. It managed:
- GPIO (general-purpose input/output) for hardware like sensors, buttons, and motors.
  - The [gpiozero library](https://gpiozero.readthedocs.io/en/latest/) was used to simplify interaction with physical components such as buttons, limit switches, and LED indicators.
- GUI rendering using [Pygame](https://www.pygame.org/docs/) and [Pygame-Menu](https://pygame-menu.readthedocs.io/en/latest/)
- Trivia logic and player state tracking

Some notable code snippets:
- [Gameplay Logic](https://gist.github.com/N0RT0/cc54c6ff8e9c1963a5c0a29fb8c59443) (snippet from main.py)
- [Trivia Player Class](https://gist.github.com/N0RT0/3510a3cf266fbb23167ad176025d8ba7)
- [Category Database](https://gist.github.com/N0RT0/e67512a727a45455f855948047ef7054)

## Hardware & Electronics

Main components included:
- Raspberry Pi 4B
- IR Breakbeam Sensors (for scoring)
- Servo Motor (for ball return)
- Arcade Style Light Up Buttons
- 5V Power Supply (for powering the servo)
- Sound Bar
- Flat Screen TV

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/4_electronics.jpg" title="Electronics" class="img-fluid rounded z-depth-1" caption="Raspberry Pi, ADC, and Button Circuit" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/4_Pinout.png" title="Raspberry Pi Pinout" class="img-fluid rounded z-depth-1" caption="Raspberry Pi Pinout" zoomable=true %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/4_button_circuits.png" title="Button Circuits" class="img-fluid rounded z-depth-1" caption="Raspberry Pi Pinout" zoomable=true %}
    </div>
</div>

Suggested Diagram:

 Wiring layout with labeled GPIO pins

 Photo of the assembled hardware and game setup

## Challenges & Lessons Learned

Coordinating timing between GPIO input and GUI updates.
This project started with a Raspberry Pi 3B as the brain, instead of the Raspberry Pi 4B. With the GUI running, the sensors were much too slow to pick up the balls when the player scored.

Organizing code with classes and methods that made sense as the project grew

Translating user interactions into clean state changes

This project helped me gain real experience with hardware/software integration, event-driven programming, and user-centered design.

## Remarks
This skee-ball trivia project was a fun and challenging introduction to Python programming and system design. It gave me hands-on experience in not only writing Python code and building circuits but also in working within a team to deliver a complete, interactive experience.