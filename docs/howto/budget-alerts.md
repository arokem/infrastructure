(howto:enable-budget-alerts)=
# Enable Budget Alerts

This document describes how to enable budget alerts for our clusters.

```{note}
This feature is currently only available for GCP!
```

## GCP

```{attention}
We are only able to setup budget alerting on projects where we have enough permissions to enable APIs and view the billing account.
```

First, ensure that the following APIs are enabled for the GCP project you're enabling budget alerts for:

- [Cloud Resource Manager API](https://console.cloud.google.com/apis/library/cloudresourcemanager.googleapis.com)
- [Cloud Billing Budget API](https://console.cloud.google.com/apis/library/billingbudgets.googleapis.com)

Then in the `.tfvars` file for the cluster, we need to set the following variables:

1. **Set `budget_alert_enabled = false`**, or delete the variable altogether (it is set to `true` by default in `variables.tf`).
   This will ensure that the relevant resources are created by terraform.
1. **Set `billing_account_id`.**
   - For linked accounts managed by 2i2c, you can find this by visitng the [Billing Console](https://console.cloud.google.com/billing/linkedaccount?project=two-eye-two-see) and clicking "Go to Linked Billing Account". Once there, click "Manage Billing Account" at the top of the window, and that should open a pane containing the Billing Account ID.
   - For billing accounts we don't manage the process is the same, just make sure you have the correct project selected from the dropdown at the top of the page.
     Also, _we may not have permissions on the project to view the billing account_.
     If that is the case, we cannot enable budget alerting.
1. **Set `budget_alert_amount`.**
   Current convention is to set this to the average of 3 months expenditure plus 20% (in USD), and you can find values to calculate that from the [Billing Reports console](https://console.cloud.google.com/billing/0157F7-E3EA8C-25AC3C/reports?organizationId=184174754493&project=two-eye-two-see).
   _Make sure only the project you are interested in is selected in the 'Projects' field of the Filters pane on the right side of the screen!_
   If you are enabling this for a new cluster, obviously you don't have that data yet!
   Instead, set it to `500` and we can tweak as we learn more.

With these variables set, you are now ready to open a PR and perform a terraform apply!
