# Using webhooks to track events and trigger actions

**Automatically get notified about events that happen on your Highway account.**

Highway uses webhooks to notify your application any time an event happens on your account. Set up webhooks for events that Highway already notify you of, like when a plan's status changes, a new route is created, or a service is done by your drivers.

A webhook endpoint is like a phone number that Highway calls when certain things happen in your Highway account. Highway creates an Event object for each occurrence in your Highway account that’s worth notifying you about, like the creation of a new customer in your Highway account, or a payout to your bank account. Each event contains data explaining what happened.

When you create a webhook handler, it listens for specific events, then takes action whenever those events get sent to the endpoint. Technically, a webhook handler is just a script in a server-side language, like Ruby or PHP, that handles any events you specify in the Dashboard. See all event types.

?> **Tip** Your webhook endpoints should be configured to receive only the types of events required by your integration. Listening for extra events (or all events) will put undue strain on your server and is not recommended.

Webhooks can also be used to provide state and API responses to services or systems that use Highway data for things like replication, analytics, or alerting.

## When to use webhooks

Webhooks are necessary for active user actions performed in the web application, or events comming from external sources such as vehicle position tracking.

Some Highway requests (e.g., changing a plan status) generate results that Highway reports synchronously to your code. These integrations don’t require your verification, but the events allow you to share information with other systems. You can also use webhooks in these synchronous integrations to automate business tasks:

- Update the routes in your database when route planning step is over
- Get the feedback of the orders (images, comments, problems)
- Log routes followed by your drivers in their working time

## See also

If webhooks are what you where looking for, continue reading our guides

- [Create webhooks](/developer/create_webhooks.md)
