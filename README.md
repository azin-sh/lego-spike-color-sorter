LEGO SPIKE Prime Color Sorter 🤖
Course: CS 5 - Introduction to Machine Learning

Date: February 1, 2026

Developer: Azin Shahrokhi

📋 Project Overview
This project involved constructing and programming an automated sorting machine using the LEGO SPIKE Prime system. The goal was to build a system capable of identifying specific colors and physically categorizing them into distinct bins using Python-based logic.

🛠️ Hardware Configuration
The build features a gravity-fed hopper and a rotating delivery ramp designed for high-precision sorting.

Port E: Color Sensor (calibrated to filter out environmental noise).

Port C: Drop Motor (set to 20% speed for controlled brick release).

Port F: Ramp Motor (set to 10% speed for accurate bin alignment).

Categorization: Calibrated for Red, Yellow, Green, and Blue.

💻 Software Logic (Python)
The system uses asynchronous programming (async/await) to manage motor movements and sensor readings without blocking the main execution loop.
# Week 1 Lab: LEGO SPIKE Prime Color Sorter 🤖
**Course:** CS 5 - Introduction to Machine Learning  
**Date:** February 1, 2026  

## 📋 Project Overview
This project involved constructing and programming a motorized sorting machine using the LEGO SPIKE Prime system. The goal was to build a system capable of identifying specific colors and physically categorizing them into distinct bins using Python-based logic.

## 🛠️ Hardware Configuration
The build follows the "Prof. Bricks" design, featuring a gravity-fed hopper and a rotating delivery ramp.

- **Port E:** Color Sensor (calibrated to filter out the Cyan ramp color).
- **Port C:** Drop Motor (set to 20% speed for controlled brick release).
- **Port F:** Ramp Motor (set to 10% speed for high-precision bin alignment).
- **Categorization:** Calibrated for Red, Yellow, Green, Blue, and Magenta.

## 💻 Software Logic (Python)
The system uses asynchronous programming (`async/await`) to manage motor movements and sensor readings simultaneously.

```python
import motor
import color_sensor
import color
import runloop
from hub import port, light_matrix

# Configuration
COLOR_SENSOR_PORT = port.E
DROP_MOTOR_PORT = port.C
RAMP_MOTOR_PORT = port.F

async def drop_at_position(angle):
    # Move ramp to bin
    await motor.run_to_absolute_position(RAMP_MOTOR_PORT, angle, 200, direction=motor.SHORTEST_PATH)
    await runloop.sleep_ms(500) # Stabilization buffer
    
    # Drop the piece
    await motor.run_for_degrees(DROP_MOTOR_PORT, -80, 100)
    await motor.run_for_degrees(DROP_MOTOR_PORT, 80, 100)

    # Return to start
    await motor.run_to_absolute_position(RAMP_MOTOR_PORT, 0, 200, direction=motor.SHORTEST_PATH)

async def main():
    while True:
        detected = color_sensor.color(COLOR_SENSOR_PORT)
        if detected == color.RED:
            await drop_at_position(80)
        elif detected == color.YELLOW:
            await drop_at_position(130)
        elif detected == color.GREEN:
            await drop_at_position(270)
        elif detected == color.BLUE:
            await drop_at_position(-145)
        else:
            await light_matrix.write('Not Detected')
        
        await runloop.sleep_ms(1)

runloop.run(main())
 Testing & Debugging
Challenges Faced
Angle Calibration: Initial angles from the demonstration video did not align with our specific hardware setup, causing the blocks to miss the bins.

Mechanical Issues: The drop motor initially "shot" blocks upward because the return angle was too shallow (30-degree difference).

Adjustments Made
Motor Refinement: Changed drop motor initialization from a 30-degree difference to a full -80/80 range to ensure a clean return to the starting position.

Manual Calibration: Instead of relying on pre-set angles, I manually rotated the motor to the exact bin locations and recorded the coordinates to ensure 100% accuracy.

💡 Reflection & Future Improvements
This project was a great introduction to Python's interaction with hardware. I learned that "logical" code doesn't always translate perfectly to "physical" hardware without manual calibration.

Future Goals:

Implement software buffers (e.g., runloop.sleep_ms(300)) to ensure pieces have fully cleared the sensor before the next detection cycle.

Focus on "coordinate-based" logic rather than just mathematical assumptions.

Continue practicing teamwork by leveraging individual strengths in coding and hardware assembly.
