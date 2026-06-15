# DeLi
Robot Programming Team4  
김지성 김민서 임성우 지선우

## Environment
- **Robot**: TurtleBot3 Burger
- **SBC**: Raspberry Pi 4
- **OS**: Ubuntu 24.04
- **Middleware**: ROS2 Jazzy
- **System**: Automated delivery system (receipt recognition → navigation to destination → bell pressing → return home)

- 
<img width="1213" height="676" alt="image" src="https://github.com/user-attachments/assets/aabcae7b-4dd2-4e14-bb3f-124b7b1005dc" />


## 🚀 How to Run

### 1. Clone External Packages

```bash
cd ~/turtlebot3_ws/src
git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3.git
git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
git clone -b jazzy https://github.com/ros-navigation/navigation2.git
```

### 2. Prerequisites

```bash
sudo apt update
sudo apt install ros-jazzy-cv-bridge ros-jazzy-nav2-msgs
pip install -r requirements.txt
```

### 3. Build

```bash
cd ~/turtlebot3_ws
colcon build --packages-select delivery_interfaces
source install/setup.bash
colcon build --symlink-install
source install/setup.bash
```

### 4. Execution

Open **4 terminals** and run in order.

**[Terminal 1] SBC (Raspberry Pi) — Robot bringup**
```bash
export TURTLEBOT3_MODEL=burger
export LDS_MODEL=LDS-02
ros2 launch turtlebot3_bringup robot.launch.py
```

**[Terminal 2] SBC (Raspberry Pi) — Camera**
```bash
ros2 launch turtlebot3_bringup camera.launch.py
```

**[Terminal 3] Remote PC — Nav2**
```bash
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_navigation2 navigation2.launch.py map:=~/map/map.yaml
```

**[Terminal 4] Remote PC — Delivery system** *(after Terminals 1–3 are ready)*
```bash
cd ~/turtlebot3_ws
source install/setup.bash
export ANTHROPIC_API_KEY=sk-ant-...
ros2 launch delivery_sm delivery.launch.py
```

## 💻 Usage Flow

1. **Capture Receipt**: Point receipt at Receipt window and press **SPACE** → VLM analysis
2. **Confirm Result**: **G**(proceed) / **R**(recapture) / **Q·ESC**(exit)
3. **Automatic Delivery**: Robot navigates to destination and presses doorbell
4. **Return Home**: Press **F** to automatically return to home position

## ⚙️ Configuration

Modify in `delivery_nav/delivery_nav/config.py`:
- `HOME_POSITION`: Home location coordinates
- `GUARD_ROOM_GOAL`: Guard room coordinates
- `ROOM_GOALS`: Coordinates and orientations for each room number

## 🔍 Debugging

```bash
# Check interfaces
ros2 interface show delivery_interfaces/srv/AnalyzeReceipt
ros2 interface show delivery_interfaces/action/ApproachBell

# Check nodes and communication
ros2 node list
ros2 service list | grep -E 'analyze|verify'
ros2 action list | grep approach
ros2 topic echo /delivery_status
```
## 🎥 Demo

### Full Delivery Process
<!-- Drag & drop your video here, or paste a YouTube link -->

### Screen Recording (Receipt Recognition → Navigation → Bell Pressing)
<!-- Drag & drop your screen recording here -->

## 📊 Results

* **Receipt Recognition**: Accurately extracts the destination (room number) from the receipt using a VLM.
* **Autonomous Navigation**: Successfully navigates to the designated destination using Nav2.
* **Bell Pressing**: Recognizes the doorbell location via VLM upon arrival, then approaches and presses it.
* **Return Home**: Automatically returns to the home position after delivery is complete.

https://github.com/user-attachments/assets/262ab007-8418-433a-b4a0-f6de129939ce


