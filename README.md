# Moneybox iOS Technical Challenge

## The Brief:

To create a 'light' version of the Moneybox app that will allow existing users to login and check their account balance as well as viewing their moneybox savings.

### The app should have
- A login screen to allow existing users to login
- A screen to show the accounts the user holds, e.g. ISA, GIA
- A screen to show some detail of the account, including a simple button to add money to their moneybox.
  - The button should be add a fixed amount of say £10. It should use the `POST /oneoffpayments` endpoint (described below) and their Moneybox amount would be updated.
- A user should be able to navigate back and forth between screens.

A prototype wireframe is provided below and can be used as a guideline. You are free to change any elements of the screen and provide additional information if you wish.

![](https://s3.amazonaws.com/uploads.hipchat.com/402730/2470705/6JJwTcVMpFzsqba/ios%20wireframe.png)

##### What we are looking for:
 - Demonstration of coding style, conventions, structure and use of any 3rd party libraries.
 - Any form of unit testing would be a bonus.
 - The application must run on iOS 10 or later.
 - The application must compile and run on Xcode and be debugged in Xcode's iOS simulator

##### How to Submit your solution:
 - Create a public repo in github, bitbucket or a suitable alternative and provide a link to the repository.
 - (Optional) Provide a readme in markdown which details the app that you have created and any necessary steps in order to launch or run the solution.

### API Usage
This a brief summary of api endpoints of the moneybox sandbox environment. There a lot of other additional properties from the json responses that are either not relevant to this technical task and some are marked as obsolete. You are free to use any information and you will not be penalised for misinterpreting  the information used.

In order to make requests https must be used and the following headers must be included in each request.

|  Key | Value |
| ------------- | ------------- |
| AppId  | 8cb2237d0679ca88db6464  |
| Content-Type  | application/json  |
| appVersion | 4.0.0 |
| apiVersion | 3.0.0 |

### Authentication
After obtaining a bearer token via `POST /users/login` (sample json response below), an Authorization header must be provided for all other endpoints.
Note: The BearerToken has a sliding expiration of 5 mins.
```
"Session": {
        "BearerToken": "TsMWRkbrcu3NGrpf84gi2+pg0iOMVymyKklmkY0oI84=",
        "ExternalSessionId": "4ff0eab7-7d3f-40c5-b87b-68d4a4961983", -- not used, obsolete
        "SessionExternalId": "4ff0eab7-7d3f-40c5-b87b-68d4a4961983", -- not used, obsolete
        "ExpiryInSeconds": 0 -- not used, obsolete
    }
```

Authorization Header to be used

|  Key          | Value         |
| ------------- | ------------- |
| Authorization  | Bearer TsMWRkbrcu3NGrpf84gi2+pg0iOMVymyKklmkY0oI84=  |


### API Endpoints
#### Login
Logs in an existing user
```
POST /users/login
{
  "Email": "testing@moneyboxapp.com",
  "Password": "supersafepassword1234",
  "Idfa": "the idfa of the ios device"
}
```
Sample json response - has been trimmed down, and below are the core properties that are of use.
```
{
    "User": {
        "UserId": "74d21315-538e-4abc-a4a8-38cb0291216f",
        "HasVerifiedEmail": true,
        "IsPinSet": true,
        "RegistrationStatus": "IsComplete",
        "DateCreated": "2017-11-17T10:19:06.537",
        "MoneyboxRegistrationStatus": "IsComplete",
        "Email": "test+sample@moneyboxapp.com",
        "FirstName": "Sample",
        "LastName": "User",
    },
    "Session": {
        "BearerToken": "Kcuf/DOjXgwDioE6wOdM1XyR/+ncwzdT0N9bJjl+O6g=", ----------> This is used for authentication
        "ExternalSessionId": "0f1ae000-9eda-4f59-ad78-5773b9d71315", ----------> not used, obsolete
        "SessionExternalId": "0f1ae000-9eda-4f59-ad78-5773b9d71315", ----------> not used, obsolete
        "ExpiryInSeconds": 0 ----------> not used, obsolete
    }
}
```

### Logout
Ends the current session for the user
```
POST users/logout
```

#### This Week
Provides product and account information for a user. There are alot of properties, however, some notes have been used to describe some properties that you may want to use.
```
GET /investorproduct/thisweek
```

Sample json response
```
{
    "Products": [
        {
            "InvestorProductId": 3229,
            "InvestorProductType": "Isa",
            "ProductId": 1,
            "Moneybox": 130,  ----------> How much the user has saved this week so far and is the users 'Moneybox'
            "PreviousMoneybox": 130, ----------> not used, obsolete
            "SubscriptionAmount": 30,  ----------> What the current weekly subscription is set to
            "PlanValue": 5235,  ----------> The current account balance
            "Sytd": 1235,  ----------> How much the user has contributed in the current tax year
            "TransferInSytd": 4000,  ----------> The amount the user has transfered from another provider
            "MaximumWithdrawal": 0,
            "MaximumDeposit": 14765,  ----------> The remaining amount that can be contributed to the current tax year
            "TotalContributions": 0,
            "TotalReturnValue": 0,
            "TotalReturnPercentage": 0,
            "CashInTransit": 0,
            "ResidualCash": 0,
            "TotalFees": 0,
            "TotalReturnValueGross": 0,
            "PendingWithdrawal": 0,
            "IsPendingRebalance": false,
            "PendingDeposit": 0,
            "Product": {
                "Name": "ISA",
                "Type": "Isa",
                "AnnualLimit": 20000,
                "DepositLimit": 0,
                "FriendlyName": "Stocks & Shares ISA"  
            },
            "DateModified": "2017-11-17T10:21:10.437",
            "Valuations": [],
            "IsSelected": true,
            "IsFavourite": true
        },
        {
            "InvestorProductId": 3230,
            "InvestorProductType": "Gia",
            "ProductId": 2,
            "Moneybox": 20,
            "PreviousMoneybox": 20,
            "SubscriptionAmount": 50,
            "PlanValue": 2000,
            "Sytd": 2000,
            "TransferInSytd": 0,
            "MaximumWithdrawal": 0,
            "MaximumDeposit": 20000,
            "TotalContributions": 0,
            "TotalReturnValue": 0,
            "TotalReturnPercentage": 0,
            "CashInTransit": 0,
            "ResidualCash": 0,
            "TotalFees": 0,
            "TotalReturnValueGross": 0,
            "PendingWithdrawal": 0,
            "IsPendingRebalance": false,
            "PendingDeposit": 0,
            "Product": {
                "Name": "GIA",
                "Type": "Gia",
                "AnnualLimit": 0,
                "DepositLimit": 20000,
                "FriendlyName": "General Investment Account"
            },
            "DateModified": "2017-11-17T10:21:48.650",
            "Valuations": [],
            "IsSelected": false,
            "IsFavourite": false
        }
    ],
    "SelectedInvestorProductId": 3229,
    "FavouriteInvestorProductId": 3229,
    "Transactions": [],
    "Links": []
}
}
```

### One off payments
Adds a one off amount to the users moneybox.
```
POST /oneoffpayments
{
  "Amount": 20,
  "InvestorProductId": 3230 ------> The InvestorProductId from investorproduct/thisweek endpoint
}
```

Sample json response
```
{
  "Moneybox": 20
}
```








