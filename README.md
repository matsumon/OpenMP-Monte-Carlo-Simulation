# OpenMP-Monte-Carlo-Simulation
Introduction

Monte Carlo simulation is used to determine the range of outcomes for a series of parameters, each of which has a probability distribution showing how likely each option is to happen. In this project, you will take a scenario and develop a Monte Carlo simulation of it, determining how likely a particular output is to happen.

Clearly, this is very parallelizable -- it is the same computation being run on many permutations of the input parameters. You will run this with OpenMP, testing it on different numbers of threads (1, 2, and 4, but more are OK).

The Scenario

A castle sits on top of a cliff. An amateur band of merceneries is attempting to destroy it.

Normally this would be a pretty straightforward geometric calculation, but these are amateurs. What makes them amateurs you ask? It's because they are not very good at estimating distances, and not very good at aiming their cannon. They can only determine the 5 input parameters within certain ranges.

Your job is to figure out the probability that these doofuses will actually hit the castle. This is a job for multicore Monte Carlo simulation!

Requirements:

    The ranges are:
    Variable	Meaning	Range
    g	Ground distance to the cliff face	20. - 30.
    h	Height of the cliff face	10. - 20.
    d	Upper deck distance to the castle	10. - 20.
    v	Cannonball initial velocity	10. - 30.
    θ	Cannon firing angle	30. - 70.

    In addition you have:
    vx = v*cos(θ)
    vy = v*sin(θ)
    TOL = 5.0
    GRAVITY = -9.8

    Run this for some combinations of trials and threads. Do timing for each combination. Like we talked about in the Project Notes, run each experiment some number of tries, NUMTRIES, and record just the peak performance.

    Do a table and two graphs. The two graphs need to be:
        Performance versus the number of Monte Carlo trials, with the colored lines being the number of OpenMP threads.
        Performance versus the number OpenMP threads, with the colored lines being the number of Monte Carlo trials.. 
    (See the Project Notes to see an example of this and how to get Excel to most of the work for you.)

    Chosing one of the runs (the one with the maximum number of trials would be good), tell me what you think the actual probability is.

    Compute Fp, the Parallel Fraction, for this computation. 

Equations

To find out at what time the ball hits y = 0. (returns to the ground level), solve this for t (time):
y = 0. = 0. + vy*t + 0.5*GRAVITY*t2
Ignore the t=0. solution -- that's when it started.
To see how far the ball went horizontally in that time: x = 0. + vx*t
If this is less than g, then the ball never even reached the cliff face -- they missed the castle.

To find out what time the ball gets even with x = g (the cliff face), solve this for t (time):
x = g = 0. + vx*t
To see how far the ball went vertically in that time:
y = 0. + vy*t + 0.5*GRAVITY*t2
If this is less than h, then the ball was unable to clear the cliff face -- they missed the castle.

To find out at what time the ball hits y = h (the upper deck), solve this for t (time):
y = h = 0. + vy*t + 0.5*GRAVITY*t2
using the quadratic formula. (Here you thought you were done with this in high school.)

If you think of this equation as being in the form: at2 + bt + c = 0., then the value of t is:
−b ± √   b2 − 4ac 
          2a          

This will give you two solutions for time. Choose the larger of the two. The larger of the two gives you the time the cannonball reaches y=h on the way down. (The smaller of the two gives you the time the cannonball reaches y=h on the way up, which doesn't help.)
To see how far the ball went horizontally in that time use:
x = 0. + vx*t
If fabs(x-g - d) <= TOL, then the cannonball actually destroyed the castle. Even a blind squirrel finds an acorn every so often... 
