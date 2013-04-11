doo-forex
=========

DooPHP Forex backend for enyo-forex

###Quick Start:

##Installation:

* Install dependancies:
  * MySQL Server 5.6+ [requires millisecond datetime]
  * Redis Server 2.4+ [used for cacheing]
  * screen [optional for running forex updater]
  * php-cli
  * php-pdo
  * php-mysql
  
* Install Doo PHP Framework http://doophp.com/
* Clone doo-forex and init submodules

```
git clone https://github.com/hurdad/doo-forex.git /usr/share/doo-forex
cd /usr/share/do-forex/protected/class/
git submodule update --init
```
* Configure Doo Framework path:

```
vi doo-forex/protected/config/common.conf.php
```

```
$config['BASE_PATH'] = realpath('..').'/dooframework/';
```
* Configure Apache alias:

```
vi /etc/httpd/conf.d/doo-forex.conf
```

```
Alias /doo-forex /usr/share/doo-forex
<Directory "/usr/share/doo-forex">
  AllowOverride All
</Directory>
````
* Install Database Schema

```
php /usr/share/doo-forex/setup_db.php
```

##Backfill Historical Data [Optional]
  *  Download free historical data from truefx : http://truefx.com/dev/data/
  *  Unzip to .csv
  *  Load into database

```
cd /usr/share/doo-forex
php cli.php forex_loader /path/to/EURUSD.csv
```
  * Note: you will need to manually run hour & day aggregations for any backfill'd data

```
php cli.php quote_aggregator day EUR/USD '2012-01-01'
```

##Start Forex Update

```
cd /usr/share/doo-forex
screen -A -m -d -S forex php cli.php forex_update
```

##JSONP API Rotues
* OHLC [open high low close] candle stick

```
http://localhost/doo-forex/ohlc?pair=EUR/USD&callback=myjsonpcallback
```

* TA [technical analysis]

```
http://localhost/doo-forex/ta?function=RSI&pair=EUR/USD&callback=myjsonpcallback
```
