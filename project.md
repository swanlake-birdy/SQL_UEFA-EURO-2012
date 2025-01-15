Data Available --> http://sqlzoo.net/euro2012.sql

1. Display Match Details for German Goals, retriving the matchid and the player name for all goals scored by German players.

```sql
SELECT matchid, player
From goal 
WHERE teamid = 'GER';
```

2. Step 2: List Teams and Players for Goals Scored by Mario
Create a query to display the team1, team2, and the player for every goal scored by a player whose name starts with "Mario". Use a condition with player LIKE 'Mario%' to filter the results.

```sql
SELECT game.team1, game.team2, goal.player
FROM game
JOIN goal ON game.id = goal.matchid
WHERE player LIKE 'Mario%'
```

3. Step 3: Retrieve Goal Details in the First 10 Minutes
Join the goal table with the eteam table using teamid = id. Write a query to display the player, teamid, coach, and gtime for all goals scored within the first 10 minutes of the match. Use a condition gtime <= 10 to filter the results.

```sql
SELECT goal.player, goal.teamid, eteam.coach, goal.gtime
FROM goal
JOIN eteam ON goal.teamid = eteam.id
WHERE goal.gtime <= 10;
```

4. List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach

```sql
SELECT game.mdate, eteam.teamname
FROM game
JOIN eteam ON game.team1 = eteam.id
WHERE coach = 'Fernando Santos';
```

5. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

```sql
SELECT goal.player
FROM goal
JOIN game ON goal.matchid = game.id
WHERE game.stadium = 'National Stadium, Warsaw';
```

6. Show teamname and the total number of goals scored.

```sql
SELECT eteam.teamname, COUNT(goal.teamid) 
FROM eteam
JOIN goal ON eteam.id = goal.teamid
GROUP BY teamid
ORDER BY 1
```

7. Show the stadium and the number of goals scored in each stadium.

```sql
SELECT game.stadium, count(goal.teamid)
FROM game
JOIN goal ON game.id = goal.matchid
GROUP BY game.stadium
ORDER BY 1
```

8. For every match involving 'POL', show the matchid, date and the number of goals scored.

```sql
SELECT goal.matchid, game.mdate, count(game.id) as count
FROM goal
JOIN game ON game.id = goal.matchid
WHERE game.team1 = 'POL' OR game.team2 = 'POL'
GROUP BY game.id
```

9. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

```sql
SELECT goal.matchid, game.mdate, count(game.id) as count
FROM goal
JOIN game ON game.id = goal.matchid
WHERE goal.teamid = 'GER'
group by game.id
```
10. List every match with the goals scored by each team as shown.

```sql
SELECT 
    game.mdate,
    game.team1,
    SUM(CASE WHEN goal.teamid = game.team1 THEN 1 ELSE 0 END) AS score1,
    game.team2,
    SUM(CASE WHEN goal.teamid = game.team2 THEN 1 ELSE 0 END) AS score2
FROM game
LEFT JOIN goal ON game.id = goal.matchid
GROUP BY game.mdate, game.team1, game.team2
ORDER BY game.mdate, game.id, game.team1, game.team2;
```
