![npm](https://img.shields.io/npm/v/@2bttns/sdk?style=for-the-badge)
![npm](https://img.shields.io/npm/dw/@2bttns/sdk?style=for-the-badge)
![npm](https://img.shields.io/npm/l/@2bttns/sdk?style=for-the-badge)

The official 2bttns Node.js SDK.

# Getting Started

The SDK provides a set of API endpoints for interacting with your Console that manages players, games, and game objects.

## Installation

```bash
npm install @2bttns/sdk
```

## Configuration

To use the SDK, you should instantiate the `TwoBttns` class using your 2bttns App ID, App Secret, and the base URL of your 2bttns app.

```typescript
/*  server/some/path/twobttns.ts  */
import TwoBttns from "@2bttns/sdk";
export const twobttns = new TwoBttns({
  appId: process.env.TWOBTTNS_APP_ID,
  secret: process.env.TWOBTTNS_APP_SECRET,
  url: process.env.TWOBTTNS_BASE_URL,
});
```

## Usage

> **Warning**
>
> The 2bttns SDK is intended for server-side use only.
>
> It should not be used by client-side code, because API requests are made using an access token generated using an API Key secret that should not be exposed.
>
> This SDK will throw an error if it detects it is being used in a browser environment.

After configuring the SDK, you can use your exported `twobttns` instance to interact with your 2bttns app.

### Create a Play URL

You can create a play URL using the `.generatePlayUrl(...)` method. This method will return a secure URL that you redirect your users to in order to play a 2bttns game you specify.

```typescript
/*  server/path/to/api/handler.ts  */
const url = twobttns.generatePlayUrl({
  game_id: "game_id",
  user_id: "user_id",
  num_items: "ALL",
  callback_url: "https://example.com/callback",
});
```

### Making 2bttns API Calls

You can perform API calls against the 2bttns API using the `.callApi(...)` method. The authentication process is handled automatically via a JWT generated from your App ID and App Secret.

```typescript
/*  server/path/to/api/handler.ts  */
const { data } = await twobttns.callApi("/players", "get");
```

<br/>

Easily pass parameters (body, query, path) through the `callApi` method:

```typescript
/*  server/path/to/api/handler.ts  */
const { data } = await twobttns.callApi("/example/create", "post", {
  id: playedId,
});
```


## API Endpoints

#### Games
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | [/games/getPlayerScores](#get--gamesgetplayerscores) | Get a player's score data for a specific game |
| GET | [/game-objects/ranked](#get--game-objectsranked) | Get ranked Game Object results for a player |

#### Tags
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | [/tags](#get--tags) | Get all tags |

#### Players
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | [/players/create](#post--playerscreate) | Create a player with a given ID and an optional name |
| GET | [/players](#get--players) | Get all players |
| GET | [/players/count](#get--playerscount) | Get player count |
| GET | [/players/{id}](#get--playersid) | Get player by ID |
| PUT | [/players/{id}](#put--playersid) | Update player by ID |
| DELETE | [/players/{id}](#delete--playersid) | Delete player by ID |

---

### Game Endpoints

#### `GET` /games/getPlayerScores 

Get a player's score data for a specific game.

**Required Parameters**
```typescript
{
  "gameId": "string",
  "playerId": "string"
}
```

**Response**
```json
{
  "playerScores": [
    {
      "createdAt": "string",
      "updatedAt": "string",
      "score": 0,
      "playerId": "string",
      "gameObjectId": "string",
      "gameObject": {
        "id": "string",
        "createdAt": "string",
        "updatedAt": "string",
        "name": "string",
        "description": "string"
      }
    }
  ]
}
```

---

#### `GET` /game-objects/ranked 

Get ranked Game Object results for a player.

**Required Parameters**
```typescript
{
  "playerId": "string",
  "inputTags": "string",
  "outputTag": "string"
}
```

**Response**
```json
{
  "scores": [
    {
      "gameObject": {
        "id": "string",
        "name": "string"
      },
      "score": 0
    }
  ]
}
```

---

### Tag Endpoints

#### `GET` /tags

Get all tags.

**Response**
```json
{
  "tags": [
    {
      "id": "string",
      "name": "string",
      "description": "string",
      "createdAt": "string",
      "updatedAt": "string"
    }
  ]
}
```

---

### Player Endpoints

#### `POST` /players/create 

Create a player with a given ID and an optional name.

**Required Parameters**
```typescript
{
  "id": "string",
  "name": "string"
}
```

**Response**
```json
{
  "createdPlayer": {
    "id": "HXXwoC3uzCiAv8p4kDx32iLu0SMk-V9MRD2whCxQ0rI2qKnemgUftm8bEqozW4GIXsb_LtJx_Xm",
    "name": "string",
    "createdAt": "string",
    "updatedAt": "string"
  }
}
```

---

#### `GET` /players

Get all players.

**Response**
```json
{
  "players": [
    {
      "id": "string",
      "name": "string",
      "createdAt": "string",
      "updatedAt": "string"
    }
  ]
}
```

---

#### `GET` /players/count

Get player count.

**Response**
```json
{
  "count": int
}
```

---

#### `GET` /players/{id}

Get player by ID.

**Required Parameters**
```typescript
{
  "id": "string"
}
```

**Response**
```json
{
  "player": {
    "id": "string",
    "name": "string",
    "createdAt": "string",
    "

updatedAt": "string"
  }
}
```

---

####  `PUT` /players/{id}

Update player by ID.

**Required Parameters**
```typescript
{
  "id": "string",
  "name": "string"
}
```

**Response**
```json
{
  "updatedPlayer": {
    "id": "string",
    "name": "string",
    "createdAt": "string",
    "updatedAt": "string"
  }
}
```

---

#### `DELETE` /players/{id}

Delete player by ID.

**Response**
```json
{
  "deletedPlayer": {
    "id": "string",
    "name": "string",
    "createdAt": "string",
    "updatedAt": "string"
  }
}
```

---

## License

[2bttns License 1.0](https://github.com/2bttns/.github/blob/main/profile/2bttns_LICENSE.md)