# Dandelion Optimizer

### Summary

Our project is the Dandelion Optimizer Algorithm (DO). The algorithm uses the movement of Dandelions and then uses these movements as a way to solve continuous optimization problems.

In the project, we implemented all the different stages of the algorithm and used a variety of CEC2017 problems to test the performance of our implementation with the performance of the authors. We performed statistical analysis by comparing our solutions' mean and standard deviation averaged over 30 Monte Carlo simulations.

Furthermore, we worked on adapting the algorithm for the TSP problem which is a discrete optimization problem. We tested our adaptation of the algorithm on 15 different TSPLIB instances and compared the performance with the Discrete Bat Algorithm, which we used for reference while adapting our algorithm to TSP.

### Material & Methods

**_Continuous Optimization:_**<br>
First, we transcribed the code in our template paper for Dandelion Optimizer into Julia in order to compare our algorithm with the template paper. We had access to some MATLAB code that we translated into Julia. Most of this was one-to-one. Even so, we did need to make some minor adjustments to account for syntax in Julia being different from syntax in MATLAB.

Then, we compared our algorithm with the template paper with the exact same parameters in order to determine if our algorithm performs similarly or better compared to the template paper. With this, we can determine if our DO algorithm is performing correctly. To evaluate this, we compared the mean and standard deviation for seven functions from CEC 2017 Benchmarking Functions. We picked a variety of different functions including one unimodal function, two simple multimodal functions, one hybrid function, and three composite functions. Along with this, we compared the convergence curves for our DO algorithm compared to the template paper. We performed this analysis for 1000 iterations, 30 dandelion seeds, and 30 Monte Carlo simulations. This was done for a dimension of 50 and a dimension of 100.

**_TSP Adaption:_**<br>
Afterwards, we attempted to adapt the DO algorithm to work for the Traveling Salesman Problem (TSP). The initialization was the same. The only difference was using random permutations from 1 to the number of dimensions to create a random tour for each seed. In general, we were not able to replicate the mathematical formulas present in the template paper’s implementation, but we were able to extract the general ideas from these formulas and implement each stage with these ideas. We had 3 stages that correspond with the dandelion seed dispersal: rising, descending, and landing stages.

For the rising stage, we split it into two algorithms for clear days and rainy days. For the clear day, we decided to use a scramble mutation. For this, we pick a random size from 1 to half of the city count. Then, we pick a random start index and shuffle the segment of the random size such that we create a new subtour. The random size is decided by alpha, which decreases from 1 to 0. With this, the earlier iterations are more likely to explore with bigger size. The later iterations will have sizes near 1 or 2 to promote exploitation. For rainy days, the general idea is to use exploitative algorithms to find the minimum as the seeds do not “travel” as far. To mimic this, we used a two-opt algorithm to continuously switch two edges until it finds an improvement. This algorithm is more exploitative as it will always try to improve the cost with little changes.

For the descending stage, we were unable to exactly implement the Brownian motion. Instead, we used a conditional statement based on Brownian value to determine if our descending stage occurs. In terms of replicating the descending stage formula, this was tricky as the changes occurred based on the mean value of all seeds. This is very hard to calculate for TSPs. Instead, we decided to use exchange mutation, which is when we switch two random values. This is based on a mutation chance calculated by multiplying the current Brownian value by alpha. We perform the exchange if this value is greater than 1.5, which was a value we tweaked for optimality. This will sufficiently simulate the erratic movement of dandelion seeds.

For the landing stage, it relies on the elite solution, which can be adapted to TSP. First, we find our elite solution. Then, we perform 2-opt to improve the elite solution. Next, we set a random chance that an ordered crossover occurs. In this ordered crossover, a random section of the elite solution is selected and put in the offspring at the same indices. Then, the remaining values of the current seed are added to the offspring. Any value that was contained in the random section is ignored. This should simulate the seed “moving” towards the elite solution.

In order to analyze our TSP, we observed a drawing of the TSP in order to find any intersections. Intersections help us determine if our algorithm is suboptimal for TSPs. Along with this, we decided to compare our algorithm with the Bat Algorithm as it is somewhat similar to our Dandelion Optimizer as they are both swarm algorithms. We compared the best cost, worst cost, average cost, median, standard deviation, a percentage difference between the average cost and from optimal cost, the percentage difference between the best cost from optimal cost, and the time to run the algorithm. Along with this, we compared our DO algorithm with more standard optimization algorithms for TSPs. This list includes a genetic algorithm, an iterative-deepening genetic algorithm, and evolutionary simulated annealing. We also included the Discrete Firefly Algorithm in our analysis as an uncommon algorithm to look at. The data for these are all located in the Bat Algorithm paper shown in the references. We compared the best cost, average cost, standard deviation, and time. We ran all of these with 20 dandelion seeds and 100 iterations. Our team believes that this is sufficient data to judge the Dandelion Algorithm in order to decide whether it is better compared to other algorithms as we can easily compare.

### Results

**_Continuous Optimization:_**<br>
Information on functions:<br>

- F1: Shifted and Rotated Bent Cigar Function is an **Unimodal** Function
- F7: Shifted and Rotated Lunacek bi-Rastrigin Function is a **Basic** Function
- F16: Hybrid Function 2 as the name suggests is a **Hybrid** Function
- F19: Expanded Rosenbrock's plus Griewangk's Function is a **Basic** Function
- F22, F24, F25 are **Composite** functions.

We selected these specific functions because these functions are also used in the CEC2020 problem set.

Information on settings:

- 50 DImensions, 1000 Iterations, 30 Search Agents, 30 Monte Carlo Simulations
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/f3bb53a9-eb99-4721-8550-0930226d3e95)

*From the above table, we can observe that the performance of our implementation is almost as close to the results that the authors of the paper received.*
<br><br>

Information on settings:
- 100 DImensions, 1000 Iterations, 30 Search Agents, 30 Monte Carlo Simulations
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/bb1e8f4d-1807-4a33-bbf3-64ffbdd873d6)

*From the above table, we can observe that the performance of our implementation even for 100 dimensions is almost as close to the results that the authors of the paper received.*

**Convergence Curves**<br><br>
In the diagrams below the red line is the performance of the DO algorithm. The convergence curves are for 100-dimension functions.
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/209c0b8c-5da7-4aa6-b900-2ac46956d43a)
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/3a7b360e-505e-41b2-bcaf-ddcc83b19604)
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/0c61d71c-81a6-4932-a579-299f9970ea52)

These were the only convergence curves provided in the paper that overlapped with our function selection.<br><br>
All these figures and more are available in the notebook itself.

___TSP:___<br>
List of tsp functions used<br>
tsp_list = [:gr17 :bays29 :swiss42 :eil51 :berlin52 :st70 :eil76 :rat99 :kroA100 :kroC100 :kroE100 :eil101 :lin105 :pr107 :pr124]
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/f2cc1aa6-6017-4042-996c-cc7dff124d0c)
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/82633497-a918-4697-8819-7545b98d6956)

The above table provides the performance of the Dandelion Optimizer compared to the Discrete Bat Algorithm, on 15 TSPLIB instances.
An important thing to note is that we performed this with a population size of 20 and max iterations of 100. We could probably achieve even better results with a larger population or with more number of iterations.
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/7f70751e-586f-451a-80f6-29ccacd567a4)
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/b5683f6d-3f27-4f12-95d7-fba1a54636a6)
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/c818c3cb-a66f-4d5e-80a5-a485133a3303)

The above table shows the performance of different algorithms on various TSPLIB instances. The performance of the other algorithms is taken from the paper on the Discrete Bat Algorithm and the parameter settings for each algorithm are mentioned below.
<br><br>
![image](https://github.com/AtharvaThorve/Dandelion-Optimizer/assets/50790953/f13b60f3-8842-4b3f-a08f-c41eefa74dda)

The TSP art can be recreated by executing the functions for drawing TSP, loading the image, and then using the sample function. We have a few examples of generating TSP art in the notebook, where we provide it with the path of the image (preferably one with a white background), and generate TSP art from it, by sampling points.

To recreate results for TSP and continuous optimization problems we have specified cells, which perform this analysis as long as the cells before it have been executed. It also contains a list of functions/TSP instances being provided along with other parameters like population size, max iterations, dimensions, etc.

We also have one cell where we can provide it with randomly generated points, and execute the algorithm to solve the TSP.

**_TSP Art:_**<br>
To generate TSP art, we convert our image into a grayscale such that all colored parts are turned into gray. Then, we randomly generate 500 points across all gray areas. Afterward, we generate our TSP for these points with our Dandelion Optimizer. Initially, we attempted to use Weighted Voronoi Stippling to generate our points. We struggled to get this method to work in creating art, so we decided to manually implement a function to generate points on the image.

### Discussion
Based on our results from Table 1, we can see that our implementation of DO is similar to or better than the implementation in the paper. With this, we can sufficiently say that we correctly implemented the dandelion optimizer. Table 2 shows this same conclusion but for larger dimensions. Figures 1, 2, and 3 show that the convergence curves are very similar for our implementation and the paper. This is a very good sign as it corroborates the idea that our implementation is correct.

For the TSP, we have noticed that our Dandelion Optimizer does not perform as well compared to other algorithms. In Table 3, our algorithm consistently has the worst averages across the board compared to the Bat Algorithm. Even so, it is still possible to find the best cost in our Monte Carlo simulations. On top of this, our algorithm is much faster compared to the Bat Algorithm. Compared to standard algorithms as shown in Table 4, our algorithm can be somewhat volatile. Even so, Tables 5 and 6 show that our algorithm is generally 3rd place out of 5. Even so, DO is fast compared to other algorithms for this volatile performance.

In terms of what did not work, for our rising stage in TSP implementation, we planned to implement 3-opt for exploration. Not only was this slow, but we also realized that 3-opt is more exploitative instead of explorative. This was a surprising issue for us as we believed that this would work. Because of this, we pivoted to a scramble mutation where we scrambled a random section. Along with this, we initially planned to use 2-opt for the landing stage. We realized this was not what the landing stage represented as an elite solution does not influence this stage. We switched to an ordered crossover to keep the general idea of the Dandelion Optimizer. 

One major limitation we came across was the implementation of the descending stage. We were unable to use a mean solution to figure this out. In fact, we were unsure what the meaning of all TSPs would be. Instead, we decided to simulate all other aspects such as Brownian motion.

### References
- [MathWorks, 2024](https://www.mathworks.com/matlabcentral/fileexchange/114680-dandelion-optimizer) "Dandelion Optimizer." Accessed 19 Apr. 2024.

- [Saji et al., 2021](https://www.sciencedirect.com/science/article/pii/S0957417421000804) "A Discrete Bat Algorithm Based on Lévy Flights for Euclidean Traveling Salesman Problem." Expert Systems with Applications. Accessed 19 Apr. 2024.

- [Evanfields](https://github.com/evanfields/TSPImages.jl) "Evanfields/TSPImages.Jl: Rough Julia Script to Draw Traveling Salesman Style Images." Accessed 19 Apr. 2024.

- [Larranaga et al., 1999](https://link.springer.com/article/10.1023/A:1006529012972) "Genetic algorithms for the travelling salesman problem: A review of representations and operators." Artificial Intelligence Review 13 (1999): 129-170.
