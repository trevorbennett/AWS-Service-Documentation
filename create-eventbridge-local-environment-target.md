# Consuming events across AWS accounts with multiple development enviornments

If you find yourself needing to consume events with a non trivial number of consumers, particularly in a cross account event bus scenario where the consumers all have their own bus, here is how it should be handled, *per consumer*.

1. Create a single bus that serves as the source for all of the additional consumers. In my case this will be a bus called sample-bus-develop
2. Create a rule attached to this eventbridge bus 
3. Select "All Events" as the Event source ***(WARNING: Do not do this on the default event bus or you will create an expensive infinite loop of events triggering events)***
4. (optional) if you want to only send a certain subset of events from one bus to another, select the event source of your choice
5. Set the event pattern to `{"source": [{"prefix": "" }]}` to consume all events
6. Set the target to be the additional consumer(s), in my case two eventbridge buses named *sample-bus-local* and *sample-bus-feature*
7. Finish creating the rule

Congratulations, now all messages that are receieved by *sample-bus-develop* will also be forwarded to *sample-bus-local* and *sample-bus-feature*
