# ARTheaterCueing
Claire Murray - Rochester Institute of Technology

CS Capstone Spring 2023

## Description
This project presents a system developed in Unreal Engine 4.26 for integrating AR Headsets as stage elements that can be cued using modern cueing software.
The system uses a multi-layer architecture and a networked approach in order to achieve a smooth, synchronous experience across multiple devices. The system can be integrated with commonly used theater cueing software with minimal setup required by the user. Open Sound Control messages control the application remotely, and through the use of the Unreal Sequencer, a vast variety of effects can be triggered across several devices. These sequences of effects can then be handed back to professional cueing software on a separate machine, and have the cues needed to run those effects generated automatically. A limited number of effects are currently implemented in this sample version. However, the framework is able to be further expanded to enable a large scope of Augmented Reality effects.

## Demo Video


https://user-images.githubusercontent.com/46585221/236295047-08df6df0-acf7-48e4-91c9-46566f0d3975.mp4


## How to Run

### System Requirements
In order to use this application you should have the following at minimum:
* A PC running Windows 10 or higher
* A Computer capable of running [QLab](https://qlab.app/)
* A [Magic Leap](https://www.magicleap.com/en-us/) Generation 1 device

### Build Guide
  When building for the server, in the Unreal Project you should use `File/Package Project/Windows64`
  
  A detailed guide on how to build and deploy Unreal apps for the Magic Leap 1 can be viewed [here](https://drive.google.com/file/d/1NtPB8l1UmfEzSEFfTifMmqznxfXZJ2j2/view?usp=sharing).
  When building for the Magic Leap, verify in the `Minimal_Default` level blueprint that the IP for the server to join is set correctly.
  
  ![Set IP of Server]()
  
### QLab Setup
  In your QLab workspace, you will need to create a new network patch. To do this, go to `Window/Workspace Settings/Network` and click "New Patch."
  Name it something clear, and set the type to "Address", the network to "Automatic", the destination to the IP of your server, and the port to 8094
  
  * If you are not modifying the code, please set the path number of this patch to "2"
  
  ![QLab Network Patch]()

### Command Line Arguments
The following command should be used when running the application as the server:

```./ARListenServer -server -workspace=[workspace ID] -remote=[remote IP address]```

`-server` is required in order to properly launch as a server instead of a client.
`-workspace` is the ID of the QLab workspace to link to. This can be viewed with QLab open by navigating to `Window/Workspace Status` and clicking the
"Info" tab. `-remote` is the IP of the machine from which you are running QLab.

![QLab Workspace ID]()

### Steps to Run
After you have built and deployed the project, follow these steps to run it:

1. From a PC Shell, navigate to the directory where you packaged your server build, and run the command from the section above this.
You should after a few seconds see a scene with two cubes and a sphere. If you see an empty map, check the debug messages on the screen,
and verify that your command line parameters are set correctly. The server must be started before any clients may join.

2. In QLab, verify that sequence cues were generated correctly. If they were not, your app may still work, but you will have to enter cues manually.
If you do not, verify that your workspace ID
was entered correctly from the command line. If not, quit and re lauch the app.

3. From the Magic Leap device, launch the deployed app. After a few seconds, you should see the same scene. If you do not, verify that the server IP was
set correctly when you built and deployed the app.

## Integration Guide
In order to integrate this system with an existing project, one should follow these guidelines:

### Entry Level
* The level `Minimal_Default` and all its contents should be migrated into the project
* The default map of the game should be set to `Minimal_Default`
* The IP in the level blueprint should be changed to that of your server machine

### ListenLevel
* The level `ListenLevel` and all its contents should be migrated into the project
* Any Actors and Sequences that you wish to be cued should be copied into `ListenLevel`
* Any of the demonstration Actors/Sequences in `ListenLevel` can be removed. Note however that this may break references in a few demonstration functions
* Any functions in the level blueprint for `ListenLevel` beginning with the prefix `Client_` can have their references removed and be deleted if you wish.
* DO NOT delete/unlink the function `Make Sequence Cues`
* Inside the `Make Sequence Cues` function, find the commented node within "Edit Patch number of created cue" and set the integer value to the patch number 
of your AR Cue network patch

![Edit Cue Patch Number]()

### Creating Cues

#### API
The formula for creating new API bindings is as follows in the image:

![Create API Binding]()

#### Sequences
It is **highly recommended** that as many cues as possible be handled through level sequences, as this affords the highest level of flexibility.
Make sure that any and all sequence actors are set to "replicate playback"

#### Blueprint Actors
Blueprint Actors must have their replication functionality programmed manually. Refer to the following blueprint snippets for examples

* Level Bluepring Set Light Color: ![Level Bluepring Set Light Color]()

* PointLight_Blueprint OnRep_CurrentColor: ![PointLight_Blueprint OnRep_CurrentColor]

* PointLight_Blueprint Event Graph: ![PointLight_Blueprint Event Graph]()

## Full Report
The full report for this project can be viewed [here]()
