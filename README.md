# My Bank - Learn Project

This project are created to help people to learn how to create a backend API using:

- MSC (Model-Service-Controller)[https://medium.com/code-write/criando-uma-api-node-js-na-estrutura-models-services-controllers-routers-907be9175cf4] pattern to organize
- [Express.js](https://expressjs.com/pt-br/) as http server
- [Joi](https://joi.dev)
- [Postgres](https://www.postgresql.org/) as persistance layer

It is expected that when you finish this project you will understand how the separation of layers of an application works, the validation of data inputs, the manipulation and persistence and also the authentication and authorization.

# Introduction

A banking system works by recording incoming and outgoing transactions. These records together define the amount of money an account contains. It is also common for an account to have investments, which can generate income from time to time.

Let's create an application that allows users to register and authenticate using email and password to identify each account, and that also allows the registration of inflows and outflows of amounts in reais (BRL). In addition, to increase the complexity, we will simulate an investment structure that, each day that an amount is invested, will yield a percentage (for example, 0.003 per day).

# User History

In order for a person to manage their money, they usually register with a bank and then carry out incoming and outgoing transactions.

A person can be registered with several accounts and an account is linked to only one person. In addition, an account can make transactions related with own account.

# Use cases

## Register user

- For register, user need to pass these properties:
  - username: required, must be unique in solution
  - password: required, must have one lower, one upper, one digit and min of 8 characters.
- Case the sended email already in use, return a specific message for error with status "Conflit"
- Case doesn't have any problem create the account return status "Created"

## Login user

- For login, user need to pass these properties:
  - username: required
  - password: required
- Case username doesn't exists or password is invalid, return a message with status "Unauthorized".
- Case doesn't have any problem return an jwt token with an expiration time of 

## Overview user

- For access the overview, user needs to send an received and valid access_token in header
- When token isn't received or is invalid or expired, return a message with status "Unauthorized"
- When token is valid, return the current value of user and all invested values of them

## Add transaction

- For add a transaction, user needs to send an received and valid access_token in header and these properties:
  - value: a positive float value
- When token isn't received or is invalid or expired, return a message with status "Unauthorized"
- When all rules are valid, add the transaction in database, recalculate values, save in user overview and return transaction id

## Cancel transaction

- For cancel a transaction, user needs to send an received and valid access_token in header and these properties:
  - transaction id: the specific reference of transaction to cancel
- When token isn't received or is invalid or expired, return a message with status "Unauthorized"
- When transaction doesn't exists or already canceled, returns message with status "Not found"
- When created transaction isn't related with own user the transaction cannot be canceled, return a message with status "Forbidden"
- When transaction exists and isn't canceled and user is the creator, cancel the transaction returning status "Ok"

## History transaction

- For show history of transactions, user needs to send an received and valid access_token in header
- When token isn't received or is invalid or expired, return a message with status "Unauthorized"
- When token is valid, return the list of transactions created by user in decrescent order with values and statuses

# Database diagram

[Link here](https://mermaid.live/view#pako:eNq1UsFuwjAM_ZXK54LalK40t0lo0k47wC5TL1bjQTSaoDQpYy3_vlBgdDBNu8ynZ78n-9lJC6UWBBzIzCQuDVaFCnw812SCrhuNuq7HTw2ZRtKWB85nj-JKpdtgYVDVWFqp1UU0kLVH3OdOikCKS2GGlhayoqA05KG4tz9wbiOuubk1Ui37aQoruiE2WNdbbU6T9kM_54V-9dUXhgsf4mGt0QYNrh1d9R2c4F_W_eIMVbr5zv3RKoRQkalQCv_mvccC7Ir87YB7KNC8FVCogw6d1fOdKoFb4ygEo91yBfwV17XPjvZOf-Ys2aB60XqYAm_hHTiLonGcJxlLo3g6yVjCQtgBj9NkzNJpkiTZNEpZnLN9CB99h2h8l2f5JI58neV5uv8ES_nOag)