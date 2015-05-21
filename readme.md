Agar.io Reverse Engineering
========

**Author:**
 * DnAp

## Feature ##
 * Reverse engineering main_out.js with user frendly variables names
 * Greater field of view - zoom rate from 10 to 10.35
 * Depict circle around big points
 * Show mass for all points
 * Color enemy

## Color enemy ##
 * Green - small food
 * Sea green - food
 * Blue - friend, safe mass
 * Red - predator

### To start ###

#### Without php ####
Copy files to your server and open their as http://localhost/ or http://agar.io/

#### code logic ####
Websocket API
### send data format ###
1. send 5 bytes(1st byte=255, 4 bytes=1) when opened connection
2. send name: 0(1byte) + characters ascii(16 byte each, to support other languages)
3. send actions: 1 byte number(command)
	- 1 : spectate
 	- 17: space key(split)
	- 18: Q key
	- 19: Q keyup, close game
	- 21: W key(eject mass)
4. send normalized location: 21 bytes
	- 1-1 bytes:16
	- 2-9 bytes: x pos(float)
	- 10-17 bytes: y pos(float)
	- 18-21 bytes: 0

### receive data format ###
received in data buffer with first byte is command, as follows
16

17
	returns normalization params, px, py and ratio
	this message never came
20
	resets points.
	this message never came
32
	new bucket. probably your own id for bucket
48
	elements with name. also doesnt come here
49
	leaderboard: name and ids
64
	size of canvas, comes when select region. fixed for all regions

tasks on server
1. generate user at random location
2. generate seeds at random location
3. split into multiple parts if collides with splitter.
4. handle commands to split, eject
5. logic to eat others given some constraints.
6. if user have several parts, keep them together.
