# Moneybox iOS Technical Challenge

## The Brief:

To create a 'light' version of the Moneybox app that will allow existing users to login and check their account balance as well as viewing their moneybox savings.

### The app should have
- A login screen to allow existing users to login
- A screen to show the accounts the user holds, e.g. ISA, GIA
- A screen to show some detail of the account, including a simple button to add money to their moneybox.
  - The button should be add a fixed amount of say £10. It should use the `POST /oneoffpayments` endpoint (described below) and their Moneybox amount would be updated.
- A user should be able to navigate back and forth between screens.

A prototype wireframe of all 3 screens is provided as a guideline. You are free to change any elements of the screen and provide additional information if you wish.
![](wireframe.png)

##### What we are looking for:
 - Demonstration of coding style, conventions, structure and use of any 3rd party libraries.
 - Any form of unit testing would be a bonus.
 - The application must run on iOS 11 or later.
 - The application must compile and run on Xcode and be debugged in Xcode's iOS simulator.

##### How to Submit your solution:
 - To provide a link of the Github repository where you have implemented the solution.
 - (Optional) Provide a readme in markdown which details the app that you have created and any necessary steps in order to launch or run the solution.

### API Usage
This a brief summary of api endpoints of the moneybox sandbox environment. There a lot of other additional properties from the json responses that are either not relevant to this technical task and some are marked as obsolete. You are free to use any information and you will not be penalised for misinterpreting  the information used.

#### Base URL & Test User
The base URL for the moneybox sandbox environment is `https://api-test02.moneyboxapp.com/`.
You can log into test your app using the following user:

|  Username          | Password         |
| ------------- | ------------- |
| test+ios@moneyboxapp.com  | P455word12  |

#### Headers

In order to make requests https must be used and the following headers must be included in each request.

|  Key | Value |
| ------------- | ------------- |
| AppId  | 8cb2237d0679ca88db6464  |
| Content-Type  | application/json  |
| appVersion | 7.10.0 |
| apiVersion | 3.0.0 |

#### Authentication
To login with this user to retrieve a bearer token you need to call `POST /users/login`.
```
POST /users/login
{
  "Email": "test+ios@moneyboxapp.com",
  "Password": "P455word12",
  "Idfa": "ANYTHING"
}
```
Sample json response
```
"Session": {
        "BearerToken": "TsMWRkbrcu3NGrpf84gi2+pg0iOMVymyKklmkY0oI84=",
        "ExternalSessionId": "4ff0eab7-7d3f-40c5-b87b-68d4a4961983", -- not used
        "SessionExternalId": "4ff0eab7-7d3f-40c5-b87b-68d4a4961983", -- not used
        "ExpiryInSeconds": 0 -- not used
    }
```
After obtaining a bearer token an Authorization header must be provided for all other endpoints along with the headers listed above (Note: The BearerToken has a sliding expiration of 5 mins).

|  Key          | Value         |
| ------------- | ------------- |
| Authorization  | Bearer TsMWRkbrcu3NGrpf84gi2+pg0iOMVymyKklmkY0oI84=  |

#### Investor Products
Provides product and account information for a user that will be needed for the two additional screens.
```
GET /investorproducts
```
### One off payments
Adds a one off amount to the users moneybox.
```
POST /oneoffpayments
{
  "Amount": 20,
  "InvestorProductId": 3230 ------> The InvestorProductId from /investorproducts endpoint
}
```
Good luck!








