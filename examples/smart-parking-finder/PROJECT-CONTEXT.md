<!--
A filled-in EXAMPLE for a fictional project. It shows what a finished
PROJECT-CONTEXT.md looks like: specific, honest about open problems, and
clear about what's deliberately out of scope.
-->

# Project context

**Project name:** ParkFinder
**One-line description:** Per-spot sensors in a multi-level parking garage
report which spaces are free to a live web map.
**Type of project:** Capstone project
**Institution / program:** *(left out — the template allows this)*
**Team:** 2 software (web app; backend + database), 2 hardware (sensor
nodes; gateway firmware)
**Key deadline(s):** Prototype demo in 10 weeks; final report 2 weeks
after that

## Outside the project

### Problem and who it's for

Drivers in a busy downtown garage spend minutes circling level after level
looking for a space. It wastes their time and adds congestion on the ramps.

### What already exists

Commercial garages use per-spot sensors with red/green lights above each
space, but these are expensive to install and closed to integration. Some
garages show only a "spaces free on this level" count at each ramp. That
tells you a space exists, not where it is.

### How it's evaluated

A capstone advisor reviews progress weekly. The final grade comes from a
live demo and a written report. There's a required report template and a
rubric that weights design justification and testing evidence more than
presentation. Both files are kept locally in `reference/`.

## Inside the project

### Core idea

Camera-based detection was the obvious first idea and was rejected on
purpose. It's expensive to run inference on, and cameras that can read
license plates raise privacy questions the team didn't want to take on in
one semester. Instead, each spot gets one cheap ultrasonic distance sensor
pointing down. The real differentiator is the debounce logic: a spot only
flips to "occupied" after 5 continuous seconds of detection, and only back
to "free" after 5 continuous seconds of nothing. That filters out a person
walking through a spot, or a car slowing down to look, from a car that has
actually parked.

### Tech stack

SvelteKit frontend. Node.js + Express backend. PostgreSQL. Each spot node
is an RP2040 microcontroller with an HC-SR04 ultrasonic sensor and a LoRa
radio module. One gateway (a Raspberry Pi) per garage level receives the
LoRa messages and publishes them to the backend over MQTT.

### Architecture / data model

`levels` (one row per garage level) → `spots` (belongs to a level, has the
sensor node's id) → `occupancy_events` (append-only log: spot, new state,
timestamp). A spot's current state is taken from its latest event, not
stored separately. `users` has 2 roles: operator (full access) and viewer
(read-only map; viewing needs no login).

### Decisions already made

- **5-second debounce, not instant state changes.** Chosen so a pedestrian
  or a slow-rolling car doesn't make the map flicker. "Validated" by timing
  one team member walking across a spot (~1.5s), so 5s has margin.
- **LoRa, not Wi-Fi per node.** Concrete floors block Wi-Fi too badly to
  reach every spot. LoRa trades bandwidth for range and penetration, which
  is fine: each node sends only a tiny state-change message.
- **A sensor per spot, not a count at each ramp.** A ramp count can't say
  *which* space is free, and that's the whole point of the project.

### Open problems

- Readings are unreliable next to concrete pillars: the echo off a pillar
  can look like a parked car. The current plan is to angle the sensor away
  from the pillar. Untested.
- Each level's gateway is a single point of failure. If it goes down, that
  whole level stops reporting. No redundancy is planned this semester.
- The map polls the backend every 10 seconds. Real-time push was
  considered and cut for time.

### Deliberately out of scope

- A mobile app (the web app is responsive; no native app).
- Reservations or payments (display only, no booking).
- A historical analytics dashboard. The `occupancy_events` log would
  support one later without a schema change.

## Sensitive items — never restate in output

- The test MQTT broker's credentials live only in the team's local `.env`
  files. `.env.example` holds placeholders.
