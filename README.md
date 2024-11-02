# 🌍 DynamicGangZone
This include creates new features that do not natively exist in the samp, functions such as synchronizing the gangzone without having to always redisplay it to the player during the connection and several other features that will help in creating systems on your server.

# Functions
```pawn
- CreateDynamicGangZone(Float:minx, Float:miny, Float:maxx, Float:maxy, color, interior = -1, virtual = -1)
- DestroyDynamicGangZone(id)
- FlashDynamicGangZone(id, color)
- StopFlashDynamicGangZone(id)
- SetColorDynamicGangZone(id, color)
- SetColorFlashDynamicGangZone(id, color)
- GetDynamicGangZoneArea(id)
- IsDynamicGangZoneHide(playerid, id)
- SetDynamicGangZonePersist(id, bool:enabled) - This function keeps this gangzone active even if you disable it for the player or for everyone
- SetDynamicGangZoneHide(playerid, id)
- RemoveDynamicGangZoneHide(playerid, id)
- SetDynamicGangZoneHideAll(playerid)
- SetDynamicGangZoneShowAll(playerid)
```
