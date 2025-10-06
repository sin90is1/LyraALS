# **Advanced Locomotion System (ALS) Study in Unreal Engine**

This project is a practical implementation and study of the core concepts from the 'Advanced Locomotion System' course by Unreal Magic. The goal was to build a sophisticated, AAA-quality character locomotion system in Unreal Engine, focusing on performance, modularity, and realism.

A more detailed visual summary of these concepts can be found in my LinkedIn post:

View Project Summary on [LinkedIn](https://www.linkedin.com/posts/sina-ghavami-adel-b1b92a1b2_some-concepts-in-advanced-locomotion-system-activity-7357730959703560192-vDS9?utm_source=share&utm_medium=member_desktop&rcm=ACoAADGdd8cBudtM0EOoOoqx5HHzuUlMiM9gU4A)

## **Key Concepts Implemented**

### **1\. Thread-Safe Animation with Property Access**

A key design goal of ALS is to decouple animation logic from the game thread to improve performance.

* This project avoids old communication methods like casting in favor of **Property Access**, which is a thread-safe and optimized way for the Animation Blueprint to get data like character velocity from the Game Thread. This prevents blocking the main thread and ensures smooth animation updates.

### **2\. Modular Architecture with Animation Layers**

To avoid a single, monolithic Animation Blueprint, this system uses a modular architecture built on **Animation Layers**.

* An **Animation Layer Interface (ALI)** defines a shared structure for different animation states (e.g., unarmed, pistol, rifle).  
* A main ABP\_Base blueprint handles the core AnimGraph and state logic, while ABP\_Layers implement the interface and child blueprints like ABP\_Pistol override only the animation assets.  
* At runtime, the correct animation layer is attached to the character's mesh using the Link Anim Class Layers node, allowing for dynamic and organized state management.

### **3\. Dynamic Control with Sequence Player & Evaluator**

Instead of relying solely on state machines, this project uses the powerful **Sequence Player** and **Sequence Evaluator** nodes for runtime animation control.

* **Sequence Player**: Allows for dynamically swapping animations by binding functions to its update events, making it highly flexible.  
* **Sequence Evaluator**: Provides precise, frame-by-frame control over an animation's playback.

### **4\. Advanced Techniques for Realism**

Several advanced features were implemented to eliminate common animation problems like foot sliding and to enhance visual polish.

* **Animation Warping**: The Animation Warping plugin was used to dynamically adjust animations.  
  * **Stride Warping** adjusts the length of footsteps to match the character's actual movement speed.  
  * **Orientation Warping** rotates existing animations to create smooth movement in all directions without needing separate assets for every angle.  
  * **Distance Matching**: To create a natural-looking stop, this system predicts the character's ground movement stop location and uses a Distance Match to Target node to sync the animation's motion curve with the character's deceleration.  
* **Sync Groups**: When blending between two animations (like a run cycle and a pivot), Sync Groups are used to align the foot positions of both animations, ensuring a seamless and slide-free transition.  
* **Hand IK Retargeting**: For holding weapons, a combination of **Virtual Bones**, the **Two Bone IK** node, and the **Hand IK Retargeting** node ensures the character's hands stay correctly placed on the weapon, even when retargeting animations to skeletons with different proportions.

