---
title: "Strategy Design Pattern Practice Qestions"
description: "2023"
date: "2023-01-23"
tags:
- fundamentals
---

1. Payment Processing System  : Develop a payment processing system that supports multiple payment methods (e.g., Credit Card, PayPal, Bitcoin). 
* Define an interface PaymentStrategy with a method pay(double amount). 
* Implement concrete classes CreditCardPayment, PayPalPayment, and BitcoinPayment that implement the PaymentStrategy interface. 
* Create an Order class that uses a PaymentStrategy to process payments. 
* Demonstrate switching between different payment strategies at runtime.

2. Text Formatting : Create a text editor that can format text in various ways (e.g., plain text, HTML, Markdown).
* Define an interface TextFormatter with a method format(String text).
* Implement concrete classes PlainTextFormatter, HTMLFormatter, and MarkdownFormatter that implement the TextFormatter interface.
* Create a TextEditor class that uses a TextFormatter to format text.
* Demonstrate changing the text formatting strategy at runtime.

3. Travel Route Planning : Develop a travel planning application that can find routes using different transportation methods (e.g., Car, Bike, Walking).
* Define an interface RouteStrategy with a method calculateRoute(String start, String end).
* Implement concrete classes CarRoute, BikeRoute, and WalkingRoute that implement the RouteStrategy interface.
* Create a TravelPlanner class that uses a RouteStrategy to calculate routes.
* Demonstrate changing the route calculation strategy at runtime.


4. Context-Aware Strategies : Create a recommendation system that provides different recommendations based on user context (e.g., Movie Recommendations, Book Recommendations, Product Recommendations).
* Define an interface RecommendationStrategy with a method recommend(String userContext).
* Implement concrete classes MovieRecommendation, BookRecommendation, and ProductRecommendation that implement the RecommendationStrategy interface.
* Create a Recommender class that uses a RecommendationStrategy to generate recommendations.
* Demonstrate adapting the recommendation strategy based on different user contexts.