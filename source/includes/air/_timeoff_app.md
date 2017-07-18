# Time Off App

## Complete time off profile onboarding.

> Returns nothing:

Set onboarding for current profile as completed.

### HTTP Request

`POST /apps/timeoff/onboarding/complete`


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
    "effectiveAsOf": "2017-01-01",
    "isConfigured": false
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
    "isConfigured": false,
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
    "profileId": "some_uuid",
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



## Get policy for current profile

> Returns JSON structured like this:

```json
  {
      "id": "some_uuid",
      "name": "Default",
      "workDays": [1, 2, 3, 4, 5],
      "workDayHours": 8,
      "effectiveAsOf": "2017-01-01",
      "isConfigured": false
  }
```

Get policy which assigned on current profile.

### HTTP Request

`GET /apps/timeoff/policies/assigns/profile`

### Errors

 Id  | Description
---- | -----------
timeOffPolicy.timeOffPolicyNotFound | Throws if no one policy not assigned for current profile.

---
## Get regular holidays

> Returns JSON structured like this:

```json
  {
      "scotland": [{
          "id": "uuid",
          "name": "Day of my awesomeness",
          "startDate": "2017-01-01",
          "endDate": "2017-01-02"
      }],
      "england-and-wales": []

  }
```

Returns regular holidays.

### HTTP Request

`GET /apps/timeoff/policies/holidays`


## Get policy holidays setup

> Returns JSON structured like this:

```json
  {
      "regular":{
        "ids": [ "uuid"],
        "zone" :"scotland"
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


## Get enabled policy holidays

> Returns JSON structured like this:

```json
   [{
      "id": "uuid",
      "name": "Day of my awesomeness",
      "startDate": "2017-01-01",
      "endDate": "2017-01-02",
      "isSubstituted": true,
      "isEnabled": true
   }]
```

Returns enabled regular and custom holidays attached to policy.
Filtered by year.

### HTTP Request

`GET /apps/timeoff/policies/:policy_id:/holidays/enabled`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
year | T | Int | Filter by year.

## Get policies summary

> Returns JSON structured like this:

```json
  {
      "amountOfHolidays": 12,
      "types": [{
          "id": "some_uuid",
          "policyTypeName": "Custom",
          "name": "My custom type"
      }],
      "amountOfAssignees": 20,
      "workDayHours": 8,
      "workDays": [1, 2, 3, 4, 5],
      "effectiveAsOf": "2017-01-01"
  }
```

Get policy summary by id.

### HTTP Request

`GET /apps/timeoff/policies/:policy_id:/summary`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.


## Setup holidays

> Returns JSON structured like this:

```json
  {
     "regular":{
        "ids": [ "uuid"],
        "zone" :"scotland"
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
regular | F | RegularHolidaysMutation | Array of enabled regular holidays. Not required if regular settings was not changed.
custom | T | [HolidayMutation] | Array of mutations for custom holidays.

**RegularHolidaysMutation:**

Field | Required|  Type  | Description
--------- | ------- | ------ | -----------
zone | T | String | england-and-wales, scotland, northern-ireland
ids | T | Seq[String] | Array of holiday ids.

**HolidayMutation:**

Field | Required|  Type  | Description
--------- | ------- | ------ | -----------
id | F | String | If id doesn't exist it is creation, if exists it is update.
name | F | String |
startDate | F | String | yyyy-MM-dd
endDate | F | String | yyyy-MM-dd
isEnabled | T | String |
isDeletion | T | String | If true, holiday will be deleted.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.

---


## Update policy name

> Returns JSON structured like this:

```json
    {
      "id": "some_uuid",
      "name": "new name",
      "workDays": [1, 2, 3, 4, 5],
      "workDayHours": 8,
      "effectiveAsOf": "2017-01-01",
      "isConfigured": false
    }
```

Updates policy name and returns policy.

### HTTP Request

`POST /apps/timeoff/policies/:policy_id:/name`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
name | T | String |

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if policy is updating not by admin.
---


## Setup work schedule

> Returns JSON structured like this:

```json
  {
    "id": "some_uuid",
    "name": "Default",
    "workDays": [1, 2, 3, 4, 5],
    "workDayHours": 8,
    "isConfigured": false
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
    "effectiveAsOf": "2017-01-01",
    "isConfigured": false
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
profileIds | T | [String] | Sequence of employee ids assigned to policy.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
---



## Assign employees to policy

> Returns nothing:

It links employees to specified policy. Only admin endpoint.

### HTTP Request

`PUT /apps/timeoff/policies/:p_id:/eligibility`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
profileIds | T | [String] | Sequence of employee ids to be assigned to policy.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
---


## Get total allowance and remains

> Returns JSON structured like this:

```json
  [{
    "profileId": "uuid",
    "totalAllowance": 100,
    "remain": 100
  }]
```

Get total allowance for all employees.

### HTTP Request

`GET /apps/timeoff/policies/:p_id:/types/:t_id:/balances`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.



## Get total allowance and remains only for not initialized profiles

> Returns JSON structured like this:

```json
  {"typeId": [{
      "profileId": "uuid",
      "totalAllowance": 100,
      "remain": 100
    }]
  }
```

Get total allowance for employees.

### HTTP Request

`GET /apps/timeoff/policies/:p_id:/balances/new`

###  Query Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
profiles  | T       |[String]| Array of profile ids.


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
remain | T |  Int   | Balance remaining on first day.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.




## Finis types balances step

> Returns nothing:

Finish step.

### HTTP Request

`POST /apps/timeoff/policies/:p_id:/types/balances/finish`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.


---



## Request time off

> Returns JSON structured like this:

```json
  {
      "id": "uuid",
      "typeId": "uuid",
      "status": "Active",
      "startDate": "2017-02-01",
      "endDate": "2017-02-01",
      "moreThanDay": "true",
      "duration":{
        "hours": 1,
        "days": 1
       },
      "comment": "some comment",
      "approverId": "uuid"
  }
```


It requests time off and returns result request object.

### HTTP Request

`POST /apps/timeoff/requests`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
profileId | T       | String | Id profile that needs a time off.
typeId    | T       | String | Id of time off type.
startDate | T       | String | When time off starts.
endDate   | T       | String | When time off ends.
moreThanDay| T       | String | If time off more than day.
hours     | F       | String | Amount of hours if time off less than day.
comment   | F       | String | Optional comment.



### Errors

 Id  | Description
---- | -----------
timeOffPolicy.policyTypeNotFound | Throws if there was attempt to request with not existing policy type.
timeOffPolicy.notFullyConfigured | Throws if was attempt to book time off in not fully configured policy.
timeOff.notAvailableYet | Throws if was attempt to book time before allowed period (before effectiveAsOf or before end of waitingPeriod).
timeOffPolicy.invalidTimeOff | Throws if time off request parameters are not valid.




## Time offs report

> Returns JSON structured like this:

```json
  [{
      "timeOffId": "uuid",
      "policyId": "uuid",
      "typeName": "Holidays",
      "status": "Active",
      "startDate": "2017-02-01",
      "endDate": "2017-02-01",
      "moreThanDay": "true",
      "duration":{
        "hours": 1,
        "days": 1
       },
      "comment": "some comment",
      "description": "some description",
      "profileId": "uuid",
      "firstName": "Donald",
      "lastName": "Duck"
  }]
```

Get time off events for report

### HTTP Request

`GET /apps/timeoff/report`

###  Query Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
policyId  | F       | String | Id of policy, if not specified timeoffs for all policies.
typeId    | F       | String | Id of time off type, if not specified timeoffs for all types.
startDate | T       | String | Start of filtering period.
endDate   | T       | String | End of filtering period.


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin tries to get access to report.



## Request time off duration

> Returns JSON structured like this:

```json
  {
      "days": 5.3,
      "hours": 42.4
  }
```


It requests time off and returns result request object.

### HTTP Request

`GET /apps/timeoff/requests/duration`

###  Query Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
profileId | T       | String | Id profile that needs a time off.
typeId    | T       | String | Id of time off type.
startDate | T       | String | When time off starts.
endDate   | T       | String | When time off ends.
moreThanDay| T       | String | If time off more than day.
hours     | F       | String | Amount of hours if time off less than day.


### Errors

 Id  | Description
---- | -----------
timeOffPolicy.policyTypeNotFound | Throws if there was attempt to request with not existing policy type.
timeOffPolicy.notFullyConfigured | Throws if was attempt to book time off in not fully configured policy.
timeOff.notAvailableYet | Throws if was attempt to book time before allowed period (before effectiveAsOf or before end of waitingPeriod).
timeOffPolicy.invalidTimeOff | Throws if time off request parameters are not valid.



## Upcoming time offs

> Returns JSON structured like this:

```json
  [{
        "id": "uuid",
        "typeId": "uuid",
        "status": "Approved",
        "startDate": "2017-02-01",
        "endDate": "2017-02-01",
        "moreThanDay": "true",
        "duration":{
          "hours": 1,
          "days": 1
        },
        "comment": "some comment",
        "approverId": "uuid"
    }]
```


Get all current and upcoming timeoffs which are Approved.

### HTTP Request

`GET /apps/timeoff/requests/upcoming`




## Update timeoff

> Returns JSON structured like this:

```json
  {
      "id": "uuid",
      "typeId": "uuid",
      "status": "AwaitingApproval",
      "startDate": "2017-02-01",
      "endDate": "2017-02-01",
      "moreThanDay": "true",
      "duration": {
          "hours": 1,
          "days": 1
      },
      "comment": "some comment",
      "approverId": "uuid"
  }
```

Update time off for person.

### HTTP Request

`POST: /apps/timeoff/requests/:time_off_id:`


### Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
typeId    | T       | String | Id of time off type.
startDate | T       | String | When time off starts.
endDate   | T       | String | When time off ends.
moreThanDay| T       | String | If time off more than day.
hours     | F       | String | Amount of hours if time off less than day.
comment   | F       | String | Comment.


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.



## Delete timeoff
> Returns nothing:

Delete time off for person.

### HTTP Request

`DELETE: /apps/timeoff/requests/:time_off_id:`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.



## Change timeoff status

> Returns JSON structured like this:

```json
  {
      "id": "uuid",
      "typeId": "uuid",
      "status": "Approved",
      "startDate": "2017-02-01",
      "endDate": "2017-02-01",
      "moreThanDay": "true",
      "duration": {
          "hours": 1,
          "days": 1
      },
      "comment": "some comment",
      "approverId": "uuid"
  }
```

Update time off status.

### HTTP Request

`POST: /apps/timeoff/requests/:time_off_id:/status`

### Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
status    | T       | String | Approved, Declined.
comment   | F       | String |

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.



## Get timeoff overlaps

> Returns JSON structured like this:
> For unlimited types balance will be empty.
> balance - current balance of time off owner.

```json
  {
      "isUnlimited": true,
      "balance": {
         "hours": 1,
         "days": 1
      },
      "overlaps": [{
          "id": "uuid",
          "typeId": "uuid",
          "status": "AwaitingApproval",
          "startDate": "2017-02-01",
          "endDate": "2017-02-01",
          "moreThanDay": "true",
          "duration": {
              "hours": 1,
              "days": 1
          },
          "comment": "some comment",
          "approverId": "uuid"
      }]
  }
```

Get balance and time off overlaps.

### HTTP Request

`GET: /apps/timeoff/requests/:time_off_id:/overlaps`


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.


## Get timeoffs overview

> Returns JSON structured like this:

```json
  {
      "requests": [{
          "id": "uuid",
          "typeId": "uuid",
          "status": "AwaitingApproval",
          "startDate": "2017-02-01",
          "endDate": "2017-02-01",
          "moreThanDay": "true",
          "duration": {
              "hours": 1,
              "days": 1
          },
          "comment": "some comment"
      }],
      "outOfOffice": [{
          "id": "uuid",
          "typeId": "uuid",
          "status": "Approved",
          "startDate": "2017-02-01",
          "endDate": "2017-02-01",
          "moreThanDay": "true",
          "duration": {
              "hours": 1,
              "days": 1
          },
          "comment": "some comment",
          "approverId": "uuid"
      }],
      "upcoming": [{
          "id": "uuid",
          "typeId": "uuid",
          "status": "Approved",
          "startDate": "2017-02-01",
          "endDate": "2017-02-01",
          "moreThanDay": "true",
          "duration": {
              "hours": 1,
              "days": 1
          },
          "comment": "some comment",
          "approverId": "uuid"
      }]
  }
```

Get policy type for profile.

### HTTP Request

`GET: /apps/timeoff/requests/overview`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.



## Unassign employee from policy

> Returns nothing:

Remove policy - employee link. Only admin endpoint.

### HTTP Request

`DELETE /apps/timeoff/profiles/:profile_id:/policy`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
---



## Assign employee to policy

> Returns nothing:

Assign employee to specified policy. Only admin endpoint.

### HTTP Request

`PUT /apps/timeoff/profiles/:profile_id:/policy`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
policyId | T | String |

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
---



## Get policy by profile

> Returns JSON:

```json
  {
    "id": "some_uuid",
    "name": "Default",
    "workDays": [1, 2, 3, 4, 5],
    "workDayHours": 8,
    "effectiveAsOf": "2017-01-01",
    "isConfigured": false
  }
```

It returns policy for specified profile. Only admin endpoint.

### HTTP Request

`GET /apps/timeoff/profiles/:profile_id:/policy`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
---



## Get total allowance by profile

> Returns JSON:

```json
  {
    "type_id": {
        "profileId": "uuid",
        "totalAllowance": 100,
        "remain": 100
    }
  }
```

It returns total allowance for specified profile and policy
 by policy type. Only admin endpoint.

### HTTP Request

`GET /apps/timeoff/profiles/:profile_id:/allowance`
###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
policyId  |    T    | String |

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if request is not made by admin.
---


## Get types for specified profile

> Returns JSON structured like this:

```json
  [{
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
      "isEnabled": "true",
      "availableFrom": "2017-02-01"
  }]

```

Get policy type for profile.

### HTTP Request

`GET: /apps/timeoff/profiles/:profile_id:/policy/types`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get other types settings.



## Get balance summary

> Returns JSON structured like this:
> 'remain' is absent for future accrual periods.

```json
  {
      "type_id": {
          "policyType": {
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
              "isEnabled": "true",
              "availableFrom": "2017-02-01"
          },
          "remain": {
              "days": 10,
              "hours": 80
          },
          "notApproved": [{
              "id": "uuid",
              "typeId": "uuid",
              "status": "AwaitingApproval",
              "startDate": "2017-02-01",
              "endDate": "2017-02-01",
              "moreThanDay": "true",
              "duration": {
                  "hours": 1,
                  "days": 1
              },
              "comment": "some comment",
             "approverId": "uuid"
          }],
          "taken": [],
          "upcoming": []
      }
  }
```

Get balance for all policy types in current accrual year.

### HTTP Request

`GET: /apps/timeoff/profiles/:profile_id:/balances`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.



## Get balance summary for type

> Returns JSON structured like this:
> 'remain' is absent for future accrual periods.

```json
  {
      "policyType": {
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
            "isEnabled": "true",
            "availableFrom": "2017-02-01"
      },
      "remain": {
          "days": 10,
          "hours": 80
      },
      "notApproved": [{
          "id": "uuid",
          "typeId": "uuid",
          "status": "AwaitingApproval",
          "startDate": "2017-02-01",
          "endDate": "2017-02-01",
          "moreThanDay": "true",
          "duration": {
              "hours": 1,
              "days": 1
          },
          "comment": "some comment",
          "approverId": "uuid"
      }],
      "taken": [],
      "upcoming": []
  }
```

Get balance for specified type and time range.

### HTTP Request

`GET: /apps/timeoff/profiles/:profile_id:/balances/:type_id:`

###  Query Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
startDate | T       | String |
endDate   | T       | String |

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.



## Get activity

> Returns JSON structured like this:
> names: TimeOffPolicyAssigned, TimeOffPolicyUnassigned, TimeOffRequested, TimeOffApproved, TimeOffDeclined, TimeOffDeleted, TimeOffChanged

```json

      [{
          "name": "TimeOffApproved",
          "actorProfileId": "e8c28c12-ab1e-437b-a6b6-70016077802e",
          "targetProfileId": "e8c28c12-ab1e-437b-a6b6-70016077802e",
          "timeOffId": "a0c6631a-31e6-43be-be3e-3fd759bae1fb",
          "comment": "Default",
          "createdAt": "2017-02-23T09:01:06Z"
      }, {
          "name": "TimeOffPolicyAssigned",
          "actorProfileId": "e8c28c12-ab1e-437b-a6b6-70016077802e",
          "targetProfileId": "e8c28c12-ab1e-437b-a6b6-70016077802e",
          "policyId": "a0c6631a-31e6-43be-be3e-3fd759bae1fb",
          "policyName": "Default",
          "createdAt": "2017-02-23T09:01:06Z"
      }, {
          "name": "TimeOffRequested",
          "actorProfileId": "e8c28c12-ab1e-437b-a6b6-70016077802e",
          "targetProfileId": "e8c28c12-ab1e-437b-a6b6-70016077802e",
          "timeOff": {
              "id": "9129649b-f91d-4588-b3fb-6a383e9d536c",
              "typeId": "127ffe97-3b21-4190-a57a-9fae5d14773a",
              "typeName": "Holiday",
              "profileId": "e8c28c12-ab1e-437b-a6b6-70016077802e",
              "status": "AwaitingApproval",
              "startDate": "2017-02-28",
              "endDate": "2017-02-28",
              "moreThanDay": true,
              "duration": {
                  "hours": 8,
                  "days": 1
              },
              "comment": "comment for time off"
          },
          "comment": "comment for event",
          "createdAt": "2017-02-27T06:45:58Z"
      }]

```

Get balance for specified type and time range.

### HTTP Request

`GET: /apps/timeoff/profiles/:profile_id:/activity`



## Get balances history

> Returns JSON structured like this:

```json

      [{
          "createdAt": 1489581786168,
          "actor": "Timesheet",
          "change": 3.1415926,
          "balance": 23.1415926,
          "description": "Bonus Holiday"
      }]

```

Get a history of balance changes.

### HTTP Request

`GET: /apps/timeoff/profiles/:profile_id:/balances/history`

###  Query Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
typeId    | T       | String | Type identifier

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.



## Make adjustment.

> Returns JSON structured like this:

```json

      [{
          "createdAt": 1489581786168,
          "actor": "Batman",
          "change": 3.1415926,
          "balance": 23.1415926,
          "description": "Balance adjustment: For cool API."
      }]

```

Make custom balance changes.

### HTTP Request

`POST: /apps/timeoff/profiles/:profile_id:/balances/adjustment`

###  Query Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
typeId    | T       | String | Type identifier
value     | T       | Double | Amount of adjustment with sign.
comment   | F       | String | Optional comment.

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if not admin or manager tries to get access to other profile data.


## Change APP status.

> Returns nothing.

Change app status. Use only for manual app activation

### HTTP Request

`POST: /apps/timeoff/_changeStatus`

###  Parameters

Parameter | Required|  Type  | Description
--------- | ------- | ------ | -----------
value     | T       | String | value = "Active"

### Errors

 Id  | Description
---- | -----------
payments.higherPlanRequired | Throws if plan is not premium.



## Update time off profile settings

> Returns JSON structured like this:

```json
  {
      "isAutoApprove": false,
      "approvers": ["uuid1", "uuid2"],
  }
```

Update profile timeoff settings.

### HTTP Request

`POST: /apps/timeoff/profiles/:profile_id:/settings`

###  Parameters

Parameter    | Required |   Type   | Description
------------ | -------- | -------- | -----------
isAutoApprov |    T     | Boolean  | Is need to approve time offs automatically.
approvers    |    T     | [String] | List of approvers profiles identifiers, should be empty if auto approve is enabled.


### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if actor has not access to profile.



## Get time off profile settings

> Returns JSON structured like this:

```json
  {
      "isAutoApprove": false,
      "approvers": ["uuid1", "uuid2"],
  }
```

Get profile timeoff settings.

### HTTP Request

`GET: /apps/timeoff/profiles/:profile_id:/settings`

### Errors

 Id  | Description
---- | -----------
person.unauthorizedAction | Throws if actor has not .
