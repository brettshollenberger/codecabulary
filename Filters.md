| filter:{formatted: startDate.date}:min
| filter:{formatted: endDate.date}:max

$scope.min = function(actual, expected) {
        return actual >= expected;
      };

      $scope.max = function(actual, expected) {
        return actual <= expected;
      };

