# oxy-runs-qbus
oxy runs for qbus

add this to you client consumables.lua

RegisterNetEvent("consumables:client:Oxy")
AddEventHandler("consumables:client:Oxy", function(itemName)
    TriggerEvent('animations:client:EmoteCommandStart', {"drink"})
    QBCore.Functions.Progressbar("drink_something", "Down the pills..", 2500, false, true, {
        disableMovement = false,
        disableCarMovement = false,
		disableMouse = false,
		disableCombat = true,
    }, {}, {}, {}, function() -- Done
        TriggerEvent("inventory:client:ItemBox", QBCore.Shared.Items[itemName], "remove")
        TriggerEvent('animations:client:EmoteCommandStart', {"c"})
        TriggerServerEvent('rrp-hud:Server:RelieveStress', math.random(40, 50))
    end)
end)

add this to you server consumables.lua

QBCore.Functions.CreateUseableItem("oxy", function(source, item)
    local Player = QBCore.Functions.GetPlayer(source)
	if Player.Functions.RemoveItem(item.name, 1, item.slot) then
        TriggerClientEvent("consumables:client:Oxy", source)
    end
end)

and last but not least add this to your shared.lua in the core

["oxy"] 			             = {["name"] = "oxy", 				            ["label"] = "Oxy", 				        ["weight"] = 700, 		["type"] = "item", 		["image"] = "oxy.png", 		            ["unique"] = false, 	["useable"] = true, 	["shouldClose"] = true,	   ["combinable"] = nil,   ["description"] = "Get that stress GONE"},
