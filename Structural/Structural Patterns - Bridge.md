---
attachments: [factory.png]
tags: [Design Patterns]
title: Structural Patterns - Bridge
created: "2021-05-08T06:51:39.679Z"
modified: "2021-05-08T17:39:37.199Z"
---

// BRIDGE
// Intent:
// to split up the abstraction and implementation layers of a large class or a set of closely related classes
// Design:
// divide a dimension of the class into its own orthogonal class

# Structural Patterns - Bridge

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Structural patterns are concerned with how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

Bridge lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.

#### Objects

- **Controller** provides high-level control logic. It relies on the Implementor object to do the actual low-level work.
- **Implementor** are abstract/concrete products
  <img src="https://refactoring.guru/images/patterns/diagrams/bridge/example-en-2x.png" width="500" />

#### Application

- Use the pattern when you need to extend a class in several orthogonal (independent) dimensions.

#### Code

```typescript

// Abstract products provide interface
interface Device is
    function isEnabled()
    function enable()
    function disable()
    function getVolume()
    function setVolume(percent)
    function getChannel()
    function setChannel(channel)


// Concrete products implement
class Tv implements Device is
    // ...

class Radio implements Device is
    // ...


// Controller maintains a reference to the Abstract Product and delegates all of the real work to it.
class RemoteControl is
    var device: Device
    constructor RemoteControl(device: Device) is
        this.device = device
    function togglePower() is
        if (device.isEnabled()) then
            device.disable()
        else
            device.enable()
    function volumeDown() is
        device.setVolume(device.getVolume() - 10)
    function volumeUp() is
        device.setVolume(device.getVolume() + 10)
    function channelDown() is
        device.setChannel(device.getChannel() - 1)
    function channelUp() is
        device.setChannel(device.getChannel() + 1)


// Extended Controller.
class AdvancedRemoteControl extends RemoteControl is
    function mute() is
        device.setVolume(0)


// Somewhere in client code.
tv = new Tv()
remote = new RemoteControl(tv)
remote.togglePower()

radio = new Radio()
remote = new AdvancedRemoteControl(radio)

// The bridge pattern allows separate components with separate interfaces to work together.

class RemoteControl {
	constructor(ac) {
		this.ac = ac;

		this.on = function () {
			this.ac.on();
		};

		this.off = function () {
			this.ac.off();
		};

		this.setTemp = function (temp) {
			this.ac.setTemp(temp);
		};
	}
}

class AC {
	constructor() {
		this.on = function () {
			console.log(`AC turning on...`);
		};

		this.off = function () {
			console.log(`AC turning off...`);
		};

		this.setTemp = function (temp) {
			console.log(`Setting temperature to ${temp}`);
		};
	}
}

let someAC = new AC();
let someRemote = new RemoteControl(someAC);

someRemote.on();
someRemote.setTemp(65);


```
