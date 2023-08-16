General information about the CTL course available at https://ctl.polyphys.mat.ethz.ch/

# :wave: PROJECT many-particle-billiard

This project simulates the deterministic trajectories of *N* hard, and elastically colliding circles (radius: CircleRadius) on a two-dimensional circular billiard table (radius: TableRadius). All circles have identical mass. 
Five moving hard circles are shown below, at two different times. The pink circle is shown during collision with the table wall, the blue and yellow circles elastically collide, and the green and red circle just propagate with their current velocities. 

<img src="https://www.complexfluids.ethz.ch/images/PROJECT-many-particle-billiard.png" width="30%">

During an elastic collision of two identical circles, the velocities of both circles are altered such that the total momentum, the total angular momentum, and the kinetic energy are identical before and after the collision. The problem of determining the velocities after the collision and the time of collision can be greatly simplified by switching into the rest-center-of-mass system of the two circles. If **c** denotes the center of mass of two circles, and **v** the velocity of the center of mass, the new center coordinates **r**<sub>1</sub> and **r**<sub>2</sub> of the two circles in this co-moving frame have opposite sign, **r**<sub>1</sub>=-**r**<sub>2</sub>, and the circles collide exactly at the origin (if they collide at all). At the time *t* of collision **r**<sub>1</sub>(t) is at distance CircleRadius from the origin, this determines *t*. The line connecting the two centers of the colliding circles at the moment of collision is then parallel to  **r**<sub>1</sub>(t), and each circle collides as if it hits a wall with surface normal parallel to  **r**<sub>1</sub>(t). Let **n** denote this surface normal. **n** = **r**<sub>1</sub>(t)/|**r**<sub>1</sub>(t)|. The new velocity of particle 1 directly after collision is then **v**<sub>1</sub>' = **v**<sub>1</sub> - 2(**n**,**v**<sub>1</sub>)**n**.  The new velocity of particle 2 is **v**<sub>2</sub>' = **v**<sub>2</sub> - 2(**n**,**v**<sub>2</sub>)**n**.  

<img src="https://www.complexfluids.ethz.ch/images/PROJECT-billiard-elastic-collision-2D.gif" width="50%">

The final script billard.py should accept three arguments: TableRadius,CircleRadius,*N* and could have the following structure: 

    def init_billiard(TableRadius,CircleRadius,N):
        ...
        return coordinates
    
    def init_velocities(N,coordinates):
        ...
        return velocities
    
    def propagate(N,coordinates,velocities):
        ...
        return coordinates,velocities
    
    if len(sys.argv)-1==3:
        TableRadius = float(sys.argv[1])
        CircleRadius = float(sys.argv[2])
        N = int(sys.argv[3])
    else:
        TableRadius = 10
        CircleRadius = 1
        N = 10
        
    coordinates = init_billiard(TableRadius,CircleRadius,N)
    velocities = init_velocities(N,coordinates)
    coordinates,velocities = propagate(TableRadius,CircleRadius,N,coordinates,velocities)
    ...
        

## Task 1: init_billiard(TableRadius,CircleRadius,N)

This function 
1. randomly creates initial center positions of N non-overlapping identical circles (radius CircleRadius) on a circlular billiard table (a circle of radius TableRadius, centered at the origin)
2. returns an array named coordinates (this array has N rows and 2 columns)

## Task 2: init_velocities(N,coordinates)

This function assigns initial two-dimensional random velocities to all the N circles and returns these velocities.

## Task 3: visualize_table(TableRadius,CircleRadius,N,coordinates)

This function visualizes the walls of the billiard table, and the N circles of equal radius at their coordinates. 

## Task 4: propagate(TableRadius,CircleRadius,N,coordinates,velocities)

This function does this
1. calculates for each circle separately the time to the next collision with either another circle, or the wall of the billard table.
2. Determines the smallest of these times (denoted as tcollision), and the circle (circlenumber) responsible for this smallest time, as well as if the collision is with the wall or with another circle (othercirclenumber) 
3. Calculates new coordinates assuming that all circles move in straight lines in direction of their current velocities for a duration tcollision. 
4. Calculates new velocities. If the collision is with a wall, the new velocities are identical with the old velocities with the exception of the velocities of circle circlenumber (ideal reflection at the wall). If the collision is between two circles (circlenumber and othercirclenumber), the velocities of both circles change (elastic collision). 
5. returns the updated coordinates and velocities

## Task 5: movie(TableRadius,CircleRadius,N,collisions)

This function calls the above propagate function for a number of times (collisions times), and visualizes the table using the above visualize_table routine.

## Task 6: kinetic_energy(N,velocities)

This function calculates the mean squared velocity of the N identical, hard circles. Use this function to test if the mean squared velocity is preserved in the coarse of time. If not, devise a method that corrects for the numerical drift. 
