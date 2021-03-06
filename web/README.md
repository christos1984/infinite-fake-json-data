# fast prototyping with AngularJs against Json-server

Installation:

```sh
sudo npm install -g json-server
sudo npm install -g http-server
bower install restangular
```

Create data JSON file

```json
{
  "issues": [
    {
      "id": 101,
      "text": "something is not right"
    },
    {
      "id": 102,
      "text": "crash on login"
    }
  ]
}
```

Start the JSON REST server

```sh
json-server -f issues.json
curl http://localhost:3000/issues
curl http://localhost:3000/issues/101
```

you can try different verbs: PUT, DELETE, POST, GET

Start web server with angularjs

```sh
http-server -p 3001
```

Point restangular at JSON server's base url

```js
angular.module('project', ['restangular']).
    config(function(RestangularProvider) {
        RestangularProvider.setBaseUrl('http://localhost:3000/');
    });
...

// fetch all issues
$scope.issues = Restangular.all('issues').getList().$object;
```
Map new / edit / delete actions implicitly via Restangular.
In each case, redirect back to the index page to show updated list.

```js
function redirect() {
    $location.path('/list');
}

// add new issue
$scope.add = function() {
    // $scope.issue is copied from first one, see code
    Restangular.all('issues')
        .post($scope.issue)
        .then(redirect);
}

$scope.destroy = function() {
    original.remove().then(redirect);
};

// edit
$scope.save = function() {
    $scope.issue.put().then(redirect);
};
```
