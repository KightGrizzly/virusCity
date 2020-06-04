# virusCity
// Web-based Real-time Strategy game with 3D view and dynamic pathfinding!
////////////////////////////////////////////////////////////

- The project idea consists of a multi agent epidemic simulation that shows the spread of a virus in a city environment.
- How it came to be: (reference the inspiration by corona situation / remote lectures..) 


     - The idea is based on the SIR modelling approach that is also used in real-world models in epidemiology.
      The SIR approach classifies people into three groups "susceptible", "infected" and "removed" (healed) 
       - healthy people will be susceptible and get infected by infected people.
       - "Removed" people gained immunity after being healed and they don't contribute to infections.

- Objective of the game: How does closing schools stop a virus??

////////////////////////////////////////////////////////////
// HTML5 Canvas Rendring in JavaScript
////////////////////////////////////////////////////////////

- We implemented such a modelling game as a web application using JavaScript and HTML5 canvas rendering. 
- The web application mainly consists of a full size HTML canvas on which the layout of a city is rendered.

////////////////////////////////////////////////////////////
// Isometric graphics like simCity
////////////////////////////////////////////////////////////

- The graphical concept is closely related to realtime-strategy games like simCity
- The 2D grid is shown as an isometric view that gives the impression of a 3D environment. 
  - The tiles are drawn from a texture map by using an isometric projection formula that makes it look pseudo 3D
- The city itself is made actually up of a 50x50 grid with 2.5k two dimensional tiles
  - The tiles can contain buildings or streets that each have certain properties.

////////////////////////////////////////////////////////////
// Database Persistance in Active Record
////////////////////////////////////////////////////////////

We have a database persistence by dynamically storing tiles in a backend, using Active record in Ruby on Rails:
- This allows us to interactively edit tiles and permanently store changes!

- People (inhabitants of the city) are simulated by agents that move across a two-dimensional map (shown as dots)
  - You can see: infected (red) people and healthy (blue) people. Healthy people get infected whenever they pass by infected people (same tile).

Now the simulation part:
 - The simulation shows the behavior of people over a certain amount of time.

 - We have an event loop running that executes the main logic a few times (ticks) per second.
 - After each change/timestep we render the entire scene to the canvas. 

////////////////////////////////////////
// Complex Database Model: People have Schedues that relate to tiles
////////////////////////////////////////

 - People live in buildings and they have a schedule (going to work, going to school, etc.)
 - The People's schedule associates a person to a destination tile to which that person will go at a certain time (i.e., go to work).
 - A global timer ensures that a people's schedule is executed when the appropriate time is due. 
  
/////////////////////////////
// We use an actual A* Pathfinding approach like real strategy games do!
//////////////////////////

 - We then invoke the A* Pathfinding Algorithm to efficiently compute the shortest path for people to move on (like in real strategy games!)
 - It explores tiles based on their properties (if people can realistically move on them (building, street, water, each having different properties)
 - The pathfinding explores all possible neighbour tiles until a path is found or there is no remaining tile to move to. 
 - Note how people only follow streets, as they can not walk on non accessible tiles!


///////////////////////////////
// Details about the A* Pathfinding approach
//////////////////////////

- The grid consists of nxn tiles
- We are looking for a (shortest) path between start tile A and destination tile B
- General Idea: Breadth First Search:
 - We start by exploring all neighbor tiles of A - Question: Which neighbor tiles are accessible?
 - We save all accessible neighbor tiles in a list (list of found tiles that we still need to explore)
 - We pick the most promising neighbor tile from the list (based on the remaining distance to desitnation tile B)
 - We go to the selected neighbor tile and again explore the neighbor tiles of this tile to add them to our list (Important: only add tiles we did not visit yet!)
 - After enough steps we will finally arrive at the destination tile

