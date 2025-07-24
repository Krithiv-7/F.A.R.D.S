# Phase 8: HQ Communication & Dashboard

  **Objective:** To enable the drone to transmit its findings (images, coordinates, status) in real-time to a simple web-based dashboard at a ground station.

  ---

  ### Part A: Hardware - The Communication Link

  1.  **Component:** A 4G LTE or 5G USB Dongle compatible with the Raspberry Pi.
  2.  **Setup:**
      * Insert an activated SIM card with a data plan into the dongle.
      * Connect the dongle to one of the Raspberry Pi's USB 3.0 ports.
      * Most modern Raspberry Pi OS versions will automatically detect the dongle as a network interface. You can configure it using the network manager tools if needed.
  3.  **Test:** Ensure the Pi can connect to the internet through the dongle by running a command like `ping google.com`.

  ---

  ### Part B: The Backend Server (The "HQ")

  This is a simple web server that will listen for data from the drone. It can be run on a cloud service (like Heroku, AWS, etc.) or even on your laptop for testing. We'll use Python with the Flask framework for simplicity.

  1.  **Setup (on your server/laptop):**
      ```bash
      pip install Flask
      ```
  2.  **Create `hq_server.py`:**
      ```python
      from flask import Flask, request, jsonify, render_template_string
      import os
      import base64

      app = Flask(__name__)

      # In-memory storage for the latest report
      latest_report = {}

      @app.route('/')
      def dashboard():
          # Simple HTML dashboard to display the latest report
          html = """
          <html>
              <head>
                  <title>F.A.R.D.S. Dashboard</title>
                  <meta http-equiv="refresh" content="5">
              </head>
              <body>
                  <h1>F.A.R.D.S. Live Report</h1>
                  {% if report %}
                      <p><b>Timestamp:</b> {{ report.get('timestamp') }}</p>
                      <p><b>Location:</b> {{ report.get('lat') }}, {{ report.get('lon') }}</p>
                      <p><b>Confidence:</b> {{ report.get('confidence') }}%</p>
                      <img src="data:image/jpeg;base64,{{ report.get('image') }}" width="640">
                  {% else %}
                      <p>Waiting for first report from drone...</p>
                  {% endif %}
              </body>
          </html>
          """
          return render_template_string(html, report=latest_report)

      @app.route('/report', methods=['POST'])
      def receive_report():
          global latest_report
          data = request.json
          print(f"Received report: {data}")
          
          # Update the latest report
          latest_report = data
          
          return jsonify({"status": "success", "message": "Report received"}), 200

      if __name__ == '__main__':
          # Make sure to run on 0.0.0.0 to be accessible from the network
          app.run(host='0.0.0.0', port=5000)
      ```

  ---

  ### Part C: Updating the Drone's Master Script

  We need to modify `master_mission.py` from Phase 7 to send the report over the network.

  1.  **Add `requests` library:** On the Pi, install the library: `pip3 install requests`.
  2.  **Create a `send_report` function:** Add this function to your `master_mission.py`.
      ```python
      import requests
      import json
      import base64
      from datetime import datetime

      HQ_ADDRESS = "http://<YOUR_SERVER_IP_ADDRESS>:5000/report"

      def send_report(image_path, location, confidence):
          try:
              with open(image_path, "rb") as image_file:
                  encoded_string = base64.b64encode(image_file.read()).decode('utf-8')

              report_data = {
                  "timestamp": datetime.now().isoformat(),
                  "lat": location.lat,
                  "lon": location.lon,
                  "confidence": confidence * 100,
                  "image": encoded_string
              }
              
              headers = {'Content-Type': 'application/json'}
              response = requests.post(HQ_ADDRESS, data=json.dumps(report_data), headers=headers, timeout=10)
              
              print(f"Report sent to HQ. Status: {response.status_code}")

          except Exception as e:
              print(f"Could not send report to HQ: {e}")
      ```
  3.  **Integrate into the main loop:** In your main mission logic, when fire is detected, call this new function:
      ```python
      # Inside the 'if fire detected' block:
      # ...
      image_filename = f"fire_{datetime.now().strftime('%Y%m%d_%H%M%S')}.jpg"
      # (code to save the image to image_filename)
      
      current_coords = vehicle.location.global_relative_frame
      send_report(image_filename, current_coords, confidence_score)
      ```

  ---

  ### Phase 8 Milestone: First Remote Report

  1.  **Start the server:** Run `python3 hq_server.py` on your cloud/local server.
  2.  **Run the mission:** Execute the updated `master_mission.py` on the drone for a test flight.
  3.  **Check the Dashboard:** Open a web browser and navigate to `http://<YOUR_SERVER_IP_ADDRESS>:5000`.

  **Once the drone detects the simulated fire target and the dashboard updates within seconds to show the location, confidence score, and the captured image, Phase 8 is complete.**

  You have successfully closed the loop, giving your autonomous drone the ability to report its findings to a human operator anywhere in the world.
