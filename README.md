# <p align="center">NFL Management System</p>
## Introduction
### Project Objective:
The goal of this database project is to create a relational database that models a football league management system by implementing data engineering concepts. Our group expects to manage teams, players, offense and defense, statistics, divisions, and conferences of the football league. The management system will be organized in an efficient and clear manner allowing for querying the data smoothly and updating it when needed. It is implemented using tools like postgreSQL, Jupyter Notebook, and Python (For scraping NFL data online).
### Project Scope:
The football league will be restricted only to one league, the National Football League (NFL). We will not include any college football leagues or minor professional leagues like the United States Football League (USFL) or Xtreme Football League (XFL). The NFL management system will be confined to the following:
- 32 NFL teams, each with offense, defense, and special teams
- The 2023 NFL season (the regular season which spans from September 2023 to January 2024)
- NFL postseasons from the first Super Bowl (1966) until the most recent at the time of this project (2023)
- Game and player specific statistics
- Game fixtures
- Stadium statistics 

We will not include any support staff (referees, medical, athletic training) for the NFL teams. The player reserves or tracking of injuries for the team will not be included. We will keep records for each of the 17 regular-season games that all teams participate in the 2023 NFL season, as well as post-season games for teams that qualify for the playoffs and Superbowl.  

Furthermore, we decided to include postseason game statistics from the 1966 postseason to the 2023 postseason to link NFL teams to their championships. For the previous post seasons leading up to the 2023 season, we want data that only includes the postseason games in order to answer questions regarding playoffs, championships, and superbowls (It is important to note that no player data was considered from those previous seasons)

## Entity-Relationship Diagram:
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

## Schema Diagram:
<p align="center"><img src="/readme/images/Schema Diagram.png"/></p>

## Dataset Description :

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

# Tools used
<img width = "450" img height = "175" alt="postgresimg" src = "https://tse2.mm.bing.net/th?id=OIP.5eAy58BXR6eyTD5BDjFbAwHaDZ&pid=Api&P=0&h=180">
<img width = "450" img height = "175" alt="jupyternotebookimg" src = "https://tse3.mm.bing.net/th?id=OIP.BWugDHBz7qW9EOPZfSk7fgHaFx&pid=Api&P=0&h=180">
<img width = "450" img height = "175" alt="Pythonimg" src = "https://tse4.mm.bing.net/th?id=OIP.9S6VtzL6bysprm6gao1uugHaEK&pid=Api&P=0&h=180">

