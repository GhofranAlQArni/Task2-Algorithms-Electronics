# Task2-Algorithms-Electronics
This algorithm assumes a bipedal robot with two legs, each having a hip and knee servo motor. Walking Algorithm.

`
Here's a refined version of the servo motor control algorithm for walking, enhancing efficiency and adding flexibility for different walking speeds and smoother transitions. 
This version also includes error handling and more detailed comments for clarity.

# Enhanced Servo Motor Control Algorithm for Robot Walking

```python
import time
import some_servo_library as servo  # Replace with the actual library you're using

# Initialize the servos (this will be specific to your hardware)
servo_hip_left = servo.Servo(1)  # Example servo initialization
servo_knee_left = servo.Servo(2)
servo_hip_right = servo.Servo(3)
servo_knee_right = servo.Servo(4)

# Define the servo motor positions for different parts of the walk cycle
def set_servo_positions(hip_left, knee_left, hip_right, knee_right):
    try:
        # Set the servo positions using a servo control library or API
        servo_hip_left.write(hip_left)
        servo_knee_left.write(knee_left)
        servo_hip_right.write(hip_right)
        servo_knee_right.write(knee_right)
    except Exception as e:
        print(f"Error setting servo positions: {e}")

# Define the walking cycle positions (angles in degrees)
# Adjust these positions for your specific robot
walk_cycle = [
    {"hip_left": 30, "knee_left": 60, "hip_right": -30, "knee_right": 60},  # Left leg forward, right leg back
    {"hip_left": 0, "knee_left": 45, "hip_right": 0, "knee_right": 45},    # Both legs straight
    {"hip_left": -30, "knee_left": 60, "hip_right": 30, "knee_right": 60}, # Right leg forward, left leg back
    {"hip_left": 0, "knee_left": 45, "hip_right": 0, "knee_right": 45}     # Both legs straight
]```

# Function to smoothly transition between positions
```ruby

def smooth_transition(start_pos, end_pos, steps=10):
    step_size = {key: (end_pos[key] - start_pos[key]) / steps for key in start_pos}
    for step in range(steps):
        intermediate_pos = {key: start_pos[key] + step * step_size[key] for key in start_pos}
        set_servo_positions(intermediate_pos["hip_left"], intermediate_pos["knee_left"], 
                            intermediate_pos["hip_right"], intermediate_pos["knee_right"])
        time.sleep(0.05)  # Small delay for smooth transition
```


# Walking function with adjustable speed
```ruby

def walk(cycle, speed=0.5):
    while True:  # Loop to continuously walk
        for i in range(len(cycle)):
            current_pos = cycle[i]
            next_pos = cycle[(i + 1) % len(cycle)]
            smooth_transition(current_pos, next_pos, steps=int(speed*20))
            time.sleep(speed)  # Adjust the sleep time to control the speed of the walk cycle
```


# Start walking
```ruby

walk(walk_cycle, speed=0.5)
```

### Explanation ##

1. **Servo Initialization:**
   - The servos are initialized according to your specific hardware setup. Replace `some_servo_library` with the actual library you're using.

2. **Setting Servo Positions:**
   - `set_servo_positions()` function sets the positions of the hip and knee servos for both legs. It includes error handling to manage any issues during position setting.

3. **Walking Cycle:**
   - `walk_cycle` defines a sequence of positions representing different stages of the walking motion. Adjust these positions based on your robot's geometry and capabilities.

4. **Smooth Transition:**
   - `smooth_transition()` function ensures smoother transitions between positions by breaking down the movement into smaller steps.

5. **Walking Function:**
   - `walk()` function iterates through the walk cycle positions, using `smooth_transition()` for smooth movement and `time.sleep()` to control the speed. The speed can be adjusted via the `speed` parameter.

# Notes

- **Adjust Servo Angles:** Modify the angles for the servos (`hip_left`, `knee_left`, `hip_right`, `knee_right`) based on your robot's physical capabilities.
- **Speed Control:** The `speed` parameter in the `walk()` function controls the speed of the walking cycle. Adjust this value to make the robot walk faster or slower.
- **Library Replacement:** Ensure to replace `some_servo_library` with the actual servo control library you are using.
- **Servo Calibration:** Properly initialize and calibrate the servos according to your specific hardware requirements.

This enhanced algorithm provides a more flexible and refined approach to robotic walking, with smoother transitions and adjustable speed control.
