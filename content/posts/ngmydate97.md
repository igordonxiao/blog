---
title: "Angular directive for My97Date"
tags: [
    "Angular",
]
date: "2015-08-17"
---

```javascript
angular.module('NG', []).directive('ngdatepicker', function () {
    return {
        restrict: 'A',
        require: '?ngModel',
        link: function ($scope, $element, $attrs, ngModel) {
            $element.on('focus',function(){
                WdatePicker({
                    readOnly: true,
                    onpicked: function(){
                        $scope.$digest();
                    }
                });
            });
            $scope.$watch(function(){
                return $element[0].value
            }, function(newDate){
                eval('$scope.'+$attrs.ngModel+' = newDate;');
            });
        }
    }
});

```

使用时需依赖jQuery及My97DatePicker，用法：
```html
<input ngdatepicker type="text"/>
```