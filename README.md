# <p align="center">NFL Management System</p>

## Introduction

### Project Objective

The goal of this database project is to create a relational database that models a football league management system by implementing data engineering concepts. Our group expects to manage teams, players, offense and defense, statistics, divisions, and conferences of the football league. The management system will be organized in an efficient and clear manner allowing for querying the data smoothly and updating it when needed. It is implemented using tools like postgreSQL, Jupyter Notebook, and Python (For scraping NFL data online).

### Project Scope

The football league will be restricted only to one league, the National Football League (NFL). We will not include any college football leagues or minor professional leagues like the United States Football League (USFL) or Xtreme Football League (XFL). The NFL management system will be confined to the following:
- 32 NFL teams, each with offense, defense, and special teams
- The 2023 NFL season (the regular season which spans from September 2023 to January 2024)
- NFL postseasons from the first Super Bowl (1966) until the most recent at the time of this project (2023)
- Game and player specific statistics
- Game fixtures
- Stadium statistics 

We will not include any support staff (referees, medical, athletic training) for the NFL teams. The player reserves or tracking of injuries for the team will not be included. We will keep records for each of the 17 regular-season games that all teams participate in the 2023 NFL season, as well as post-season games for teams that qualify for the playoffs and Superbowl.  

Furthermore, we decided to include postseason game statistics from the 1966 postseason to the 2023 postseason to link NFL teams to their championships. For the previous post seasons leading up to the 2023 season, we want data that only includes the postseason games in order to answer questions regarding playoffs, championships, and superbowls (It is important to note that no player data was considered from those previous seasons)

## Entity-Relationship Diagram

<p align="center"><img src="/readme/images/E-R Diagram.png"/></p>

## Relational Model: 
From the ER diagram above, the following schemas for the NFL management system were developed.
1. The entity sets and their attributes are shown below, having their primary keys underlined
   - Player (<ins>id</ins>, name, height, weight, age, position, jersey, birth_date, years_played)
   - Team (<ins>mascot</ins>, location, coach, wins, losses, standing)
   - Stadium (<ins>city</ins>, <ins>state</ins>, name, address, capacity, turf type)
   - Game (<ins>game_id</ins>, week, game_date, home_team, away_team, home_score, away_score)
   - Season (<ins>year</ins>, start_date, end_date) 

3. Schemas derived from the relationships sets in the Entity Relationship (ER) diagram
   - Plays (<ins>player_id</ins>, <ins>team_mascot</ins>)
   - Homes (<ins>team_mascot</ins>, <ins>stadium_city</ins>, <ins>stadium_state</ins>)
   - Game_season (<ins>game_id</ins>, <ins>season_year</ins>)
   - Offense_game_stats(<ins>player_id</ins>, <ins>game_id</ins>, passing_completions, passing_attempts, passing_yards, rushing_attempts, rushing_yards, fumbles)
   - Defense_game_stats(<ins>player_id</ins>, <ins>game_id</ins>, tackles, sacks, fumbles_recovered, interceptions, passes_defended)
   - Special_game_stats(<ins>player_id</ins>, <ins>game_id</ins>, field_goals, fg_attempts, extra_points, ep_attempts, punts, punt_yards) 

To overcome the redundancy of our data, we applied normalization principles up to 3rd Normal Form (3NF). As ‘plays’ is a many-to-one relationship between ‘player’ and ‘team’, we are able to combine ‘plays’ with ‘player’. Similarly, ‘homes’ describes a many-to-one relationship between ‘team’ and ‘stadium’ (as some teams share a stadium), and ‘game_season’ represents a many-to-one relationship between ‘game’ and ‘season’ (as many games belong to one season). Therefore, we were able to merge homes into team and game_season into game. 

Thus, we derived the following schemas after applying normalization techniques: 
- Player (id, name, height, weight, age, position, jersey, birth_date, years_played)
- Team (mascot, location, coach, wins, losses, standing)
- Stadium (city, state, name, address, capacity, turf type)
- Game (game_id, week, game_date, home_team, away_team, home_score, away_score)
- Season (year, start_date, end_date)
- Offense_game_stats(player_id, game_id, passing_completions, passing_attempts, passing_yards, rushing_attempts, rushing_yards, fumbles)
- Defense_game_stats(player_id, game_id, tackles, sacks, fumbles_recovered, interceptions, passes_defended)
- Special_game_stats(player_id, game_id, field_goals, fg_attempts, extra_points, ep_attempts, punts, punt_yards) 

## Schema Diagram

<p align="center"><img src="/readme/images/Schema Diagram.png"/></p>

## Dataset Description

The dataset includes a handful of related tables that highlights the key components of data every football league should include: 
- Team: Each team is identified by its mascot and is associated with details such as location, coach, home city and state, division, wins, losses, and standing, and relationships between the team and its home stadium.
- Player: Each player is identified by an id formed by the first 4 letters of their first name, 2 letters of their last name, and a 2-digit number to maintain uniqueness between players that have similar names. The player is associated with details such as their name, team, height, position, birth_date, and college, as well as their weight, age, and years_played. Players are associated with their team through a reference to the team’s mascot.
- Game: Each game is identified by a game_id that is composed of the date of the game and a 3-char abbreviation for the home team. A game is then associated with details such as the season year and type, season week of the game, date of the game, home team, away team, home score, and away score. A game is associated with a season by the season year and type, and the home and away teams by their mascot values.
- Season: Each season is identified by the year in which it started. In addition, the season holds data for its start date and end date.
- Offense_game_stats, Defense_game_stats, and Special_game_stats: These tables hold the statistics for each player for each game. Thus each tuple uses a player ID and game ID combination as the primary key. The ‘stats’ tables were split as offense, defense, and special teams track different statistics, and not needing to record null values for statistics untracked by the player’s role reduces the size of the data.
   - Offense_game_stats: As mentioned, the table’s primary key is a player ID and a game ID. Each tuple also holds values for several statistics for an offense role player in a game, including pass completions, passing attempts, passing yards, rushing attempts, rushing yards, and fumbles. The player ID references the ID in the player table, and the game ID references the game_id in the game table.
   - Defense_game_stats: As mentioned, the table’s primary key is a player ID and a game ID. Each tuple also holds values for several statistics for an offense role player in a game, including tackles, sacks, fumbles recovered, interceptions, and passes defended. The player ID references the ID in the player table, and the game ID references the game_id in the game table.
   - Special_game_stats: This table refers to special teams. As mentioned, the table’s primary key is a player ID and a game ID. Each tuple also holds values for several statistics for an offense role player in a game, including field goals, field goal attempts, extra points, extra point attempts, punts, and punt yards. The player ID references the ID in the player table, and the game ID references the game_id in the game table.
- Stadium: Stadiums are identified by their city and state and are associated with details such as their name, seating capacity, and turf type. Each stadium is connected to the team that calls it home.

The dataset will be designed to support queries to view the league’s history, such as team statistics, individual player statistics, or statistics by game, as well as organize for the future such as finding information for upcoming matches. 

## Table Creation

Included is the SQL file for creating tables for the NFL Management System database project (Team-3-DDL.sql). The tables our group decided to implement are: stadium, team, player, season, game, offense_game_stats, defense_game_stats, and special_game_stats. More detail about each table’s primary key and foreign key (if used) is explained below. 

The primary key for the stadium table is its city and state. For the team table, the primary key is the mascot and home is a foreign key to the home_city and home_state. For the player table, the primary key is the id and has a foreign key for the team which references the team’s table mascot. For the season, its primary key is the year. For the game table, the primary key is the game_id, the game’s season_year and season_type is a foreign key to season’s year and type, home_team is a foreign key that references the team's mascot, and away_team is a foreign key that references the team’s mascot. For the offense_game_stats table, the primary key is the player_id and game_id, the player_id is a foreign key that references the player’s id, and the game_id is a foreign key that references the game’s game_id. The same primary keys and foreign keys are applied to the defense_game_stats and special_game_stats table just like the offense_game_stats table.  

More detail regarding the attributes of each table is provided below: 

| Relation | Attribute | Domain |
| -------- | --------- | ------ |
| stadium | name | varchar(50) |
| stadium | city | varchar(50) |
| stadium | state | char(2) |
| stadium | address | varchar(50) |
| stadium | capacity | int |
| stadium | turf_type | varchar(50) |


| Relation | Attribute | Domain |
| -------- | --------- | ------ |
|  team  | mascot | varchar(50) |
|  team  | location | varchar(50) |
|  team  | coach | varchar(50) |
|  team  | home_city | varchar(50) |
|  team  | home_state | varchar(50) |
|  team  | division | varchar(50) |
|  team  | wins | int |
|  team  | losses | int |
|  team  | standing | int |

| Relation | Attribute | Domain |
| -------- | --------- | ------ |
| player | id | varchar(8) |
| player | name | varchar(50) |
| player | team | varchar(50) |
| player | height | varchar(4) |
| player | weight | numeric(3,0) |
| player | age | numeric(2,0) |
| player | position | varchar(3) |
| player | jersey | numeric(2,0) |
| player | birth_date | varchar(10) |
| player | years_played | smallint |
| player | college | varchar(50) |

| Relation | Attribute | Domain |
| -------- | --------- | ------ |
| season | year | numeric(4,0) |
| season | start_date | numeric(4,0) |
| season | end_date | numeric(4,0) |

| Relation | Attribute | Domain |
| -------- | --------- | ------ |
|  game  | game_id | varchar(12) |
|  game  | season_year | numeric(4,0) |
|  game  | week | varchar(9) |
|  game  | game_date | date |
|  game  | home_team | varchar(50) |
|  game  | away_team | varchar(50) |
|  game  | home_score | int |
|  game  | away_score | int |

| Relation | Attribute | Domain |
| -------- | --------- | ------ |
| offense_game_stats | player_id | varchar(8) |
| offense_game_stats | game_id | varchar(12) |
| offense_game_stats | passing_completions | int |
| offense_game_stats | passing_attempts | int |
| offense_game_stats | passing_yards | int |
| offense_game_stats | rushing_attempts | int |
| offense_game_stats | rushing_yards | int |
| offense_game_stats | fumbles | int |

| Relation | Attribute | Domain |
| -------- | --------- | ------ |
| defense_game_stats | player_id | varchar(8) |
| defense_game_stats | game_id | varchar(12) |
| defense_game_stats | tackles | int |
| defense_game_stats | sacks | int |
| defense_game_stats | fumbles_recovered | int |
| defense_game_stats | interceptions | int |
| defense_game_stats | passes_defended | int |

| Relation | Attribute | Domain |
| -------- | --------- | ------ |
| special_game_stats | player_id | varchar(8) |
| special_game_stats | game_id | varchar(12) |
| special_game_stats | field_goals | int |
| special_game_stats | fg_attempts | int |
| special_game_stats | extra_points | int |
| special_game_stats | ep_attempts | int |
| special_game_stats | punts | int |
| special_game_stats | punt_yards | int |

## Data Popoulation

Included is the SQL file for populating data for the NFL Management System database project (Team-3-Data.sql). Our group wrote a webscraper in Python using BeautifulSoup4 to obtain the data from the website \[redacted\]. The webscraper’s code located in the webscraper directory within this project. The webscraper goes to each team page to scrape the player roster, then goes to each player’s page to scrape the games they played and their statistics for that game. Then it goes to the season’s schedule to scrape the data for each game played in the season. Data not scraped, such as the stadium, season, and team data, was manually input from sources such as Wikipedia. After the program gets the data, it formats it into SQL’s data manipulation language and creates a SQL input file. 

## Query Questions 

As a part of our assignment, we created 10 questions that could be answered with our database and the queries to do so (Team-3-Queries.sql). The questions are as follows:

1. A Colt’s fan wants to learn more about the subtotals and grand totals of the offensive player’s yards run on the Colt’s team. Using rollup, list how many yards (rush yards and pass yards) each offensive player has gained on a team in a given year, and then how many rush yards and pass yards they have gained by each position in the 2023 season.

2. A sport’s analyst wants to create a report about quarterbacks rank based on the number of yards thrown. List the rankings of the quarterback's names in ascending order (Having the rank 1 quarterback at the top) along with the total number of yards thrown for each quarterback.

3. NFL defensive coaches want to analyze the key defensive player positions (linebacker, safety/defensive backs) on their opponents’ teams. Create a query that shows the total number of tackles performed by these positions by pivoting about the position names. Include the name of the team as well.

4. NFL fans want to know about old players (above the age of 30) in the NFL league who play on key special teams positions (kicker, punter, kick returner, punt returner). Find players that play in the current season as a kicker (K), punter (P), kick returner (KR) or punt returner (PR) and who are over the age of 30. Include the age of these player’s as well.

5. A Colts fan wants to know what teams the Colts have won against in the 2023 season. Write a query that lists every team the Colts have won against and the date their game occurred.

6. A sports analyst regularly needs to access the data from previous year’s SuperBowls for comparison. Create a function that accepts a season year as input and outputs the data for that season’s superbowl including: Superbowl number, participating teams, and each team’s score.

7. An offensive NFL coach wants to know more about the quarterback Patrick Mahomes' progress in cumulative passing yards. Create a window showing the progress of Patrick Mahomes by cumulative passing yards thrown each game date he plays in the current regular season.

8. A new fan wants to choose a favorite team. He wants a team that frequently makes it to the AFC or NFC conference championship game, meaning they frequently have good seasons. Write a query that will display the home and away teams for the conference championship games, organized by year and which conference they belonged to.

9. The fan from the above question now wants to move to the location of the stadium that has hosted the most conference championship games. Rank the stadiums by the number of conference championship games they have hosted. Include their city and state.

10. A common point of pride for an NFL team is how many Super Bowl championship games they have won throughout their history. Create a query that ranks teams by Super Bowl wins, excluding teams that have not won a SuperBowl.  

## Accessing via Python

Using Jupyter Notebook, SQLAlchemy, and Pandas, we replicated the 10 query questions our team had above (found in Team-3.ipynb). Queries 1-5 were performed using [SQLAlchemy](https://docs.sqlalchemy.org/en/20/contents.html) to query the database for exactly the data our queries seek to answer. Queries 6-10, however, required using tools such as views and functions, which are poorly documented. Further, searching websites such as Stack Overflow primarily yielded syntax from SQLAlchemy 1.4, while the current version (2.0) has had significant changes. As such, we loaded Pandas DataFrames with the values from our tables and used Pandas functions to find the answers to our queries. In all cases, the results are displayed in a DataFrame. 

## Conclusion

Overall, our team enjoyed the process of applying the data engineering concepts learned to a project that we were interested to learn more about, an NFL management system. We had to learn not only about databases, but about the NFL, as each of us had assumed our other two teammates had in-depth knowledge about the NFL when we selected the topic. Our team learned how to apply intermediate and advanced concepts in SQL such as joins, functions, views, and windowing for extracting questions our team really wanted to answer regarding our NFL data. Furthermore, our team has learned that it is important to carefully design the schema for the NFL management system before actually trying to query any data. 

# Tools Used

<img width = "450" img height = "175" alt="postgresimg" src = "https://tse2.mm.bing.net/th?id=OIP.5eAy58BXR6eyTD5BDjFbAwHaDZ&pid=Api&P=0&h=180">
<img width = "450" img height = "175" alt="jupyternotebookimg" src = "https://tse3.mm.bing.net/th?id=OIP.BWugDHBz7qW9EOPZfSk7fgHaFx&pid=Api&P=0&h=180">
<img width = "450" img height = "175" alt="Pythonimg" src = "https://tse4.mm.bing.net/th?id=OIP.9S6VtzL6bysprm6gao1uugHaEK&pid=Api&P=0&h=180">

