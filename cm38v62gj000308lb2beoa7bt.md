---
title: "Game Development"
datePublished: Fri Nov 08 2024 15:00:37 GMT+0000 (Coordinated Universal Time)
cuid: cm38v62gj000308lb2beoa7bt
slug: game-development
tags: unity, csharp, game-development, games, unity3d, unity-engine

---

### Introduction

I’m excited to share my experience of learning game development—a detour from my usual focus on data structures and algorithms. Working with **Unity** and **C#** is a blend of coding and creativity that offers a unique kind of challenge. While my primary focus is elsewhere, diving into Unity’s dynamic environment has taught me so much about design, game logic, and more. This post is all about my initial steps in game development, and in future posts, I’ll show you the game I’ve built and take you behind the scenes!

### Setting Up: Tools and Language

To get started, I’ve been using **Unity** as the game engine, which is popular for both 2D and 3D games. It offers powerful features for developers and artists alike, and I’m coding in **C#** using **JetBrains Rider** for script editing. C# integrates seamlessly with Unity, which makes for smooth and powerful coding interactions, especially for beginners and experts alike.

### Prefabs: Reusable Game Models

In Unity, game models are reusable assets called **prefabs**. Think of them like functions in coding—they allow us to create an object once and then reuse it as often as needed. This makes design efficient, and it also keeps the game files more manageable by avoiding redundancy.

### Scripting with C#: Start and Update Functions

Scripts bring our game elements to life, and Unity’s scripting structure is quite interesting. Each script has two key functions:

1. **Start()**: This function is executed only once when the game object is created or the scene is loaded, perfect for initializing values or setting up conditions at the beginning of the game.
    
2. **Update()**: This function is called at every frame, meaning it runs continuously, making it ideal for handling real-time actions, updates, and ongoing game mechanics.
    

This structure balances one-time setup and continuous game logic, giving full control over the game’s flow and behavior.

```csharp
using UnityEngine;

public class ScriptName : MonoBehaviour
{
    
    void Start()
    {
        // THIS IS THE START FUNCTION
    }
    
    void Update()
    {
        // THIS IS THE UPDATE FUNCTION
    }
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731077307865/985fe58b-819d-4801-b40f-73d47f89e1fe.png align="center")

### Adding Effects: Particles, Sounds, and Lighting

Unity’s assets aren’t limited to just objects or models. There’s a wide variety of **particles** (for effects like smoke or explosions), **audio** (background music or sound effects), and **lighting** (to create depth and mood) that can be added to enhance gameplay. These elements allow for creative freedom to make any scene come alive, giving players an immersive experience.

Lights :-

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731077607286/2c6b31ac-f6fd-4cab-9961-1a1638fffe4d.png align="center")

Particles :-

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731077618947/d1c82e19-926f-4421-8138-44291016e259.png align="center")

### Scenes and Levels

In Unity, each level of a game is a separate **scene**. Organizing the game into scenes allows you to set up specific conditions and elements for each level, creating diverse environments that make gameplay dynamic and engaging.

### Real-Time Control: Serializing Variables

A particularly useful feature I’ve discovered is **serialization**, which allows us to change variable values in real-time. Imagine you’re testing different values for speed or color while the game is running—serialization makes that possible without needing to stop and recompile, speeding up the testing process.

```csharp
[SerializeField] private AudioClip collision;
[SerializeField] private AudioClip success;
[SerializeField] private ParticleSystem collisionParticle;
[SerializeField] private ParticleSystem successParticle;
```

Serialized Panel :-

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731077817322/c042b169-9e4c-4847-aac2-ac59136d3d35.png align="left")

### Visuals with Materials and Asset Store

Unity offers options to give our objects different looks using **materials**—think of these as the colors or textures that bring models to life. For more complex objects, Unity’s **Asset Store** is packed with pre-made prefabs and assets, making it easier to add detail to a project without having to create everything from scratch.

### What’s Next?

I’m currently working on putting all these concepts into a game, and I’m looking forward to sharing that with you. **Stay tuned for my next blog, where I’ll showcase the game, its internal workings, and a peek at the final result!** In the meantime, here’s a preview of what’s been happening so far.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731077005764/579cf13a-d679-4578-ae86-92afb8df0270.png align="center")