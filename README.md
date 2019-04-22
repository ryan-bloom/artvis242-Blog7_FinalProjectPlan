# This is my Final Project Blog
===

## Algorithm Art + Motion Tracking
### Conceptual Description
For my final project, I am going to rework one of my previous pieces of art (and arguably my favorite project of the semester).  In this rework, I'm planning on finalizing and making this work more professional and complete than before, while implementing some new interactive concepts as well.  This final project does not really have a deeper meaning other than trying to provide users with a captivating visualization that can entertain and occupy them, allowing them to interact and impact the piece in unique and fun ways.  I hope that users come away from this piece with a new view of what constitutes artwork and how one could interact with a piece of art as well.

### Interaction Description
The artwork that I'm planning on revamping is my Algorithm Artwork, one in which there are several small bouncers flying around the screen leaving a trail behind them, with one stationary center piece in the middle.  As of right now, users can interact with this piece using the keyboard to add and remove bouncers as well as change the colors of the bouncers and the center piece.  Users can also use the mouse to change the location of newly created bouncers, and they can use the control bottons in the top right of the screen to change shapes, color, and toggle bouncer collision detections.  

Through my rework, I am going to focus on changing and adding an additional way in which users can interact with this piece.  Previously, the center object was stationary, however, after further developing this piece, I plan on allowing users to control the location of this center object using their body.  I believe that I'm going to introduce two center pieces into the scene, one that is controlled by the user's left shoulder's location, while the other is controlled by his/her right shoulder's location.  This additional interactivity will fully engage the user and provide for even more control over the piece.  Users can even turn this into a game, using their bodies to try to avoid bouncers that are flying across the screen.  

This work is intended to be set up on a larger computer screen (or television screen) where the users can back up and have room to move their bodies around in order to control the center objects.  The intended audience is really anyone who enjoys interacting with works of art and seeing unique and abstract moving images.  Finally, I believe that the motion tracking interaction will further captivate users and allow them to become even more involved in this work of art as opposed to simply setting a color or shape and standing back and watching, letting the art do its own thing.  

### Extension
As described above, I am planning on extending my algorithm art work from earlier in the semetser.  I plan on finalizing this piece by adding various scenes to it prior to the final work, in order to explain to the user what the piece is an all of the potential interactions that one can have with the piece.  This will set the stage and introduce users to the concept behind the piece as well as allow users to get the full experience, playing with and testing all their potential interactions.  I also plan on removing some functionality in order to reduce the emphasis on the keyboard for controls.  I want to reduce this use becuase I plan on having users stand away from the computer in order to control the 2 center dots with the motion of their hands (and therefore users will ikely not be within reach of the keyboard).  For example, I am going to remove the swapping of shapes from ellipses to rectangles as well as the color selector that requires the use of the mouse.  I believe that this is a fairly simple interaction that does not add too much to the piece, and removing this will allow users to focus more on their body's interaction with the work as opposed to the key input's interactions. Instead of allowing users to select specific colors, I will use their body's location to change the colors of the center objects so that they still have control over the colors of the piece, but their control is shifted from the mouse to their movement and location on the screen.

### Sketch
Here's a sketch of my project plan:
![Ryan Bloom](images/final_sketch_plan_rotate.jpg?raw=true "Ryan Bloom")

### Technical Details
In this work, I am using several libraries.  First, I am using p5.js and p5.collide2d.js as well.  The first of these two libraries is used to draw all of the shapes and movements seen on the canvas, as we have been doing all semester long.  The second libarary, p5.collide2d.js is used to detect collisions between the bouncers moving around the outside of the screen.  Another library that I am using, and a new one that I have never used before, is ml5.js.  This library is the one that allows the use of the computer's webcam to track user's motions.  I connect the user's hand locations to the locations of the center dots.  I am going to host this work on my github account (and a link to the final piece is located at the bottom of this post).

In my code, I first set up two explanation scenes that the users must click through prior to reaching the final piece.  I use scene managers in order to do this, and believe that it gives the piece a more complete and finalized feel to it while allows me to explain the piece's functionalities to the users at the same time.  In the setup method of my code's sketch.js file, I set up and initialize the video camera, and then track the poses later on.  This is seen in the code snippet below:

```java
function setup(){
    video = createCapture(VIDEO);
    video.size(width, height);

    // Create a new poseNet method with a single detection
    poseNet = ml5.poseNet(video);

    // This sets up an event that fills the global variable "poses"
    // with an array every time new poses are detected
    poseNet.on('pose', function (results) {
        poses = results;
    });
    // Hide the video element, and just show the canvas
    video.hide();
  } 
```

I only utilize 2 of the positions that are tracked, right wrist and left wrist, because I am only providing the user with 2 center pieces to track.  This tracking is done in the sketch.java file as well in the code chunck below:

```java 
// A function to draw ellipses over the detected keypoints
function drawKeypoints()  {
  //print(poses.length);
  // Loop through all the poses detected
  for (let i = 0; i < poses.length; i++) {
    // For each pose detected, loop through all the keypoints
    for (let j = 0; j < poses[i].pose.keypoints.length; j++) {
      // A keypoint is an object describing a body part (like rightArm or leftShoulder)
      let keypoint = poses[i].pose.keypoints[j];
  
      rightshoulder_point = poses[0].pose.keypoints[6]
      leftshoulder_point = poses[0].pose.keypoints[5]

      imageMode(CENTER)
      // Only draw an ellipse is the pose probability is bigger than 0.2
      if (keypoint.score > 0.2) {
        //print("HERE MOVING CENTER");
        myCenter1.centerMove(leftshoulder_point.position['x'], leftshoulder_point.position['y']);
        myCenter2.centerMove(rightshoulder_point.position['x'], rightshoulder_point.position['y']);
        
      }
    }
  }
}
```

Finally, I added a centerMove() method to my Center() class which takes in 2 parameters, an x and a y value.  These 2 parameters are the locations of the user's wrists tracked by the video camera.  Then, this method relocates the center object to the location given by the parameters, effectively attaching the object to the user's hand movements.  

[Here's a link to the pice](https://github.com/ryanAB702/MotionTrackingFinalProject)

