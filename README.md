# Spreadit - Spreadsheet data importing simplified and 100% in the browser.

Spreadit is an angular directive inspired by the guys at [Conference Badge](https://www.conferencebadge.com/).  It imports data from Excel or CSV as JSON so it
can be used in the browser or uploaded to the server.  It supports header and column detection.  Users can also rename and ignore columns before importing the data.

![It's A Screenshot!](screenshot.png)
![It's Another Screenshot!](importer-screenshot.png)

## Install It
* <a name="manual"></a>**Manual**: download latest from [here](https://github.com/blakgeek/spreadit/releases/latest)
* <a name="bower"></a>**Bower**: `bower install spreadit --save`
* <a name="npm"></a>**NPM**: `npm install spreadit`

## See It In Action
* [Basic Example](https://jsfiddle.net/blakgeek/vumyL0x1/)
* [Configure Columns](https://jsfiddle.net/blakgeek/q50cLjnz/)
* [Restrict Columns](https://jsfiddle.net/blakgeek/y2yfqydp/)
* [Post-process Data](https://jsfiddle.net/blakgeek/L4t7beeo/)

## Code Sample
HTML:
```html
<link href="spreadit.css" type="text/css" rel="stylesheet"/>

<script src="angular.min.js"></script>
<!-- needed to support csv -->
<script src="papaparse.min.js"></script>
<!-- needed to support Excel -->
<script src="xlsx.full.min.js"></script>
<script src="spreadit(.min).js"></script>

<button si-trigger="myId">Click Me To Import</button>
<si-column-manager
    si-id="myId"
    si-columns="columns"
    si-exclude-unknown-columns="false"
    si-sample-size="5"
    si-allow-renaming="true"
    si-post-processors="postProcessors"
    si-change="doStuffWithData($data, $file, $type)">
</si-column-manager>
```
Javascript:
```js
var app = angular.module('superDopeDemo', ['bg.spreadit']);

app.controller('MyCtrl', ['$scope', function ($scope) {

    $scope.doStuffWithData = function(data, file, type) {

        console.log('file type: %s', type);
        console.log(data);
    };

    $scope.columns = [
        // matches becasue of title
        {
            title: 'Email',
            property: 'emailAddress'
        },
        // matches because of property
        {
            title: 'Last Name',
            property: 'last_name'
        },
        // matches because of alias
        {
            title: 'First Name',
            property: 'firstName',
            aliases: ['first_name', 'first']
        }
    ];

    $scope.postProcessors = [

        // concatenate first and last name to form a full name
        function (data) {
            if (data.firstName || data.last_name) {
                data.fullName = data.firstName + ' ' + data.last_name;
            }
            return data;
        },
        // capitalize full name
        function (data) {
            if (data.fullName) {
                data.fullName = data.fullName.toUpperCase();
            }
            return data;
        }
    ]
}]);
```