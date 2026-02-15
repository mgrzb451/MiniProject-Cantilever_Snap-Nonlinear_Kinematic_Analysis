# Cantilever Snap Joint - Nonlinear Kinematic Analysis
This project is a bit different compared to the last few we've done. The aim of this project is purely to explore and learn about Nonlinear assembly Simulation utilizing Kinematic Connections in Nastran as well as to use what I've learned about Cyclic Loads at least once to solidify it. 
We'll design a simplified cantilever snap that could secure a latch/cover holding down a smartphone-type battery. The model will be based on really course, preliminary calculations and we'll use Simcenter Nastran to validate the geometry we got. Let's go!

# Preliminary Calculations & Design Assumptions
<img width="1116" height="709" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/hand_calc.jpg" />

# 3D Model
The snap geometry was designed with the best DFMA practices in mind (as always). Wall thickness, drafts, parting line, radii are all optimized for the real world (If you're doing something might as well do it right 🤷🏻‍♂️)
<img width="781" height="657" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/3d0.jpg" />

# SOL 402 - Multistep Nonlinear Kinematic Analysis
## FEM
We utilize a Universal Kinematic Connection - Revolute. We define all the parameters according to recommendations in the Simcenter documentation.
<img width="850" height="519" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/402fem.jpg" />

## Results
Despite DAYS 😩 of trying and fiddling with all the parameters listed in both Simcenter Pre/Post and Simcenter Nastran official documentations I was not able to get the last step (the spring-back of the snap) to converge. Maybe if I decreased the timestep even more, but I'm not willing to fry my PC for so long to test - I won't be getting a new one for a while 😢

The results we get until this point are valid though. We can see that our design certainly doesn't meet the strength criteria and needs more work.
<img width="1494" height="820" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/402displ.gif" />
<img width="1499" height="819" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/402stress.jpg" />

# SOL 401 - Multistep Nonlinear Analysis
To simulate that last step of the motion we'll use a different solution type and deform the head of the snap by `2.7mm` that is just slightly more than is needed to clear the latch on its way down.
## FEM
<img width="765" height="673" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/401fem.jpg" />

## Results
The results are quite similar to the last step we got from SOL 402, we were close there 😂 The stress we get is just as bad!
<img width="544" height="820" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/401_deform.gif" />
<img width="1247" height="764" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/401stress.jpg" />

# Modified Goodman Diagram
The *Re* of our PA66 material is `82MPa`. Let's just for fun assume we're designing the snap for 10<sup>3</sup> cycles (completely unrealistic, but it's for learning). We'll create a modified Goodman Diagram to see what our Maximum Allowable Stress is.
<img width="637" height="554" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/sn.jpg" />
<img width="1264" height="598" alt="image" src="https://github.com/mgrzb451/MiniProject-Cantilever_Snap-Nonlinear_Kinematic_Analysis/blob/main/assets/goodman.jpg" />

The Goodman Diagram doesn't even come into play cause we're over even the static Re of the material. But the Endurance Limit for our 10<sup>3</sup> cycles is `~60MPa`. So our *Factor of Safety* is below `0.7`. And we haven't even included any Endurance Limit Scale Factors! Ouch 😅

