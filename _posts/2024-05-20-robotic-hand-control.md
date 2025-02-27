---
layout: post
display: Methods for Control of a Robotic Hand
title: robotic-hand-control
categories: projects
tags: [Python, ROS, HTML, CSS, Javascript, SVG, Computer Vision, Machine Learning, BCIs]
github: https://github.com/smithjjoseph/EEG-robotic-hand
permalink: /:categories/:title
---

BEng dissertation project involving the use of a 'qb Softhand research' and various methods for its control including a look at a possible method for a Brain Computer Interface. This project also includes stimuli animated resources to aid the collection of data for training a model for a BCI.

<!--end_excerpt-->

# Abstract
Robotic analogues for human limbs can be seen in use in fields including manufacturing and prosthetics. Methods for controlling prostheses vary based on use-case however a particularly interesting technology is neuroprosthesis which makes use of cortical signals to control the movement of robotic limbs. This extraction and analysis of signals is known as a Brain Computer Interface (BCI) and is the main mechanism for enabling movement. This project aimed to implement and assess the use of EEG signals as well as computer vision models for hand movement and face movement to control a robotic hand. In this project a system is produced allowing for the use of multiple computer vision models to control a robotic hand. It was found that the implementation of computer vision models created, was too slow to be used effectively for prostheses, however the results of the models were precise enough. Due to hardware obtainment limitations the EEG portion of this project was not possible. It is hoped that this work can be continued in order to make a full implementation of the EEG portion of the project. Future work could also work on further increasing the precision of results and reducing any shakiness.

# Introduction
The primary aim for this project is to create multiple systems for the control of a robotic hand and then analyse their efficacy. This project aims to consider the effectiveness of each system in real-world use cases such as prosthetics and industrial applications.
This title and aim of this project have changed since it was started. The original name for this project was ‘Robotic Hand Control using EEGs’ however this was changed at late point in the project once it was found that there would be issues using the hardware. At this point a contingency plan was launched which involved the use of computer vision as a means for control rather than EEG signals. This contingency plan included the control of the robotic hand using computer vision for hand mimicking and facial emotion.

# Background
## Computer Vision
Computer vision is the ability for computers to be able to derive useful information from visual inputs. [2] Research for computer vision started in 1959, when scientists found that the most basic forms of image processing start with the identification of hard edges or lines. This has since evolved into object recognition and now computers can even identify the constituent parts of objects. This is possible due to deep learning (multi-layered machine learning) and convolutional neural networks which break images down into pixels for performing mathematical operations on.

## Electroencephalography
In 1875, Richard Caton, upon learning about David Ferrier's discovery of electrical signals in the brain, employed a galvanometer to observe and quantify electrical currents present on the surfaces of animal brains [3]. Nevertheless, it was only in 1924 that Hans Berger, a German psychiatrist who possessed knowledge of Caton's endeavours, demonstrated that the human brain also employs electrical waves to transmit signals. Notably, Berger conducted the inaugural recording of human brain activity known as an electroencephalogram (EEG) [4].

Modern day electroencephalography makes use of electrode caps which when in contact with the scalp, can be used to capture electrical signals produced by the brain to produce EEGs. It is important to note that electric activity is not localised on the surface of the scalp and therefore can be picked up my multiple electrodes however it can be deduced from the strength of a signal what locality it originated from. The challenge of EEG analysis is to separate the many electrical signals occurring in the brain and identify activity of interest.

# Objectives
It is important to note that the current aims and objectives of this project have shifted due to the hardware limitations found during the project and the contingency plan put in place instead.
The main aims for this project can be broken down into simple steps which are assigned criteria so that it can be assessed whether or not these objectives have been met by the end of the project:
1.	Using a webcam video stream, identify what percentage closed a hand is. Considered met if an application has been created that can identify, with good accuracy, how closed a hand is.
1.	Using a webcam video stream, classify different facial emotions. Considered met if an application has been created that can identify, with good accuracy, at least three facial emotions.
1.	Read data into a computer from an EEG system. Considered met if EEG data can be collected/stored on a computer and preferably displayed on some graph (V-t).
1.	Drive the robotic hand to change to multiple basic states, e.g. closed fist, open hand. Considered met if the hand can be programmed to change state between to at least two desired positions.
1.	Assess the accuracy of the system and potential applications. Considered met if the final report for this project contains information about the accuracy of the system and potential applications.

The above points are all considered core aims. Depending on time and complexity constraints, further stretch targets may also be met:
1.	Recreate the data collected using the EEG system. Considered met if the recreated data can be collected/stored on a computer and preferably displayed on some graph (V-t).
1.	Accurately interpret the recreated EEG data and move the robotic hand together in real time. Considered met if the recreated data can be used to timely move the robotic hand. This includes classifying any states correctly >80% of the time and the total response of the system taking less than 100ms.

# Implementation
The implementation of this project was tackled using separate modules. These modules were ROS, test-video, and vision. Before the contingency plan was implemented there was also going to be an EEG module as well however since no analysis of signals was completed, this module was removed from the project.

## ROS
The ROS module was an early part of the project to be implemented due to lack of availability of the EEG cap and its relative isolation from the rest of the code. This module is produced as a standalone component designed to be easily integrated into any other script within the project.
Communication with the robotic hand was achieved using ROS 1 Noetic which only is only available on certain Linux operating systems therefore Ubuntu 20.04 LTS was installed on the machine used for running this project. ROS could then be installed by adding ROS packages to the APT sources list and installing using the `apt install` command. The final step in installation was to add a setup script to the `.bashrc` file so that catkin, the build system for ROS would be initialised every time a new terminal window was opened.
This project used a qbrobotics SoftHand 2 research robot. The SoftHand has 2 controllable motors: `manipulation-joint` used for pinching with thumb and index finger and `synergy-joint` used for opening the hand flat and closing the hand into a fist.

![Image of qb SoftHand Research]({{site.baseurl}}/assets/images/posts/{{page.title}}/qb_softhand_research.png){: width="250" }
*Figure 1: Image of the qbrobotics SoftHand Research*

The robotic hand is set up to communicate with a ROS server. A class was written for controlling the robotic hand using ROS trajectories, which is a programmatic interface that sends commands to the ROS server. A simple method for control of the robotic hand was supplied at the start of the project. This has been retained in a modified state in the project directory as it is useful for running via the command line when setting up the SoftHand to check the working order of the connection.

The more complex control method contained in this module contains a class which has methods for control of the hand either by requesting a specific percentage of closure or by asking for small incremental movements. The percentage control is particularly useful for mimicking hand positions whereas the small incremental movements are intended for use with the facial emotion and EEG control methods.

In order to send messages to the ROS server, an object must be set up for asynchronously sending messages of a specific type (in this case `JointTrajectory`) to a specific ROS topic. Along with a node for running processes on. This class also sets a rate for which the program will try to fulfil, this program specifically attempts to send 5 messages a second. A new `JointTrajectoryPoint` is created where attributes for the point's position can be set. This also requires that the velocity, acceleration, and effort values are also set however these are not used in this script. For the purposes of this project only the synergy joint is being manipulated however it is necessary to give some values for both joints. The position value of this joint ranges from 0 to 1 with 0 being flat open hand and 1 being closed into a fist. All values for joints and attributes not being used are set to 0.

This `JointTrajectoryPoint` is then added to a `JointTrajectory` object which contains the current timestamp (used for ordering messages as they come in) and the names of the joints that are to be manipulated. Finally, the `JointTrajectory` object can be ROS server and a sleep initiated to fulfil the 5Hz rate set at the beginning of the program.

## vision
After identifying that the EEG path for this project might not have been available, multiple methods for computer vision control were decided upon. First, hand identification would be used and then facial identification.

...