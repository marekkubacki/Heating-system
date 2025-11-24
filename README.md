The application is accessible at:

➡️ https://pi.kubacki.nohost.me
 

This repository contains a complete implementation of a remote heating control system based on a Raspberry Pi 3B.
The application allows the user to remotely enable or disable a central heating pump, monitor usage statistics, and manage automatic shutdown parameters.

The system is designed to be robust, fault-tolerant and safe — even in cases of power outages, network loss, or Raspberry Pi restarts.

📌 Overview

The system consists of two main layers:

1. Web Layer (PHP)
  
  Provides the user interface.
  
  Handles password-based access control.
  
  Displays current heating status, statistics, counters, and calendar.
  
  Writes user commands and configuration changes into JSON files.

2. Control Layer (Python)

  Runs continuously on the Raspberry Pi.
  
  Reads operational data from dane.json.
  
  Directly controls GPIO pins connected to the relay module.
  
  Updates counters and daily usage statistics.
  
  Performs automatic shutdowns based on configured limits.
  
  Restarts automatically with the system.
  
  This separation ensures that even if the website becomes temporarily unavailable, the Python controller continues to maintain correct system behavior at all times.

📁 Repository Structure
  / (root)
  │
  
  ├── index.php              # Main user interface (login, control panel, stats, calendar)
  
  ├── admin.php              # Administrator panel (resetting stats, viewing JSON data)
  │
  ├── sterowanie.py          # Core controller handling GPIO and logic
  
  │
  ├── dane.json              # Main system state (on/off, counters, configuration)
  
  ├── kalendarz.json         # Daily usage statistics
  ├── czysty_kalendarz.json  # Template used for resetting yearly stats
  ├── proby.json             # Stores failed login attempts (security subsystem)
  │
  ├── style.css              # UI styling for the web application
  └── README.md              # This file

⚙️ How It Works
 
 🔌 Physical Layer
  
  Raspberry Pi controls a relay via GPIO.
  
  The relay switches a 230V heating pump on and off.
  
  The Python script sets GPIO pins as output (ON) or input (OFF) to ensure stable behavior.
  
  System restarts automatically after a blackout.


🧠 Logic Layer

  index.php writes commands to JSON, but never touches GPIO directly.
  
  sterowanie.py reads JSON values and controls hardware accordingly.
  
  Every few milliseconds, the Python script:
  
  checks if heating should be on or off,
  
  updates counters,
  
  applies auto-off logic,
  
  saves daily statistics to calendar.


🛡️ Security

  The system uses:
  
  password-based access control,
  
  rate-limited login attempts recorded in proby.json,
  
  Captcha activation after multiple failures,
  
  separate admin-level password for extended options.
