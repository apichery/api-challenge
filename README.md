# API Technical Test @ Dataiku

## Game API

This repository contains the source code of a small game similar to [Top Trumps card games](https://en.wikipedia.org/wiki/Top_Trumps). As a player, you can register and get a starter deck of cards.
Each card represents a hero from the Marvel universe and has 4 characteristics: strength, skill, size, and popularity.

![Batman](https://github.com/dataiku/api-challenge/blob/master/resources/batman.png) | Batman
------------ | -------------
Strength | 22
Skills | 17
Size | 5.7
Popularity | 9.8

To get new cards, you can fight with your cards against the computer. Each deck of card is ordered and you fight with the first card of your deck. You can have a look at
it and decide which characteristic is the most promising and challenge the computer with this characteristic. If the card of the computer has a higher
value than yours on this characteristic, you loose your card... But, if your card is better, the card from the computer is yours (and you keep you card).
In case of draw, all players keep their cards.

If you are not satisfied with your current card(s)... or if you have lost all your cards, you can buy one or more cards from the store. The new cards are added to the top of your deck.

The code for this game and its API can be found in the following locations: 
  - [server/server.js](https://github.com/dataiku/api-challenge/tree/master/server/server.js)
  - [server/game.js](https://github.com/dataiku/api-challenge/tree/master/server/game.js)

## Challenge

Your goal is to write:
 1. the documentation for the REST API of a small card game
 1. a client library in the language of your choice to ease programming against this REST API.

Game on!

## Getting Started
Install a recent version of [nodeJS](https://nodejs.org/en/download/) and npm on your computer. Open a terminal and issue the following command to confirm they are properly installed (you don't need the exact same versions).
```sh
$ node -v
v11.8.0
$ npm -v
6.10.2
```

Now clone this repository and go to the corresponding directory
```sh
npm install
npm run start
```

Open your browser and go to [http://localhost:4000/ping](http://localhost:4000/ping). If everything is setup correctly your browser should display:
```
{"status":"OK"}
```

You can then launch a demonstration of the game with the following command.
```sh
npm run demo
```
The code for the demonstration is [client/client.js](https://github.com/dataiku/api-challenge/tree/master/client/client.js)

## API Reference
The following API endpoints are available for interaction with the Top Trump game. 
  
The host for all API calls will be `http://localhost:4000` when using the default server settings.
  
  
#### GET /ping
Returns information about the state of the game server.

Example Call
```sh
  curl --location --request GET 'http://localhost:4000/ping'
```

Example Response
```sh
  {
    "status": "OK"
  }
```

*If you receive a response that is not ```{ "status": "OK" }``` confirm that the game server is running. If it is not, open a terminal and issue the following command in the corresponding directory ```npm run start```.*

<br>

#### POST /register
Creates a new user within the game. 

Save the `playerid` from the response. `playerid` is a required header parameter for all endpoints except `/ping` and `/register`.

Body Parameters
- `username` REQUIRED <br>
*Enter a username for the user*
- `birthdate` REQUIRED <br>
*Enter the birth date of the user and follow YYYY-MM-DD format*
- `email` REQUIRED <br>
*Enter an email address for the user*


Example Call
```sh
  curl --location --request POST 'http://localhost:4000/register' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --data-urlencode 'username=example' \
    --data-urlencode 'birthdate=YYYY-MM-DD' \
    --data-urlencode 'email=example@example.com' \
```

Example Response
```sh
  {
    "playerID": "0d57b4a0-a86f-11eb-94dd-e7a848edc63c",
    "cards": [
        {
            "id": "0d57b4a2-a86f-11eb-94dd-e7a848edc63c",
            "name": "Kitty Pryde",
            "strength": 49,
            "skill": 9,
            "size": 5,
            "popularity": 1.9
        },
        ...
    ]
  }
```

<br>

#### GET /profile
Returns profile information associated with a `playerid`.

Header Parameters
- `playerid` REQUIRED <br>
  *Include a user's playerid, created during `POST /register` and found in the response.*


Example Call
```sh
  curl --location --request GET 'http://localhost:4000/profile' \
    --header 'playerid: 0d57b4a0-a86f-11eb-94dd-e7a848edc63c'
```

Example Response
```sh
  {
    "username": "example",
    "birthdate": "YYYY-MM-DD",
    "email": "name@domain.com"
  }
```

<br>

#### GET /buy-card
Adds a new card to the deck of `playerid`.

Header Parameters
- `playerid` REQUIRED <br>
  *Include a user's playerid, created during `POST /register` and found in the response.*


Example Call
```sh
  curl --location --request GET 'http://localhost:4000/buy-card' \
    --header 'playerid: 0d57b4a0-a86f-11eb-94dd-e7a848edc63c'
```

Example Response
```sh
  {
    "id": "c968d920-a7d0-11eb-94dd-e7a848edc63c",
    "name": "Tigra",
    "strength": 0,
    "skill": 12,
    "size": 9.7,
    "popularity": 0.5
  }
```

<br>

#### GET /next-card
Returns information about the next card pulled from the deck of `playerid`.

Header Parameter
- `playerid` REQUIRED <br>
  *Include a user's playerid, created during `POST /register` and found in the response.*


Example Call
```sh
  curl --location --request GET 'http://localhost:4000/next-card' \
    --header 'playerid: 0d57b4a0-a86f-11eb-94dd-e7a848edc63c'
```

Example Response
```sh
  {
    "id": "c968d920-a7d0-11eb-94dd-e7a848edc63c",
    "name": "Tigra",
    "strength": 0,
    "skill": 12,
    "size": 9.7,
    "popularity": 0.5
  }
```

<br>

#### POST /battle
Returns the results of the battle between `playerid`'s next card and the computer.

Header Parameter
- `playerid` REQUIRED <br>
  *Include a user's playerid, created during `POST /register` and found in the response.*

Body Parameter
- `field` REQUIRED <br>
*Include the field the user would like to battle with. Options include `strength`, `skill`, `size`, and `popularity`.*

Example Call
```sh
  curl --location --request POST 'http://localhost:4000/battle' \
    --header 'playerid: 0d57b4a0-a86f-11eb-94dd-e7a848edc63c' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --data-urlencode 'field=strength'
```

Example Response
```sh
  {
    "outcome": "win",
    "card": {
        "id": "5d7164f0-a873-11eb-94dd-e7a848edc63c",
        "name": "Wonder Man",
        "strength": 47,
        "skill": 4,
        "size": 5.3,
        "popularity": 8.9
    },
    "opponentCard": {
        "id": "9382fa10-a7d1-11eb-94dd-e7a848edc63c",
        "name": "Daredevil",
        "strength": 44,
        "skill": 9,
        "size": 6.7,
        "popularity": 1.3
    }
  }
```

<br>

#### GET /cards
Returns an array of cards in the `playerid`'s deck.

Header Parameter
- `playerid` REQUIRED <br>
  *Include a user's playerid, created during `POST /register` and found in the response.*

Example Call
```sh
  curl --location --request GET 'http://localhost:4000/cards' \
    --header 'playerid: 0d57b4a0-a86f-11eb-94dd-e7a848edc63c'
```

Example Response
```sh
  [
    {
        "id": "0d57b4a2-a86f-11eb-94dd-e7a848edc63c",
        "name": "Kitty Pryde",
        "strength": 49,
        "skill": 9,
        "size": 5,
        "popularity": 1.9
    },
    ...
  ]
```

----
Batman icon, courtesy of [Vectoo](https://www.iconfinder.com/icons/2525034/batman_halloween_hero_super_hero_icon) under [CC BY-SA 3.0 license](https://creativecommons.org/licenses/by-sa/3.0/)
