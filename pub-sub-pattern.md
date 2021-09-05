# The Pub-Sub pattern

Hello everyone, in the last post I talked about observable pattern, and today I gonna talk about another patter called pub-sub, pub-sub has some differences with observable, and my plan is to explain this differences and show you how pub-sub works, and how you can implement it using javascript.

## How Pub-Sub works?

This pattern help you when you want to dispatch an event, and you want that just people that are interested in this event knows what is happening, as long as Observable dispatch just one event for everyone, Pub-Sub can dispatch many events, and who are interested should subscribe in a specific event.

### An Analogy

Some companies are interested in open new positions in their factories, for this reason, Ford, Volkswagen, and BMW make an announcement in the newspaper.

> Ford announcement: We are Ford, and we are very happy to announce that we have a new position for you, apply for our opportunities, and work with us, ford@ford.com

> Volkswagen announcement: We are Volkswagen, and you can work with us, apply for our opportunities, and work with us, volkswagen@volkswagen.com

> BMW announcement: We are BMW, and you can work with us building the most beautiful cars in the world, apply for our opportunities, and work with us, bmw@bmw.com

After some days, many employees become interested in these companies opportunities, and every company answered their candidate by e-mail telling them more details about the job, and the opportunity:

> Ford email: We are Ford, and we are very happy that you are interested in our new position, thanks for applying, and we will contact you soon.

> Volkswagen email: We are Volkswagen, and we are very happy that you are interested in our new position, thanks for applying, and we will contact you soon.

> BMW email: We are BMW, and we are very happy that you are interested in our new position, thanks for applying, and we will contact you soon.

So, in the end of the process, every company sended a message to employees subscribed in their opportunity, saying the about the end of the process.

### Applying the analogy

## Implementing Pub-Sub with javascript
