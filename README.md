## Fail-Safe Motion Interlock System (RSLogix 500)

This project simulates a PLC-based motion safety interlock system designed to prevent an actuator from running unless all safety conditions are met. This includes checks for pressure levels, part presence, E-stop states, and a manual reset. The system will latch a fault indicator if any condition fails and will require operator acknowledgement to resume.

### 🧠 Logic Behavior Summary
✅ Motion only activates when:

System is triggered (Start button pressed)

Part is detected

Pressure is in safe range

No fault or active E-stop

Reset has been acknowledged

🚨 Faults are latched until manually reset

🔒 E-stop logic is latching and must be cleared to proceed

## 🧾 Ladder Logic Breakdown

### 🔁 Start & Reset

B3:0/0 (Start Button)     --ONS--> B3:0/2 (System Trigger)
B3:0/5 (Reset Button)     --ONS--> B3:0/6 (Reset Trigger)

### 🔁 Part Detection

B3:0/10 (Part Sensor)     --> B3:0/9 (Part Detected)

### 🔁 E-Stop Latching Logic

[ XIC B3:0/7  (E-Stop) ]
[ XIC B3:0/12 (E-Stop Active) ]
[ XIO B3:0/14 (E-Stop Deactivate) ]
--> OTE B3:0/12 (Latch E-Stop)

### 🔁 Motion Interlock Logic

[ XIC B3:0/2  (System Trigger) ]
[ XIC B3:0/3  (Actuator) ]
[ XIO B3:0/12 (E-Stop Active) ]
[ XIC B3:0/9  (Part Detected) ]
[ LEQ F8:0    (Pressure <= 4.5) ]
[ XIO B3:0/4  (Fault Light) ]
--> OTE B3:0/3 (Enable Actuator)

### 🔁 Fault Light Logic

[ XIC B3:0/2  (System Trigger) ]
[ XIO B3:0/3  (Actuator) ]
[ XIC B3:0/4  (Fault Light) ]
[ XIO B3:0/6  (Reset Trigger) ]
--> OTE B3:0/4 (Latch Fault Light)

### 🔁 Pressure Fault Indicator

[ GRT F8:0 (Pressure > 4.5) ] --> B3:0/11 (High Pressure Alarm)


### 💡 Real-World Applications

Industrial robotic arms

Actuator motion control in hazardous environments

Assembly line interlocks with analog pressure dependencies

Systems requiring manual reset after a safety trip

### 🛠 Tools Used

RSLogix Micro Starter Lite

RSLogix Emulate 500

RSLinx Classic


