# Sonoptic

Sonoptic is an artistic residency focused on sound, visual and/or audiovisual approaches. 
It was organized at [Mixart Myrys](http://mixart-myrys.org/) from October 5 to 10, 2020.
More info here : http://sonoptic.net/

I joined the event with the aim of exploring different perspectives on data sonification and visualization.

# Team Laser

I met the Team Laser from tmplab who uses a laser interface to produce visuals based on different kinds of sources (gestural, audio, generative data, real-time, ...) and decided to investigate if my model's simulations could be used to guide the lasers.

# Agent-Based Models (ABMs)

[ABMs](https://en.wikipedia.org/wiki/Agent-based_model) (also called multi-agent systems) are a class of computational model to simulate the actions and interactions of autonomous agents, aiming at reproducing and predicting complex phenomena

## Netlogo software : models and simulations

The Netlogo software is one the most used softwares to work on agent-based models. It has a GUI allowing the user to plot multiple outputs and to control the model's parameters in real-time. The GUI also comprises a visual interface representing the agents during the simulations. More info here : https://ccl.northwestern.edu/netlogo/

### Macrophage differentiation model

I am currently working on a macrophage differentiation model, in the context of research on Chronic Lymphocytic Leukaemia, and with the long-term goal being to model the tumor microenvironment. More info here : https://github.com/Ninouchkaia/ABM (in construction). In this model, agents represent cells

### Ants

### Genetic drift

### Flocking birds

# Plugging ABM simulations to Redis in real-time

## Required output format
You'll need a text file containing the agents' coordinates and their attribute (color) for each frame (time step).
The overall format should be : 

[ [coordinates (x,y) of agent 0, black], [coordinates (x,y) of agent 0, color of agent 0], [coordinates (x,y) of agent 1, black], [coordinates (x,y) of agent 1, color of agent 1], ... [coordinates (x,y) of agent n, black], [coordinates (x,y) of agent n, color of agent 0] ] \n # data for the first timeframe  
[ [coordinates (x,y) of agent 0, black], [coordinates (x,y) of agent 0, color of agent 0], [coordinates (x,y) of agent 1, black], [coordinates (x,y) of agent 1, color of agent 1], ... [coordinates (x,y) of agent n, black], [coordinates (x,y) of agent n, color of agent 0] ] \n # data for the second timeframe  
...  
etc.  
  
Or, written differently
[ [x<sub>0</sub>,y<sub>0</sub>,0], [x<sub>0</sub>,y<sub>0</sub>,c<sub>0</sub>], [x<sub>1</sub>,y<sub>1</sub>,0], [x<sub>1</sub>,y<sub>1</sub>,c<sub>1</sub>], ..., [x<sub>n</sub>,y<sub>n</sub>,0], [x<sub>n</sub>,y<sub>n</sub>,c<sub>n</sub>] ] \n # first timeframe  
[ [x'<sub>0</sub>,y'<sub>0</sub>,0], [x'<sub>0</sub>,y'<sub>0</sub>,c'<sub>0</sub>], [x'<sub>1</sub>,y'<sub>1</sub>,0], [x'<sub>1</sub>,y'<sub>1</sub>,c'<sub>1</sub>], ..., [x'<sub>n</sub>,y'<sub>n</sub>,0], [x'<sub>n</sub>,y'<sub>n</sub>,c'<sub>n</sub>] ] \n # second timeframe  
...
[ [x''<sub>0</sub>,y''<sub>0</sub>,0], [x''<sub>0</sub>,y''<sub>0</sub>,c''<sub>0</sub>], [x''<sub>1</sub>,y''<sub>1</sub>,0], [x''<sub>1</sub>,y''<sub>1</sub>,c''<sub>1</sub>], ..., [x''<sub>n</sub>,y''<sub>n</sub>,0], [x''<sub>n</sub>,y''<sub>n</sub>,c''<sub>n</sub>] ] # third timeframe  

See output examples.  
  
You can notice that every agent's coordinates has to be duplicated with one set with black color and another one with the color we want. This is a property of the lasers, not to have all points linked together. But if you want to have them linked, just use a single set of coordinates with the color you want, this can actually give some nice visuals too !


### Coordinates

The lasers will trace points in a window of size [0;800], with origin [0,0] on top left, and Y-axis descending.
--> Transformation required before outputting the data.

### Color format

The color has to be coded in 8-bits formats. 

<img align="left" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/AdditiveColor.svg/1024px-AdditiveColor.svg.png" width=96>

I used this function from their [library](https://git.interhacker.space/teamlaser/LJ/src/branch/master/clitools/filters/colorcycle.py) to transform RGB code into the required format :

```python
def rgb2int(rgb):
    return int('0x%02x%02x%02x' % tuple(rgb),0)
```
I executed it for all colors from the [RGB color model](https://en.wikipedia.org/wiki/RGB_color_model), which resulted in the following values :

Color | RGB format | int format
------------ | ------------ | -------------
White | 255,255,255 | 16777215 
Red | 255,0,0 | 16711680 
Green| 0,255,0 | 65280 
Blue | 0,0,255 | 255 
Yellow | 255,255,0 | 16776960 
Cyan | 0,255,255 | 65535 
Magenta | 255,0,255 | 16711935
Black (not used) | 0,0,0 | 0 




## Flushing the text output to 

Command:
```bash
tail -F <path_to_netlogo_code> | python3 <path_to_clitools/exports/toRedis.py> -i 127.0.0.1 -k /pl/0/0
```

Then open a browser with URL http://127.0.0.1/simu.html


# Previous and ongoing works on data sonification
