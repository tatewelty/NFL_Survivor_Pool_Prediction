# NFL Survivor Pool Prediction  

### Challenge  
An NFL Survivor Pool (elimination pool/ knockout pool) is a season long contest for predicting the outcome of each game.  
 - Each week, you select exactly 1 team that you think will win.
 - If they win, you stay in the pool.  If they lose, you are out of the pool for the season.
 - Each team can only be selected once each season.  So, at the end you have 18 distinct team winners.
  
What is the best method to predict a survivor pool path with the highest probability to survive an entire season?

### Outcome  
After testing and validation, the best-performing model selects the highest home win probability, iteratively removing the chosen team and week from future consideration.
Here are the results from the best-performing model.  
Through taking 10,000 simulations and then finding the optimal path in each simulation.  Here are the top 5 week 1 picks:    
<img width="89" height="95" alt="image" src="https://github.com/user-attachments/assets/d99769a3-0659-455b-8194-752a003e9bec" />  
**Washington Commanders** is the clear first choice.  Vegas odds: 6.5 point favorites, 2nd highest for week 1.  
**Jacksonville Jaguars** are the next choice against the Panthers, who ended 5-12 last season.  Vegas odds: 4.5 point favorites.  
**Philadelphia Eagles**  are the reigning Super Bowl champs.  Vegas odds: 6.5 point favorites, 2nd highest for week 1.  
**Denver Broncos** playing the Titans, who ended 3-14 last season.  Vegas odds: 7.5 point favorites (highest odds for week 1)  
The Jaguars are a riskier play for week 1, but allow for easier paths down the line as the season progresses.  

As more games are played, confidence in team strength increases, but here is a full example path:
<img width="1017" height="42" alt="image" src="https://github.com/user-attachments/assets/8d0c537a-4281-4ae2-a530-81c195d672b9" />  
This starts with the following matchups:  
**Washington Commanders** vs. New York Giants  
**Miami Dolphins** vs. New England Patriots  
**Seattle Seahawks** vs. New Orleans Saints  

We'll see how it pans out for this 2025 NFL season.

### Methodology  
**Schedule data**: (week, home team, away team) was pulled from nfl_py_data python package  
**Starting ELO's**: for each team (team, ELO) were pulled from [NFL ELO](https://www.nfeloapp.com/nfl-power-ratings/)  
Home win probability is calculated by:    
![Home Team ELO Win Probability](https://latex.codecogs.com/svg.image?&space;P=\frac{1}{1&plus;10^{\frac{awayELO-homeELO&plus;HomeFieldAdvantage}{400}}})  
**Home Field Advantage (HFA)** = 50, about 5% increase, which is in line with the NFL average of 55% win rate.  
**Max ELO change per game (K)** = 20, to ensure confidence in the first few weeks  

The function 'simulate_full_season' simulates each game by:  
1. Calculating win probability from ELO ratings
2. Simulating the winner based on those probabilities
3. Updating team ELO for the next week
4. Then, simulating the entire season

3 metrics were tested for selecting optimal paths
 - **home win probability:**  Take the  maximum home win probability.  Remove that team and week from contention, and iterate.  
 - **home win ELO:**  Take the maximum home win ELO.  Remove that team and week from contention, and iterate.  
 - **home ELO difference:** Take the maximum difference of (home ELO - away ELO).  Remove that team and week from contention, and iterate.

The home win probabilities favor home teams, since home field advantage generally increases survival probability.  
If you're going to pick a team, may as well pick them at home when they have the most advantage.  

## Future Iterations  
Here are some ideas for improvements and future iterations:  
 - Select optimal path week by week for the first 3 weeks to ensure you stay in, and are able to take into account that additional data
 - Iterate through a few different ELO formulas based on previous seasons' results
 - Take into account home field advantage by team instead of a global average
