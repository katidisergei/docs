---
editable: false
sourcePath: en/_api-ref-grpc/billing/v1/api-ref/grpc/Budget/get.md
---

# Billing API, gRPC: BudgetService.Get {#Get}

Returns the specified budget.

## gRPC request

**rpc Get ([GetBudgetRequest](#yandex.cloud.billing.v1.GetBudgetRequest)) returns ([Budget](#yandex.cloud.billing.v1.Budget))**

## GetBudgetRequest {#yandex.cloud.billing.v1.GetBudgetRequest}

```json
{
  "id": "string"
}
```

#|
||Field | Description ||
|| id | **string**

Required field. ID of the budget to return.
To get the budget ID, use [BudgetService.List](/docs/billing/api-ref/grpc/Budget/list#List) request. ||
|#

## Budget {#yandex.cloud.billing.v1.Budget}

```json
{
  "id": "string",
  "name": "string",
  "createdAt": "google.protobuf.Timestamp",
  "billingAccountId": "string",
  "status": "BudgetStatus",
  // Includes only one of the fields `costBudget`, `expenseBudget`, `balanceBudget`
  "costBudget": {
    "amount": "string",
    "notificationUserAccountIds": [
      "string"
    ],
    "thresholdRules": [
      {
        "type": "ThresholdType",
        "amount": "string",
        "notificationUserAccountIds": [
          "string"
        ]
      }
    ],
    "filter": {
      "serviceIds": [
        "string"
      ],
      "cloudFoldersFilters": [
        {
          "cloudId": "string",
          "folderIds": [
            "string"
          ]
        }
      ]
    },
    // Includes only one of the fields `resetPeriod`, `startDate`
    "resetPeriod": "ResetPeriodType",
    "startDate": "string",
    // end of the list of possible fields
    "endDate": "string"
  },
  "expenseBudget": {
    "amount": "string",
    "notificationUserAccountIds": [
      "string"
    ],
    "thresholdRules": [
      {
        "type": "ThresholdType",
        "amount": "string",
        "notificationUserAccountIds": [
          "string"
        ]
      }
    ],
    "filter": {
      "serviceIds": [
        "string"
      ],
      "cloudFoldersFilters": [
        {
          "cloudId": "string",
          "folderIds": [
            "string"
          ]
        }
      ]
    },
    // Includes only one of the fields `resetPeriod`, `startDate`
    "resetPeriod": "ResetPeriodType",
    "startDate": "string",
    // end of the list of possible fields
    "endDate": "string"
  },
  "balanceBudget": {
    "amount": "string",
    "notificationUserAccountIds": [
      "string"
    ],
    "thresholdRules": [
      {
        "type": "ThresholdType",
        "amount": "string",
        "notificationUserAccountIds": [
          "string"
        ]
      }
    ],
    "startDate": "string",
    "endDate": "string"
  }
  // end of the list of possible fields
}
```

A Budget resource. For more information, see [/docs/billing/concepts/budget].

#|
||Field | Description ||
|| id | **string**

ID of the budget. ||
|| name | **string**

Name of the budget. ||
|| createdAt | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Creation timestamp. ||
|| billingAccountId | **string**

ID of the billing account that the budget belongs to. ||
|| status | enum **BudgetStatus**

Status of the budget.

- `BUDGET_STATUS_UNSPECIFIED`
- `CREATING`: The budget is being created.
- `ACTIVE`: The budget is active.
- `FINISHED`: The budget is finished. ||
|| costBudget | **[CostBudgetSpec](#yandex.cloud.billing.v1.CostBudgetSpec)**

Cost budget specification.

Includes only one of the fields `costBudget`, `expenseBudget`, `balanceBudget`.

Specification of the budget. ||
|| expenseBudget | **[ExpenseBudgetSpec](#yandex.cloud.billing.v1.ExpenseBudgetSpec)**

Expense budget specification.

Includes only one of the fields `costBudget`, `expenseBudget`, `balanceBudget`.

Specification of the budget. ||
|| balanceBudget | **[BalanceBudgetSpec](#yandex.cloud.billing.v1.BalanceBudgetSpec)**

Balance budget specification.

Includes only one of the fields `costBudget`, `expenseBudget`, `balanceBudget`.

Specification of the budget. ||
|#

## CostBudgetSpec {#yandex.cloud.billing.v1.CostBudgetSpec}

Cost budget specification describes budget that can be used to control cost of cloud resources usage.

#|
||Field | Description ||
|| amount | **string**

Required field. Max cost threshold of the budget. Amount currency is the same as corresponding [yandex.cloud.billing.v1.BillingAccount.currency](/docs/billing/api-ref/grpc/BillingAccount/get#yandex.cloud.billing.v1.BillingAccount). ||
|| notificationUserAccountIds[] | **string**

IDs of the [yandex.cloud.iam.v1.UserAccount](/docs/iam/api-ref/grpc/Federation/listUserAccounts#yandex.cloud.iam.v1.UserAccount).
Specified users will be be notified if the budget exceeds. ||
|| thresholdRules[] | **[ThresholdRule](#yandex.cloud.billing.v1.ThresholdRule)**

List of the [ThresholdRule](#yandex.cloud.billing.v1.ThresholdRule).
Rules define intermediate cost thresholds of the budget. ||
|| filter | **[ConsumptionFilter](#yandex.cloud.billing.v1.ConsumptionFilter)**

Filter that can be used for specific resources selection. Only consumption cost of selected resources are used for the budget calculation. ||
|| resetPeriod | enum **ResetPeriodType**

Periodic start type that resets budget after specified period is finished.
First time budget is calculated in the current period, i.e. current month, quarter or year.

Includes only one of the fields `resetPeriod`, `startDate`.

Start type of the budget.

- `RESET_PERIOD_TYPE_UNSPECIFIED`
- `MONTHLY`: Reset budget every month.
- `QUARTER`: Reset budget every quarter.
- `ANNUALLY`: Reset budget every year. ||
|| startDate | **string**

Custom start date of the budget.
Must be the first day of a month and must be formatted like YYYY-MM-DD.

Includes only one of the fields `resetPeriod`, `startDate`.

Start type of the budget. ||
|| endDate | **string**

Required field. End date of the budget.
Must be the last day of a month and must be formatted like YYYY-MM-DD. ||
|#

## ThresholdRule {#yandex.cloud.billing.v1.ThresholdRule}

Rules that define intermediate cost thresholds of the budget.

#|
||Field | Description ||
|| type | enum **ThresholdType**

Required field. Type of the rule.

- `THRESHOLD_TYPE_UNSPECIFIED`
- `PERCENT`: Percent.
- `AMOUNT`: The same as budget amount. ||
|| amount | **string**

Required field. Amount of the rule.
* Must be less than 100 if type is PERCENT.
* Must be less than budget's amount if type is AMOUNT. ||
|| notificationUserAccountIds[] | **string**

IDs of the [yandex.cloud.iam.v1.UserAccount](/docs/iam/api-ref/grpc/Federation/listUserAccounts#yandex.cloud.iam.v1.UserAccount).
Specified users will be be notified if the threshold exceeds. ||
|#

## ConsumptionFilter {#yandex.cloud.billing.v1.ConsumptionFilter}

Filter that can be used for specific resources selection.

#|
||Field | Description ||
|| serviceIds[] | **string**

IDs of the [yandex.cloud.billing.v1.Service](/docs/billing/api-ref/grpc/Service/get#yandex.cloud.billing.v1.Service).
Only consumption of resources corresponding to the given services is used for the budget calculation.
Empty sequence means no services filters. ||
|| cloudFoldersFilters[] | **[CloudFoldersConsumptionFilter](#yandex.cloud.billing.v1.CloudFoldersConsumptionFilter)**

Cloud and folders consumption filter.
Only consumption within specified clouds and folders is used for the budget calculation.
Empty sequence means no cloud and folders filters. ||
|#

## CloudFoldersConsumptionFilter {#yandex.cloud.billing.v1.CloudFoldersConsumptionFilter}

Filter that can be used for specific cloud and its folders selection.

#|
||Field | Description ||
|| cloudId | **string**

ID of the [yandex.cloud.resourcemanager.v1.Cloud](/docs/resource-manager/api-ref/grpc/Cloud/get#yandex.cloud.resourcemanager.v1.Cloud).
Only consumption within specified cloud is used for the budget calculation. ||
|| folderIds[] | **string**

IDs of the [yandex.cloud.resourcemanager.v1.Folder](/docs/resource-manager/api-ref/grpc/Folder/get#yandex.cloud.resourcemanager.v1.Folder).
Only consumption within specified folders of the given cloud is used for the budget calculation.
Empty sequence means no folders filters and the whole cloud consumption will be used. ||
|#

## ExpenseBudgetSpec {#yandex.cloud.billing.v1.ExpenseBudgetSpec}

Expense budget specification describes budget that can be used to control expense of cloud resources usage.

#|
||Field | Description ||
|| amount | **string**

Required field. Max expense threshold of the budget. Amount currency is the same as corresponding [yandex.cloud.billing.v1.BillingAccount.currency](/docs/billing/api-ref/grpc/BillingAccount/get#yandex.cloud.billing.v1.BillingAccount). ||
|| notificationUserAccountIds[] | **string**

IDs of the [yandex.cloud.iam.v1.UserAccount](/docs/iam/api-ref/grpc/Federation/listUserAccounts#yandex.cloud.iam.v1.UserAccount).
Specified users will be be notified if the budget exceeds. ||
|| thresholdRules[] | **[ThresholdRule](#yandex.cloud.billing.v1.ThresholdRule)**

List of the [ThresholdRule](#yandex.cloud.billing.v1.ThresholdRule).
Rules define intermediate expense thresholds of the budget. ||
|| filter | **[ConsumptionFilter](#yandex.cloud.billing.v1.ConsumptionFilter)**

Filter that can be used for specific resources selection. Only consumption expense of selected resources are used for the budget calculation. ||
|| resetPeriod | enum **ResetPeriodType**

Periodic start type that resets budget after specified period is finished.
First time budget is calculated in the current period, i.e. current month, quarter or year.

Includes only one of the fields `resetPeriod`, `startDate`.

Start type of the budget.

- `RESET_PERIOD_TYPE_UNSPECIFIED`
- `MONTHLY`: Reset budget every month.
- `QUARTER`: Reset budget every quarter.
- `ANNUALLY`: Reset budget every year. ||
|| startDate | **string**

Custom start date of the budget.
Must be the first day of a month and must be formatted like YYYY-MM-DD.

Includes only one of the fields `resetPeriod`, `startDate`.

Start type of the budget. ||
|| endDate | **string**

Required field. End date of the budget.
Must be the last day of a month and must be formatted like YYYY-MM-DD. ||
|#

## BalanceBudgetSpec {#yandex.cloud.billing.v1.BalanceBudgetSpec}

Balance budget specification describes budget that can be used to control [yandex.cloud.billing.v1.BillingAccount.balance](/docs/billing/api-ref/grpc/BillingAccount/get#yandex.cloud.billing.v1.BillingAccount).

#|
||Field | Description ||
|| amount | **string**

Required field. Max balance threshold of the budget. Amount currency is the same as corresponding [yandex.cloud.billing.v1.BillingAccount.currency](/docs/billing/api-ref/grpc/BillingAccount/get#yandex.cloud.billing.v1.BillingAccount). ||
|| notificationUserAccountIds[] | **string**

IDs of the [yandex.cloud.iam.v1.UserAccount](/docs/iam/api-ref/grpc/Federation/listUserAccounts#yandex.cloud.iam.v1.UserAccount).
Specified users will be be notified if the budget exceeds. ||
|| thresholdRules[] | **[ThresholdRule](#yandex.cloud.billing.v1.ThresholdRule)**

List of the [ThresholdRule](#yandex.cloud.billing.v1.ThresholdRule).
Rules define intermediate balance thresholds of the budget. ||
|| startDate | **string**

Start_date of the budget.
Must be the first day of a month and must be formatted like YYYY-MM-DD. ||
|| endDate | **string**

Required field. End date of the budget.
Must be the last day of a month and must be formatted like YYYY-MM-DD. ||
|#