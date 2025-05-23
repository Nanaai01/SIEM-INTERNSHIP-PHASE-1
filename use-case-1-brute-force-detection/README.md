# Brute Force Detection Use Case

## Overview

This use case demonstrates how to detect brute force login attempts using a SIEM platform.

## Attack Simulation

- Simulated repeated login failures from a single IP address.
- The attack script is located in the `detection-logic/` folder.

## Detection Logic

- Monitor failed login events.
- Trigger alerts if multiple failed attempts occur within a short time window from the same source.

## How to Test

1. Run the attack simulation script in `detection-logic/`.
2. Watch my SIEM dashboard for alerts.
3. Review example alerts in `screenshots/`.
4. Read detailed reports in `writeups/`.

