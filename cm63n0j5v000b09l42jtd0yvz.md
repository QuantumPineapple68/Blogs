---
title: "Project Connectra"
datePublished: Sun Jan 19 2025 13:12:38 GMT+0000 (Coordinated Universal Time)
cuid: cm63n0j5v000b09l42jtd0yvz
slug: project-connectra
tags: android-app-development, java, projects, android, android-studio, apk

---

### Building Connectra: A Social Media Platform for Skill Exchange

Connectra is more than just an app; it’s a solution to a problem many of us face in today’s world - learning new skills without burning a hole in our pockets. Imagine a world where students and professionals alike can exchange their knowledge and expertise without expensive course fees. With Connectra, I’ve turned this idea into reality. This blog dives deep into my journey of building Connectra, the features it offers, the challenges I overcame, and how it shaped me as a developer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742287735117/e1b8aa83-7456-4181-a251-14990a7362eb.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742287740013/2eae9f8f-5201-4762-ad38-3f1dc89b8fe3.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742287745617/f336665a-a55c-48d4-b353-5ba3cddf8f31.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742287748339/8f86cd4e-a566-4175-a0df-36291f3de763.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742287752599/62c49e24-2586-4c8e-a334-2f3424b5a551.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742287754233/4f716d07-4284-41db-a8ce-9194a1a78062.jpeg align="center")

## **The Vision Behind Connectra**

In an era where learning a new skill can cost thousands or even lakhs of rupees, I wanted to create a platform where people could connect with others to learn and teach skills. The idea is simple yet powerful: find someone proficient in a skill you’re interested in, learn from them, and in return, teach them a skill you excel in. Connectra bridges the gap between learners and teachers, fostering a community of knowledge-sharing and collaboration.

## **My Journey: From Beginner to Creator**

When I started this project four months ago, I knew almost nothing about Android development. The journey from knowing little to building an entire application by myself has been nothing short of transformative. Every feature, every screen, every bug taught me something new. Connectra isn’t just an app; it’s a testament to my growth as a developer.

## **Features of Connectra**

### **1\. Registration and Authentication**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737285687278/22da7eaf-13dd-4804-8ca3-93f68c2ee08f.jpeg align="center")

To begin your journey on Connectra, users must register an account. The authentication is powered by Firebase, ensuring a seamless and secure sign-in experience. Once registered, users can start exploring the app and connecting with others.

### **2\. Home Section: Discover Profiles**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737285752702/4a08e2a4-9b61-4975-9a75-e308e5340ff2.jpeg align="center")

The Home section is where the magic begins. Here, you can find random profiles of people on the platform. Each profile offers detailed information, including:

* **Profile Picture**
    
* **Name and Username**
    
* **Bio**
    
* **Age and Gender**
    
* **Skillsets**
    
* **Certifications in Their Skills**
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737285818601/f4558c84-737e-46c0-b0d9-2e215b095f6f.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737285866073/e4a5d71f-fc43-4fd7-bc12-87aad13ee4e7.jpeg align="center")

#### **Interacting with Profiles**

* **Rating System**: Users can rate each other based on their experience. This helps others decide whom to learn from.
    
* **Messaging**: Directly message users to initiate conversations. Push notifications ensure users never miss a message, even if the app isn’t open.
    
* **Learning Sessions**: After discussions, you can switch to platforms like Meet or Zoom for interactive learning sessions.
    

### **3\. Search Section: Find Skills**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737285920036/429f6c4b-99af-4fa5-b8e9-fdf45ed73bba.jpeg align="center")

The Search section lets users find people by the skills they want to learn. The rating system plays a crucial role here, allowing users to filter and choose among multiple profiles with the same skill set. This ensures that learners connect with the best possible mentors.

### **4\. Schedule Section: Organize Your Learning**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742287820626/8e3043bc-8142-45e5-a6e6-f34c971f0465.jpeg align="center")

This section doubles as a Real-time Collaborative to-do list and calendar app, allowing users to manage their learning schedules effectively.

* **Features**:
    
    * Send Requests and connect to your learning Partner
        
    * Assign tasks for them to specific dates.
        
    * Delete tasks when completed or no longer needed.
        
    * Check-Box to Strikethrough
        
    * Cloud storage powered by Firebase ensures tasks are saved and synchronized across devices in real-time.
        

### **5\. Profile Section: Personalize Your Identity**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737286016265/b1c43f61-311b-45b5-841e-7d0a5bc1bc0b.jpeg align="center")

In the Profile section, users can:

* Upload a profile picture.
    
* Update their skillsets and certifications.
    
* Set a bio to introduce themselves.
    

This section is all about showcasing your unique expertise and personality to others.

## **Technical Details**

### **Technology Stack**

* **Frontend**: XML for designing a clean and intuitive user interface.
    
* **Backend**: Java for robust and efficient app logic.
    
* **Firebase**:
    
    * Authentication
        
    * Realtime Database
        
    * Storage
        

### **Dependencies Used**

* **Glide**: For smooth loading and caching of profile images.
    
* **CircularImageView**: To enhance the visual appeal of profile pictures.
    

## Connectra (Admin Version)

In addition to the Connectra app, I also developed a **dedicated admin application**. This admin app serves as a control center for managing the **real-time database** connected to the primary app. Here's what makes it special:

1. **Real-Time Control**:
    
    * The admin app uses a **ValueEventListener** that continuously monitors a boolean value called `isActive`, stored under the `detonator` branch in the database.
        
    * When `isActive` is set to `true`, the user-facing app behaves normally. However, when switched to `false`, the app triggers a popup (like the one shown in the second image), notifying users that the app is under maintenance - regardless of which page they are on.
        
2. **User Management**:
    
    * The admin app includes a **search feature** to locate specific users by skill or email.
        
    * If a user exhibits inappropriate behavior or violates guidelines, the admin can delete their account entirely with a single tap.
        
3. **Seamless Integration**:
    
    * Both the admin app and Connectra share the same Firebase database, ensuring real-time synchronization and streamlined operations.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1737566012538/76fb8538-cf09-4113-a0c2-fb30ff95d59a.png align="center")

By creating this admin panel, I ensured robust backend control, providing an extra layer of functionality to manage the Connectra platform efficiently.

## **Overcoming Challenges**

### **1\. Firebase Storage Costs**

When Firebase introduced its new policy, storage was no longer free. To minimize expenses, I used my friend’s Firebase project for storage while leveraging my Firebase project for authentication and Realtime Database. I even wrote a [dedicated blog](https://my-learnt-tech-stacks.hashnode.dev/cracking-dual-firebase-setup-to-save-costs) on how I achieved this workaround.

### **2\. Learning Curve**

Starting with minimal knowledge of Android development, every step required learning - from integrating Firebase to managing push notifications. AI tools were instrumental in guiding me through challenges, but hands-on debugging and perseverance played a crucial role.

## **Lessons Learned**

* **Time Management**: Balancing feature development with learning was challenging but rewarding.
    
* **Problem-Solving**: Encountering and resolving technical issues enhanced my analytical skills.
    
* **Self-Reliance**: Building an entire project solo taught me to trust my instincts and abilities.
    

## **Why Connectra Matters**

Connectra isn’t just another app, it’s a platform that democratizes learning. By connecting people based on mutual interests and skills, it eliminates the financial barriers to education. In doing so, it creates a community where knowledge is shared freely, and learning is accessible to everyone.

## **Conclusion**

Building Connectra has been a journey of growth, learning, and innovation. From conceptualizing the idea to implementing features and overcoming challenges, every step has added to my experience as a developer. Connectra stands as proof of what’s possible with determination and a passion for solving real-world problems.

If you’re looking for a platform to exchange skills, Connectra is here for you.