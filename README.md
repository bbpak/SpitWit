# SpitWit

SpitWit is an online multiplayer party game clone of [Quiplash](https://jackboxgames.com/project/quiplash/). Each game can have 3-8 players in which players will submit answers for prompts, and then vote on their favorite answers. The prompts and answers are meant to be facetious and witty, and may contain some offensive content.

# Built with

- React
- Ruby on Rails
- ActionCable
- PostgreSQL

# Getting started

These instructions will get you a copy of the project up and running on your local machine for development.

## Prerequisites

- Ruby on Rails: https://guides.rubyonrails.org/v5.0/getting_started.html
- PostgreSQL: https://www.postgresql.org/download/

## Development setup

Clone the repository to the local machine. In the root directory, the application is split into its client and server files. The server and client will both have to be running for the application to run in development.

### Server

The server will serve as the Rails API endpoint for the client application.
First, start up PostgreSQL. Then set up the database with these commands.

```sh
rails db:create
rails db:migrate
rails db:seed
```

Once the development database is set up on PostgreSQL, run this command to start the Rails server. The application will be running on `localhost:3000`.

```sh
rails s
```

### Client

Install the dependencies for the app with the following command.

```sh
yarn install
```

Then start the React app with this command. Since the Rails API is already running on `localhost:3000`, you will have to choose a different port for the client. The React app will prompt the user to switch to `localhost:3001` by default.

```sh
yarn start
```

Make sure that the API endpoint in the client code refers to `localhost:3000` when running the local development server.

## Deployment

To deploy the app onto a live system to allow multiplayer gameplay, both the server and client will have to be hosted on a platform such as heroku.

### Separating client and server

In the application root, remove git with this command.

```sh
rm -rf .git
```

The client and server will have to be set up as individual git repositories so that they can be deployed separately. In this example, heroku will be used as the hosting platform. In each of the `client` and `server` directories, run the following, replacing `heroku` with the platform of choice.

```sh
heroku create
git add .
git commit -m "init"
git push heroku master
```

Change the API endpoint in the client code such that it refers to the correct URL for the server API. Then set up the database in the `server`. Seeding the database will create all of the prompts seeded from Quiplash XL.

```sh
heroku run rake db:migrate
heroku run rake db:seed
```

# Usage

On the homepage, a user can create a new game with the create game button, or join an existing one by entering in a room code or through quick join.

## Creating/Joining a game

If a user creates a game, the user will be the host for that game instance, and be placed into a lobby prompting for the player name. The lobby contains the room code which the host should share with other players so that they can join the same game.

A user can join a game by entering the 4-character room code, or by pressing the quick join button. Quick join will place the player into the most recently created game that has not yet started. Users will be placed into the respective lobbies by joining games.

## Playing the game
The game can be played at https://spitwit.herokuapp.com

### Answering prompts
Once the host starts the game when there are at least three players in the lobby, each of the players will be given 4 prompts to answer. The goal of the answers is to be witty such that other players will vote for your answer. Getting more votes awards more points, and the player with the most points at the end of the game will win the game. If the player does not submit an answer, the server will automatically fill in an answer seeded from Cards Against Humanity answers.

### Voting rounds
After the prompts are submitted and the timer runs out, each of the players will be given the prompts and the respective answers. The game will first display the prompt and answers during which players can vote. Then after the timer for the voting round expires, the game will display the number of votes each answer recieved as well as the corresponding points awarded. 

### Game end
At the end of the game, players will be able to see the leaderboard as well as the answer that received the most votes in the game. 
