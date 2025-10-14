---
title: "Multiplayer Game Engine"
date: 2025-01-14
draft: false
summary: "A multiplayer game engine with a C++ server and Java client that automates object replication and serialisation."
tags: ["C++", "SDL", "Networking", "Test-Driven Development", "MessagePack", "GoogleTest"]
params:
    links:
        - GitHub: "https://github.com/harryjduke/multiplayer-game_server"
---

## Goal

The aim of this project was to create a reusable multiplayer game engine server in C++ that could automatically replicate game objects. A Java client was used to present an additional challenge. I designed the replication API with the goal of keeping it as simple as possible with minimal boilerplate. So that the engine could be reused to create different games I was careful to maintain strict separation between the engine and game logic. Additionally I wanted to develop this project using the test-driven development methodology to help maintain a good architecture, keep my code robust and simplify testing and debugging.

## Outcome

I created a functional multiplayer game engine that automatically replicated objects with an API that requires only a few lines of code per replicated object. The following example replicates the Boolean `state`:

```cpp
class MyReplicatedObject : public Replicatable<MyReplicatedObject> {
public:
    static constexpr typeId{"MyReplicatedObject"};
	
    MyReplicatedObject(NetworkEngine& engine, bool initialState)
        : Replicatable<MyReplicatedObject>(engine)
        , state_(initialState) {}
	
    MSGPACK_DEFINE(state_);
	
private:
    bool state_;
};
```

I achieved this by using the [CRTP (curiously repeating template pattern)](https://en.cppreference.com/w/cpp/language/crtp.html) to automatically register and deregister the created object. Although this interface is adequately minimal, a custom macro that wraps the MessagePack one would help to hide the implementation details and possibly reduce boilerplate further.

Through my research I found that a custom protocol built on top of UDP would be ideal for my game, however, building a protocol like this from scratch would be time consuming and complex. Libraries that provided UDP-based protocols for games exist such as [ENet](http://enet.bespin.org/) and [GameNetworkingSockets](https://github.com/ValveSoftware/GameNetworkingSockets) but they do not have ports for Java. Using TCP would not be ideal should the engine be used for a larger project. This left me with three options:

 1. Use TCP and possibly have to completely rewrite the protocol in the future
 2. Use a UDP-based library and write my own JNI wrapper
 3. Write my own UDP-based protocol

I also had a fourth option: write a general interface that follows the [strategy pattern](https://refactoring.guru/design-patterns/strategy) so that I can create multiple implementations, starting with the simplest: TCP. This option also meant that I was following the port-adaptor architecture and keeping my code decoupled which helped with testing.

I used [GoogleTest](https://google.github.io/googletest/) to write my unit tests for test-driven development and I found it very beneficial in guiding my architecture design and ensuring that my code worked as expected.