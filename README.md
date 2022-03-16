<p align="center">
  <img alt="Riot logo" src="https://tryriot.com/wp-content/themes/riot-2020-production/images/logo-purple.svg" />
  <br>
  <br>
</p>

# Riot Backend-challenge

Hi there!

Since you got here, you're probably taking a part in our recruitment process for Back-end developer position, right?

We're super happy to hear that! Getting right to it — the main purpose of this test is to check out your backend skills. We'd like to get to know your approach of solving the following problems:

- Understand specs requirements.
- Manipulate Data structure.
- Create a small API in Node.js.

Feel free to open an issue if you got any questions or suggestions! Once it's ready, send us a repository link at louis@tryriot.com.

Happy coding and cheers,

Louis, CTO @ Riot

## Table of Contents

- [Riot Backend-challenge](#riot-backend-challenge)
  - [Table of Contents](#table-of-contents)
    - [Exercise 1 : Counters & Metrics Service](#exercise-1--counters--metrics-service)
      - [Problem](#problem)
      - [API](#api)
        - [Counters](#counters)
        - [Metrics](#metrics)
      - [Clarifications](#clarifications)
      - [GraphQL schema](#graphql-schema)
    - [Exercise 3 : 30 minutes technical interview and debriefing](#exercise-3--30-minutes-technical-interview-and-debriefing)

### Exercise 1 : Counters & Metrics Service

#### Problem

Build a counters & metrisc logging and reporting service that sums metrics. You will build a lightweight web server that implements the API defined below.

#### API

##### Counters

**Increment counter Mutation**

Increment the counter with the provided value for the given key and return the counter.

**Counters Query**

Returns all the counters ordered by the highest value.

**Counter Query**

Return the counter for a given key, if not provided return the counter with highest value. if no counter is found, return `null`.

##### Metrics

**Record metric Mutation**

Record the metric with the provided value for the given key and return the metric.

**Metrics Query**

Returns all the metrics ordered by the highest sum.

**Metric Query**

Return the metric for a given key. if no metric is found, return `null`.

#### Clarifications

- For the sake of the problem, persistence is not required. Therefore don't use a database but just use in-memory data structures or file storage only.
- Unless you have a strong preference otherwise, just use a the boilerplate given in the repository.
- You should optimize for both readability of your code and performance.
- All values will be rounded to the nearest integer.

#### GraphQL schema

```gql
input IncrementInput {
  key: String!
  value: Int!
}

type IncrementPayload {
  counter: Counter!
}

input RecordInput {
  key: String!
  value: Int!
}

type RecordPayload {
  metric: Metric!
}

type Counter {
  key: String!
  value: Int!
}

type Metric {
  key: String!
  value: Int!
  sum: Int!
}

type Query {
  counters: [Counter!]!
  counter(key: String): Counter
  metrics: [Metric!]!
  metric(key: String!): Metric
}

type Mutation {
  increment(input: IncrementInput!): IncrementPayload
  record(input: RecordInput!): RecordPayload
}
```

### Exercise 3 : 30 minutes technical interview and debriefing

Once finished, send me your repository link by email: louis@tryriot.com & book a call [HERE](http://calendly.com/louis-cibot/30min)
