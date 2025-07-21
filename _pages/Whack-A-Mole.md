---
layout: page
title: Whack-A-Mole
description: A Digitial Whack-A-Mole Game
img: assets/img/4_gameplay_0.jpg
importance: 5
category: school
toc:
    sidebar: left
---

## Whack-A-Mole Embedded Game (STM32)

A complete mini-game system was implemented on an STM32F4 microcontroller platform: an interactive LCD UI, keypad input for “whacks,” hardware push-buttons for mode control, servo-based visual game timer, high-score feedback LED, pseudo-random mole placement, and a compact 3D-printed enclosure. What began as a lab exercise grew into a self-contained demo unit suitable for lab bench demos, recruiting events, or an interactive résumé artifact.

> Tip: Insert a short looping gameplay GIF near the top to engage visitors. Capture LCD, keypad hits, LED flash, and servo sweep in the same frame.

## Quick Feature Snapshot

* 15-s timed rounds with live score update
* Pseudo-random mole positions on a 2-D LCD grid
* Instant high-score indication (red LED)
* Rolling recent-scores list
* Startup, countdown, gameplay, and results screens
* Servo sweep providing an analog “time left” cue
* Snap-fit 3D-printed enclosure

> Media suggestion: A still collage or short slider showing the four LCD screens (startup, countdown, gameplay, score sheet).

## Hardware Platform

STM32F446RETx Nucleo board was used as the controller. Peripheral connections were mapped as follows:


> Media suggestion: Add annotated wiring schematic here. A clean KiCad export or Fritzing-style diagram works well for portfolio readers who are less hardware-oriented.

## Firmware Architecture Overview

Firmware was organized as a simple event-driven state machine: Startup → Countdown → Gameplay → Score Sheet → Idle/Restart. Timing events were driven off a 1-s interrupt counter.

> Media suggestion: Add a state-machine diagram (PNG/SVG). Color the timed states differently from event-triggered states.

## LCD UI Subsystem

A reusable LCD driver library from a prior lab (Lab 7) was extended. The core driver handled register writes (command, data) and positioned strings arbitrarily on the display. For the game, wrapper functions were added for:

* Startup screen (static splash; timed auto-advance).
* Countdown screen (hardcoded text that stepped 3-2-1).
* Gameplay screen (grid region reserved for moles + score field).
* Score-sheet screen (rolling list of most recent three scores).
* Score update routine (integer-to-string convert and write at fixed cursor).

Mole animation used a 2-D reference array of LCD character cells. A mole-show function accepted pseudo-random row/column indices; a mole-clear function over-wrote underscores across that array region to “erase” the mole.

> Media suggestion: Include side-by-side LCD captures: empty grid vs mole popped vs score update. A short sprite sheet image showing the custom mole character byte pattern (CGRAM map) is a nice technical touch.

## Pseudo-Random Mole Placement

A lightweight PRNG was built by seeding `srand()` with the captured seconds count at the moment the Start button interrupt fired. Each mole event pulled a row and column via `rand() % n_rows` and `rand() % n_cols`, indexing into the LCD grid map. While not cryptographically strong, the approach ensured round-to-round variation that was user-perceptible and reproducible when a constant seed was forced in debug builds.

> Media suggestion: Add a code snippet block (5–10 lines) showing seeding and selection; readers appreciate seeing actual C call flow.

## Scoring & High-Score Indication

Hits registered through keypad events were tallied in a round score variable. After each hit, the LCD score field was refreshed. At round start, the current high score was compared to the stored maximum; on surpassing that maximum, the red LED on PA9 was asserted. An in-RAM ring buffer preserved the three most recent round scores for display on the score-sheet screen.

> Optional enhancement to mention if accurate: Persist high score in non-volatile flash so it survived resets. If that was not implemented, note it as a future upgrade.

## Timing & Servo Sweep

Round timing used a timer to generate a 1-Hz interrupt. A global counter was incremented in the interrupt service routine and polled by the state machine. At defined thresholds (3-s countdown, 15-s gameplay, 3-s score sheet), state transitions were triggered and the counter was reset.

A continuous visual countdown was provided by a hobby servo driven from TIM2 CH1 (PA5) configured for 50 Hz PWM. Duty cycle was stepped once per second across a calibrated range (\~1–2 ms pulse width), corresponding to roughly 12 degrees per step for the selected servo, yielding a \~180-degree sweep over a 15-s round.

> Media suggestion: Add a short 5-s video clip: servo arm sweeping as LCD countdown runs. Overlay captions.

## Input Hardware

Two panel push-buttons (PC10 Start, PC11 Score Sheet) were configured as external interrupts on falling edges. Simple software debouncing (time gate or state qualifier) prevented multiple triggers.

Mole “whacks” came from a small matrix keypad; rows were driven and columns read (or vice versa) and mapped to mole indices. Any keypress corresponding to the active mole position counted as a hit.

> Media suggestion: Photo of the control panel showing buttons + keypad labeled. Annotate pin mapping in caption.

## Enclosure & Mechanical Integration

A snap-fit 3D-printed enclosure (Figure placeholder) was modeled to seat the LCD flush to a front window, align push-buttons and keypad openings, capture the servo mounting boss, and route wiring toward the MCU board. Tolerance stack-ups made the fit tighter than expected; additional clearance would have eased assembly and rework. Even so, the enclosure transformed a breadboarded lab into a portable demo with improved usability and visual appeal.

> Media suggestion: Gallery (3–4 images): CAD render, print in progress, internal wiring before close, final assembled unit.

## Development Process & Integration Notes

Subsystem code (LCD, servo timer, input interrupts, scoring logic) was initially developed in parallel. Integration exposed interface mismatches: inconsistent naming, blocking delays that clashed with interrupt timing, and duplicated global variables. Consolidation into a shared header, adoption of a state machine, and a short in-person code review session resolved most issues.

Pre-existing peripheral libraries saved considerable effort; without them, register-level bring-up would have extended the schedule significantly. Mechanical packaging consumed more time than expected; routing ribbon cables and ensuring connector access in a thin enclosure proved non-trivial.

> Media suggestion: Include a screenshot of a whiteboard or digital flowchart used during integration—this humanizes the development story.

## Lessons Learned

Clear module boundaries and shared conventions reduced merge pain.
Reusable driver libraries dramatically shortened development.
Mechanical design needed to account for wiring bend radii and connector service loops.
Timing-critical code benefited from interrupt-driven structure rather than blocking delays.
Quick physical prototypes (cardboard mockups) before 3D printing would have flagged clearance issues earlier.

> Callout idea: Use an Obsidian admonition block (e.g., > \[!summary]) listing “What would be done differently next time.”

## Conclusion

An STM32-based Whack-A-Mole game was successfully implemented and packaged as a self-contained demo unit. Core embedded concepts—GPIO interrupts, timer-driven PWM, periodic task scheduling, character LCD control, simple PRNG use, and multi-state UI flow—were integrated into a playful user experience. The project highlighted both the leverage gained from prior peripheral libraries and the integration friction that arose when code styles diverged. Mechanical packaging, often

