# MMM-PublicTransportVVO


- **[MMM-PublicTransportVVO](https://github.com/ChristianGeie/MMM-PublicTransportVVO)** <br> Display live departures from public passenger transport service of the 'Verkehrsverbund Oberelbe' (VVO).


MMM-PublicTransportVVO is a module for the [MagicMirror](https://github.com/MichMich/MagicMirror) project by
[Michael Teeuw](https://github.com/MichMich).

It shows live public transport information for Verkehrsverbund Oberelbe (Germany) based on http://widgets.vvo-online.de api data.

You can enter a delay time for "How long does it take to get to my station?".
Then the module calculates the next reachable departures and draws all unreachable departures in a different style or color.

## Screenshot

The module looks like this:

![Example for Dresden, Postplatz with  delay of 2 minutes, colored true, 5 minute timeorminuteBorder and departuretimeDecoration is true ](img/MMM-PublicTransportVVO_screenshot.png)

## Installation

Just clone the module into your MagicMirror modules folder:

```bash
git clone https://github.com/ChristianGeie/MMM-PublicTransportVVO.git
```

## The `stationId`

### How to get it

You will need a `stationId` for your module. Its a little bit tricky to get it. Go to https://www.vvo-online.de/de/fahrplan/haltestellenfahrplan/index.cshtml. Turn on the "Investigation-Mode" in your Browser, so you can see the HTML source code. Put your favorite station name in the input field do a search. After this, do a search in the source code for the same station name. when you find a block like this:

```html
<select class="select " id="stopid" name="stopid" data-placeholder="Haltestelle" aria-label="Haltestelle" aria-required="true" data-stops-only="true" data-regional-only="true" data-pointstring="33000037|||Postplatz|5658730|4621656|0||" style="display: none;"><option value="33000037"><img style="display:inline" aria-label="Haltestelle" src="/assets/img/trans-icon/ico-stop-15x15.png" data-hoversrc="/assets/img/trans-icon/ico-stop-15x15-h.png" data-src="/assets/img/trans-icon/ico-stop-15x15.png"><span class="clearfix"><strong class="right"></strong><span class="displayname">Postplatz, Dresden</span></span></option></select>
```
The important flag is `data-pointstring`. The first integer is our `stationId`, in my example 33000037.

### How to verify it

```bash
curl http://widgets.vvo-online.de/abfahrtsmonitor/Haltestelle.do?hst=33000037
```
returns a array including station name, location and given `stationId` like this
```
[[["Dresden"]],[["Postplatz","Dresden","33000037"]]]
```

## Configuration

The module quite configurable. These are the possible options:

|Option|Description|
|---|---|
|`name`|The name of the module instance (if you want multiple modules).<br><br>**Type:** `string`<br>**Default value:** `MMM-PublicTransportVVO`|
|`stationId`|The ID of the station. How to get the ID for your station is described below.<br><br>**Type:** `integer`<BR>**Default value:** `33000037`<br> This value is **Required**.|
|`marqueeLongDirections`|Makes a marquee/ticker text out of all direction descriptions with more than 25 characters. If this value is false, the descriptions are trimmed to the station names. If the movement is not fluent enough for you, you should turn it off.<br><br>**Type:** `boolean`<br>**Default value:** `true`|
|`updateInterval`|How often the module should be updated. The value is given in milliseconds.<br><br>**Type:** `integer`<br>**Default value:** `30000` // 30 seconds|
|`timeorminuteBorder`|Departures after this border are displayed in the time format (HH:mm), all others shown simple minutes The value is given in minutes.<br><br>**Type:** `integer`<br>**Default value:** `15`|
|`departuretimeDecoration`|If you set `timeorminuteBorder` to a value above 1 minute with this value you have the possibility to show arrival time not only plain. <br><br>**Type:** `boolean`<br>**Default value:** `true`|
|`hidden`|Visibility of the module.<br><br>**Type:** `boolean`<br>**Default value:** `false`|
|`delay`|How long does it take you to get from the mirror to the station? The value is given in minutes.<br><br>**Type:** `integer`<br>**Default value:** `10` // 10 minutes|
|`showTableHeaders`|Show the table headers with information about location and station name.<br><br>**Type:** `boolean`<br>**Default value:** `true`|
|`showTableHeadersAsSymbols`|Show the table headers as text or symbols.<br><br>**Type:** `boolean`<br>**Default value:** `false`|

Here is an example of an entry in `config/config.js`:

```javaScript
{
    module: 'MMM-PublicTransportVVO',
    position: 'top_right',
    config: {
        stationId: 33000313,
        hidden: false,
        delay: 0,
        updateInterval: 120000,
        marqueeLongDirections: false,
        showTableHeaders: true,  
        showTableHeadersAsSymbols: true,
    }
},
```

## Multiple Modules

Multiple instances of this module are possible. Just add another entry of the MMM-PublicTransportVVO module to the `config/config.js` of your mirror.

## Special Thanks

* [Michael Teeuw](https://github.com/MichMich) for the great tool and many others to build a MagicMirror.
* [Bangee44](https://github.com/Bangee44) for creating the [MMM-swisstransport](https://github.com/Bangee44/MMM-swisstransport) module, on which this one is heavily based.
* The community of [magicmirror.builders](https://magicmirror.builders) for help in the development process.

## Issues

If you find any problems, bugs or have questions, please [open a GitHub issue](https://github.com/ChristianGeie/MMM-PublicTransportVVO/issues) in this repository.
