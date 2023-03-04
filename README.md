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
	local objects = {}
    local POIOffsets = {}
    POIOffsets.exit = json.decode('{"x": 0.80353, "y": 1.94699, "z": 0.960894, "h": 270.76}')
    POIOffsets.clothes = json.decode('{"x": -7.04442, "y": -2.97699, "z": 0.960894, "h": 181.75}')
    POIOffsets.stash = json.decode('{"x": -3.04442, "y": 2.17699, "z": 0.960894, "h": 181.75}')
    POIOffsets.logout = json.decode('{"x": 1.010176, "y": 2.29546, "z": 0.960894, "h": 91.18}')

	local spawnPointX = 0.089353
	local spawnPointY = -2.67699
	local spawnPointZ = 0.760894
	local spawnPointH = 270.76

    DoScreenFadeOut(500)
    while not IsScreenFadedOut() do
        Wait(10)
    end
	RequestModel(`lev_apartment_shell`)
	while not HasModelLoaded(`lev_apartment_shell`) do
		Wait(3)
	end

	local house = CreateObject(`lev_apartment_shell`, spawn.x, spawn.y, spawn.z, false, false, false)
    FreezeEntityPosition(house, true)
    objects[#objects+1] = house
	TeleportToInterior(spawn.x + spawnPointX, spawn.y + spawnPointY, spawn.z + spawnPointZ, spawnPointH)

	if IsNew then
		SetTimeout(750, function()
			TriggerEvent('qb-clothes:client:CreateFirstCharacter')
			IsNew = false
		end)
	end
    return { objects, POIOffsets }
  end)
  
## Optional
* If you want lighting effect that changes during night and day, go to **qb-apartments / client / main.lua** and find **local function EnterApartment(...)** and replace ```TriggerEvent('qb-weathersync:client:DisableSync')``` with ```TriggerEvent('qb-weathersync:client:EnableSync')```
