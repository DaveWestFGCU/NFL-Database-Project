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


# Tools used
<img width = "450" img height = "175" alt="postgresimg" src = "https://tse2.mm.bing.net/th?id=OIP.5eAy58BXR6eyTD5BDjFbAwHaDZ&pid=Api&P=0&h=180">
<img width = "450" img height = "175" alt="jupyternotebookimg" src = "https://tse3.mm.bing.net/th?id=OIP.BWugDHBz7qW9EOPZfSk7fgHaFx&pid=Api&P=0&h=180">
<img width = "450" img height = "175" alt="Pythonimg" src = "https://tse4.mm.bing.net/th?id=OIP.9S6VtzL6bysprm6gao1uugHaEK&pid=Api&P=0&h=180">

