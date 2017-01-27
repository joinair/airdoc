# Time Off App

## Create policy

> Returns JSON structured like this:

```json
  {
    "id": "some_uuid"
  }
```

Creates policy and returns id.

### HTTP Request

`POST /apps/timeoff/policies`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
name | F | String | "Default" if not specified.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.



## Delete policy

> Returns JSON structured like this:

```json
  {
    "policies": ["some_uuid"]
  }
```

Deletes policy and returns remaining policyIds.

### HTTP Request

`DELETE /apps/timeoff/policies/:policy_id:`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.


## Get policy

> Returns JSON structured like this:

```json
  {
    "id": "some_uuid",
    "name": "Default",
    "workDays": [1, 2, 3, 4, 5],
    "workDayHours": 8,
    "effectiveAsOf": "2017-01-01"
  }
```

Returns policy by id.

### HTTP Request

`GET /apps/timeoff/policies/:policy_id:`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
timeOffPolicy.timeOffPolicyNotFound | Throws if policy with this id does not exist.


## Get policies setup progress

> Returns JSON structured like this:

```json
  [{
    "id": "some_uuid",
    "name": "Policy name",
    "steps": ["Holidays", "WorkSchedule", "TypesAndRules"]
  }]
```

Get all company policies and their steps.

Available steps: Holidays, WorkSchedule, CreateTypes, EditTypes, Eligibility, EffectiveAsOf, Balances

### HTTP Request

`GET /apps/timeoff/policies/setup`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.



## Get policies assigns

> Returns JSON structured like this:

```json
  [{
    "employeeId": "some_uuid",
    "policyId": "some_uuid",
    "policyName": "My luxury policy"
  }]
```

Get employees with assigned policies.

### HTTP Request

`GET /apps/timeoff/policies/assigns`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.



---
## Get regular holidays

> Returns JSON structured like this:

```json
  {
      "Scotland": [{
          "id": "uuid",
          "name": "Day of my awesomeness",
          "startDate": "2017-01-01",
          "endDate": "2017-01-02",
          "isEnabled": true
      }],
      "England & Wales": []

  }
```

Returns regular holidays.

### HTTP Request

`GET /apps/timeoff/policies/holidays`


## Get policy holidays

> Returns JSON structured like this:

```json
  {
      "regular":{
        "ids": [ "uuid"],
        "zone" :"Scotland"
       },
      "custom": [{
          "id": "uuid",
          "name": "Day of my awesomeness",
          "startDate": "2017-01-01",
          "endDate": "2017-01-02",
          "isEnabled": true
      }]
  }
```

Returns regular and custom holidays attached to policy.

### HTTP Request

`GET /apps/timeoff/policies/:policy_id:/holidays`



## Setup holidays

> Returns JSON structured like this:

```json
  {
     "regular":{
        "ids": [ "uuid"],
        "zone" :"Scotland"
     },
    "custom": [{
       "id": "uuid",
       "name": "Day of my awesomeness",
       "startDate": "2017-01-01",
       "endDate": "2017-01-02",
       "isEnabled": true
    }]
  }
```

Update company holidays.

### HTTP Request

`POST /apps/timeoff/policies/:policy_id:/holidays`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
regular | T | RegularHolidaysMutation | Array of enabled regular holidays.
custom | T | [HolidayMutation] | Array of mutations for custom holidays.

**RegularHolidaysMutation:**

Field | Required|  Type  | Description
--------- | ------- | ------ | -----------
zone | F | String | England & Wales; Scotland; Northern Ireland
disable | T | Seq[String] | Array .of holiday ids to disable.
enable | T | Seq[String] |  Array of holiday ids to enable.

**HolidayMutation:**

Field | Required|  Type  | Description
--------- | ------- | ------ | -----------
id | F | String | If id doesn't exist it is creation, if exists it is update.
name | F | String |
startDate | F | String | yyyy-MM-dd
endDate | F | String | yyyy-MM-dd
isEnabled | F | String |
isDeletion | F | String | If true, holiday will be deleted.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.

---

## Setup work schedule

> Returns JSON structured like this:

```json
  {
    "id": "some_uuid",
    "name": "Default",
    "workDays": [1, 2, 3, 4, 5],
    "workDayHours": 8
  }
```

Updates work day hours and week days off, returns policy.

### HTTP Request

`POST /apps/timeoff/policies/:policy_id:/schedule`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
workDayHours | T | Int |
workDays | T | [Int] |

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if policy is updating not by admin.
timeOffPolicy.invalidWorkDaysFormat | Throws if workDays field value is not allowed.
timeOffPolicy.invalidWorkDayHoursFormat | Throws if workDayHours field value is not allowed.
timeOffPolicy.timeOffPolicyNotFound | Throws if policy with this id does not exist.
---

## Setup policy start date

> Returns JSON structured like this:

```json
  {
    "id": "some_uuid",
    "name": "Default",
    "workDays": [1, 2, 3, 4, 5],
    "workDayHours": 8,
    "effectiveAsOf": "2017-01-01"
  }
```

Setups day when employee time off balance will be correct and when time
off will start accuring.

### HTTP Request

`POST /apps/timeoff/policies/:policy_id:/effective`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
effectiveAsOf | T | String | yyyy-MM-dd

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.

---



## Get types

> Returns JSON structured like this:

```json
  {
      "types": [{
          "id": "uuid",
          "name": "My awesome type",
          "policyTypeName": "Custom",
          "allowance": 20,
          "accrualFrequency": "Yearly",
          "accrualStartDate": "2017-02-01",
          "isNeedToRenew": "true",
          "renewDateMonth": "12",
          "renewDateDay": "31",
          "isCarriedOver": "true",
          "carryOverLimitDays": "10",
          "isWaitingPeriodEnabled": "true",
          "waitingPeriod": "90",
          "isAccruedOnWait": "false",
          "isUnlimited": "false",
          "isEnabled": "true"
      }]
  }
```

Get all policy types.

### HTTP Request

`GET: /apps/timeoff/policies/:policy_id:/types`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.


## Get type

> Returns JSON structured like this:

```json
  {
      "types": {
          "id": "uuid",
          "name": "My awesome type",
          "policyTypeName": "Custom",
          "allowance": 20,
          "accrualFrequency": "Yearly",
          "accrualStartDate": "2017-02-01",
          "isNeedToRenew": "true",
          "renewDateMonth": "12",
          "renewDateDay": "31",
          "isCarriedOver": "true",
          "carryOverLimitDays": "10",
          "isWaitingPeriodEnabled": "true",
          "waitingPeriod": "90",
          "isAccruedOnWait": "false",
          "isUnlimited": "false",
          "isEnabled": "true"
      }
  }
```

Get policy type by id.

### HTTP Request

`GET: /apps/timeoff/policies/:policy_id:/types/:type_id:`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
timeOffPolicy.policyTypeNotFound | Throws if not exist policy type with this id.


## Setup types

> Returns JSON structured like this:

```json
  {
      "types": [{
          "id": "uuid",
          "name": "My awesome type",
          "policyTypeName": "Custom",
          "allowance": 20,
          "accrualFrequency": "Yearly",
          "accrualStartDate": "2017-02-01",
          "isNeedToRenew": "true",
          "renewDateMonth": "12",
          "renewDateDay": "31",
          "isCarriedOver": "true",
          "carryOverLimitDays": "10",
          "isWaitingPeriodEnabled": "true",
          "waitingPeriod": "90",
          "isAccruedOnWait": "false",
          "isUnlimited": "false",
          "isEnabled": "true"
      }]
  }
```

Create policy types.

### HTTP Request

`POST /apps/timeoff/policies/:policy_id:/types`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
mutations | T | [PolicyTypeMutation] | Array of type mutations.

**PolicyTypeMutation:**

Field | Required|  Type  | Description
--------- | ------- | ------ | -----------
id | F | String | Create new policy type if not specified.
name | F | String | Type name.
isEnabled | T | String |
isDeletion | T | Boolean| Delete policy if specified.


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
timeOffPolicy.policyTypeNotFound | Throws if there was attempt to update not existing policy type.
timeOffPolicy.canNotUpdateType | Throws if was attempt to update predefined type name.
timeOffPolicy.canNotDeleteType | Throws if was attempt to delete predefined type.
## Setup specific type

> Returns JSON structured like this:

```json
  {
    "id": "uuid",
    "name": "My awesome type",
    "policyTypeName": "Custom",
    "allowance": 20,
    "accrualFrequency": "Yearly",
    "accrualStartDate": "2017-02-01",
    "isNeedToRenew": "true",
    "renewDateMonth": "12",
    "renewDateDay": "31",
    "isCarriedOver": "true",
    "carryOverLimitDays": "10",
    "isWaitingPeriodEnabled": "true",
    "waitingPeriod": "90",
    "isAccruedOnWait": "false",
    "isUnlimited": "false",
    "isEnabled": "true"
  }
```

Edit specific policy type by id.

### HTTP Request

`POST /apps/timeoff/policies/:p_id:/types/:t_id:`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
name | T | String |
policyTypeName | T | String | Holiday, Sickness, Custom
allowance | T | Boolean | How much days employee can get per year.
accrualFrequency | T | String | Yearly, Monthly, Weekly.
accrualStartDate | T | String | 2017-02-03.
isNeedToRenew | T | Boolean |
renewDateMonth | F | Int | Only if isNeedToRenew = true.
renewDateDay | F |  Int | Only if isNeedToRenew = true.
isCarriedOver | T | Boolean |
carryOverLimitDays | F | Int | Only if isCarriedOver = true.
isWaitingPeriodEnabled | T | Boolean|
waitingPeriod | F | Int | Only if isWaitingPeriodEnabled = true.
isAccruedOnWait | F | Boolean | Only if isWaitingPeriodEnabled = true.
isUnlimited | T | Boolean | If specified other fields ignored.
isEnabled| T | Boolean | Enable or disable task for policy.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.



## Finis editing types step

> Returns nothing:

Finish step.

### HTTP Request

`POST /apps/timeoff/policies/:p_id:/types/finish`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.

---



## Setup eligibility

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
person.unauthorizedAction | Throws if request is not made by admin.

---


## Get total allowance

> Returns JSON structured like this:

```json
  [{
    "employeeId": "uuid",
    "totalAllowance": 100
  }]
```

Get total allowance for all employees.

### HTTP Request

`GET /apps/timeoff/policies/:p_id:/types/:t_id:/balances`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.



## Setup balances

> Returns nothing:

Setup remaining profile balances.

### HTTP Request

`POST /apps/timeoff/policies/:p_id:/types/:t_id:/balances`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
balances | T | [Balance] | Balance config for each employee.

**Balance:**

Field | Required|  Type  | Description
----- | ------- | ------ | -----------
profileId | T | String | UUID
remaining | T | Int | Balance remaining on first day.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
