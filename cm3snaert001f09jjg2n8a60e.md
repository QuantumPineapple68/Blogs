---
title: "Continuing Game Development"
datePublished: Fri Nov 22 2024 11:15:26 GMT+0000 (Coordinated Universal Time)
cuid: cm3snaert001f09jjg2n8a60e
slug: continuing-game-development
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1732274071413/f050bb3e-fc70-40e4-828c-cbbcde67ad0c.png
tags: unity, csharp, game-development, games

---

In this second part of our journey in game development, we will build upon the foundational work done in the previous blog. The focus will be on enhancing player controls, introducing visual and audio feedback, implementing collision mechanics, and refining the gameplay loop. This blog will include an extremely detailed explanation of all concepts, supported with code snippets. Additionally, I will share resources of game internals and provide a video link for gameplay reference.

### Adding Rocket Movement Functionality

The core of any rocket-based game lies in its movement mechanics. To simulate realistic rocket behavior, we need to carefully design vertical thrust and rotational control.

#### Vertical Thrust

The rocket's vertical movement is controlled by applying an upward force when the player presses the spacebar.

1. **Input Detection**
    

Unity's `Input.GetKey` method is used to check if a specific key is pressed.  
`if (Input.GetKey(`[`KeyCode.Space`](http://KeyCode.Space)`))`

* [`KeyCode.Space`](http://KeyCode.Space) refers to the spacebar key on the keyboard.
    
* This method continuously checks for key presses during gameplay.
    

2. **Applying Force**
    

When the spacebar is pressed, an upward force is applied to the rocket's Rigidbody component. The Rigidbody handles the physics of the rocket, such as velocity and collisions.  
`rb.AddRelativeForce(Vector3.up (thrust Time.deltaTime));`

* `AddRelativeForce`: Adds a force relative to the rocket's orientation. This ensures the thrust aligns with the rocket's direction even when it rotates.
    
* `Vector3.up`: Specifies the direction of the thrust (upward in local space).
    
* `thrust`: A serialized variable representing the magnitude of the force.
    
* `Time.deltaTime`: Normalizes the movement to account for different frame rates, making the game consistent across devices.
    

#### Rotational Control

To simulate realistic steering, players can rotate the rocket using the &lt; and &gt; keys.

1. **Preventing Unwanted Rotation**
    

Before applying manual rotation, we must prevent the Rigidbody from being affected by external forces, such as collisions. `rb.freezeRotation = true;`

* This ensures that the player's inputs directly control the rotation without interference from physics forces.
    

2. **Manual Rotation**
    

Rotation is applied by adjusting the transform property of the rocket.

`transform.Rotate(Vector3.forward (rotationSpeed Time.deltaTime));`

* `Vector3.forward`: Defines the axis of rotation in local space.
    
* `rotationSpeed`: A serialized variable controlling how quickly the rocket rotates.
    

This method allows fine control over rotation speed and direction based on player input.

### Adding Visual and Audio Effects

Visual and audio feedback significantly enhance the player's experience. Here, we will add sound effects for the rocket's engine and particle effects to simulate thrust.

#### Adding Audio Effects

1. **Configuring the Audio Source**
    

Attach an AudioSource component to the rocket in Unity. This component will handle audio playback.

2. **Playing Audio**
    

When the player presses the spacebar, the rocket's engine sound should start playing. `audioSource.PlayOneShot(mainEngine);`

* `PlayOneShot`: Plays the specified audio clip without interrupting other sounds.
    
* `mainEngine`: A serialized AudioClip assigned through the Unity Inspector.
    

3. **Stopping Audio**
    

When the spacebar is released, the engine sound should stop.`audioSource.Stop();` This ensures that the audio feedback accurately reflects the player's actions.

#### Adding Particle Effects

1. **Configuring Particle Systems**
    

Attach ParticleSystem components to the rocket for thrust visualization. Assign these systems to serialized fields for easy access in code.

`[SerializeField] private ParticleSystem mainThrust, rightThrust, leftThrust;`

2. **Controlling Particles**
    

Start the thrust particles when the player applies thrust: [`mainThrust.Play`](http://mainThrust.Play)`();`

Stop the particles when thrust is no longer applied: `mainThrust.Stop();`

These effects visually communicate the rocket's actions to the player.

Here is the Complete Movement Script :-

```csharp
using System;
using UnityEngine;
using UnityEngine.UI;

public class Movement : MonoBehaviour
{
    private InputSystem controls;
    private Rigidbody rb;
    private AudioSource au;
    private float thrust = 150f;
    private float roatationSpeed = 200f;
    [SerializeField] private AudioClip mainEngine;
    [SerializeField] private ParticleSystem mainThrust;
    [SerializeField] private ParticleSystem rightThrust;
    [SerializeField] private ParticleSystem leftThrust;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
        au = GetComponent<AudioSource>();
    }
    
    void Update()
    {
            ProcessMove();
    }
    
    void ProcessMove()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            rb.AddRelativeForce(Vector3.up * (thrust * Time.deltaTime));
            if (!au.isPlaying)
            {
                au.PlayOneShot(mainEngine);
                mainThrust.Play();
            }
        }
        else
        {
            au.Stop();
            mainThrust.Stop();
        }

        if (Input.GetKey(KeyCode.RightArrow))
        {
            rb.freezeRotation = true;
            transform.Rotate(Vector3.forward * (roatationSpeed * Time.deltaTime));
            if (!rightThrust.isPlaying)
            {
                leftThrust.Play();
            }
            rb.freezeRotation = false;
        }
        else if (Input.GetKey(KeyCode.LeftArrow))
        {
            rb.freezeRotation = true;
            transform.Rotate(Vector3.back * (roatationSpeed * Time.deltaTime));
            if (!rightThrust.isPlaying)
            {
                rightThrust.Play();
            }
            rb.freezeRotation = false;
        }
        else
        {
            leftThrust.Stop();
            rightThrust.Stop();
        }
    }
    
}
```

### Collision Handling

Collisions determine the outcome of the rocket's interactions with its environment.

#### Crash Scenario

When the rocket collides with a harmful object:

1. Play a crash sound and particle effect.
    
2. Disable player controls.
    
3. Reload the level after a short delay.
    

```csharp
void CrashSequence()
{
    collisionParticle.Play();
    audioSource.Stop();
    audioSource.PlayOneShot(crashSound);
    GetComponent<Movement>().enabled = false;
    Invoke("ReloadLevel", 2f);
}
```

* [`collisionParticle.Play`](http://collisionParticle.Play)`()`: Triggers the crash visual effect.
    
* `audioSource.PlayOneShot(crashSound)`: Plays the crash sound.
    
* `Invoke`: Calls the `ReloadLevel` method after a 2-second delay.
    

#### Success Scenario

When the rocket reaches the finish platform:

1. Play a success sound.
    
2. Transition to the next level.
    

```csharp
void SuccessSequence()
{
    successParticle.Play();
    audioSource.Stop();
    audioSource.PlayOneShot(successSound);
    Invoke("LoadNextLevel", 2f);
}
```

* `LoadNextLevel` loads the subsequent scene.
    

#### Collision Detection

The `OnCollisionEnter` method determines the type of collision based on tags assigned to objects.

```csharp
void OnCollisionEnter(Collision other)
{
    switch (other.gameObject.tag)
    {
        case "Friendly":
            break;
        case "Finish":
            SuccessSequence();
            break;
        default:
            CrashSequence();
            break;
    }
}
```

* Tags such as "Friendly" and "Finish" are defined in Unity's Tag Manager.
    
* The `default` case handles untagged or harmful objects.
    

### Oscillating Obstacles

To increase difficulty, introduce obstacles that oscillate between two points.

```csharp
void Update()
{
    if (period <= Mathf.Epsilon) return;

    float cycles = Time.time / period;
    const float tau = Mathf.PI * 2;
    float sinWave = Mathf.Sin(cycles * tau);
    movementFactor = (sinWave + 1f) / 2;

    Vector3 offset = movementFactor * movementVector;
    transform.position = startingPosition + offset;
}
```

* `Mathf.Sin`: Generates a sine wave for smooth oscillation.
    
* `movementFactor`: Maps sine wave values to a range between 0 and 1.
    
* `offset`: Calculates the object's position based on the sine wave.
    

### **Quit Functionality**

To allow players to exit the game:

```csharp
if (Input.GetKey(KeyCode.Escape))
{
   Application.Quit();
}
```

### **Look At The Actual game**

1. **Peek into Game Internals:**
    

%[https://youtu.be/lFdfSh85VNU] 

1. **Gameplay Demonstration:**
    

%[https://youtu.be/ekrIvQi0pK4] 

### **<mark>Play the Game</mark>**

* **GitHub Repository:** [RocketGame Repo](https://github.com/QuantumPineapple68/Rocket)
    

### Certification

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732273005013/1945af2a-875f-4676-9810-4b6945deb51b.jpeg align="center")

### **Conclusion**

With these features, we’ve added interactivity, dynamics, and a touch of realism to our game. From movement mechanics to collision handling and visual effects, every detail enhances the player's experience.

This marks the end of my exploration into game development, a journey I embarked on as part of my continuous learning hobby. It has been an exciting and fulfilling experience, helping me expand my knowledge of physics-based mechanics, visual feedback systems, and game design principles.

However, as my learning journey evolves, I will now focus on a different area of technology that aligns with my career aspirations—**Android development**. My upcoming blogs will dive into the fascinating world of building mobile applications, exploring Android Studio, Kotlin, Firebase, and more.

I hope this series on game development has been insightful and inspiring for you. Stay tuned for my future blogs, where I aim to bring the same level of detail and passion to Android development. As always, feel free to share your thoughts or questions. Let’s continue learning and building together!