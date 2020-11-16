## Project Update
This document will keep track of the project updates as per the instruction listed in the syllabus.


### Update 1-(due Nov 17, 2020)
In the first phase of the project, we started working on data preparation and feature engineering. 
As the goal of the project is to predict outcome of the match given previous statistics, we first work on extracting some useful features from the raw data which can be found here. 
[Link to data](https://www.kaggle.com/saife245/english-premier-league)

##### Data Engineering
We extracted the folllowing features from the data:
1. Strength of a team: We measure strength of the team by it's position (standing) in the last year's leagure. Every season, bottom three teams in the table are relegated from the next year's league and three new teams enter the league. Hence, sometimes it happens that we do not have information of team's performance in the last season's league. In such cases, we assign a position of 30 to the teams which did not play in the league in last season.
2. Current form: Current form of the home and away teams: Each season of EPL goes for almost 8-9 months. Hence, players go through different phases of performance and injuries which also affects the team's performance as a whole. To account for this behavior, we measure current form of the team by calculating exponential decay function of number of goals scored in the last 5 games with he most recent game getting the highest weight. 

$Form=\sum_{i=1}^5ST*(\exp^{-\alphai})$ where ST stands for shots on target.

We believe that shots on target is a better measure of performance than goals as it's a better measure of team's attacking tactics than goals. 

3. Previous encounters: If Team A (Home) is facing Team B, we all account for all previous encounters of those teams. In particular, we give a point of +1 to Team A for a win in the past, 0 for a draw, and -1 for a loss. We then take an average of these scores.
4. Recent performance: To account for team's recent performance, we also consider an average of the following stats over the last three games. \
FTHG = Full Time Home Team Goals\
FTAG = Full Time Away Team Goals\
FTR = Full Time Result (H=Home Win, D=Draw, A=Away Win)\
HTHG = Half Time Home Team Goals\
HTAG = Half Time Away Team Goals\
HTR = Half Time Result (H=Home Win, D=Draw, A=Away Win)\
HS = Home Team Shots\
AS = Away Team Shots\
HST = Home Team Shots on Target\
AST = Away Team Shots on Target\
HHW = Home Team Hit Woodwork\
AHW = Away Team Hit Woodwork\
HC = Home Team Corners\
AC = Away Team Corners\
HF = Home Team Fouls Committed\
AF = Away Team Fouls Committed\
HY = Home Team Yellow Cards\
AY = Away Team Yellow Cards\
HR = Home Team Red Cards\
AR = Away Team Red Cards\
Note that we calculate this separately for a home or  an away game. This implies that if a team is playing a home game, we only consider last 3 home games to measure the recent performance of team in home games. Similarly for an away game, we only consider last three away games of the team.

We converted the targets (Win, Loss, Draw) to numerical features which are equivalent to +1,0,-1 respectively.

We wrote the data with the required features and target variables to the file finalData.csv in data/archieve/Datasets/finalData.csv

Our preliminary data engineering can be found in src/data.ipynb

##### Models
We tested performance of Support Vector Machine. As our problem is a multi-classification problem (Win, Loss, Draw),  we use a one vs rest strategy. Preliminary experiments showed accuracy in the range of 0.65-0.70 on training and testing set.

