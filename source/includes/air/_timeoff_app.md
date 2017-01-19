# Time Off App

## Create policy

> Returns JSON structured like this:

```json
  {
    "id": some_uuid
  }
```

Creates policy and returns id.

### HTTP Request

`POST /apps/timeoff/policies`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
name | F | String | "Default" if not specified

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin creating policy.


## Update work schedule.

> Returns JSON structured like this:

```json
  {
    "id": some_uuid,
    "name": "Default",
    "weekDaysOff": [6,7],
    "workDayHours": 8
  }
```

Update work day hours and week days off, returns policy.

### HTTP Request

`POST /apps/timeoff/policies/:id:/schedule`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
workDayHours | T | Int |
workDays | T | [Int] |

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin updating policy.



## Update policy start date.

> Returns JSON structured like this:

```json
  {
    "id": some_uuid,
    "name": "Default",
    "weekDaysOff": [6,7],
    "workDayHours": 8,
    "effectiveAsOf": "2017-01-01"
  }
```

Setup day when employee time off balance will be correct and when time
off start accuring.

### HTTP Request

`POST /apps/timeoff/policies/:id:/effective`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
effectiveAsOf | T | String | yyyy-MM-dd

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin creating policy.


## Update holidays ???.

> Returns JSON structured like this:

```json
  {
    "regular": {
       "enabled": ["uuid1", "uuid2"],
    },
    "custom": [{
       "id": "uuid",
       "name": "Day of my awesomeness",
       "startDay": "2017-01-01",
       "endDay": "2017-01-02",
       "isEnabled": true
    }]
  }
```

Update company holidays.

### HTTP Request

`POST /apps/timeoff/policies/:id:/holidays`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
enabled | T | [String] | Array of enabled holidays.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin creating policy.


## Setup types.

> Returns JSON structured like this:

```json
  {
      "types": [{
          "id": "uuid",
          "name": "Holiday",
          "allowance": 20,
          "accrualFrequency": "Yearly",
          "accrualStartDate": "2017-02-01",
          "isNeedToRenew": "true",
          "renewDateMonth": "12",
          "renewDateDay": "31",
          "isCarriedOver": "true",
          "carryOverLimitDays": "10",
          "waitingPeriodEnabled": "true",
          "waitingPeriod": "90",
          "isAccruedOnWait": "false",
          "isUnlimited": "false",
          "isActive": "true"
      }]
  }
```

Create policy types.

### HTTP Request

`POST /apps/timeoff/policies/:id:/types`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
enableHolidays | T | Boolean | Create and enable Holidays type.
enableSickness | T | Boolean | Create and enable Sickness type.
custom | T | [String] | Array of custom types.


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin creating policy.


## Setup specific type.

> Returns JSON structured like this:

```json
  {
    "id": "uuid",
    "name": "Holiday",
    "allowance": 20,
    "accrualFrequency": "Yearly",
    "accrualStartDate": "2017-02-01",
    "isNeedToRenew": "true",
    "renewDateMonth": "12",
    "renewDateDay": "31",
    "isCarriedOver": "true",
    "carryOverLimitDays": "10",
    "waitingPeriodEnabled": "true",
    "waitingPeriod": "90",
    "isAccruedOnWait": "false",
    "isUnlimited": "false",
    "isActive": "true"
  }
```

Create policy types.

### HTTP Request

`POST /apps/timeoff/policies/:p_id:/types/:t_id:`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
allowance | T | Boolean | How much days can get employee per year.
accrualFrequency | T | String | Yearly, Monthly, Quarterly, SemiMonthly, BiWeekly, Weekly.
accrualStartDate | T | String | 2017-02-03.
isNeedToRenew | T | Boolean |
renewDateMonth | F | Int | Only if isNeedToRenew = true.
renewDateDay | F |  Int | Only if isNeedToRenew = true.
isCarriedOver | T | Boolean |
carryOverLimitDays | F | Int | Only if isCarriedOver = true.
waitingPeriodEnabled | T | Boolean|
waitingPeriod | F | Int | Only if waitingPeriodEnabled = true.
isAccruedOnWait | F | Boolean | Only if waitingPeriodEnabled = true.
isUnlimited | T | Boolean | If specified other fields ignored.


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin creating policy.


## Setup eligibility.

> Returns nothing:

Setup policy eligibility.

### HTTP Request

`POST /apps/timeoff/policies/:p_id:/eligibility`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
employeeIds | T | [String] | Sequence of employee ids assigned to policy.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin creating policy.



## Setup balances.

> Returns nothing:

Setup remaining profile balances.

### HTTP Request

`POST /apps/timeoff/policies/:p_id:/balances`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
balances | T | [Balance] | Balance config for each employee.

*Balance:*

Field | Required|  Type  | Description
----- | ------- | ------ | -----------
profileId | T | String | UUID
startDate | T | String | yyyy-MM-dd
remaining | T | Int | Balance remaining on first day.


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin creating policy.
