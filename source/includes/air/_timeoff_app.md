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
person.unauthorizedAction | Throws if not admin make request.



## Delete policy

> Returns JSON structured like this:

```json
  {
    "policies": ["some_uuid]
  }
```

Deletes policy and returns remaining policyIds.

### HTTP Request

`DELETE /apps/timeoff/policies/:id:`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin make request.


## Get policies setup progress

> Returns JSON structured like this:

```json
  [{
    "id": "some_uuid",
    "name": "Policy name",
    "steps": ["Holidays", "WorkSchedule", "TypesAndRules"]
  }]
```

Get all company policies and they steps.

Available steps: Holidays, WorkSchedule, CreateTypes, EditTypes, Eligibility, EffectiveAsOf, Balances

### HTTP Request

`GET /apps/timeoff/policies/setup`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin make request.



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
person.unauthorizedAction | Throws if not admin make request.



---
## Get holidays

> Returns JSON structured like this:

```json
  {
      "regular": [{
          "id": "uuid",
          "name": "New Years Day",
          "startDate": "2017-01-01",
          "endDate": "2017-01-02",
          "isEnabled": true
      }],
      "custom": [{
          "id": "uuid",
          "name": "Day of my awesomeness",
          "startDate": "2017-01-01",
          "endDate": "2017-01-02",
          "isEnabled": true
      }]
  }
```

Returns regular and custom policy holidays.

### HTTP Request

`GET /apps/timeoff/policies/:id:/holidays`



## Setup holidays

> Returns JSON structured like this:

```json
  {
    "regular":  ["uuid1", "uuid2"],
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

`POST /apps/timeoff/policies/:id:/holidays`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
regular | T | [String] | Array of enabled regular holidays.
custom | T | [HolidayMutation] | Array of mutations for custom holidays.

**HolidayMutation:**

Field | Required|  Type  | Description
--------- | ------- | ------ | -----------
id | F | String | If id didn't exist it is creation, else it is update.
name | F | String |
startDate | F | String | yyyy-MM-dd
endDate | F | String | yyyy-MM-dd
isEnabled | F | String |
isDeletion | F | String | If true holiday will be deleted

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin make request.

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
person.unauthorizedAction | Throws if not admin make request.

---



## Get types

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

Get all policy types.

### HTTP Request

`GET: /apps/timeoff/policies/:id:/types`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin make request.



## Setup types

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
custom | T | [PolicyTypeMutation] | Array of type mutations.

**PolicyTypeMutation:**

Field | Required|  Type  | Description
--------- | ------- | ------ | -----------
id | T | String | Create new policy type if not specified.
name | T | String | Type name.
isDeletion | T | Boolean| Delete policy if specified.


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin make request.



## Setup specific type

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

Edit specific policy type by id.

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
person.unauthorizedAction | Throws if not admin make request.



## Finis editing types step

> Returns nothing:

Finish step.

### HTTP Request

`POST /apps/timeoff/policies/:p_id:/types/finish`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin make request.

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
person.unauthorizedAction | Throws if not admin make request.

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
person.unauthorizedAction | Throws if not admin make request.



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
person.unauthorizedAction | Throws if not admin make request.
