Pequeños Aventureras
=====================

Create small adventurers. Create small bands of adventurers. Teach your small
adventurers.

# Notes

Send a POST to the Mojang authentication servers with a payload of json, where
the keys _username_ and _password_ must be supplied and a _username_ is the
email of a mojang account.

```
curl -H "Content-Type: application/json" -X POST \
   --data '{"agent":{"name":"Minecraft","version":1},"username":"...","password":"..."}' \
   https://authserver.mojang.com/authenticate
```
