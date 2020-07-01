[Think Stats Chapter 8 Exercise 3](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77)

`def SimulateGame(lam):`
    
    `"""Simulates a game and returns the estimated goal-scoring rate.`

    `lam: actual goal scoring rate in goals per game`
    
    `"""`
    
    `goals = 0`
    
    `t = 0`
    
    `while True:`
    
       `time_between_goals = random.expovariate(lam)`
        
        `t += time_between_goals`
        
        `if t > 1:`
        
          `break`
        
        `goals += 1`

    `# estimated goal-scoring rate is the actual number of goals scored`
     
     `L = goals`
    
    `return L

`SimulateGame(3)`

Output: `2`

`def GamesSim(game_score, iterations):`
    
    `est_game_scores = []`
    
    `for i in range(iterations):`
    
        `est_game_scores.append(SimulateGame(game_score))`
    
    `rmse = RMSE(est_game_scores, game_score)`
    
    `mean_error = MeanError(est_game_scores, game_score)` 
    
    `return ('Mean Error:', mean_error, 'RMSE:', rmse)`
    
`GamesSim(3, 1000)`

Output: `('Mean Error:', -0.067, 'RMSE:', 1.7404022523543228)`

`GamesSim(3, 9)`

Output: `('Mean Error:', -0.4444444444444444, 'RMSE:', 1.4142135623730951)`

`GamesSim(3,1000000)`

Output: `('Mean Error:', -0.001236, 'RMSE:', 1.7315865557343646)`

Since the mean error is not 0, this tells us that this estimate is biased. Since the mean error is negative, this means that the estimate tends to be lower than the actual score of the game. As mu (iterations) increases, the mean error gets closer to 0, indicating that the estimated game score is getting closer to being the actual game score. 
