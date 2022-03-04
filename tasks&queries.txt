1. Finding a team with the highest average weight of its batters on a given year(Heaviest Hitters):

SELECT  
batting.yearid AS "year", 
ROUND(AVG(people.weight),1) AS "average weight",
teams.name AS "team"
FROM people
JOIN batting ON people.playerid = batting.playerid
JOIN teams ON batting.team_id = teams.name
GROUP BY 3
ORDER BY 1 DESC, 2 DESC;

2. Finding a team with the smallest average height of its batters on a given year(Shortest Sluggers):

SELECT  
batting.yearid AS "year", 
ROUND(AVG(people.height), 1) AS "average height",
teams.name AS "team"
FROM people
JOIN batting ON people.playerid = batting.playerid
JOIN teams ON batting.team_id = teams.id
GROUP BY 1, 3
ORDER BY 1 DESC, 2 ASC;

3. Finding the team with the largest total salary of all players in a given year(Biggest Spenders):

SELECT 
salaries.yearid AS "year",
teams.name AS "team name",
SUM(salary) AS "total salary"
FROM salaries
JOIN teams ON salaries.teamid = teams.teamid
GROUP BY 1, 2
ORDER BY 1 DESC, 3 DESC;

4. Finding the team that had the smallest “cost per win” in 2010. Cost per win is determined by the total salary of the team divided by the number of wins in a given year(Most Bang For Their Buck In 2010): 

SELECT 
salaries.yearid AS "year",
teams.name AS "team name",
teams.w AS "number of wins",
ROUND(SUM(salary) / teams.w) AS "cost per win"
FROM salaries
JOIN teams ON salaries.teamid = teams.teamid
AND
salaries.yearid = teams.yearid
WHERE salaries.yearid = 2010
GROUP BY 1, 2, 3
ORDER BY 4 ASC;

5. Finding the pitcher who, in a given year, cost the most money per game in which they were the starting pitcher. The pitcher had to start at least 10 games(Priciest Starter): 

SELECT
pitching.yearid AS "year",
people.namegiven AS "player name",
pitching.gs AS "games started",
pitching.g AS "games played",
ROUND(salaries.salary / pitching.g) AS "cost per game"
FROM pitching
JOIN salaries ON pitching.playerid = salaries.playerid
AND pitching.yearid = salaries.yearid
AND pitching.teamid = salaries.teamid
JOIN people ON people.playerid = salaries.playerid
WHERE pitching.gs >= 10
GROUP BY 1, 2, 3, 4, 5
ORDER BY 1 DESC, 5 DESC;

Alternative solution:

SELECT
pitching.yearid AS "Year",
people.namefirst AS "First Name",
people.namelast AS "Last Name",
pitching.gs AS "Games Started",
pitching.g AS "Games Played",
ROUND(salaries.salary / pitching.g) AS "Cost Per Game"
FROM pitching
JOIN salaries ON pitching.playerid = salaries.playerid
AND pitching.yearid = salaries.yearid
AND pitching.teamid = salaries.teamid
JOIN people ON people.playerid = salaries.playerid
WHERE pitching.gs >= 10
GROUP BY 1, 2, 3, 4, 5, 6
ORDER BY 1 DESC, 6 DESC;
