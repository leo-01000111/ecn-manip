# ecn_manip — ops cheatsheet

## Start / stop the VM
- Open VirtualBox/VMware → select the VM → Start. Never re-import the .ova.
- Stop: shut down inside the VM, or Save state/Suspend to resume later.
- Snapshot before any `sudo apt` work.

## Every new terminal
```bash
cd ~/ros2/src/manip/ecn_manip
ros2ws
```

## Build
```bash
colbuild -t
```
Clean rebuild if things get weird:
```bash
rm -rf ~/ros2/build ~/ros2/install ~/ros2/log && colbuild -t
```

## Run the simulation (one robot at a time)
```bash
ros2 launch ecn_manip simulation_launch.py robot:=kr16
ros2 launch ecn_manip simulation_launch.py robot:=turret
ros2 launch ecn_manip simulation_launch.py robot:=ur10
```
Opens RViz + the lab GUI (exercise buttons, sliders, plots).

## Run your control code
```bash
ideconf        # opens Qt Creator (gqt on older VMs)
```
Open `CMakeLists.txt` → hammer (build) → green triangle (run) → red square (stop).
The sliders only move the robot while this is running.

## DH code generator
```bash
ros2 run ecn_manip dh_code.py my_table.yml            # add --display / --wrist as needed
```

## Back up
```bash
git add . && git commit -m "lab X progress" && git push
```

## Known fixes
| Problem | Fix |
|---|---|
| GUI dies: `Cannot load backend 'TkAgg'` | `export MPLBACKEND=QtAgg` (should already be in ~/.bashrc) |
| Robot flickers between sliders and zeros | Don't run `joint_state_publisher_gui`; use the lab GUI |
| apt "unmet dependencies" | `sudo apt --fix-broken install`, then `sudo apt full-upgrade` |
| Missing ViSP | `sudo apt install ros-jazzy-visp` (or `libvisp-dev`) |
| `ros--visp` / empty names in commands | No `$` before distro names: `ros-jazzy-...` |
| KDL warning about base_link inertia | Harmless, ignore |
