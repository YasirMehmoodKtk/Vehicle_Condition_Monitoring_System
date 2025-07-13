# Fuel Monitoring System

## Overview
The **Fuel Monitoring System** is a Python-based implementation designed to monitor changes in a vehicle's fuel level and detect events like fuel filling or fuel theft. The system also accounts for scenarios where fuel decreases due to generator operation.

## Features
1. **Fuel Filling Detection**:
   - Detects when the fuel level increases and reports it as a fuel filling event.

2. **Fuel Theft Detection**:
   - Detects when the fuel level decreases unexpectedly (without generator operation) and flags it as fuel theft.

3. **Generator Operation Tracking**:
   - Differentiates between fuel decreases due to generator consumption and unauthorized fuel removal.

## How It Works
- The system maintains two fuel levels:
  - **Previous Fuel Level**: The fuel level from the last update.
  - **Current Fuel Level**: The fuel level from the latest reading.

- The system compares the `current_fuel_level` to the `previous_fuel_level` to determine changes:
  - **Increase**: Fuel filling is detected.
  - **Decrease**: 
    - If the generator is running, it's considered normal consumption.
    - Otherwise, fuel theft is detected.
  - **No Change**: No action is taken.

## Example Usage
Here's how to use the Fuel Monitoring System:

```python
class FuelMonitoringSystem:
    def __init__(self, initial_fuel_level):
        self.previous_fuel_level = initial_fuel_level
        self.current_fuel_level = initial_fuel_level

    def update_fuel_level(self, new_fuel_level, generator_running=False):
        self.current_fuel_level = new_fuel_level

        # Check for changes in fuel level
        if self.current_fuel_level > self.previous_fuel_level:
            self.report_fuel_filling()
        elif self.current_fuel_level < self.previous_fuel_level:
            if generator_running:
                print("Fuel is decreasing due to generator operation.")
            else:
                self.report_fuel_theft()
        else:
            print("No change in fuel level.")

        # Update the previous fuel level
        self.previous_fuel_level = self.current_fuel_level

    def report_fuel_filling(self):
        print("Fuel level increased. Fuel filling detected.")

    def report_fuel_theft(self):
        print("Fuel level decreased. Fuel theft detected!")

# Example usage
if __name__ == "__main__":
    # Initialize the system with an initial fuel level
    fuel_system = FuelMonitoringSystem(initial_fuel_level=50)  # Example initial fuel level: 50 liters

    # Simulate fuel updates
    fuel_system.update_fuel_level(60)  # Fuel filling example
    fuel_system.update_fuel_level(40, generator_running=False)  # Fuel theft example
    fuel_system.update_fuel_level(30, generator_running=True)  # Generator running example
    fuel_system.update_fuel_level(30)  # No change in fuel level


Output
Based on the example usage, the system produces the following output:

vbnet
Copy code
Fuel level increased. Fuel filling detected.
Fuel level decreased. Fuel theft detected!
Fuel is decreasing due to generator operation.
No change in fuel level.
Customization
Initial Fuel Level: Set during system initialization.
Fuel Update Frequency: Can be adapted based on the data source (e.g., real-time sensors, database).
Generator Status: Passed as a parameter during fuel updates to distinguish normal consumption from theft.
Requirements
Python 3.x
Future Enhancements
Integrate with real-time data sources such as vehicle ECU databases or sensors.
Send notifications (e.g., email or SMS) in case of fuel theft.
Extend support for hybrid or electric vehicles.
