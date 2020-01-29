![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process


![SQL Database Mail]( https://mikesdatawork.files.wordpress.com/2015/07/sql-database-mail-sp_send_dbmail-mid.png "sp_send_dbmail")
 
<p>Here's some quick sql logic you can use to configure the SQL Database Mail aka sp_send_dbmail process.</p>      


## SQL-Logic
```SQL
-- configure sql database mail with the sql logic below.
 
-- enable sql database mail profile
exec master..sp_configure 'show advanced options',1
reconfigure;
exec master..sp_configure 'Database Mail XPs',1
reconfigure;
go
 
-- creating a profile
execute msdb.dbo.sysmail_add_profile_sp
    @profile_name       = 'SQLDatabaseMailProfile'
,   @description        = 'SQLDatabaseMail' ;
 
-- create a mail account.
execute msdb.dbo.sysmail_add_account_sp
    @account_name       = 'EmailAddressYouWantToAppearHere' &lt;-- Does not have to be a real email aaddress.  Parameters are required for process, but are not needed to work.
,   @email_address      = 'EmailAddressYouWantToAppearHere' &lt;-- Does not have to be a real email aaddress.  Parameters are required for process, but are not needed to work.
,   @mailserver_name    = 'MySMTPServerNameGoesHere' 
--, @port               = MyPort --optional
--, @enable_ssl         = 1 --optional
--, @username           ='SQLDatabaseMailProfile' --optional
--, @password           ='MyPassword' --optional
 
-- adding the account to the profile
execute msdb.dbo.sysmail_add_profileaccount_sp
    @profile_name       = 'SQLDatabaseMailProfile'
,   @account_name       = 'sqldatabasemail@mydomain.com'
,   @sequence_number    = 1 ;
 
-- grant access to DatabaseMailUserRole
execute msdb.dbo.sysmail_add_principalprofile_sp
    @profile_name       = 'SQLDatabaseMailProfile'
,   @principal_id       = 0
,   @is_default         = 1 ;
 
-- send email.
EXEC msdb.dbo.sp_send_dbmail
    @profile_name       = 'SQLDatabaseMailProfile'
,   @recipients         = 'MyEmailAddress'  &lt;-- Distribution group always preferred here.
,   @subject            = 'Database Mail Subject...'
,   @body               = 'Database Mail Body...';
 
 
-- check to see if mail is being qued up to be sent.  
-- several seconds after you see email sent to your inbox.
select * from msdb..sysmail_allitems 
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

   
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

