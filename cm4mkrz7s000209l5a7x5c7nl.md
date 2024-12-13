---
title: "Cracking Dual Firebase Setup to Save Costs"
datePublished: Fri Dec 13 2024 09:58:13 GMT+0000 (Coordinated Universal Time)
cuid: cm4mkrz7s000209l5a7x5c7nl
slug: cracking-dual-firebase-setup-to-save-costs
tags: app-development, java, firebase, android, android-studio

---

Firebase is a powerful platform that simplifies app development by offering features like Authentication, Realtime Database, Firestore, and Storage. However, when Firebase Storage became a premium feature, I encountered a significant challenge while working on my app, *Connectra*. It is going to be a skill exchange social media platform which we’ll talk about in upcoming blogs. My app requires all these Firebase services, and transitioning to a paid plan for Storage was not feasible at this stage as I’m a student.

After some brainstorming and exploration, I devised a creative solution: utilizing the free storage available in our older Firebase project, *GenZcrop*, while continuing to use my Firebase project, *Connectra*, for Authentication, Realtime Database, and *Firestore*. In this blog, I’ll share how I implemented this dual Firebase setup and managed to solve the problem efficiently.

## The Problem

Firebase’s pricing model had made Storage a premium feature, affecting my ability to store user-uploaded images in my app without incurring additional costs. Our older Firebase project, *GenZcrop*, still had free storage since it was created before the pricing change. This led me to the idea of leveraging *GenZcrop*'s free Storage while keeping other services in my own Firebase project, *Connectra*.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734037452502/e04f84cc-1c3b-41b0-a8c1-b1d5275042eb.png align="center")

## The Solution: Dual Firebase Integration

To achieve this, I needed to:

1. Use *Connectra* for Authentication, Realtime Database, and Firestore.
    
2. Use *GenZcrop* for Storage.
    
3. Implement a dual Firebase setup in my app.
    

Here’s how I implemented the solution.

### Step 1: Setting Up the Application Class

The `Application` class allows me to initialize Firebase configurations when the app starts. By defining multiple Firebase projects here, I could seamlessly access both *Connectra* and *GenZcrop*.

Here’s the code :

```java
package com.example.connectra;

import android.app.Application;
import android.os.Handler;
import android.os.Looper;
import android.util.Log;

import com.google.firebase.FirebaseApp;
import com.google.firebase.FirebaseOptions;
import com.google.firebase.storage.FirebaseStorage;

public class MyApplication extends Application {
    private static final String TAG = "FirebaseInit";

    @Override
    public void onCreate() {
        super.onCreate();

        // Initializing the primary Firebase app (default project)
        FirebaseApp.initializeApp(this);

        for (FirebaseApp app : FirebaseApp.getApps(this)) {
            Log.d(TAG, "Available FirebaseApp: " + app.getName());
        }

        // Delay initialization of the secondary Firebase app slightly
        new Handler(Looper.getMainLooper()).postDelayed(this::initializeSecondaryApp, 200);
    }

    private void initializeSecondaryApp() {
        // Setting up FirebaseOptions for the secondary project
        FirebaseOptions secondaryOptions = new FirebaseOptions.Builder()
                .setProjectId("") // Project ID
                .setApplicationId("") // Application ID
                .setApiKey("") // API key
                .setStorageBucket("")
                .build();

        // Initialize secondary Firebase app with name "secondary"
        FirebaseApp secondaryApp = FirebaseApp.initializeApp(this, secondaryOptions, "secondary");
        Log.d(TAG, "Secondary FirebaseApp initialized successfully.");

        FirebaseStorage secondaryStorage = FirebaseStorage.getInstance(secondaryApp);
        Log.d(TAG, "Successfully accessed Firebase Storage from secondary FirebaseApp.");
    }
}
```

This setup ensures that both Firebase projects (*Connectra* and *GenZcrop*) are initialized when the app starts.

### Step 2: Using the Secondary Firebase Project in Code

I needed to use *GenZcrop*'s Storage for user-uploaded images. In the *ProfileFragment*, I configured `FirebaseStorage` to access the secondary app.

Here’s the Implementation:

#### ***ProfileFragment.java***

```java
package com.example.connectra.Fragments;

import com.google.firebase.FirebaseApp;
import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.storage.FirebaseStorage;
import com.google.firebase.storage.StorageReference;
import com.google.firebase.storage.UploadTask;
import com.google.firebase.database.FirebaseDatabase;
import com.google.firebase.firestore.FirebaseFirestore;

public class ProfileFragment extends Fragment {

    // ACCESSING SECONDARY DATABASE FOR STORAGE
    FirebaseStorage secondaryStorage = FirebaseStorage.getInstance(FirebaseApp.getInstance("secondary"));
    
    
    /*
    ACCESSING PRIMARY DATABASE FOR AUTH AND FIRESTORE
    FirebaseFirestore mRootRef;
    FirebaseAuth auth;
    
    (Store instances Inside onCreate)
    auth = FirebaseAuth.getInstance();
    mRootRef= FirebaseFirestore.getInstance();
    */


    private void uploadImage() {
        ProgressDialog pd = new ProgressDialog(getActivity());
        pd.setMessage("Uploading...");
        pd.show();

        if (imageUri != null){
            StorageReference fireRef = secondaryStorage.getReference().child("connectra").child(System.currentTimeMillis() + "." + getFileExtension(imageUri));

            fireRef.putFile(imageUri).addOnCompleteListener(new OnCompleteListener<UploadTask.TaskSnapshot>() {
    @Override
    public void onComplete(@NonNull Task<UploadTask.TaskSnapshot> task) {
        if (task.isSuccessful()) {
            fireRef.getDownloadUrl().addOnSuccessListener(new OnSuccessListener<Uri>() {
                @Override
                public void onSuccess(Uri uri) {
                    String url = uri.toString();
                    Log.d("DownloadUrl", url);
                    pd.dismiss();
                    Toast.makeText(getActivity(), "Image Upload Successful!", Toast.LENGTH_SHORT).show();
                }
            });
        } else {
            pd.dismiss();
            Toast.makeText(getActivity(), "Image Upload Failed: " + task.getException().getMessage(), Toast.LENGTH_SHORT).show();
        }
    }
});

        }
    }
}
```

This function implementation ensures that user-uploaded images are stored in *GenZcrop*'s Firebase Storage while the rest of the app functionalities use *Connectra*.

### Reason for Using `Handler` with a Delay

1. **Avoiding Race Conditions**  
    Firebase services might need some time to fully initialize the default `FirebaseApp` when the app starts. By adding a slight delay, the code ensures that the primary app's initialization is completed before the secondary app is initialized. This is especially relevant in apps with complex configurations or large initializations.
    
2. **Ensuring Main Thread Execution**  
    Firebase initialization and related operations are thread-sensitive. By explicitly posting the task to the main thread using `Handler(Looper.getMainLooper())`, it ensures that the initialization happens safely on the main thread, where Firebase APIs are designed to work without issues.
    
3. **Allowing Dependencies to Load**  
    If there are asynchronous processes or dependencies that the secondary Firebase app relies on (e.g., loading `google-services.json` or resolving Firebase configurations), the delay provides time for these processes to complete.
    
4. **Reducing Startup Jitters**  
    In some cases, initializing two Firebase apps simultaneously during the app's startup can lead to performance issues or conflicts. Staggering the initialization with a delay can help distribute the workload and avoid these problems.
    

While the delay isn't always mandatory, it acts as an additional safeguard to ensure the apps initialize in the correct order without timing-related issues.

## **Challenges**

Implementing a dual Firebase setup presented several hurdles:

1. **Incorrect API Keys:**  
    During the secondary Firebase app initialization, mismatched API keys caused repeated failures. Double-checking the Firebase Console settings and verifying every parameter eventually resolved this issue.
    
2. **Initialization Order:**  
    The FirebaseApp instances were not recognized due to improper initialization sequences. This was fixed by reordering the initialization process and ensuring all Firebase projects were initialized early during the app startup.
    
3. **Storage Bucket Access:**  
    Managing access to the secondary project’s storage required careful configuration to ensure the app didn’t face authorization issues.
    

## **Learnings**

Through this process, I gained several valuable insights:

1. **Configuration Verification:**  
    It’s crucial to systematically verify all configurations when dealing with multiple Firebase projects to avoid unnecessary debugging.
    
2. **Debugging Complex Setups:**  
    Debugging dual setups required a methodical approach. Breaking down the problem into smaller parts (e.g., focusing on API key verification first) helped resolve issues faster.
    
3. **Cost-Effective Solutions:**  
    This project taught me the value of exploring creative solutions to reduce costs without compromising functionality—a critical skill for student developers and startups.
    

## Conclusion

Here’s what I learned and achieved:

1. By leveraging multiple Firebase projects, I managed to reduce costs while maintaining functionality.
    
2. Debugging and verifying configurations are critical in complex setups.
    

If you face similar challenges, consider using a dual Firebase setup—it might just save your project!

### A Note of Thanks

A big shoutout to my friend **Hariom Kankatti** for being there throughout this process. Your guidance and suggestions were super helpful in cracking the dual Firebase setup. Couldn't have done it this smoothly without your support—thanks, buddy!