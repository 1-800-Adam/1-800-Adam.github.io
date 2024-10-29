- Project Name: Multi-Position Servo Application
- Project Completed: 10/29 
- Prepared By: Adam
## Project Purpose:
---
- Design a PLC Program for a modular water treatment system that is to be integrated into an existing system.
- PLC will control a Cam Servo to trigger 4 switches:
	- 1 - Home Switch
	- 2 - Tank Fill Switch
	- 3 - Drain Tank Switch
	- 4 - Flush Tank Switch
- PLC will receive a Cycle Call signal to run through the above sequence of operations 
- Machine will also have a RESET button that will turn on the Homing sequence
## Project Description:
---
- Diagram of the System is below:
	- ![[https://1-800-adam.github.io/assets/docs/Project-4/attachments/DIAGRAM.svg]]
- Machine will have several Positions (P1...P4) and States (S0...S8)
	- [[STATESPOSITIONS.png]]
	- **Machine Positions**: describe the position of the cam lobe
	- **Machine States**: describes the states of the machine to control logic and motor operation
- Machine States are defined as the following:
	- [[Pasted image 20241029143725.png]]
## Desired Results:
---
- System should be able to start in the Homing state to relocate the Cam to the Home Switch position
- System should be able to receive a cycle call signal (only when it is in the Home state) to cycle through the 4 positions
- There should also be an integer that outputs the Position and State information
	- Added functionality such that a string is updated and outputted for the State and Positions information 
- Control Sequence is:
	1. Home -> Fill
	2. Fill for 10s
	3. Fill -> Drain
	4. Drain for 20s
	5. Drain -> Flush
	6. Flush for 10s
	7. Flush -> Home
## Exclusions:
---
- Machine does not need an ESTOP button, but instead will use a RESET to interrupt the sequence and skip all positions
## Acceptance Criteria:
---
- Passes instructor outlined test cases per: [Project 4 - Multi-Position Servo Application.pdf](https://1-800-adam.github.io/assets/docs/Project-4/Project%204%20-%20Multi-Position%20Servo%20Application.pdf)
## Project Walkthrough:
---
- There are 4 project files:
	- 2 - Main
		- Runs through the program in the order below
	- 23 - IO
		- Manages the Program Inputs and Outputs
		- Inputs: Home Sw, Fill Sw, Drain Sw, Flush Sw, Cycle Call, Reset
		- Output: Cam Servo 
	- 24 - State Ctr
		- Controls the Machine States according the above state diagram
	- 25 - Control
		- Digital Control Logic for the Cam Servo, if the machine is in a Traveling State, Cam Servo will activate, else it will interrupt the hold in circuit
	- 26 - Cam Pos
		- Outputs the Cam Position based on the Sensor Activation and if the Machine is in a traveling state
	- 27 - Str Mgr
		- Management of the Strings to Output the Machine State and Position to the string files
			- ST9 - MC POS, Machine Position Strings
				- [[Pasted image 20241025173706.png]]
			- ST11 - MC STATE, Machine State Strings
				- [[Pasted image 20241025173720.png]]
	- 255 - Init
		- Initializes the program to state 0 for Homing
## Program PDF:
---
- Link to the Project Program RSLogix 500 is here: https://www.plcdojo.com/courses/take/applied-logic/lessons/15204142-project-specification
## Post-Project Notes:
---
- Lessons Learned
	- If there is a need to re-organize the project files and structure, it is possible to restart the numbering sequence to another base 10. For example:
		- If the original organization is:
			- 2 - Main
			- 3 - I/O
			- 4 - Controls
		- You can re-organize the project to another base-10
			- 22 - Main v2
			- 23 - I/O v2
			- 24 - Controls v2
	- Any Digital Control Logic Hold-In circuits should only have One Trigger and One Interrupt. If there are Multiple, the hold-in circuit will fluctuate between states
