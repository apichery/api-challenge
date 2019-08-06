# API Technical Test @ Dataiku

## Challenge

The goal is to write the documentation for the REST API of a small card game, and optionally to write a client library in the language of your choice to ease programming against this API.

## Game API

This API allow players to play a game similar to Top Trumps card games. As a player, you can register and get a starter deck of cards.
Each card represents a hero from the Marvel universe and has 4 characteristics: strength, skill, size, and popularity.

To get new cards, you fight with your cards. Each deck of card is ordered and you fight with the first card of your deck. You can have a look at
it and decide which characteristic is the most promising and challenge the computer with this characteristic. If the card of the computer has a higher
value than yours on this characteristic, you loose your card... But, if your card is better, the card from the computer is yours (and you keep you card).
In case of draw, all players keep their cards.

If you are not satisfied with your current card(s)... or if you have lost all your cards, you can buy one or more cards from the store. The new cards are added to the top of your deck.

The code for this game and its API is just two files: 
  - [server/server.js](https://github.com/dataiku/api-challenge/tree/master/server/server.js)
  - [server/game.js](https://github.com/dataiku/api-challenge/tree/master/server/game.js)

Game on!

## Getting Started

```sh
npm install
npm run start
```

Open your browser and go to [http://localhost:4000/ping](http://localhost:4000/ping). If everything is setup correctly you should get `{"status":"OK"}`.

You can then launch a demonstration of the game with the following command. You can see the code in [client/client.js](https://github.com/dataiku/api-challenge/tree/master/client/client.js)
```sh
npm install
npm run demo
```
