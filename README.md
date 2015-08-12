TaupeGoons ICFP Contest 2015 Postmortem
=======================================

Final rank: 22th

The team
--------
The TaupeGoons teams started this year with four members.

* Marc
* Jean-Julien 
* Loic who started to work by himself and eventually submitted under the GaupeToons team
* Laurent who had some computer issues which lead him to forfeit

Apart from Marc, the three team members were new to the contest.

The problem
-----------

This year problem consisted in writing an AI for TETRIS with an hexagonal grid and variable pieces (some of them being not connected and/or with an external rotation pivot).

Each movement could be coded with different ascii character enabling the inclusion of plain english sentences in solution.

Part of the contest consisted in finding 18 hidden *power phrases* which gave more points when appearing in a solution.

## Our solution

Marc did a generic BFS solver in Ocaml:

* start with a piece in its spawning position
* do a breadth-first search using allowed move
* record all positions where a move is invalid (above the current board height, there's no point in locking a piece on top of an empty board)

To take into account the power phrases, we used the following observation: when doing a breadth-first search the move used when seeing a new position will always be chosen to move to that position. We start the visit of a position by adding possible target positions after having played a power phrase. Of course such position might be valid but not the intermediary positions taken when using the power phrase so it needed to be taken into account. The fact that this step was first ensured that if a position could be attained by using a power phrase it will always be.

Then we score each of these final positions using various criteria:

* number of empty cells around the bottom of the piece (to minimize)
* number of filled or almost filled lines (to maximize)
* number of enclosed empty cells downward the piece (to minimize)
* number of power phrases used in the path (to maximize)

and we choose the best one to play it.

For gathering the different criteria into one unique value we needed a weighted sum. To find the best parameters to use, Jean-Julien designed a genetic programming finder. He did find a good one that we used in the final submission.

Finally, to handle multitasking and scheduling a little Python script was made. The approach was to try first the small board.

### Videos of some of ours solutions
* [![Problem 7](http://img.youtube.com/vi/npVK8Q8moA8/0.jpg)](http://www.youtube.com/watch?v=npVK8Q8moA8)
* [![Problem 12](http://img.youtube.com/vi/Wd1MTq7aXVA/0.jpg)](http://www.youtube.com/watch?v=Wd1MTq7aXVA)
* [![Problem 18](http://img.youtube.com/vi/wVeUVFWgGy0/0.jpg)](http://www.youtube.com/watch?v=wVeUVFWgGy0)

## What we did during the contest
### Friday afternoon
The common language of our starting team being Python we started to work in this language. 

As Marc was well versed in the arcane of json and web submission, he started to work on a valid submission system as soon as possible. Marc did an interactive interface where a human could play the game and submit it to the server. All of these submission lead to errors and we spent a lot of time trying to find them.

### Friday evening
Everybody was searching strategies to solve the problem.

To be able to get started, Marc wrote a crude ncurses visualization and a stupid BFS solver. Using ncurses proved to be really bad as any error broke our console. The BFS solver was not so bad and we kept it.

Batch submission of the problems were made, a lot of errors on some puzzle.

### Saturday morning
The night batch proved us that Python was not fit for the job, so Marc started rewriting everything in Ocaml. Laurent tried to install ubuntu to be able to work with us.

Using the sample play provided by the contest organizers we found stupid bugs and fixed them.

### Saturday at launch
Loic came to Marc's house for a BBQ and it was decided that Loic could work by himself on a solution. He ended up entering the contest with the GaupeToons team.

### Saturday afternoon
The Ocaml version was almost working when Laurent started to work on finding phrases of power in a solution, it was the last time we heard of him as we learned after the end of the contest that he was trapped in the black hole of an ubuntu installation ;-)

### Saturday evening
The Ocaml version that Marc did was working and used a lot of different parameters for the scoring function. Jean-Julien started to work on a genetic algorithm to find the best parameters.

### Sunday morning
Marc found how to include phrases of power in the solver by making them extended moves, adding yet another parameter for Jean-Julien optimizer.

### Sunday afternoon and evening
The hunt for power phrases began and Marc did small optimizations of the solver.

### Monday morning
It was time to work on a task-scheduler for the final submission.

Unfortunately a bad version of the scheduler was submitted, we did contact the organizers 5 minutes after the end of the contest and we hope they will make the 10 characters changes if we enter the final.

### Thoughts
We lost a day by choosing Python and then switching to Ocaml.

Marc thought that having a working base of code would help others working on the real issue. It was wrong because it ended up being too lengthy to get into and other team members had issues modifying an already long file.

It's the real reason of the GaupeToons split and it's noteworthy that Loic did this well while starting really late (basically Saturday nigh).

## Marc's review of the contest

### Pros

* a *simple* problem to which it was easy to give a poor solution (contrasting with last year were writing a compiler was almost mandatory)
* a Lovecraft theme well developed by the organizers (even when doing small talk on IRC)

### Cons

* a *simple* problem which in the end looked like a glorified algorithms 201 exercise
* no last minute twist which made us fine tuning our algorithm instead of programming the last day
* a lacking online leaderboard which was not well tested before, and with a 3 days contest you can't lose time fixing it
* no reference implementation and with leaderboard issues it made tracking stupid mistakes harder
* no clear instructions on what was taken into account for the final round, we didn't knew that the leaderboard was meaningful until late and we still have no clue to what will be important in the final problems: making lines or scoring power phrases
* the whole power phrases charades were really not needed, finding special prefixes in 2007 challenge was cool because it was self-contained, everything was in the endo dna. Here we had to google for book we didn't want to know and do some trial-and-error to get which word was important on the wiki page. It has nothing to do with a *programming* challenge

So for me it was not a great year. But I'd say I'm biased because the contest teaser hinted at 2006 or 2007 type of contest (computational archeology, turing machines, ...) and I was really hyped. Therefore when the task was announced I felt quite disappointed.
