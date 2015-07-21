# Grafana-Zabbix
## Zabbix API datasource for Grafana dashboard

See latest version, docs and more at https://github.com/alexanderzobnin/grafana-zabbix

Display your Zabbix data directly in [Grafana](http://grafana.org) dashboards!

![2015-05-31 17-51-00 grafana - zabbix datasource - google chrome](https://cloud.githubusercontent.com/assets/4932851/7902354/fdf66368-07bf-11e5-991d-1e9892b2d0b0.png)

Useful metric editor with host group and application filtering:

![2015-05-31 17-53-23](https://cloud.githubusercontent.com/assets/4932851/7902360/156a9366-07c0-11e5-905b-4c21b52f1f44.png)


## Installation

### Grafana 1.9.x
See [grafana-1.9](../../tree/grafana-1.9) branch or Grafana-Zabbix [wiki](https://github.com/alexanderzobnin/grafana-zabbix/wiki).

### Grafana 2.0.x
Download source code from master branch and put `zabbix` directory into `<your grafana-2 installation>/public/app/plugins/datasource/`.
  * Edit plugin.json (located in `zabbix` directory) and set your `username` and `password`
  
    ```
    {
      "pluginType": "datasource",
      "name": "Zabbix",

      "type": "zabbix",
      "serviceName": "ZabbixAPIDatasource",

      "module": "plugins/datasource/zabbix/datasource",

      "partials": {
        "config": "app/plugins/datasource/zabbix/partials/config.html",
        "query": "app/plugins/datasource/zabbix/partials/query.editor.html",
        "annotations": "app/plugins/datasource/zabbix/partials/annotations.editor.html"
      },

      "username": "guest",
      "password": "",

      "metrics": true,
      "annotations": true
    }

    ```
  * Restart grafana server.
  * Add zabbix datasource in Grafana's "Data Sources" menu (see [Data Sources docs](http://docs.grafana.org/datasources/graphite/) for more info) and setup your Zabbix API url.
  * **Important!** Change `Access` to `direct`!
    ![2015-05-18 12-46-03 grafana - zabbix org - mozilla firefox](https://cloud.githubusercontent.com/assets/4932851/7678429/b42a9cda-fd5c-11e4-84a3-07aa765769d3.png)

#### Note for Zabbix 2.2 or less
Zabbix API (api_jsonrpc.php) before zabbix 2.4 don't allow cross-domain requests (CORS). And you can get HTTP error 412 (Precondition Failed).
To fix it add this code to api_jsonrpc.php immediately after the copyright
```
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Headers: Content-Type');
header('Access-Control-Allow-Methods: POST');
header('Access-Control-Max-Age: 1000');

if ($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
	return;
}
```
before 
```
require_once dirname(__FILE__).'/include/func.inc.php';
require_once dirname(__FILE__).'/include/classes/core/CHttpRequest.php';
```
[Full fix listing](https://gist.github.com/alexanderzobnin/f2348f318d7a93466a0c).
For more info see zabbix issues [ZBXNEXT-1377](https://support.zabbix.com/browse/ZBXNEXT-1377) and [ZBX-8459](https://support.zabbix.com/browse/ZBX-8459).

#### Note about browser cache
After updating plugin, clear browser cache and reload application page. See details for [Chrome](https://support.google.com/chrome/answer/95582), [Firefox](https://support.mozilla.org/en-US/kb/how-clear-firefox-cache). You need to clear cache only, not cookies, history and other data.

## Troubleshooting
See [Grafana troubleshooting](http://docs.grafana.org/installation/troubleshooting/) for general connection issues. If you have a problem with Zabbix datasource, you should open a [support issue](https://github.com/alexanderzobnin/grafana-zabbix/issues). Before you do that please search the existing closed or open issues.
