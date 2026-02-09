# IPL-Dashboard
Indian Premier League (IPL) Performance Analysis Dashboard

🔧 Tech Stack:
🐬 SQL for data querying
📊 Power BI Desktop for visualization
📈 Excel for data cleaning & manipulation
🧮 DAX Language for measures & calculations

🎓 Techniques Applied:
📥 Data Collection, Cleaning, and Transformation
🗂️ Data Modeling using Star/Snowflake Schema
🧮 Creating Calculated Columns & Measures
🔄 Data Transformation with Power Query
➗ Advanced DAX Functions (CALCULATE, FILTER, ALL, ALLEXCEPT, RELATED, SELECTEDVALUE, SUMX, YTD)
📊 KPI Cards & Custom Visuals
🏷️ Dynamic Titles Based on Season Filters
🔘 Interactive Slicers & Toggle Buttons
🎨 Conditional Formatting for insights
✔️ Data Validation & Performance Optimization
🌐 Publishing Dashboard to Power BI Service

📈 Key Metrics Analyzed:
🏏 Total Matches, Teams & Venues
🥇 Season-wise Champions & Runners-up
📊 Points Table (Matches, Wins, Losses, Points)
🔥 Top Run Scorers & Wicket Takers
💥 Most Fours & Sixes
📆 Season-wise Performance Trends

💼 Insights Delivered:
📊 Team Performance & Standings Analysis
🏏 Player Performance Comparison
📈 Season-wise Tournament Trends

KPI's : 
1. Total_Centuries = 
 VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seson_wise_data = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn )
VAR batter_run = SUMMARIZE(Ball_By_Ball_Data,Ball_By_Ball_Data[match_id],Ball_By_Ball_Data[batter],"Totalruns",SUM(Ball_By_Ball_Data[batter_runs]))
VAR cnt = FILTER(batter_run,[Totalruns]>=100) 
return COUNTROWS(cnt)

2.Total_Fours = CALCULATE(COUNT(Ball_By_Ball_Data[batter_runs]),Ball_By_Ball_Data[batter_runs]=4,SUMMARIZE(Ball_By_Ball_Data,IPL_Matches_Data[season]))

3.Total_HalfCenturies = 
 VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seson_wise_data = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn )
VAR batter_run = SUMMARIZE(Ball_By_Ball_Data,Ball_By_Ball_Data[match_id],Ball_By_Ball_Data[batter],"Totalruns",SUM(Ball_By_Ball_Data[batter_runs]))
VAR cnt = FILTER(batter_run,[Totalruns]>=50 && [Totalruns]<=100) 
return COUNTROWS(cnt)

4.Total_Matches = CALCULATE(DISTINCTCOUNT(IPL_Matches_Data[match_id]))
5.Total_Sixes = CALCULATE(COUNTROWS(Ball_By_Ball_Data),Ball_By_Ball_Data[batter_runs]=6,KEEPFILTERS(VALUES(IPL_Matches_Data[season_id])))
6.Total_Teams = CALCULATE(DISTINCTCOUNT(Ball_By_Ball_Data[team_batting]))
7.Total_Venues = DISTINCTCOUNT(IPL_Matches_Data[venue])

Measures :
1.Matches_Lost = 
var ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR team1lm = CALCULATE(COUNTROWS(IPL_Matches_Data),USERELATIONSHIP(IPL_Matches_Data[team1],Teams_Data[team_name]),
                       IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_type]="T20",not ISBLANK(IPL_Matches_Data[match_number]),
                       IPL_Matches_Data[match_winner]<> IPL_Matches_Data[team1])
VAR team2lm = CALCULATE(COUNTROWS(IPL_Matches_Data),USERELATIONSHIP(IPL_Matches_Data[team2],Teams_Data[team_name]),
                       IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_type]="T20",not ISBLANK(IPL_Matches_Data[match_number]),
                       IPL_Matches_Data[match_winner]<>IPL_Matches_Data[team2]) 
return team1lm+team2lm

2.Matches_No_Result = 
var ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR team1lm = CALCULATE(COUNTROWS(IPL_Matches_Data),USERELATIONSHIP(IPL_Matches_Data[team1],Teams_Data[team_name]),
                       IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_type]="T20",not ISBLANK(IPL_Matches_Data[match_number]),
                       IPL_Matches_Data[match_winner]<> IPL_Matches_Data[team1],IPL_Matches_Data[result]="no result")
VAR team2lm = CALCULATE(COUNTROWS(IPL_Matches_Data),USERELATIONSHIP(IPL_Matches_Data[team2],Teams_Data[team_name]),
                       IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_type]="T20",not ISBLANK(IPL_Matches_Data[match_number]),
                       IPL_Matches_Data[match_winner]<>IPL_Matches_Data[team2],IPL_Matches_Data[result]="no result") 
return team1lm+team2lm

3.Matches_tied = 
var ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR team1lm = CALCULATE(COUNTROWS(IPL_Matches_Data),USERELATIONSHIP(IPL_Matches_Data[team1],Teams_Data[team_name]),
                       IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_type]="T20",not ISBLANK(IPL_Matches_Data[match_number]),
                       IPL_Matches_Data[match_winner]<> IPL_Matches_Data[team1],IPL_Matches_Data[result]="tie")
VAR team2lm = CALCULATE(COUNTROWS(IPL_Matches_Data),USERELATIONSHIP(IPL_Matches_Data[team2],Teams_Data[team_name]),
                       IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_type]="T20",not ISBLANK(IPL_Matches_Data[match_number]),
                       IPL_Matches_Data[match_winner]<>IPL_Matches_Data[team2],IPL_Matches_Data[result]="tie") 
return team1lm+team2lm

4.Matches_Won = 
var ssn = SELECTEDVALUE(IPL_Matches_Data[season])
var curnteam = SELECTEDVALUE(Teams_Data[team_name])
return CALCULATE(COUNTROWS(IPL_Matches_Data),IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_winner]=curnteam,
                                IPL_Matches_Data[match_type]="T20", NOT ISBLANK(IPL_Matches_Data[result]))

5.Mathces_Played = VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR team1m = CALCULATE(COUNTROWS(IPL_Matches_Data),USERELATIONSHIP(IPL_Matches_Data[team1],Teams_Data[team_name]),
                       IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_type]="T20")
VAR team2m = CALCULATE(COUNTROWS(IPL_Matches_Data),USERELATIONSHIP(IPL_Matches_Data[team2],Teams_Data[team_name]),
                       IPL_Matches_Data[season]=ssn,IPL_Matches_Data[match_type]="T20")  
return team1m+team2m

6.Orange_Cup_Holder = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR ssndata = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn)
var batter_runs = SUMMARIZE(ssndata,Ball_By_Ball_Data[batter],"Totalruns",SUM(Ball_By_Ball_Data[batter_runs]))
VAR maxruns = MAXX(batter_runs,[Totalruns])
VAR topscorer = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(batter_runs,[Totalruns]=maxruns))
RETURN topscorer

7.Orange_cup_image = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR ssndata = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn)
var batter_runs = SUMMARIZE(ssndata,Ball_By_Ball_Data[batter],"Totalruns",SUM(Ball_By_Ball_Data[batter_runs]))
VAR maxruns = MAXX(batter_runs,[Totalruns])
VAR topscorer = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(batter_runs,[Totalruns]=maxruns))
var img = LOOKUPVALUE(Players_Data[player_image],Players_Data[player_name],MAXX(topscorer,Ball_By_Ball_Data[batter]))
-- VAR img = CALCULATE(MAX(Players_Data[player_image]),Players_Data[player_name]=topscorer)
return img

8.Orange_Cup_Runs = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR ssndata = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn)
var batter_runs = SUMMARIZE(ssndata,Ball_By_Ball_Data[batter],"Totalruns",SUM(Ball_By_Ball_Data[batter_runs]))
VAR maxruns = MAXX(batter_runs,[Totalruns])
-- VAR topscorer = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(batter_runs,[Totalruns]=maxruns))
RETURN maxruns

9.Orange_Cup_Team = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR ssndata = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn)
var batter_runs = SUMMARIZE(ssndata,Ball_By_Ball_Data[batter],"Totalruns",SUM(Ball_By_Ball_Data[batter_runs]))
VAR maxruns = MAXX(batter_runs,[Totalruns])
VAR topscorer = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(batter_runs,[Totalruns]=maxruns))
VAR team =  CALCULATE(MAX(Ball_By_Ball_Data[team_batting]),FILTER(ssndata,Ball_By_Ball_Data[batter]=MAXX(topscorer,Ball_By_Ball_Data[batter])))
return team

10.Purple_Cap_holder = 
var season = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasnwickets = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=season && 
                        Ball_By_Ball_Data[is_wicket]=TRUE() && 
                         NOT Ball_By_Ball_Data[wicket_kind] IN { "run out","retired hurt","obstructing the field","retired out" }
                         )
VAR wicktsumry = SUMMARIZE(seasnwickets,Ball_By_Ball_Data[bowler],"wicktcnt",COUNTROWS(FILTER(seasnwickets,Ball_By_Ball_Data[bowler]=EARLIER(Ball_By_Ball_Data[bowler]))))
var maxwickets = MAXX(wicktsumry,[wicktcnt])
VAR topbowler = CALCULATETABLE(VALUES(Ball_By_Ball_Data[bowler]),FILTER(wicktsumry,[wicktcnt]=maxwickets))
RETURN MAXX(topbowler,Ball_By_Ball_Data[bowler])

11.Purple_Cap_Image = 
var ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasnwickets = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && 
                        Ball_By_Ball_Data[is_wicket]=TRUE() && 
                         NOT Ball_By_Ball_Data[wicket_kind] IN {"run out","retired hurt","obstructing the field","retired out" }
                         )
VAR wicktsumry = SUMMARIZE(seasnwickets,Ball_By_Ball_Data[bowler],"wicktcnt",COUNTROWS(FILTER(seasnwickets,Ball_By_Ball_Data[bowler]=EARLIER(Ball_By_Ball_Data[bowler]))))
var maxwickets = MAXX(wicktsumry,[wicktcnt])
VAR topbowler = CALCULATETABLE(VALUES(Ball_By_Ball_Data[bowler]),FILTER(wicktsumry,[wicktcnt]=maxwickets))
-- VAR team =  CALCULATE(MAX(Ball_By_Ball_Data[team_bowling]),FILTER(seasnwickets,Ball_By_Ball_Data[bowler]=MAXX(topbowler,Ball_By_Ball_Data[bowler])))
return LOOKUPVALUE(Players_Data[player_image],Players_Data[player_name],topbowler)

12.Purple_Cap_Team = 
var ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasnwickets = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && 
                        Ball_By_Ball_Data[is_wicket]=TRUE() && 
                         NOT Ball_By_Ball_Data[wicket_kind] IN {"run out","retired hurt","obstructing the field","retired out" }
                         )
VAR wicktsumry = SUMMARIZE(seasnwickets,Ball_By_Ball_Data[bowler],"wicktcnt",COUNTROWS(FILTER(seasnwickets,Ball_By_Ball_Data[bowler]=EARLIER(Ball_By_Ball_Data[bowler]))))
var maxwickets = MAXX(wicktsumry,[wicktcnt])
VAR topbowler = CALCULATETABLE(VALUES(Ball_By_Ball_Data[bowler]),FILTER(wicktsumry,[wicktcnt]=maxwickets))
VAR team =  CALCULATE(MAX(Ball_By_Ball_Data[team_bowling]),FILTER(seasnwickets,Ball_By_Ball_Data[bowler]=MAXX(topbowler,Ball_By_Ball_Data[bowler])))
return team

13.Purple_Cap_Wickets = 
var season = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasnwickets = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=season && 
                        Ball_By_Ball_Data[is_wicket]=TRUE() && 
                         NOT Ball_By_Ball_Data[wicket_kind] IN { "run out","retired hurt","obstructing the field","retired out" }
                         )
VAR wicktsumry = SUMMARIZE(seasnwickets,Ball_By_Ball_Data[bowler],"wicktcnt",COUNTROWS(FILTER(seasnwickets,Ball_By_Ball_Data[bowler]=EARLIER(Ball_By_Ball_Data[bowler]))))
var maxwickets = MAXX(wicktsumry,[wicktcnt])
-- VAR topbowler = CALCULATETABLE(VALUES(Ball_By_Ball_Data[bowler]),FILTER(wicktsumry,[wicktcnt]=maxwickets))
RETURN maxwickets

14.Runner_Up = 
var ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR max_date = CALCULATE(MAX(IPL_Matches_Data[match_date]),IPL_Matches_Data[season]=ssn)
VAR team1 =  LOOKUPVALUE(IPL_Matches_Data[team1],IPL_Matches_Data[match_date],max_date)
VAR team2 =  LOOKUPVALUE(IPL_Matches_Data[team2],IPL_Matches_Data[match_date],max_date)
return IF([Season_Winner]=team1,team2,team1)

15.Runnerup_logo = 
var ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR max_date = CALCULATE(MAX(IPL_Matches_Data[match_date]),IPL_Matches_Data[season]=ssn)
VAR team1 =  LOOKUPVALUE(IPL_Matches_Data[team1],IPL_Matches_Data[match_date],max_date)
VAR team2 =  LOOKUPVALUE(IPL_Matches_Data[team2],IPL_Matches_Data[match_date],max_date)
VAR run = IF([Season_Winner]=team1,team2,team1)
return LOOKUPVALUE(Teams_Data[image_url],Teams_Data[team_name],run)

16.Season_Winner = 
VAR selectedseason = SELECTEDVALUE(IPL_Matches_Data[season])  --- 2025

var final_match_date = CALCULATE(MAX(IPL_Matches_Data[match_date]),IPL_Matches_Data[season]=selectedseason) --max_date=3 June 2025
 VAR final_match_winner = CALCULATE(MAX(IPL_Matches_Data[match_winner]),IPL_Matches_Data[season]=selectedseason,IPL_Matches_Data[match_date]=final_match_date) -- Winner_team
 RETURN final_match_winner

 17.Total_Fours_cnt = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasfours = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && Ball_By_Ball_Data[batter_runs]=4)

VAR foursummry = SUMMARIZE(seasfours,Ball_By_Ball_Data[batter],"Fourcnt",
                        COUNTROWS(FILTER(seasfours,Ball_By_Ball_Data[batter]=EARLIER(Ball_By_Ball_Data[batter]))))
VAR maxfrs = MAXX(foursummry,[Fourcnt])
-- VAR playername = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(foursummry,[Fourcnt]=maxfrs))
RETURN maxfrs

18.Total_Fours_Player = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasfours = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && Ball_By_Ball_Data[batter_runs]=4)

VAR foursummry = SUMMARIZE(seasfours,Ball_By_Ball_Data[batter],"Fourcnt",
                        COUNTROWS(FILTER(seasfours,Ball_By_Ball_Data[batter]=EARLIER(Ball_By_Ball_Data[batter]))))
VAR maxfrs = MAXX(foursummry,[Fourcnt])
VAR playername = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(foursummry,[Fourcnt]=maxfrs))
RETURN MAXX(playername,Ball_By_Ball_Data[batter])

19.Total_Fours_Player_Image = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasfours = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && Ball_By_Ball_Data[batter_runs]=4)

VAR foursummry = SUMMARIZE(seasfours,Ball_By_Ball_Data[batter],"Fourcnt",
                        COUNTROWS(FILTER(seasfours,Ball_By_Ball_Data[batter]=EARLIER(Ball_By_Ball_Data[batter]))))
VAR maxfrs = MAXX(foursummry,[Fourcnt])
VAR playername = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(foursummry,[Fourcnt]=maxfrs))
RETURN LOOKUPVALUE(Players_Data[player_image],Players_Data[player_name],playername)

20.Total_Fours_team = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasfours = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && Ball_By_Ball_Data[batter_runs]=4)

VAR foursummry = SUMMARIZE(seasfours,Ball_By_Ball_Data[batter],"Fourcnt",
                        COUNTROWS(FILTER(seasfours,Ball_By_Ball_Data[batter]=EARLIER(Ball_By_Ball_Data[batter]))))
VAR maxfrs = MAXX(foursummry,[Fourcnt])
VAR playername = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(foursummry,[Fourcnt]=maxfrs))
RETURN CALCULATE(MAX(Ball_By_Ball_Data[team_batting]),FILTER(seasfours,Ball_By_Ball_Data[batter]=MAXX(playername,Ball_By_Ball_Data[batter])))

21.Total_Points = [Matches_Won]*2+[Matches_No_Result]*1

22.Total_Sixes_cnt = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasfours = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && Ball_By_Ball_Data[batter_runs]=6)

VAR foursummry = SUMMARIZE(seasfours,Ball_By_Ball_Data[batter],"sixcnt",
                        COUNTROWS(FILTER(seasfours,Ball_By_Ball_Data[batter]=EARLIER(Ball_By_Ball_Data[batter]))))
VAR maxsixes = MAXX(foursummry,[sixcnt])
-- VAR playername = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(foursummry,[Fourcnt]=maxfrs))
RETURN maxsixes

23.Total_Sixes_Player = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasfours = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && Ball_By_Ball_Data[batter_runs]=6)

VAR foursummry = SUMMARIZE(seasfours,Ball_By_Ball_Data[batter],"sixescnt",
                        COUNTROWS(FILTER(seasfours,Ball_By_Ball_Data[batter]=EARLIER(Ball_By_Ball_Data[batter]))))
VAR maxsixes = MAXX(foursummry,[sixescnt])
VAR playername = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(foursummry,[sixescnt]=maxsixes))
RETURN MAXX(playername,Ball_By_Ball_Data[batter])

24.Total_Sixes_Player_Image = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasfours = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && Ball_By_Ball_Data[batter_runs]=6)

VAR foursummry = SUMMARIZE(seasfours,Ball_By_Ball_Data[batter],"Fourcnt",
                        COUNTROWS(FILTER(seasfours,Ball_By_Ball_Data[batter]=EARLIER(Ball_By_Ball_Data[batter]))))
VAR maxfrs = MAXX(foursummry,[Fourcnt])
VAR playername = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(foursummry,[Fourcnt]=maxfrs))
RETURN LOOKUPVALUE(Players_Data[player_image],Players_Data[player_name],playername)

25.Total_Sixes_team = 
VAR ssn = SELECTEDVALUE(IPL_Matches_Data[season])
VAR seasfours = FILTER(Ball_By_Ball_Data,RELATED(IPL_Matches_Data[season])=ssn && Ball_By_Ball_Data[batter_runs]=6)

VAR foursummry = SUMMARIZE(seasfours,Ball_By_Ball_Data[batter],"Fourcnt",
                        COUNTROWS(FILTER(seasfours,Ball_By_Ball_Data[batter]=EARLIER(Ball_By_Ball_Data[batter]))))
VAR maxfrs = MAXX(foursummry,[Fourcnt])
VAR playername = CALCULATETABLE(VALUES(Ball_By_Ball_Data[batter]),FILTER(foursummry,[Fourcnt]=maxfrs))
RETURN CALCULATE(MAX(Ball_By_Ball_Data[team_batting]),FILTER(seasfours,Ball_By_Ball_Data[batter]=MAXX(playername,Ball_By_Ball_Data[batter])))

26.Winner_Logo = VAR selectedseason = SELECTEDVALUE(IPL_Matches_Data[season])  --- 2025

var final_match_date = CALCULATE(MAX(IPL_Matches_Data[match_date]),IPL_Matches_Data[season]=selectedseason) --max_date=3 June 2025
 VAR final_match_winner = CALCULATE(MAX(IPL_Matches_Data[match_winner]),IPL_Matches_Data[season]=selectedseason,IPL_Matches_Data[match_date]=final_match_date) -- Winner_team
 RETURN LOOKUPVALUE(Teams_Data[image_url],Teams_Data[team_name],final_match_winner)

 27.year = SELECTEDVALUE(IPL_Matches_Data[season])
