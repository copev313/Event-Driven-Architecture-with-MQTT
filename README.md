# Event-Driven-Architecture-with-MQTT

This repo depicts an example project I found intriguing which demonstrates the use of MQTT for realtime pub-sub communication between several interconnected services. Check out the article for yourself [here](https://itnext.io/how-to-create-an-event-driven-architecture-eda-in-python-1c47666bc088).

------

This project's architecture is a collection of five services, all interacting through an MQTT Broker, depicted as a message bus.

1. **Gateway Guardian Service**: A service tied to a sensor at a supermarket’s entrance. When a customer enters, and their loyalty card is scanned, the service publishes a ‘CustomerArrival’ event, including the identification from the customer’s loyalty card.

2. **Hello Helper Service**: This service is linked to a large display at the supermarket’s entrance. It subscribes to the ‘CustomerArrival’ events. Upon receiving one, it fetches relevant information from its local database using the customer identification received. It then displays a personalized welcome message on the screen.

3. **Gate-exit Guardian Service**: Acting as the supermarket’s cash register, this service triggers when a customer passes through the checkout’s detection gates. It automatically scans all items in the customer’s bag and publishes a message on the customer-departed topic, including identifying the purchased items.

4. **Inventory Intel Service**: This service subscribes to the topic customer-departed events, adjusting its database to reflect current stock levels. If the stock of a particular item falls below a defined threshold, it triggers a ‘RestockNeeded’ event. This event is sent to an external message bus connected to the supermarket’s logistics department, alerting them to replenish the stock.

5. **Fastlane Finale Service**: It acts as a financial liaison in our system. It listens to events on the customer-departed topic, calculates the total price of the customer’s shopping using its knowledge of product prices, and subsequently publishes a message on the payment-due topic containing the customer’s ID and total price.
