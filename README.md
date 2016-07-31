# Kozette-Client

[![npm](https://img.shields.io/npm/l/express.svg?maxAge=2592000)](./LICENSE.md)

![logo kozette](kozette-client/static/img/logo/kozette_large_transparent.png)



Kozette is a simple chat built with Feathers (backend) and Vue (frontend). Tends to go for an old school style, kinda web app terminal like.

![interface kozette](kozette-client/static/kozette_interface.jpg)

---

List of available commands :

- `/nick [*nickname*]` : change user nickname (and is username for login)
- `/color [*color*]` : change user color
- `/me [*message*]` : add a direct message linked to the user and his color
- `/status [*status(empty to remove)*]` : add a status on the users list
- `/pin [*message*]` : in addition to adding a classic message, pin the message to stay always visible on the pinned messages panel
- `/mute [ 'off' || *anything else* ]` : to mute sounds
- `/notify [ 'off' || *anything else* ]` : to mute the notifications

User visual roles :

- `@` : administrator
- `&` : moderator
- `+` : online basic user
- `-` : offline basic user

---
Special thanks to [Sion](https://github.com/fueledbycoffee), from fueledbc.com for his help with the code, the logo and the sounds !

Kozette is distributed under the [MIT License](http://opensource.org/licenses/MIT)

