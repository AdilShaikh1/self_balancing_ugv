# Changelog

All notable changes to this project are listed here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow
[Semantic Versioning](https://semver.org/).

## [1.0.0] - 2026-11-26

The software, firmware and simulation of a self-balancing ground robot, from the team's e-Yantra Ideas Competition 2019-20 entry,
with a README that matches the repository.

### Added

- This copy of the team's 2019-20 work holds the Arduino balancing sketch, the face detection and recognition scripts, the YOLO detection and
  tracking scripts, the voice command script, the SMS script, the web page video stream and the video and audio streamers.
- The ROS and Gazebo simulation files, with the robot's URDF, meshes, launch files and Python controllers, and the `pid` package.
- The e-Yantra Ideas Competition 2019-20 proposal (`eyic proposal final.pdf`).

### Changed

- `.DS_Store` is in `.gitignore`.
- The README is rewritten. It covers what the project does, its hardware, the repository layout, how to run each part, the team and
  the license.

### Removed

- The nine tracked `.DS_Store` files.

[1.0.0]: https://github.com/AdilShaikh1/self_balancing_ugv/releases/tag/v1.0.0
