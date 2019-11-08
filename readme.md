# Ogario Protocol

This file contains detailed information about the client-server communications for Ogario servers.

## Data Types

Ogario uses standard data types, the table below is to be used a reference of some of the commonly used data types.

| Type      | Description                                                | Size
|-----------|------------------------------------------------------------|------
| uint8     | Unsigned integer (0 to 2<sup>8</sup>-1)                                | 1 byte
| uint32    | Unsigned integer (0 to 2<sup>32</sup>-1)                   | 4 bytes
| int32     | Signed integer (-2<sup>31</sup> to 2<sup>31</sup>-1)       | 4 bytes
| string16    | Null-terminated UTF-16 string (ends when next byte is NULL) | length + 2 bytes
| boolean   | True or false value                                        | 1 byte

_**Note:** Ogario encodes bytes as little-endian._

## Packet Structure

All packets sent in Ogario are formatted as such (with the opcode being the first byte):

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Operation code (opcode)
| -        | -         | -

## Server Packets (Server &rarr; Client)

| Opcode | Name
|--------|------
| 0      | [Player ID](#server-packet-0-player-id)
| 1      | [Request Player Update](#server-packet-1-request-player-update)
| 20     | [Update Team Player](#server-packet-20-update-team-player)
| 30     | [Update Team Player Position](#server-packet-30-update-team-player-position)
| 100    | [Chat Message](#server-packet-100-chat-message)


### Server Packet 0: Player ID

This packet is sent after handshake, it includes the player ID.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (0)
| 1        | uint32    | Player ID


### Server Packet 1: Request Player Update

This packet is sent when the server needs player info. The client needs to send [Player Update](#client-packet-20-player-update)

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (1)


### Server Packet 20: Update Team Player

This packet is sent when the client joins the room or another user joins changes their info.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (20)
| 1        | uint32     | Player ID
| 5        | string16     | Player Nick
| -        | string16     | Player Skin
| -        | string16     | Player Custom Color
| -        | string16     | Player Cell Color


### Server Packet 30: Update Team Player Position

This packet is sent every second and contains the position and mass of a team player.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (30)
| 1        | uint32     | Player ID
| 5        | int32     | Player X
| 9        | int32     | Player Y
| 13        | uint32     | Player Mass


### Server Packet 100: Chat Message

This packet is sent when another user or the client sends a chat message.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (100)
| 1        | uint8     | Message Type 100 or 101
| 2        | uint32     | Player ID
| 6        | uint32     | ???
| 10        | string16     | String cotaining the message e.g. "[USER]: Hello"


## Client Packets (Client &rarr; Server)

| Opcode | Name
|--------|------
| 0      | [Server Handshake](#client-packet-0-handshake)
| 1      | [Client Join](#client-packet-1-player-join)
| 2      | [Client Spawn](#client-packet-2-player-spawn)
| 3      | [Client Death](#client-packet-3-player-death)
| 10      | [Client Nick](#client-packet-10-player-nick)
| 11      | [Client Tag](#client-packet-11-player-tag)
| 15      | [Client Party](#client-packet-15-party-token)
| 16      | [Client Server Token](#client-packet-16-server-token)
| 17      | [Client Region](#client-packet-17-player-region)
| 18      | [Client Gamemode](#client-packet-18-player-gamemode)
| 20      | [Client Update](#client-packet-20-player-update)
| 30      | [Client Position Update](#client-packet-30-player-position-update)
| 100      | [Chat Message](#client-packet-100-chat-message)


### Client Packet 0: Handshake

This packet is sent to the WebSocket connection opens, if it is not sent or the wrong key is provided you get kicked from the server.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (0)
| 1        | uint32     | Handshake Key(401)



### Client Packet 1: Player Join

This packet is sent to the server when the client joins a new Agar.io server.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (1)


### Client Packet 2: Player Spawn

This packet is sent to the server when the client spawns in an Agar.io server.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (2)


### Client Packet 3: Player Death

This packet is sent to the server when the client dies in an Agar.io server.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (3)

### Client Packet 10: Player Nick

This packet is sent after the WebSocket Opens or after [Client Join](#client-packet-1-player-join).

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (10)
| 1        | string16    | Client Nick

### Client Packet 11: Player Tag

This packet is sent after the WebSocket Opens or after [Client Join](#client-packet-1-player-join).

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (11)
| 1        | string16    | Client Tag

### Client Packet 15: Party Token

This packet is sent after the WebSocket Opens or after [Client Join](#client-packet-1-player-join).

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (15)
| 1        | string16    | Client Party Token

### Client Packet 16: Server Token

This packet is sent after the WebSocket Opens or after [Client Join](#client-packet-1-player-join).

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (16)
| 1        | string16    | Client Server Token if server ip is "live-arena-12mxpoc.agar.io" you would send "12mxpoc"



### Client Packet 17: Player Region

This packet is sent after the WebSocket Opens or after [Client Join](#client-packet-1-player-join).

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (17)
| 1        | string16    | Client Region. Instead of "EU-London" you would send "EU"

### Client Packet 18: Player Gamemode

This packet is sent after the WebSocket Opens or after [Client Join](#client-packet-1-player-join).

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (18)
| 1        | string16    | Client Gamemode "FFA" or "PTY" or "EXP"

### Client Packet 20: Player Update

This packet is sent when the server sends [Request Player Update](#server-packet-1-request-player-update).

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (20)
| 1        | uint32    | Client ID
| 5        | string16    | Client Nick
| -        | string16    | Client Skin
| -        | string16    | Client Custom Color
| -        | string16    | Client Cell Color

### Client Packet 30: Player Position Update

This packet is sent every second when the player is alive.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (30)
| 1        | uint32    | Client ID
| 5        | int32    | Client X
| 9        | int32    | Client Y
| 13        | int32    | Client Mass

### Client Packet 100: Chat Message

This packet is sent when the client wants to send a chat message.

| Position | Data Type | Description
|----------|-----------|-------------
| 0        | uint8     | Opcode (100)
| 1        | uint8     | Message Type 100 or 101
| 2        | uint32     | Player ID
| 6        | uint32     | ??? not sure what this is, just send 0
| 10        | string16     | String cotaining the message e.g. "[USER]: Hello"
