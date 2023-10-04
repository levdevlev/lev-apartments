# lev-apartments
Furnished Apartment Shell

[How to use (QBCore)](#how-to-use)
------
<img src="https://i.imgur.com/OQ8J0xk.png" width=50% height=50%><img src="https://i.imgur.com/g3IhkD1.png" width=50% height=50%>
<img src="https://i.imgur.com/YxW2v8C.png" width=50% height=50%><img src="https://i.imgur.com/1DYVrIJ.png" width=50% height=50%>
<img src="https://i.imgur.com/fLBWXMX.png" width=50% height=50%><img src="https://i.imgur.com/F9GnC2x.png" width=50% height=50%>
------

# How to use
* Put lev_apartment folder to resources folder (make sure you start it via server.cfg)
* Open **qb-interior / client / main.lua** and replace ```CreateApartmentFurnished``` export with this code:
  *
  ```lua
	exports('CreateApartmentFurnished', function(spawn)

	    local exit = json.decode('{"x": -0.3308, "y": -2.4607, "z": 1.3895, "h": 273.6090}')

	    local model = "lev_apartment_shell"
	    local obj = CreateShell(spawn, exit, model)
	    if obj and obj[2] then
			obj[2].clothes = json.decode('{"x": -7.04442, "y": -2.97699, "z": 0.960894, "h": 181.75}')
			obj[2].stash = json.decode('{"x": -0.221, "y": 0.1859, "z": 1.3895, "h": 90.00}')
			obj[2].logout = json.decode('{"x": 0.9797, "y": 2.1922, "z": 1.4326, "h": 270}')
		end
	    if IsNew then
		SetTimeout(750, function()
		    TriggerEvent('qb-clothes:client:CreateFirstCharacter')
		    IsNew = false
		end)
	    end
	    return { obj[1], obj[2] }
	end)
	```
  
## Optional
* If you want lighting effect that changes during night and day, go to **qb-apartments / client / main.lua** and find **local function EnterApartment(...)** and replace ```TriggerEvent('qb-weathersync:client:DisableSync')``` with ```TriggerEvent('qb-weathersync:client:EnableSync')```

## Notes
* If you are on **latest** QB-Core, use this snippet instead:
```lua
local function CreateShell(spawn, exitXYZH, model)
    local objects = {}
    local POIOffsets = {}
    POIOffsets.exit = exitXYZH
    DoScreenFadeOut(500)
    while not IsScreenFadedOut() do
        Wait(10)
    end
    RequestModel(model)
    while not HasModelLoaded(model) do
        Wait(1000)
    end
    local house = CreateObject(model, spawn.x, spawn.y, spawn.z, false, false, false)

    local spawnPointX = 0.089353
    local spawnPointY = -2.67699
    local spawnPointZ = 0.760894
    local spawnPointH = 270.76

    FreezeEntityPosition(house, true)
    objects[#objects+1] = house
    TeleportToInterior(spawn.x + spawnPointX, spawn.y + spawnPointY, spawn.z + spawnPointZ, spawnPointH)
    
    return { objects, POIOffsets }
end

exports('CreateShell', function(spawn, exitXYZH, model)
    return CreateShell(spawn, exitXYZH, model)
end)

-- Starting Apartment

exports('CreateApartmentFurnished', function(spawn)

    local exit = json.decode('{"x": 0.80353, "y": 1.94699, "z": 0.960894, "h": 270.76}')
    
    local model = "lev_apartment_shell"
    local obj = CreateShell(spawn, exit, model)
    if obj and obj[2] then
        obj[2].clothes = json.decode('{"x": -7.04442, "y": -2.97699, "z": 0.960894, "h": 181.75}')
        obj[2].stash = json.decode('{"x": -3.04442, "y": 2.17699, "z": 0.960894, "h": 181.75}')
        obj[2].logout = json.decode('{"x": 1.010176, "y": 2.29546, "z": 0.960894, "h": 91.18}')
    end
    if IsNew then
        SetTimeout(750, function()
            TriggerEvent('qb-clothes:client:CreateFirstCharacter')
            IsNew = false
        end)
    end
    return { obj[1], obj[2] }
end)
```


# How to use with PS-Housing
* Put lev_apartment folder to resources folder (make sure you start it via server.cfg)
* Open **ps-housing / shared / config.lua** and add code below on ```Config.Shells```
  *
```lua
["Lev Apartment"] = {
        label = "Lev Apartment",
        hash = `lev_apartment_shell`,
        doorOffset = { x = -0.460083, y = -2.334961, z = -1.524574, h = 271.055664, width = 2.0 },
        stash = {
            maxweight = 350000, 
            slots = 12,
        },
        imgs = {
            {
                url = "https://media.discordapp.net/attachments/1075836266005938216/1135668239083503646/image.png",
                label = "Entrance and Bedroom",
            },

        },
    },
```
If you want to use as apartment , you should change the ```Config.Apartments``` shell  to  ```shell = "Lev Apartment"```


# How to use with NoLag_Properties
* Put lev_apartment folder to resources folder (make sure you start it via server.cfg)
* Open **nolag_properties / custom / shells** and make a new file called LevShells.lua below in the file
  *
```lua
CreateThread(function()
    local LevShells = {
        -- Free Apartment Shell by Lev

        {
            nameTier = `lev_apartment_shell`,
            offset = vector4(0.089353, -2.67699, 0.760894, 270.76),
            label = 'Lev Apartment',
            inventory = { weight = 500000, slots = 40 }
        },
    }
    insertInteriors(LevShells)
end)
```
