<p align="left"><img width=50% src="https://github.com/ACM40960/project-leongill/blob/main/Figures/readme_gif.GIF"></p>


# Installing the Package
## Preparation

If you are using a laptop, click the battery icon in the bottom right and set the slider to the rightmost option as shown in the image below. This increases processing speed dramatically. Failure to do this will result in 50% longer computation times if left on the standard power setting. On Mac, simply plug the charger in to achieve the same effect.
<p align="left"><img width=50% src="https://github.com/ACM40960/project-leongill/blob/main/Figures/Readme_perf_figure.png"></p>

To install the package, first download R 4.2.1 at the following address and follow the prompts to complete the installation:
```text
https://cran.r-project.org/bin/windows/base/
```
Next, download the R-Studio IDE and follow the promts to complete the installation:

```text
https://www.rstudio.com/products/rstudio/download/#download
```

You now have a fully functioning R IDE. Lastly, install Rtools from the following link
```text
https://cran.r-project.org/bin/windows/Rtools/rtools42/rtools.html
```

You should now open R-Studio. Install the following packages which are required for the use of my package. This is done by pasting the following lines of text into the R console and hitting `enter`:
```R
install.packages("hash")
install.packages("Rcpp")
```

You are now ready to install the package. Download the Blackjackr.zip file from this GitHub repository. Unzip the file with `Right-click` + `unzip all`. Open this folder and then open the "blackjackr package" folder. From here, open the blackjackr R project object which will take you to R-Studio. On the top of the screen, hit `Build` -> `Install package`. Wait until this process completes. Close R and return to the Blackjackr folder. 
# Using the Package

After closing R, open Blackjack_Investigations.RMD which acts as a complete exhibition of the package's functionality(Closing R ensures that you use the correct working directory). When you first open the document, a prompt will appear at the top of the notebook asking to install the required packages. Hit `install` to initiate this process as these packages enable the graphs used in the investigations. This button is shown below:
<p align="left"><img width=50% src="https://github.com/ACM40960/project-leongill/blob/main/Figures/install_packages.png"></p>

You are now able to use all the functionality of the package. First, type the following line into the console which lists all of the functions in the package:
```R
lsf.str("package:blackjackr")   
```
To learn more about a function, type `?` followed by a function name to read extensive documentation for each included function. The project documentation should be viewed as an extension of this README. All simulation functions use parallel computing to speed up the simulation process. If you wish to use only a single core to maintain your computer's performance, pass the `cores = 1` argument to the relevant function which will result in a roughly 75% performance decrease on a 4-core machine.

I recommend going through the Blackjack_Investigations.RMD file sequentially to learn about the package. I will now describe how to use the package's main functions. The first function of interest is full_strategy_par(). The arguments can be seen using ?full_strategy_par. This function generates an optimal strategy using Monte Carlo simulation. The "par" suffix implies simulation are done in parallel by default. The number of simulations is set using the `number_of_simulations` argument. Other arguments allow the ruleset to be changed in thousands of possible combinations using the `include_double_down`, `include_surrender` and `number_of_decks` arguments with even more options available. All arguments have defaults so you can test the function with no arguments. An example strategy with surrendering enabled, doubling down disabled, splitting allowed up to four hands with one deck in play using 1000 simulations per stickingng position is:

```R
strat = full_strategy_par(number_of_decks = 1, number_of_simulations = 10^3, include_double_down  = F, include_surrender = T, include_split = 4)
```

The output is a hash map where each key is a position. The associated value is the move which maximizes player return given the inputted ruleset. Values of 0, 1, 2, 3 and 4 correspond to stick, hit, double down, surrender and split, respectively. An example of the first 11 terms of this output is shown below:

#### Output
```text
100000000002 : 1
100000000003 : 1
100000000011 : 0
100000000012 : 1
100000000013 : 1
100000000020 : 0
100000000021 : 0
100000000101 : 1
100000000102 : 1
100000000103 : 1
100000000110 : 1
```

The next function called play_games_split_par() plays games with the above strategy and is capable of playing games where splitting and all other rules are enabled or disabled. Enabling splitting requires an extensive framework so when splitting is disabled in your strategy (done with the `include_split = 0` argument), use play_games_par() instead as it is twice as fast. An example is shown below where the output is the player's average return over `number_of_simulations` games:
```R
return_per_game = play_games_split_par(number_of_simulations = 10^8, number_of_decks = 1, include_split = 4, strategy = strat)
```
#### Output
```text
 -0.01702554
```

The same strategy and game simulation without splitting is done as follows:
```R
strat = full_strategy_par(number_of_decks = 1, number_of_simulations = 10^3, include_double_down  = F, include_surrender = T, include_split = 0)
return_per_game = play_games_par(number_of_simulations = 10^8, number_of_decks = 1, strategy = strat)
```
Most other function are derivatives of - or are used extensively within - the functions above. To see how the remaining functions are used, go through the Blackjack_Investigations.RMD which includes some interesing applications of the package as well as runtimes for each function. It is essential that you close R before opening this file as it uses files in the same working directory as Blackjack_Investigations.RMD. If you open Blackjack_Investigations.RMD after installing the package without closing R-Studio between these actions, you will be in the package's working directory which will incur errors for some functions. The source code for the R and C++ functions is in the  "Blackjackr package" folder in the folders R and src respectively.
