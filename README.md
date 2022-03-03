<h2>Embedded systems project</h2>

<h3>Introduction</h3>
<p>For this project, I used STM32F769i disocovery board and HC-SR04 ultrasonic distance sensor.<p>
<p>The goal is to set up the board using the sensor to read the distance of an object from the sensor itself and display it on the LCD.</p>
    
https://user-images.githubusercontent.com/60989050/156579203-58848413-cfea-4c9f-9e16-199b8d569d2b.mp4


<h3>How-To</h3>
<p>I am using 1 semaphore, 1 message queue and 3 threads (button, read, display).<p>
<p>At the start, on the screen "Press the button to start!!" is displayed. We press the button to start. Distance is displayed on the LCD screen and I we want to save our current distance, we press the button again. Now if we move closer than the saved distance, red light is activated, and if we move further away, green light is activated. And finally, I added a "hidden feature", when you press and hold the button for more than 2 seconds, the screen is cleared and rain is displayed. To go back, you just press the button again.</p>

<h4>Button thread</h4>
<p>This thread handles all of the button functionalities for the project. At the beginning to start, the button has to be pressed to release the semaphore so that the other threads can start executing. If it is started and the button is pressed again, it saves the current distance and displays it on the LCD screen. And if the button is pressed for longer than 2 seconds, we go in "hidden feature" mode. This mode is just visualization, a black background with dark blue rain falling infinitely. We excape this mode by pressing the button again.

<h4>Read distance thread</h4>
<p>This thread is blocked untill the button is pressed. After pressing the button, the thread begins to read distances. 
First, I reset the TRIG port (low state) and wait 2 microseconds. After that, I set the TRIG port (high state), wait 10 microseconds, and reset it again. Now I wait for ECHO port to go to high state, and when it does I count how long is in high state. Then I use the following formula to calculate the distance in cm <b>(end_time - start_time) * 0.034 / 2</b>, and send it to the message queue.</p>

<h4>Display distance thread</h4>
<p>This thread waits for information from the message queue. When information is sent, it displays the distnace on the LCD screen and if we have a "saved distance", it checks if the current distance is lower or higher then the saved distance. If it is lower, red light is turned on, if it is higher, green light is turned on.</p>


![vgrs](https://user-images.githubusercontent.com/60989050/156603829-bfed0ebd-9f39-4528-89c8-2b374a7bd2e8.jpg)
