<h1 align="center">Self-Balancing UGV</h1>

<p align="center"><strong>Vision, voice and streaming software for a self-balancing ground robot.</strong></p>

<p align="center">The software, Arduino firmware and Gazebo simulation of a self-balancing unmanned ground vehicle.<br>The repository also holds the team's proposal for the e-Yantra Ideas Competition 2019-20.</p>

<p align="center"><a href="#what-it-does">What&nbsp;it&nbsp;does</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="#hardware">Hardware</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="#repository-layout">Repository&nbsp;layout</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="#how-to-run">How&nbsp;to&nbsp;run</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="#team-and-credits">Team&nbsp;and&nbsp;credits</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="#license">License</a></p>

<br />

## What it does

- **Balancing.** An Arduino sketch reads an MPU6050 IMU and runs an LQR controller on the base. `bot_arduino_code/bot_arduino_code.ino` holds the controller and the motor and encoder code.
- **Face detection and recognition.** OpenCV's DNN face detector finds faces. A recognizer trained on collected face images names them. Training uses a Haar cascade file.
- **Object detection and tracking.** YOLOv3 through OpenCV's DNN module detects objects in an image, a video or a webcam. A second script tracks detected objects between frames.
- **Voice commands.** A speech script listens to the microphone, recognizes spoken commands such as forward, backward, left and right, and maps them to numeric command codes.
- **Video and audio streaming.** A Flask app streams webcam video to a web page, with a placeholder `/register` page. Separate scripts stream video and audio between two machines on one network.
- **SMS alert.** A script sends a text message through an SMS web service.
- **Simulation.** ROS and Gazebo files model the robot, with a PID controller package.

## Hardware

- An Arduino board running `bot_arduino_code.ino`.
- An MPU6050 IMU, read with the I2Cdevlib libraries in `bot_arduino_code/libraries/`.
- Motors with encoders, driven from the Arduino.
- A webcam and a microphone for the Python scripts.

## Repository layout

- `bot_arduino_code/`: Arduino sketch and the I2Cdev and MPU6050 libraries
- `FaceDetection_and_recognition/`: face detection, training and recognition scripts
- `ObjectDetection-YOLO/`: YOLOv3 detection and tracking scripts
- `voice_command/`: `speech.py`, the voice command script
- `Stream video on webpage/`: Flask video stream with its templates
- `Video_audio processing/`: streamer and viewer scripts for video and audio
- `SMS/`: `send_sms.py`
- `gazebo_simulation/`: ROS packages `gazebo_sim` (URDF, meshes, launch files), `motion_plan` (Python controllers) and `pid`
- `eyic proposal final.pdf`: the e-Yantra Ideas Competition 2019-20 proposal
- `requirements.txt`: Python packages for the scripts
- `CHANGELOG.md`: release history

## How to run

**Python scripts.** Install the packages with Python 3, then run each script from its own folder.

```bash
pip3 install -r requirements.txt
```

These notes come from reading the code, not from a test run. The scripts date from 2020 and the repository pins no versions. Face recognition needs `opencv-contrib-python`. The YOLO scripts expect OpenCV older than 4.5.4, and `Viewer_video.py` expects NumPy older than 2.0. `speech.py` sends audio to Google's web speech service.

**Voice commands.**

```bash
cd voice_command
python3 speech.py
```

**Face detection and recognition.** The scripts load `opencv_face_detector_uint8.pb` and `opencv_face_detector.pbtxt` from a `models` folder, save captured faces to `dataset` and keep the trained model in `training`. None of these folders is in the repository, so create them. The scripts also use absolute paths that start at `/FaceDetection_and_recognition/`, so edit those paths for your machine first.

```bash
cd FaceDetection_and_recognition
python3 face_detection_opencv_dnn.py
python3 face_training.py
python3 face_recognition_dnn.py
```

**Object detection and tracking.** The scripts need `yolov3.cfg`, `yolov3.weights` and `coco.names` in the `ObjectDetection-YOLO` folder. The repository does not include them, and `.gitignore` excludes the weights. `object_detection_yolo.py` takes `--image` or `--video` and uses the webcam without either. `tracking_with_yolo.py` always uses the webcam.

```bash
cd ObjectDetection-YOLO
python3 object_detection_yolo.py
python3 tracking_with_yolo.py
```

**Video on a web page.**

```bash
cd "Stream video on webpage"
python3 webstreaming.py --ip 0.0.0.0 --port 8080
```

`webstreaming.py` runs Flask in debug mode, so use it only on a trusted network.

**Video and audio between two machines.** `Viewer_video.py` binds port 12043 and `Streamer_video.py` connects to `tcp://localhost:12043`, so set the viewer's address in `Streamer_video.py` first. `Streamer_audio.py` binds port 8080 and `Viewer_audio.py` connects to the `HOST` set in the file. Video flows from the streamer to the viewer. Audio flows the other way, from the viewing machine's microphone, and `Viewer_audio.py` sets `input_device_index` in the file. Each script runs until stopped, so give each its own terminal.

On the camera machine:

```bash
cd "Video_audio processing"
python3 Streamer_video.py
python3 Streamer_audio.py
```

On the viewing machine:

```bash
cd "Video_audio processing"
python3 Viewer_video.py
python3 Viewer_audio.py
```

**SMS.** `send_sms.py` asks for a phone number and posts a test message to an SMS service. Set your own service credentials in the file first.

```bash
cd SMS
python3 send_sms.py
```

**Arduino firmware.** Open `bot_arduino_code/bot_arduino_code.ino` in the Arduino IDE and upload it. The repository does not name the board. Copy the two folders in `bot_arduino_code/libraries/` into your Arduino `libraries` folder, and install PinChangeInt.

**Simulation.** The ROS packages are in `gazebo_simulation/`. `gazebo_sim/launch/asb_gzb.launch` loads `config/asb_con.yaml`, which is not in the repository, and the URDF meshes point at a home directory path. The repository gives no tested launch command. Fix those two references before running it.

## Team and credits

Adil Shaikh and Soofiyan Atar built the robot together. Adil worked on the vision, voice and streaming software. The code first appeared in Soofiyan Atar's repository [Automa_Surveillance_robot](https://github.com/Soofiyan/Automa_Surveillance_robot), and its file headers list Soofiyan Atar.

The project acknowledges PyImageSearch. The `pid` package in `gazebo_simulation/pid/` carries its own license, `license.txt`, with Andy Zelenak and Paul Bouchier as contributors.

## License

MIT, Copyright (c) 2020 Soofiyan Atar. See `LICENSE`.
