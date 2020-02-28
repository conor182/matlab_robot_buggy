Worksheet 4 - Input from the real world using the camera
========================================================

Worksheet 3
Wile group work is fun, it’s also fun to do your own thing.  So for this worksheet, all parts are to be done individually, except Question 0 which you will have to do in a group. The usual plagiarism rules will apply for all but question 0. As ever, the demonstrators in the lab are here to help you.  For all questions but number 0, you can use matlab on the computers in C19/C20, you don’t need the buggy.
Question 0: You will have noticed that your buggy has a camera.  In this worksheet we will be using the camera to detect objects and ultimately, to help the buggy avoid objects (although that will be in the next lab).
Step 1: Make a new octave (MATLAB) script, add the addpath(‘/home/pi/lib’) command as usual. Then add the command camera() to the script. Save and run the script and you will see that an image has been written to /home/pi/image.jpg .  To open this image, click on the folder icon in the top left of the screen (it is the third icon from the left, next to the raspberry).  If you now double click on image.jpg you will see a picture of what the camera was looking at.
Step 2: Now, put the buggy on the floor, with all the wires still plugged in. Make sure the buggy has a clear view of the floor, with no wires, cables, bags books etc.. in the way. Then take another image using  your script.  Rename the image by right clicking on it, and call it ‘floor.jpg’.
Step 3: Next lab session your buggy will have to navigate an obstetrical course, see figure 1.  Your buggy will have to find the exit (marked by a white piece of paper), avoid being eaten by the rabbit, which is lives in an area surrounded by yellow tape, avoid bumping into the walls, and ideally find the patch of smarties before exiting the course.  These tasks can be accomplished by using a combination of the camera and the ultrasonic sensor.
However to do this task we need sample images of the yellow tape, smarties and the white piece of paper.  Ask Rod (or a demonstrator ) for these objects.  Take pictures of each of the objects with the buggy camera naming them yellow_tape.jpg, smarties.jpg and exit.jpg.  [Hint: I would not eat the smarties… they have spent quite a long time kicking around my office!]
Step 4: Insert a USB stick into the PI, and copy off the images to a USB stick.  If you  can’t work out how to do this, ask a demonstrator.  We will be using these images in the rest of the questions.
The rest of the work sheet will be individual work, not group work, done on the PCs in the computer room, on your laptop or at home.  (Usual plagiarism rules apply… sorry) When you finished the programming your code, we will copy your files back to the PI, and test them.

Question 1: Start up MATLAB on your PC.  We are now going to write some simple code to do some image recognition.  The image recognition will be based on colour detection.  In a new script called q1.m use the imread and imshow command to read in the image and display the image.  What do you notice about the image?  Use the manual to look up what the flipud command does.  Have a go at using it.  What does it do to the image?  Save this script as q1.m

Question 2: We are now going to write a function, to calculate the average colour of a image.


Three outputs:
r: the average value of the intensity red pixels within the image.
g: the average value of the intensity green pixels within the image.
b: the average value of the intensity blue pixels within the image.

The function will be called like this:
[r, g, b]=get_color(a)
 We will use the values of r, g and b to identify the object in front of the camera.  For example the blue carpet may have lots of blue in it, so if the function returns the value of 0,0,200 we could guess we are looking at the carpet.  If the function returned a value with lots of red in it i.e. 200,0,0 we could guess that we were looking at something red, like an apple.  This type of image recognition is good for object detection and identifying green apples from oranges, but not for complex tasks such as number plate recognition.
Step 1: Make a new script called get_color.m (note US spelling, as it is common in computing), and define the function get_color within the script.  If you can’t remember how to make functions (or never knew because you missed the lecture….. :) ), go and revise this from the lecture material.  If you get stuck with this ask a demonstrator.  Just for now and to test your function, make your function return the values of 50,50,50.
Step 2: In the command window, load your image into variable a, using the imread command.
Step 3: Test your function by typing [r,g,b]=get_color(a,200,200,20,20) into the command line, this should return 50,50,50, if you set up your function correctly.
Step 4: Inside the function get_color, define two nested for loops, one which counts using the variable x and one which counts using the variable y. Make the x loop count from x_start to x_start+width, and make the y loop count from y_start to y_start+height. To test if this works, use the sprintf command to print out the values of x and y.  When you execute get_color(a,200,200,20,20), again, the function should print out all the x,y values over which the loop counnts.
Step 5: If you have done the example sheet on image processing, you will know all about how images are stored as red green and blue values in a matrix. [This is the example sheet with the cat with different colored eyes.] If you have not done this example sheet, I suggest you go and revise it, it will explain basic image processing to you.  In there I explain about how colored images are stored in detail.  I will now quickly revise the topic….  Black and white images are stored in a 2D matrix, a value of 255 stored in the matrix, represents white pixel, while a value of 0 represents a black pixel, values in between represent different grey colours.  Coloured images, work in the same way, except instead of using a 2D array, they use a 3D array, where the last dimension selects the colour.  If for example you had a coloured image stored in the variable ‘a’ and wanted to access the red pixel at 50,50 you would write a(50,50,1), if you wanted to access the green value stored in the same location you would type a(50,50,2), and finally the blue value would be accessed by typing, a(50,50,3).  Edit your sprtinf statement to display the red, green and blue values of the pixels at every x,y value your nested for loops count over.
Step 6: We are now going to edit the function so that it sums, all the values of the red pixels in the box defined, by the inputs to the function.  At the top of your function define the counter r=0.0, in the loop add the line r=r+int32(a(x,y,1)).  By writing int32 before the a, we are just telling MATLAB/Octave to use an int32 type variable to store the sum of all the red pixels.  An int32, is a special type of variable which can store very big numbers, we are using this as we expect the value of r to get very big.  The technical term for this is type casting, I won’t go into it in detail now, but you can google it if you are interested.  Now add corresponding lies for the blue, and green colours, just as we did for the red colour.  Now test out the function again on the command line.  You should get some large values for r,g and b returned. (In step one you set r,g,b to 50, if you have not already done this take out those liens of code.)
Step 7: Returning the sum of red,green and blue, pixels is not so useful. What we want really is the function to return the average values of the red, green and blue pixels.  So add to your function a variable (called count) which adds up the number of pixels over which the r,g,b values are summed.  Then at the end of your function, divide r,g and b values by count.  Test your function out again on the command line.  It should now return the average pixel intensity, for the region defined in figure 1.
Step 8: Try to load desk.jpg and object.jpg and run them through your function.  What values do you get out?  Are they different for each object?  If so how different?

Question 2: We have now written a function to detect the colour of an object.  All we need now is another function to compare the detected r,g,b values to know r,g,b values to decide if an object has been detected.  Define a new function in a new .m file which is called delta as follows:
ret=delta(r0,g0,b0,r1,g1,b1,tol)
This function will take two sets of RGB values and compare them.  It will return -1 if they differ more than tol (tolerance), and 0 if they differ by less than tol.  Calculate the difference between the two sets of RGB values using the equation.


Question 3: Use the function you developed in question 1 to identify the r,g,b values for the picture of the desk and the picture of the object.  Write a script to load in an image and tell the user using the disp command if it is a picture of the desk or the object.
Question 4: On the raspberry PI, if you wanted to get an image from the camera and identify what it was.  You would use the camera() command to take a new image which will save it in /home/pi/image.jpg, and then the imread command to read in the image, followed by the flipud command to orientate the image correctly, followed by get_color to extract the average rgb values.  You will need to do this a lot, so it therefore makes sense to put them all in a single function.
Step 1: Define a new function, called get_real_world_rgb, in a new file called get_real_world_rgb.m which calls, camera(), imread(), and get_color in order, and returns the r,g,b values returned by get_color.
Step 2: Clearly the camera() command won’t work on the PC as it does not have a camera attached to it, and if you try to run this function it will just return an error.  Therefore, edit  get_real_world_rgb so that it accepts a variable called count.  In the function get_real_world_rgb, move the function calls camera(), imread(), and get_color commands in side an if statement, and only allow them the be executed if count is equal to -1.  Thus the functions which only work on the buggy, will only be called if count==-1.
Step 3: Now edit the function again, to display the words ‘running on a PC’, if count is not equal to -1.  By changing the value of count, you can now select if the functions applicable to the PI are run or not, thus your code is now multi platform. :)
Step 4: Very often you won’t have access to a piece of equipment you have been asked to program.  An example of this could be developing code to run a cruise missile, not every programmer who programs the missile will have one on his desk.  So often programmers need to write functions to act like hardware would act even though the hardware is not present.
We are now going to edit the function,  get_real_world_rgb, to act as if it were on the PI, even though we are using it on a PC.  Within the if statement where ‘running on a PC’ is printed out, add an if else statement, where if the value of count equals 5, the image object.jpg is loaded into variable ‘a’, and  if the value of count is not equal to 5 the image desk.jpg is loaded into variable ‘a’, then follow this by the  get_color commands to enable the function to return the r,g,b values of the image.  What you should have now is a function, which will capture data from the camera of the PI, when count is equal to -1, and then count is above -1 it will load images from disk to pretend the function is running on the PI.
Question 5:
Step 1: Make a new script buggy.m, which defines the variable count.  Then using the variable count, counts from 0 to 100, using a while loop.  In side the while loop, use the get_real_world_rgb command to get r,g,b values.  Pass count to get_real_world_rgb.
Step 2: Using the delta function, within the loop, write out the text. ‘moving buggy forwards’, if  get_real_world_rgb returns rgb values corresponding to the desk.  And writes out the text ‘turn and reverse!!!!! Object detected!!!’ if the rgb values don’t correspond to the desk.
Step 3: Edit your script so that if an object is detected, it:
Prints out “1111”
waits for 1 second.
Prints out “0000”
waits for 1 second.
Prints out “1111”
waits for 1 second.
Prints out “0000”
waits for 1 second.

Question 6: Your script buggy.m is now ready to be copied over to the buggy and tested. Before you do that, in buggy.m where you pass the variable count to get_real_world_rgb, replace it with -1.  This will make get_real_world_rgb call the camera() command. Also replace the disp command you used to print 1111 and 0000 to the screen with the command to turn on and off the LEDs.  Finally, replace ‘moving buggy forwards’ and ‘turn and reverse!!!!! Object detected!!!’ with commands to move the motors of the buggy.  Get a demonstrator to check your script before you copy it to the buggy, and test it. [Hint, if you want to test the buggy on the carpet you will need to determine the rgb values of the carpet for the buggy to work]
Question 7: Edit your script so that, when it starts it automatic takes a picture of what it first sees then compares all subsequent images to these values to determine if an object has been encountered.

Old
---
out the values from the echo sensor.  Now using an if statement, run the motors for 1 second if the distance detected is more than 45cm, if a distance of less than 45 cm, is detected make the code wait for 5 second using the sleep command.


Question 5: Write a program to turn on the LEDs when the distance detected from the echo sensor is less than 10cm, and to turn them off when the the distance is larger than 10cm.  Save this as echo_led.m

Q3) We are now going to edit the script to reverse the direction of the buggy if, an object is detected.  Define a variable outside the while loop called ‘direction’ set it to 1.0.  Now, if a distance below 45 cm is detected make the script multiply direction by -1.0 and wait for one second.  Now, edit the script by using an ‘if-else’ statement, to drive the buggy forwards if direction is set to 1.0 and backwards if direction is set to -1.0.
Q4) 
    • Note 1: autorun.m will only be executed, when no keyboard or mouse are present.  If you want to test autorun.m, while the screen is still connected to you PC, just unplug the keyboard and mouse, and it will be executed.
    • Note 2: Do remember to turn off the buggy before connecting it to the PC again.  If you don’t there is a risk power will be fed from the batteries to the USB port of the PC, which won’t have a good outcome. [https://www.youtube.com/watch?v=2SopsQEfoc4]
Q5) By setting one motor to a much lower power level than the other, it is possible to make the buggy turn.  Edit your script, so that when the buggy detects an object, it will:
    • Reverse for 2 seconds
    • Start going forwards again, but it run one motor slower than the other for a 3 seconds to make it turn.
    • Go forwards on full power with both motors for 3 seconds.
    • Then run the other motor slow for 3 seconds to straighten up the buggy.
This sect of actions should enable your buggy to move around one of your team mates stood in the way.  Pay with the code until it works, you may have to adjust the timings, and use a bit of creativity.  Also it may or not work so well on carpet, feel free to go and find a surface which is not carpet – like the ESLC.

Input from the outside world
Ask a demonstrator (or me :) ) for a ‘block of red switches’, I have not put these in the kits as the pins get damaged very easily.  In this final section to the work sheet, we are going to be using the switches to get input from the outside world.  Again, just like with the LEDs, these switches could be replaced with any type of sensors, gas sensors, light sensors, heat sensors, you name it you can connect it to the PI.  However, to keep things simple we are first going to play with simple off on switches first.
The bread board is getting pretty crowed now, but if you squeeze you resisters for your echo sensor right to the end, and then push the LEDs just up against them, there is enough room for the red block of switches.  This can be seen in figure 5.

Figure 5: Adding the switches to the bread board.

Wire up the switches according to the wiring diagram in figure 6, if you get lost ask a demonstrator for help.

Figure 6: Adding the switches to the bread board. The yellow boxes are resistors, they are the same as the 330 Ohm ones you used for the LEDs.

Once the switches are installed you can read their state using the pin_in() function in octave, this returns a 1D array containing the position of the switches.  Try playing with the switches and see what pin_in() returns.  The switches don’t stick to the bread board very well because they have short legs.  To turn the switches on and off I suggest you hold the red switch block down with one fingure, and toggle the switches with the a screwdriver.

Question 1: We are now going to write a script to make the buggy drive in a big circle.  If switch 1 is on, then your buggy will drive in a clockwise circle, if switch 1 is off it will drive anti clockwise circle.  When the buggy meets an object, it will stop for one second, then display the following sequences on the LEDs 1111,1110,1101,1100,1011,1010, 1001,1000,0111,0110,0101,0100,0011,0010,0001, 0000, with a 0.1 second wait between each combination of numbers.  These numbers are actually binary numbers counting from 15 to 0, (we will learn about binary numbers in second year).  Once the buggy has counted from 1111 to 0000, it should resume driving in a circle.  The task has been broken down into a series of steps below.  
    • Step 1: Make a new script called circle.m and save it. Then, use the pin_in() function, to determine if switch 1 is on or off.
    • Step 2: Using an if statement, if switch 1 is on drive the left motor faster than the right motor for 1 second.  If switch 1 is off, drive the right motor faster than the left motor for 1 second.  Test your script.
    • Step 3: After the commands for running the motors, use the echo sensor to test for an object closer than 20 cm.  If an object is detected, make the buggy code wait for 5 seconds.
    • Step 4: Use a while loop to (hint while(1)…...end)  to make your loop run forever. Now, using a series of pin_out(“xxxx”,0.1) statements, and sleep commands make the buggy display the series of binary numbers given in the above question, if an object is detected.  As soon as the number 0000 is displayed on the LEDs, the buggy should resume driving in a circle.
    • Step 5: Once your code is working take your buggy down to the Atrium of the ESLC, and test it in the big open space.

Once you have completed all the tasks, use a usb stick to extract the .m files from the buggy, copy them onto your PC, zip them, then upload them to moodle.  Each group member must do this individually although it’s a group exercise.  Next week’s session will not be so group orientated.


----------

The echo sensor


We are now going to start using the PI to control hardware.  Let’s first start off with the echo sensor as we already have it installed.  Make a new script called echo_test.m, and save this under /home/pi/.  To control the hardware of the buggy, we are going to have to use functions, which know how to talk to the hardware.  These functions are stored in /home/pi/lib.  Add the line

addpath(‘/home/pi/lib’)

to the top of your script, this will tell Octave where the functions are stored.

Now under that add the line:

echo_sensor()

and run the script.

Try placing your hand in front of the echo sensor and then moving it away, how does the number change? If the number does not change, or you get a minus number check the wiring on your echo sensor very carefully.  If all else fails call a demonstrator over for help.

Q1) Using a while loop and the sleep command, make Octave print out a distance measurement every second.  Something funny happens, when you run this script.  It will only show you the output once the script ends.  Octave does this, to make the program quicker to run [MATLAB does not do this], printing text to the screen is very slow.  However, sometimes it is useful to see the text generated by our program as it runs.  To force Octave to print out text, as the program runs add the command

fflush(stdout)
just after the command used to print the distance.

Q2) Edit your program so that it prints out the words “Too close!”, when you hold less than 5 cm front of the sensor, “I miss you!”, when your hand is not there, and “Just right”, when your hand is 20-25 cm away. Save this all under echo_test.m
