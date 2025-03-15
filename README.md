# Tracking Top Clash Royale Player Performances & Strategies via Official API

## Introduction

This project pulls game data from the global hit strategy game, [Clash Royale](https://en.wikipedia.org/wiki/Clash_Royale), via its official API and aims to provide recent match performance and strategic meta insights for the top 100 Path of Legend game mode players (ranked by season 2/25 final results). The work utilizes Python to make the API requests and perform EDA and Tableau for reporting. While mostly a fun passion project, it is a viable way to track ones favorite elite players, what cards they're using / winning with / losing to, etc.

## Key Outputs

The final product and insights of this project are showcased in the following:

[Clash Royale Interactive Tableau Dashboard](https://public.tableau.com/views/ClashRoyaleDashboard/BattleLogDashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

**Dashboard Preview 1:** Recent Battle Log - focuses on recent match performance for selected player.

<img src="visualizations/01. battle_log_dashboard.png" alt="battle_log_dashboard" width="1000"/>

**Dashboard Preview 2:** Meta Analysis - highlights card usage and performance across top players and their opponents.

<img src="visualizations/02. meta_analysis_dashboard.png" alt="meta_analysis_dashboard" width="1000"/>

## Prerequisites

- Python with the following libraries:
	- pandas
	- config
	- requests
	- matplotlib
- Tableau

## Data Source

The data for this project is pulled from the [Official Clash Royale API](https://developer.clashroyale.com/#/getting-started), which provides near realtime game data across all players and game modes. Note that an API key is required to access the data (see below).

## How to Use

1. Visit the [Official Clash Royale API](https://developer.clashroyale.com/#/getting-started) to create a developer account and receive an API key.

2. Create a config.py file and in it write and save:
	```
	API_KEY = 'put given api key here'
	```
3. Download the [Exploring Clash Royale Data.ipynb](https://github.com/Yishak-Ali/Clash-Royale-API-Data-Analysis/blob/main/Exploring%20Clash%20Royale%20Data.ipynb) script and upload to your local Jupyter Notebook environment.

3. In the .ipynb script, modify code chunk # 4, line 1 to select which season's top 100 players to examine during the current running season (optional):
	```
	season = 'replace with relevant season in yyyy-mm format'
	```
	Note: the API only provides seasonal global Path of Legend rankings up to the previous season (not current running season); default is set to 2025-02 season in script.

4. Additionally, update code chunk # 6, line 1 to match the time frame of match data analyzed to current season:
	```
	current_season = pd.Series(['first day of current season in yyyy-mm-dd H:M format', 'last day of current season in yyyy-mm-dd H:M format']) 
	```
	Note: to get the first and last days, search for the range of the current season on Google and use 10:00 for the H:M section; currently set to filter between 2025-03-03 10:00 and 2025-04-07 10:00, the range of season 3/25.

5. Uncomment all 3 lines of the last code chunk in the script, under the 'Table export' section, to ensure data tables export as Excel files.

6. Run the .ipynb script.

7. To set up in Tableau, import and connect the 3 resulting Excel files in Tableau; note that self joins of team_deck_df and opponent_deck_df tables on Battle Time = Battle Time and Card Name <> Card Name are required to replicate the dashboards exactly (mainly sankey chart).

## Limitations & Future Work

The biggest limitation of this project is that it doesn't update underlying data automatically. In order to refresh, a manual API request by running the .ipynb script is required. This new data then needs to be exported and connected to Tableau to update the dashboards.

Another limitation is that the API only returns up to the last 30 games of a given player at the time of request. This means we're limited to analyzing a max of 30 matches per player and a max of 3000 total matches, assuming each player has played 30 consecutive Path of Legend matches in their last 30 games. This presents one area of future work in automating the .ipynb script such that it makes periodic API requests and stores the cleaned responses. This way we can collect data and analyze trends over a longer time frame.