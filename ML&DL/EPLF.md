# England Premier League Forecasting

#### 数据获取

```
pip install [--upgrade] soccerdata
[pip show soccerdata]
```

```
import soccerdata as sd
```

**注** soccerdata 会在C:\Users\下创建soccerdata文件夹相当于缓存数据库

#### MatchHistory类

爬取网站 www.football-data.co.uk 易被反爬 不如手动下载高效

```
print(sd.MatchHistory.available_leagues())  //查可用联赛
data = sd.MatchHistory(leagues = "ENG-Premier League", seasons = "24-25", proxy = "tor", no_cache = True)
dataTemp = data.read_games()
print(dataTemp.head(10))  //只看前10场
```

#### FBref类

```
```

