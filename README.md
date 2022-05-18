# Apollo Lighthouse Subscription Link

An apollo link to subscribe to Lighthouse graphql subscriptions using Laravel Echo presence channels.

## Installation

```bash
npm install @thekonz/apollo-lighthouse-subscription-link
# or
yarn add @thekonz/apollo-lighthouse-subscription-link
```

## Lighthouse compatibility

| Lighthouse version | Link version  | Comment                                                                            |
| ------------------ | ------------- | ---------------------------------------------------------------------------------- |
| 5.2 and below      | 1.1 and below |                                                                                    |
| 5.3 and above      | 1.2 and above | The event name changed from 'lighthouse.subscription' to 'lighthouse-subscription' |
| 5.3 and above      | 1.3 and above | Support for 'lighthouse.subscription.version' = 2                                  |

## Usage

```javascript
import ApolloClient from "@apollo/client";
import gql from "graphql-tag";
import Echo from "laravel-echo";

import { createLighthouseSubscriptionLink } from "@thekonz/apollo-lighthouse-subscription-link";

const echoClient = new Echo({...});

const client = new ApolloClient({
  link: ApolloLink.from([
    createLighthouseSubscriptionLink(echoClient),
    httpLink, // your existing http link to your graphql api
  ]),
  cache: new InMemoryCache(),
});

const subscriber = client
  .subscribe({
    query: gql`
      subscription {
        postUpdated {
          id
          title
        }
      }
    `,
  })
  .subscribe((postUpdated) => {
    console.log(postUpdated); // { id: 2, title: "New title" }
  });

// stop listening to events
subscriber.unsubscribe()
```

## Contributing and issues

Feel free to contribute to this package using the issue system and pull requests on the `develop` branch.

Automated unit tests must be added or changed to cover your changes or reproduce bugs.
