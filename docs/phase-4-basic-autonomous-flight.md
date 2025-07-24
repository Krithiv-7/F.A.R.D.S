# Phase 4: Basic Autonomous Flight

  **Objective:** To have the Raspberry Pi command the drone through a complete, simple, autonomous mission: take off, fly to a point, and land.

  ---

  ### Part A: Safety First

  Autonomous flight is a significant step up in complexity and potential risk. **This entire phase must be conducted outdoors in a large, open field, clear of people, trees, and buildings.**

  1.  **RC Transmitter is Key:** Always have your RC transmitter powered on and ready. You must be prepared to instantly switch the drone from `AUTO` mode to a manual mode (like `STABILIZE` or `LOITER`) to regain control if anything goes wrong.
  2.  **Test Without Propellers First:** Before attempting a real flight, you should run your script with the propellers removed to ensure the logic works as expected (i.e., the motors spin up when "takeoff" is commanded).
  3.  **Establish a Safe Test Area:** Define your flight area and do not let the drone leave it. Start with very short distances and low altitudes.

  ---

  ### Part B: The Autonomous Mission Script

  This script builds upon the connection test from Phase 3. It will run on the Raspberry Pi to send high-level commands to the Pixhawk.

  1.  **SSH into the Pi** while it's on the drone and powered up.
  2.  **Create the Script:** Create a new file named `mission_1.py` and add the following Python code.

      ```python
      from dronekit import connect, VehicleMode, LocationGlobalRelative
      import time
      import math

      #-- Set the connection string
      connection_string = '/dev/ttyAMA0'
      baud_rate = 57600

      #-- Connect to the vehicle
      print(f"Connecting to vehicle on: {connection_string}...")
      vehicle = connect(connection_string, baud=baud_rate, wait_ready=True)

      def arm_and_takeoff(aTargetAltitude):
          """
          Arms vehicle and fly to aTargetAltitude.
          """
          print("Basic pre-arm checks")
          # Don't try to arm until autopilot is ready
          while not vehicle.is_armable:
              print(" Waiting for vehicle to initialise...")
              time.sleep(1)

          print("Arming motors")
          # Copter should arm in GUIDED mode
          vehicle.mode = VehicleMode("GUIDED")
          vehicle.armed = True

          # Confirm vehicle armed before attempting to take off
          while not vehicle.armed:
              print(" Waiting for arming...")
              time.sleep(1)

          print("Taking off!")
          vehicle.simple_takeoff(aTargetAltitude)  # Take off to target altitude

          # Wait until the vehicle reaches a safe height before processing the goto
          while True:
              print(f" Altitude: {vehicle.location.global_relative_frame.alt}")
              # Break and return from function just below target altitude.
              if vehicle.location.global_relative_frame.alt >= aTargetAltitude * 0.95:
                  print(f"Reached target altitude of {aTargetAltitude}m")
                  break
              time.sleep(1)

      #--- Main script execution
      # Arm and take off to an altitude of 10 meters
      arm_and_takeoff(10)

      print("Setting GO TO location...")
      #--- Go to a point 10 meters North and 10 meters East of home.
      #    Note: this is a simple example, a real mission would use GPS coordinates.
      current_location = vehicle.location.global_relative_frame
      target_location = LocationGlobalRelative(current_location.lat, current_location.lon, 20) # Fly at 20m altitude
      # In a real scenario, you would get these from your mission planner
      # For this example, we'll just move it slightly
      # You can get a target location from Google Maps and input the lat/lon here.
      # For example: target_location = LocationGlobalRelative(11.3150, 78.4450, 20)

      # For this test, let's define a target point 10m North of the current location
      # This is a simplified function. For accurate distance, use a proper library.
      # Not for production use.
      # target_location.lat = current_location.lat + 0.00009
      
      # For now, we will just command it to fly to a hardcoded point near the launch site
      # PLEASE UPDATE THESE COORDINATES to a safe point in your flying area.
      # For example, a point 20 meters away from your launch position.
      target_point = LocationGlobalRelative(11.3150, 78.4450, 10) # REPLACE WITH YOUR SAFE GPS COORDS
      vehicle.simple_goto(target_point)

      # Wait for the drone to reach the location, or timeout
      for _ in range(60): # 60 second timeout
          # Add a check for distance to target
          print(f" Flying towards target...")
          time.sleep(1)
          # A more robust check would calculate the distance to the target
          # and break when it's close.

      print("Target location reached (or timeout).")


      print("Returning to Launch")
      vehicle.mode = VehicleMode("RTL") # Return to Launch

      # Wait until the drone has landed
      while vehicle.armed:
          print(f" Waiting for disarm. Altitude: {vehicle.location.global_relative_frame.alt}")
          time.sleep(1)

      print("Mission Complete")
      # Close vehicle object before exiting script
      vehicle.close()
      ```

  ---

  ### Part C: Execution and Testing

  1.  **Position the Drone:** Place the drone in the center of your safe, open test area.
  2.  **Power Up:** Connect the main LiPo battery.
  3.  **Connect and Run:** SSH into the Pi and run the script: `python3 mission_1.py`.
  4.  **Observe:** Stand back and watch the drone's behavior closely. It should:
      * Arm its motors.
      * Take off vertically to 10 meters.
      * Fly towards the specified GPS coordinate.
      * After a short flight, enter "Return to Launch" (RTL) mode.
      * Fly back to its takeoff point and land.
      * Disarm its motors.

  ---

  ### Phase 4 Milestone: First Autonomous Mission

  **Once the drone successfully completes the takeoff -> fly to point -> land sequence commanded entirely by the Raspberry Pi script, Phase 4 is complete.**

  You have proven that the brain can not only listen to the body, but can now command it to perform a full mission. This is the foundation for all future autonomous capabilities.
