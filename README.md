<p align="center">
  <img alt="Riot logo" width="120" src="https://uploads-ssl.webflow.com/6278dd61c2d8953dae931fbd/6278dd61c2d8956b07932038_logo-purple%25201-p-500.png" />
  <br>
  <br>
</p>

# Riot Backend-challenge

Hi there!

Since you got here, you're probably taking a part in our recruitment process for Back-end developer position, right?

We're super happy to hear that! Getting right to it — the main purpose of this test is to check out your backend and solving problems skills. We'd like to get to know your approach of solving the following problems:

- Understand specs requirements.
- Manipulate Data structure.
- Create an API in Node.js & Typescript.
- Basic use of Docker & Docker-compose.

Feel free to open an issue if you got any questions or suggestions! Once it's ready, send us a public repository link at louis@tryriot.com.

Happy coding and cheers,

Louis, CTO @ Riot

## Table of Contents

- [Riot Backend-challenge](#riot-backend-challenge)
  - [Table of Contents](#table-of-contents)
    - [Exercise : Unified API for employees directory](#exercise--mrr-compute-service)
      - [Problem](#problem)
      - [API](#api)
      - [Clarifications](#clarifications)
      - [Example](#example)

### Exercise : Employee directory unified API

#### Problem

When you sign in to Riot, one of the first things you will enable is the synchronisation of your employee directory. In Riot, you can synchronise your directory with Google, Slack, Microsoft, Okta. If you don't want to use a third-party service, you can create your employees manually or import them from a CSV. The advantage of a unified employee directory is that you can import the list of employees from these vendors and keep it up to date, avoiding duplicate values across vendors.

For the sake of this exercise, we are going to rebuild a small API to retrieve the employee directory for 2 providers Slack and Google.

#### Todos

1. Implement a POST `api/employees` that will take as input a name and an email address and will store the employee in the database.

```curl
curl --location '/api/employees' \
--header 'Content-Type: application/json' \
--data '{
    "name": "Louis Cibot"
    "email": "louis@tryriot.com"
}'
```

- It should return a `400` if the email address is not in the correct format.
- If the employee already exists, it should return a `409'.
- It should return a `201` otherwise.

2. Implement a POST `api/import` respecting the following specification

```curl
curl --location '/api/import' \
--header 'Content-Type: application/json' \
--data '{
    "url": "https://fake-subscriptions-api.fly.dev/tests/slack/1"
}'
```

- The url body parameter should respect the URL format and return `400` if the format is incorrect.

Fetching this URL should return a list of employees from a specific provider.

When the above endpoint is called, your program should fetch the given URL and synchronise the employees and store them in the database.

Let's do a quick example to make sure you understand 🤓

The response of the following Curl request is

```curl
curl --location '/api/import' \
--header 'Content-Type: application/json' \
--data '{
    "url": "https://fake-directory-provider.onrender.com/tests/slack/0"
}'
```

```json
{
  "provider": "Slack",
  "employees": [
    {
      "id": "642ebbfe122f71aaa9186fd3",
      "name": "Candace Foster",
      "email": "candacefoster@quarx.com",
    },
    {
      "id": "642ebbfe3ce36bfcee8df4d5",
      "name": "Latonya Morrow",
      "email": "latonyamorrow@quarx.com",
    }
  ]
}
```

After this call, in your database you should find:
  - Candace and Latonya in your employees table

Easy no? 🤠

Let's do a second call after this one.

```curl
curl --location '/api/import' \
--header 'Content-Type: application/json' \
--data '{
    "url": "https://fake-directory-provider.onrender.com/tests/google/0"
}'
```

```json
{
  "provider": "Google",
  "employees": [
    {
      "id": "642ebbfe122f71aaa9186fd3",
      "name": "Candace Foster",
      "emails": [{"address": "candacefoster@quarx.com", "isPrimary": true }, {"address": "candace.foster@gmail.com", "isPrimary": false }],
    },
    {
      "id": "642ebbfe3ce36bfcee8df4d5",
      "name": "Aaron Medrano",
      "emails": [{"address": "aaronmedrano@quarx.com", "isPrimary": true}],
    }
  ]
}
```

After the 2 calls, in your database you should find:
  - Candace from 2 providers
  - Latonya from Slack
  - Aaron from Google


To sum up the rules:
  1. Employees are matched by email address (if they have the same email address, primary or not).
  2. An employee can be created manually using the `/api/employees` API.
  3. If the manually created employee is not referenced in the `/import` call, it should not be deleted..
  4. If the manually created employee is referenced during an import and then no longer referenced, they can be deleted..

3. Implement a GET `api/employees` to retrieve all the employees stored in the database.

```curl
curl --location '/api/employees'
```

The answer should follow the format below:

```
[
  {
    "id": string,
    "name": string,
    "primaryEmailAddress": string,
    "secondaryEmailAddresses": string[],
    "googleUserId": string | null,
    "slackUserId: string | null,
  }
]
```

#### Testing the exercise

To test your exercise, you can use the following URL `https://fake-directory-provider.onrender.com/tests/:provider/:integer` for the imports
  - `:provider` can be either google or slack
  - `:integer` can iterate from 0 to 3. 

You're free to create your own way of testing. Just follow the same structure..

#### Clarifications

- For the sake of the problem, you need to use the appropriate database technology for this problem.
- Use at least Node.js + Typescript
- Unless you have a strong preference otherwise, just use a simple webserver like Express.
- You should optimise for both code readability and performance.
- Validate and use the appropriate types when you can.
- Tests are not mandatory but highly encouraged.
- An explanation of your technical choice is highly encouraged.
