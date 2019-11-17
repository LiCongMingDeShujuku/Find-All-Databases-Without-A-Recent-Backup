![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 查找最近没备份过的所有数据库
#### Find All Databases Without A Recent Backup
**发布-日期: 2015年9月3日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
查找超过一天（24小时）没被备份过的数据库。一下是一些快速的sql逻辑（logic）可以帮助你获得这些信息。这个逻辑（logic）假设这些数据库被备份过至少一次。基本上，他们需要在msdb..backupset表中输入一个条目。

## English
Find databases that haven’t had a backup in over 1 day. ( 24 hours ). Here’s some quick sql logic that will help you get that info. This logic assumes the databases that have been backed up at least once. Basically; they require an entry in the msdb..backupset table.

---
## Logic
```SQL

use master;
set nocount on
 
select
'server' = upper(@@servername)
,   'database' = upper(bs.database_name)
,   'last backup' = cast (datediff(day, max(bs.backup_finish_date), getdate()) as varchar(10)) + ' Days Old'
,   'time of backup'    = replace(replace(replace(left(max(bs.backup_finish_date), 19),':', '-'), 'AM', 'am'), 'PM', 'pm') + ' ' + datename(dw, max(bs.backup_finish_date)) from
msdb.dbo.backupset bs
where
type = 'd'
group by
bs.database_name
having
(max(bs.backup_finish_date) < dateadd(hour, -24, getdate()))

```



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

