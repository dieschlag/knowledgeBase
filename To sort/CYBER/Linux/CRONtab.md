File with formatting by the cron process to tell what to execute
	- MIN: what minute to execute at
	- HOUR
	- DOM: day of month
	- DOW: day of week 
	- MON: month
	- use crontab -e

Edit/Access cron: `crontab -e` 

Display crontab: `crontab -l`

Ex:
```bash
0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/
```