# Project: 3D Motion Planning

## Writeup / README

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
`motion_planning.py` script contains a basic drone control implementation that includes MotionPlanning class which defines how drone supposed to behave. It extends Drone class that provides basic functionality for establisihng connection with a drone and sending commands to it.

Starter code implements methods for takeoff, waypoints following, and landing. This is very similar to backyard flyer project. What is different is the addition of the new `Planning` state and how waypoinst are generated. Instead of hardcoded coordinates, it uses path planning which isn't yet completely implemented in the `plan_path` method. In the starter code `plan_path` creates plan on how to reach a goal 10m north and 10m east of the map center from the starter position, which is also a map center. At the moment, since the only allowed actions is to move 1m in cardinal directions, the path looks like a jigsaw.

`planning_utils` provides several useful functions and classes used in `plan_path`, including:
 - `create_grid` creates 2d grid map of where drone can fly based on the obstacle data and drone target altitude.
 - `Action` enum depicting basic actions drone can take.
 - `valid_actions` prunes actions that can't be performed at a specific location.
 - `a_star` implements A* algorithm for finding path between a start and a goal locations on a grid map.
 - `heuristic` provides a heuristic function for A* algorithm that estimates shortest distance to `goal_position`.


### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
Here students should read the first line of the csv file, extract lat0 and lon0 as floating point values and use the self.set_home_position() method to set global home. Explain briefly how you accomplished this in your code.

> I wrote `read_latlon` function that extracts latitude and longitude from collisions.csv file and used these parameters in `set_home_position` method.

#### 2. Set your current local position
Here as long as you successfully determine your local position relative to global home you'll be all set. Explain briefly how you accomplished this in your code.

> I determined drone current global position via `self.global_position` variable. Then converted that global position to loval one using `global_to_local` function.

#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location. As long as it works you're good to go!

>Grid start position is determined from `local_to_grid` function that I added to `planning_utils.py`.

#### 4. Set grid goal position from geodetic coords
This step is to add flexibility to the desired goal location. Should be able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid.

>To get grid goal position:
> 1. Get local goal position from global goal using global_to_local function.
> 2. Similar to start position, convert local goal position to grid goal via `local_to_grid` function.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
Minimal requirement here is to modify the code in planning_utils() to update the A* implementation to include diagonal motions on the grid that have a cost of sqrt(2), but more creative solutions are welcome. Explain the code you used to accomplish this step.

> I've added NW, NE, SE, and SW actions to `Action` enum. I've also added penalty cost to heuristic for choosing action that is different from the previous one.

#### 6. Cull waypoints 
For this step you can use a collinearity test or ray tracing method like Bresenham. The idea is simply to prune your path of unnecessary waypoints. Explain the code you used to accomplish this step.

> Waypoints are culled using `prune_path` function that utilizes `collinearity_check` to verify if points are on the same line. Only points that are not on the same line are added to the pruned path.

### Execute the flight
#### 1. Does it work?
It works!

### Double check that you've met specifications for each of the [rubric](https://review.udacity.com/#!/rubrics/1534/view) points.
  
# Extra Challenges: Real World Planning

For an extra challenge, consider implementing some of the techniques described in the "Real World Planning" lesson. You could try implementing a vehicle model to take dynamic constraints into account, or implement a replanning method to invoke if you get off course or encounter unexpected obstacles.


