from microbit import *
import random

#declare and set variables
score = 0
energy = 100 
lastTarget = 5
startGame = 0
on = 9
off = 0
y=0
x=0

#Show a vertical line at the specified X position 
def showVL(x,y,state,time):
    while y < 5:
        display.set_pixel(x, y, state)
        sleep(time)
        y=y+1  #Move down a row of 5 pixels

#Flash the vertical line at the specified X position
def flashLine(x,flash,time,wait):
    y=0;loop=0
    while loop < flash:
        showVL(x,y,off,time)
        showVL(x,y,on,time)
        sleep(wait);y=0;loop+=1 
        
#Show how many energy remain (divided by 4 to fit in the 25 pixel screen)
#No longer used - added during debugging
def showEnergy(curEnergy):
    display.clear()
    xLoop=0;yLoop=0;energyLoop=0
    curEnergy=(curEnergy // 4)+1
    while yLoop <5:
        while xLoop < 5:
            energyLoop=energyLoop+1
            if (energyLoop ) <= curEnergy:
                display.set_pixel(xLoop, yLoop, on)
            else:
                display.set_pixel(xLoop, yLoop, off)      
            xLoop+=1
        yLoop+=1
        xLoop=0
    sleep(1000)
    display.clear()

#Instructions! 
display.scroll("Make a column of 5!",wait=True,loop=False)

#Endless loop
while True:  
    
    #Show a left arrow pointing to the A button to start
    display.show(Image.ARROW_W)
    
    #Loop until A is pressed
    startGame = 0
    while startGame==0:
        if button_a.is_pressed():
            startGame=1
    
    display.clear()
    
    #Set the delay etc. for this game
    delay = 500;level = 1;energy=100;score=0;
    
    while energy > 0:
        #Repeat until the player runs out of energy
         
        #Get the target column - repeat process until a new column is selected
        target = random.randint(0, 4);
        while target == lastTarget:
            target = random.randint(0, 4);
        lastTarget=target   #Store the last target for next time
        
        y=0;x=target;endOfLevel=0;flash=0
        showVL(x,y,on,0)            #Show the column to build
        sleep(1000);y=0             #Display it for a short while
        flashLine(target,3,10,50)   #Flash it
        showVL(x,y,off,50)          #Hide the line, could have used display.clear()
        x=0;y=0
        
        while endOfLevel==0: #End of level when either out of energy or got to the bottom of the screen
            
            #clear the button cache by checking for having been pressed - otherwise the player can just hold the buttons down and win the game
            if button_a.was_pressed() or button_b.was_pressed():
                pass    #Do nothing but syntactically required
            
            display.set_pixel(x, y, on)     #Show the pixel on the screen
            sleep(delay)                    #Pause for a period of time based on delay (which decreases per level)
            display.set_pixel(x, y, off)    #Switch off the pixel to move it
            
            #Check for button being pressed or having been pressed while the pixel was illminated
            if button_a.is_pressed() or button_a.was_pressed() or button_b.is_pressed() or button_b.was_pressed():
                
                #If we are in the correct column then leave the pixel lit up and move down a row
                if x==target:
                    display.set_pixel(x, y, on)
                    y+=1
                    x=-1 
                else:
                #If not on the correct column then drop energy
                    energy-= 1
                    
                    #If below level 10 and not on the first row move back up a row as extra punishment :)
                    if y>0 and level < 10:
                        display.set_pixel(x, y, off)
                        y-=1
                        display.set_pixel(target, y, off)
                        x=-1  
            #Increase X to move the pixel across one
            x+=1
            
            #If X=5 then we have hit the edge of the screen so drop enery and go back to the start
            if x==5:
                x=0;energy-=1
            
            #If Y = 5 then we have complete line so increase score and mark as end of level
            if y==5:
                endOfLevel=1
                score+=level
            if energy == 0:
                endOfLevel=1
        
        #Assuming we have not got here due to the end of the game then do a victory flash of the column        
        if energy > 0 :
            flashLine(target,3,100,100)
        
        #Wait a short while and then increase level, clear the display and set up for the next level
        sleep(1000);display.clear();level+=1;delay-=35
        if delay <100:
            delay=100
    
    #If we are here then energy = 0 show the score and wait for the next game
    display.scroll("Score : " + str(score),wait=True,loop=False)         
    sleep(1000)
