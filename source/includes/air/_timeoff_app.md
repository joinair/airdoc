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
workDayHours | F | Int | 8 if not specified
weekDaysOff | F | [Int] | [6, 7] if not specified
effectiveAsOf | F | String | today if not specified

### Errors

 Id  | Description
---- | -----------
timeOffPolicy.notAllPoliciesFullyConfigured | Throws if pending policy already exists.
person.unauthorizedAction | Throws if not admin creating policy.
