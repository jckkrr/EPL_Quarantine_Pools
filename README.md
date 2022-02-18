
# EPL QUARANTINE POOLS

At the start of the COVID-19 lockdown in the UK, the English Premier League was left with 92 games to play - but no certainty that they could get through and bring the season to an end.

One way to mitigate the risk would have been to put the teams into a set of 'bubbles', each containing half of the competition's 20 teams.

So was there a set of bubbles where the combination of teams would make this work? This analysis used combination theory and dataviz to find out.

Due to the many trillions of calculations that would have been required, it also finds time-saving workarounds to get to the a positive conclusion.



### How many combinations of teams are there?

```
poss = set(itertools.combinations(teams, 10)) 
```
The answer, it turned out was a massive 184,756. That's a lot of possibilities to explore!

### A Look At The Remaining Games
... shows that each team needs to play roughly the same amount of games, and there is no obvious pattern showing how to divide the teams.

![image](https://user-images.githubusercontent.com/69304112/154682255-b9469028-bcea-402b-b61d-c98b6a01f0a2.png)

## BUILDING BUBBLES

Now, let's take a look at each of these 184,756 bubbles and see how many of the remaining games they might be able to get throught.

On my tired laptop, which I used for this project, calculating the 184,000+ possibile combinations of teams usually took less than 90 seconds to calculate.

But calculating their effectiveness was another matter altogether. No pair of bubbles produced more than 60 games. Some produced as few as 28.

In the chart below, which shows the 100 best results in descending order, you can see how far from achieving our target we still are (represented by the gold line).

![image](https://user-images.githubusercontent.com/69304112/154682298-cb7f8638-3ca1-4bc2-ad91-490aa2384a64.png)

Here, below, we can see the frequency of each total the bubbles produced: a beautiful bell curve.

![image](https://user-images.githubusercontent.com/69304112/154680382-ed8e5c11-afc9-4598-b9ef-ced5a44c6592.png)


## DOUBLE BUBBLES

Even with the best result (60), we don't even get through two-thirds of the remaining games. But what if we went through this process again with each set of bubbles' remaining games?

That would involve looking at each groups' remaining games, and calculating again.That's 184,000 x 184,000 calculations: which is over 34 trillion! At 90 seconds each round, my calculator says that would take 181 days.

Even with a very fast computer, this was going to take too long.


## THREE METHODS FOR FASTER COMPUTATIONS

### Removing 'complements'

Every time we create a subset, we leave behind a subset known as the complement.

When we are split a set in half, that means the complement is simply another subset that will be calculated elsewhere.

Therefore, we need to stop doubling up our iterations, by removing these complements (whose games are counted already in the primary subset's tally).

Calculating the initial set of games is now done in half the time!

![image](https://user-images.githubusercontent.com/69304112/154680864-bd7df68a-6655-4cdf-9d4b-e5ac8a2b265f.png)

We've mainted the bell curve - so it looks like the calculations are correct. But we can really see how much we've reduced the workload.

And we now have 4 Bubbles of 60 games, not 8. Now are dealing 92,378 results, instead of twice that much.

However, that would still take at least two days for my computer to process (on my laptop). A huge improvement, but still too long.


### No backtracking

There is no need for 'backtracking' calculations.

Once Group 0 has iterated through all its possible combinations, there is no need for other Bubbles to take it into consideration.

There is also no need for each Bubble to calculate against itself.

So by the time we get to the penultimate Bubble, the only Bubble it needs to check itself with is the last one.

![image](https://user-images.githubusercontent.com/69304112/154681127-f805e879-266e-42cc-9fc9-beb83e7b83d6.png)


By not 'backtracking', this would significantly decreased the time to calculate for each Bubbles other possibilities.

Instead of the 34 trillion we would have needed earlier, there are now just over 4 trillion possibilities. That's just 12 per cent of what we started with.

Below is a quick timetest (involing smaller bubbles) to show how much quicker it gets as it goes through the calculations.

![image](https://user-images.githubusercontent.com/69304112/154681319-60e77242-e850-4fad-891d-b830cff807d0.png)

This is a great result. But it would still take about a day to get through the calucalations though.

### Narrow the search

The goal is to get two combinations of Bubbles that can play 92 different games.

We know that our results for single Bubbles range from 28 to 60.

But two Bubbles of 28 can, at most, achieve a total of 56 - and that's only if they have no overlapping games.

Even boostering a 28 Bubble with a 60 Bubble would only return a maximum score of 88.

So we can see that a lot of Bubble combinations are not worth calculating.

Therefore, we could take those combinations out.

![image](https://user-images.githubusercontent.com/69304112/154681413-bb948236-a446-4436-a886-053ed4c95cac.png)


To stop my computer burning out, I would like to narrow it down further though.

How likely would it be that any low value will end up forming a double-bubble that gets a high result?

The graph below takes all Bubbles with 58 or 60 games, and calculates against the full spectrums of Bubbles.

![image](https://user-images.githubusercontent.com/69304112/154681474-52a39976-98c0-4dfd-b0ed-f45ac56f15c9.png)

As you can see, it is only once we get into the top half of Bubble results that we can achieve at least 87.

(87 is the top result I've been able to find.)

Therefore we can fairly confidently narrow down the search to all Bubbles with reults of 45 or above.

![image](https://user-images.githubusercontent.com/69304112/154681576-388a62e2-1f2d-4020-bb26-786e001074ab.png)

This last step seems justified in the circumstances, even if it not the most scientific. In fact, because of memory issues, I have narrowed it downfurther, to a results of 50 or above.

We now have less than 28,000 combos to work with - so much more manageable than a trillion.

## LET'S GO

## FINAL STEP

There are five combinations of double-bubbles found that produce a result of 87 matches.

This still leaves five games remaing, but it is very close to achieveing the desired outcome.

The final step is to see which of these combinations would require the least chopping and changing.

```
16262 ∩ 29409
A1∩B1 = 5 {'LEI', 'ARS', 'MUN', 'EVE', 'AFC'}
A1∩B2 = 5 {'TOT', 'SHE', 'CRY', 'WOL', 'AST'}

18125 ∩ 29227
A1∩B1 = 5 {'LEI', 'ARS', 'SOU', 'EVE', 'AFC'}
A1∩B2 = 5 {'TOT', 'SHE', 'MUN', 'WOL', 'AST'}

18125 ∩ 56457
A1∩B1 = 5 {'TOT', 'AST', 'WOL', 'AFC', 'SHE'}
A1∩B2 = 5 {'LEI', 'ARS', 'SOU', 'MUN', 'EVE'}

29227 ∩ 41148
A1∩B1 = 5 {'LEI', 'ARS', 'SOU', 'EVE', 'AFC'}
A1∩B2 = 5 {'MCI', 'WAT', 'BRI', 'NOR', 'LIV'}

55452 ∩ 65416
A1∩B1 = 5 {'AST', 'CRY', 'EVE', 'AFC', 'SHE'}
A1∩B2 = 5 {'NOR', 'CHE', 'WOL', 'BUR', 'LIV'}
```

As we see, each group has exactly 5 teams that would need to shift from each pool.

So let's look at the games that each combo would have left over.

```
16262 ∩ 29409
Remaining games =
10: EVE vs LIV
76: MUN vs WES
90: SOU vs SHE
28: AFC vs NEW
62: ARS vs LIV
Counter({'LIV': 2, 'EVE': 1, 'MUN': 1, 'WES': 1, 'SOU': 1, 'SHE': 1, 'AFC': 1, 'NEW': 1, 'ARS': 1})

18125 ∩ 29227
Remaining games =
37: LEI vs CRY
39: LIV vs AST
7: AFC vs CRY
25: BRI vs MUN
28: AFC vs NEW
Counter({'CRY': 2, 'AFC': 2, 'LEI': 1, 'LIV': 1, 'AST': 1, 'BRI': 1, 'MUN': 1, 'NEW': 1})

18125 ∩ 56457
Remaining games =
65: CRY vs MUN
68: MCI vs AFC
37: LEI vs CRY
39: LIV vs AST
76: MUN vs WES
Counter({'CRY': 2, 'MUN': 2, 'MCI': 1, 'AFC': 1, 'LEI': 1, 'LIV': 1, 'AST': 1, 'WES': 1})

29227 ∩ 41148
Remaining games =
66: EVE vs AST
73: AST vs ARS
15: LIV vs CRY
25: BRI vs MUN
28: AFC vs NEW
Counter({'AST': 2, 'EVE': 1, 'ARS': 1, 'LIV': 1, 'CRY': 1, 'BRI': 1, 'MUN': 1, 'AFC': 1, 'NEW': 1})

55452 ∩ 65416
Remaining games =
2: NOR vs SOU
68: MCI vs AFC
73: AST vs ARS
89: NEW vs LIV
91: WES vs AST
Counter({'AST': 2, 'NOR': 1, 'SOU': 1, 'MCI': 1, 'AFC': 1, 'ARS': 1, 'NEW': 1, 'LIV': 1, 'WES': 1})
```



There are two combos that only require eight teams. These are:

```
18125 ∩ 29227

18125 ∩ 56457
```

As the main point of having bubbles is to limit movement around the UK (to stop the spread of the virus), it clearly comes down to a choice between these two combos.

Picking one of these pools means Crystal Palace will play just 7 games in the initial stages, compared to the 9 played by most teams and the 10 played by some. But scheduling concerns are far outweighed by the need to limit movement.

Let's look at how each of these teams features in the final two combos.

![image](https://user-images.githubusercontent.com/69304112/154681988-5d027a83-86e9-4a8f-8669-2859e43f78b3.png)

So it is that Bubbles 18125 and 56427 are the ideal combination!

```
First Bubbles
A1 ['AFC', 'ARS', 'AST', 'EVE', 'LEI', 'MUN', 'SHE', 'SOU', 'TOT', 'WOL']
A2 ['BRI', 'BUR', 'CHE', 'CRY', 'LIV', 'MCI', 'NEW', 'NOR', 'WAT', 'WES']

Second Bubbles
B1 ['AFC', 'AST', 'BUR', 'CHE', 'CRY', 'MUN', 'SOU', 'WAT', 'WES', 'WOL']
B2 ['ARS', 'BRI', 'EVE', 'LEI', 'LIV', 'MCI', 'NEW', 'NOR', 'SHE', 'TOT']
```
![image](https://user-images.githubusercontent.com/69304112/154682097-367548da-34c5-4178-854f-d180a2ce6389.png)



Blue = team is in main pool of the bubble. Red = in complementary pool.

We can see here which teams will be moving (they can colors) and which will be staying put (no change).

And with that, my poor old computer needs a rest!

