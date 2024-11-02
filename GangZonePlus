
            // Definers
#define         MAX_ZONES               (1024)
#define         INVALID_GANGZONEID      (-1)

            // Includes
#include <streamer>
#include <YSI_Coding\y_hooks>

enum E_GANGZONE {
    E_GANGZONE_ID,
    E_GANGZONE_AREA,
    bool:E_GANGZONE_PERSIST,
    Float:E_GANGZONE_POSITION[4],
    E_GANGZONE_COLOR,
    E_GANGZONE_INTERIOR,
    E_GANGZONE_VIRTUAL,
    bool:E_GANGZONE_FLASHING,
    E_GANGZONE_COLOR_FLASH
};

enum E_GANGZONE_JOGADOR {
    E_GANGZONE_JOGADOR_INTERIOR,
    E_GANGZONE_JOGADOR_VIRTUAL
};

new 
    Iterator:GangZoneHide[MAX_PLAYERS]<MAX_ZONES>,
    Iterator:GangZones<MAX_ZONES>,
    e_GangZoneJogador[MAX_PLAYERS][E_GANGZONE_JOGADOR],
    e_GangZone[MAX_ZONES][E_GANGZONE];

hook OnPlayerConnect(playerid) {

    GangZoneDynamicPlayer(playerid);
    return Y_HOOKS_CONTINUE_RETURN_1;
}

hook OnPlayerDisconnect(playerid, reason) {

    static const e_Clear[E_GANGZONE_JOGADOR];
    e_GangZoneJogador[playerid] = e_Clear;

    Iter_Clear(GangZoneHide[playerid]);
    return Y_HOOKS_CONTINUE_RETURN_1;
}



/*

                    .oooooo..o ooooooooooooo   .oooooo.     .oooooo.   oooo    oooo  .oooooo..o 
                    d8P'    `Y8 8'   888   `8  d8P'  `Y8b   d8P'  `Y8b  `888   .8P'  d8P'    `Y8 
                    Y88bo.           888      888      888 888           888  d8'    Y88bo.      
                    `"Y8888o.       888      888      888 888           88888[       `"Y8888o.  
                        `"Y88b      888      888      888 888           888`88b.         `"Y88b 
                    oo     .d8P      888      `88b    d88' `88b    ooo   888  `88b.  oo     .d8P 
                    8""88888P'      o888o      `Y8bood8P'   `Y8bood8P'  o888o  o888o 8""88888P'  

*/

stock alsSetPlayerVirtualWorld(playerid, worldid) {

    e_GangZoneJogador[playerid][E_GANGZONE_JOGADOR_VIRTUAL] = worldid;

    GangZoneDynamicPlayer(playerid);
    return SetPlayerVirtualWorld(playerid, worldid);
}

stock alsSetPlayerInterior(playerid, interiorid) {

    e_GangZoneJogador[playerid][E_GANGZONE_JOGADOR_INTERIOR] = interiorid;

    GangZoneDynamicPlayer(playerid);
    return SetPlayerInterior(playerid, interiorid);
}

static stock IsValidDynamicGangZone(id) {
    if(id == INVALID_GANGZONEID) return false;
    if(Iter_Contains(GangZones, id)) return true;
    return false;
}

static stock GangZoneDynamicShow(playerid, index) {

    new zoneId = e_GangZone[index][E_GANGZONE_ID];
    GangZoneShowForPlayer(playerid, zoneId, e_GangZone[index][E_GANGZONE_COLOR]);

    if(e_GangZone[index][E_GANGZONE_FLASHING])
        GangZoneFlashForPlayer(playerid, zoneId, e_GangZone[index][E_GANGZONE_COLOR_FLASH]);
    
    Streamer_ToggleItem(playerid, STREAMER_TYPE_AREA, e_GangZone[index][E_GANGZONE_AREA], true);
}

static stock GangZoneDynamicHide(playerid, index) {

    new zoneId = e_GangZone[index][E_GANGZONE_ID];
    GangZoneHideForPlayer(playerid, zoneId);

    Streamer_ToggleItem(playerid, STREAMER_TYPE_AREA, e_GangZone[index][E_GANGZONE_AREA], false);
}

static stock GangZoneDynamicServidor() {

    foreach(new i: Player)
        GangZoneDynamicPlayer(i);
}

static stock GangZoneDynamicPlayer(playerid) {

    foreach(new i: GangZones) 
        GangZoneDynamicUpdate(playerid, i);
    
}

static stock GangZoneDynamicUpdate(playerid, id) {

    new bool:contains = Iter_Contains(GangZoneHide[playerid], id);

    if(e_GangZone[id][E_GANGZONE_INTERIOR] != -1 && e_GangZoneJogador[playerid][E_GANGZONE_JOGADOR_INTERIOR] != e_GangZone[id][E_GANGZONE_INTERIOR] || 
        e_GangZone[id][E_GANGZONE_VIRTUAL] != -1 && e_GangZoneJogador[playerid][E_GANGZONE_JOGADOR_VIRTUAL] != e_GangZone[id][E_GANGZONE_VIRTUAL] || 
        contains && !e_GangZone[id][E_GANGZONE_PERSIST]) {

        GangZoneDynamicHide(playerid, id);
        return;
    }

    GangZoneDynamicShow(playerid, id);
}

stock CreateDynamicGangZone(Float:minx, Float:miny, Float:maxx, Float:maxy, color, interior = -1, virtual = -1) {

    new index = Iter_Free(GangZones);
    
    if(index == -1) 
        return INVALID_GANGZONEID;

    e_GangZone[index][E_GANGZONE_COLOR] = color;
    e_GangZone[index][E_GANGZONE_INTERIOR] = interior;
    e_GangZone[index][E_GANGZONE_VIRTUAL] = virtual;
    e_GangZone[index][E_GANGZONE_ID] = GangZoneCreate(minx, miny, maxx, maxy);
    e_GangZone[index][E_GANGZONE_AREA] = CreateDynamicRectangle(minx, miny, maxx, maxy, virtual, interior, -1);
    Iter_Add(GangZones, index);

    GangZoneDynamicServidor();
    return index;
}

stock DestroyDynamicGangZone(id) {

    if(!IsValidDynamicGangZone(id)) 
        return;

    GangZoneDestroy(e_GangZone[id][E_GANGZONE_ID]);
    DestroyDynamicArea(e_GangZone[id][E_GANGZONE_AREA]);

    Iter_Remove(GangZones, id);
}

stock FlashDynamicGangZone(id, color) {

    if(!IsValidDynamicGangZone(id)) 
        return;

    e_GangZone[id][E_GANGZONE_FLASHING] = true;
    e_GangZone[id][E_GANGZONE_COLOR_FLASH] = color;
    GangZoneDynamicServidor();
}

stock StopFlashDynamicGangZone(id) {

    if(!IsValidDynamicGangZone(id)) 
        return;

    e_GangZone[id][E_GANGZONE_FLASHING] = false;
    GangZoneDynamicServidor();
}

stock SetColorDynamicGangZone(id, color) {

    if(!IsValidDynamicGangZone(id)) 
        return;

    e_GangZone[id][E_GANGZONE_COLOR] = color;
    GangZoneDynamicServidor();
}

stock SetColorFlashDynamicGangZone(id, color) {

    if(!IsValidDynamicGangZone(id)) 
        return;

    e_GangZone[id][E_GANGZONE_COLOR_FLASH] = color;
    GangZoneDynamicServidor();
}

stock GetDynamicGangZoneArea(id) {

    if(!IsValidDynamicGangZone(id))
        return INVALID_STREAMER_ID;

    return (e_GangZone[id][E_GANGZONE_AREA]);
}

stock IsDynamicGangZoneHide(playerid, id) {

    if(!IsValidDynamicGangZone(id)) 
        return true; 

    return (Iter_Contains(GangZoneHide[playerid], id));
}

stock SetDynamicGangZonePersist(id, bool:enabled) {

    if(e_GangZone[id][E_GANGZONE_PERSIST] == enabled)
        return;
    
    e_GangZone[id][E_GANGZONE_PERSIST] = enabled;
    GangZoneDynamicServidor();   
}

stock SetDynamicGangZoneHide(playerid, id) {

    if(IsDynamicGangZoneHide(playerid, id))
        return;

    Iter_Add(GangZoneHide[playerid], id);
    GangZoneDynamicUpdate(playerid, id);
}

stock RemoveDynamicGangZoneHide(playerid, id) {

    if(!IsDynamicGangZoneHide(playerid, id))
        return;

    Iter_Remove(GangZoneHide[playerid], id);
    GangZoneDynamicUpdate(playerid, id);
}

stock SetDynamicGangZoneHideAll(playerid) {

    foreach(new i: GangZones)
        SetDynamicGangZoneHide(playerid, i);
}

stock SetDynamicGangZoneShowAll(playerid) {

    foreach(new i: GangZones)
        RemoveDynamicGangZoneHide(playerid, i);
}



/*

                    ooo        ooooo       .o.         .oooooo.   ooooooooo.     .oooooo.    .oooooo..o 
                    `88.       .888'      .888.       d8P'  `Y8b  `888   `Y88.  d8P'  `Y8b  d8P'    `Y8 
                    888b     d'888      .8"888.     888           888   .d88' 888      888 Y88bo.      
                    8 Y88. .P  888     .8' `888.    888           888ooo88P'  888      888  `"Y8888o.  
                    8  `888'   888    .88ooo8888.   888           888`88b.    888      888      `"Y88b 
                    8    Y     888   .8'     `888.  `88b    ooo   888  `88b.  `88b    d88' oo     .d8P 
                    o8o        o888o o88o     o8888o  `Y8bood8P'  o888o  o888o  `Y8bood8P'  8""88888P'  

*/

#if defined _ALS_SetPlayerInterior
    #undef SetPlayerInterior
#else
    #define _ALS_SetPlayerInterior
#endif
 
#define SetPlayerInterior alsSetPlayerInterior

#if defined _ALS_SetPlayerVirtualWorld
    #undef SetPlayerVirtualWorld
#else
    #define _ALS_SetPlayerVirtualWorld
#endif
 
#define SetPlayerVirtualWorld alsSetPlayerVirtualWorld
