
# Basic Concepts


Always refer to [Mirror docs](https://mirror-networking.gitbook.io/docs/guides) first.

This page is just my ramblings and reminders.

### Client, Server

In Mirror `client` and `server` work as you would expect in a typical client/server model. The `server` runs the state logic, while the `clients` receive/send state updates from the `server`.

A network gameobject that is "spawned" by the server onto clients will exist on client(s) **and** the server. Remember that all instances of this object will be running game loops for each network gameobject: 

![[NetworkLocalPlayers.png]]

In this image, notice there are two instances of the Player Object "Player 1" running in "Server" and "Client 1". When writing code for Player 1, you would include both server and client code, but expect that the server portion of the code will be executing on the server, while client code will be executing only on the client.

### Host
In Mirror, the `host` is both a `client` and `server`. 

*My Questions:*
* When running the game as a `host`, you are server and client, do you run one instance of the game or two for each?
* When running as a host in the unity Editor, you will only see server logs, not client logs, why?

[OnStartClient()](https://mirror-networking.gitbook.io/docs/components/networkbehaviour) - Use this + variable `isClientOnly` to execute code only as a client and not host/server.  Example: 

```c#
public override void OnStartClient() { 

	if (!isClientOnly) {
        Debug.Log("I'm a host, don't execute");  
        return;  
    }
    
	Debug.Log("I'm 100% a client, not a server/host.")
}

```


### Spawning

To understand the relationship between **networked** (objects with netid) gameobjects between server and clients, it's important to understand how spawning works in mirror. This page is a good read for that [here](https://mirror-networking.gitbook.io/docs/guides/gameobjects/spawning-gameobjects) and [here](https://mirror-networking.gitbook.io/docs/guides/gameobjects/scene-gameobjects). 


### Player

>Unity associates one player game object for each person playing the game, and routes networking commands to that individual game object. [1](https://mirror-networking.gitbook.io/docs/guides/gameobjects/player-gameobjects)


