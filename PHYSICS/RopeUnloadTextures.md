---
ns: PHYSICS
---
## ROPE_UNLOAD_TEXTURES

```c
// 0x6CE36C35C1AC8163 0x584463E0
cs_type(Any) void ROPE_UNLOAD_TEXTURES();
```

Unlike [`REQUEST_MODEL`](#_0x963D27A58DF860AC) and [`SET_MODEL_AS_NO_LONGER_NEEDED`](#_0xE532F5D78798DAAB) this **is not reference counted** by the game engine, meaning that the textures will be instantly unloaded, leaving the ropes invisible.

You should only call this once you are done with all ropes on the map.

Unloads **all** ropes textures.

# Examples
```lua
local function cleanup_rope_textures()
	-- we only want to cleanup if there are no other ropes still left on the map
	-- otherwise we'll make them go invisible.
	if #GetAllRopes() == 0 then
		-- there are no ropes on the map, we're safe to unload the textures.
		RopeUnloadTextures()
	end
end

CreateThread(function()
	-- if textures aren't loaded then we need to load them
	if not RopeAreTexturesLoaded() then
		-- load the textures so we can see the rope
		RopeLoadTextures()
		while not RopeAreTexturesLoaded() do
			Wait(0)
		end
	end

	-- Create a rope and store the handle
	local rope = AddRope(-2096.096436, -311.906891, 14.510918, 0.0, 0.0, 0.0, 10.0, 1, 10.0, 0.0, 1.0, false, false, false, 1.0, false, 0)
	-- Check if the rope exists.
	if not DoesRopeExist(rope) then
		cleanup_rope_textures()
		-- If the rope does not exist, end the execution of the code here.
		return
	end
	-- Let the rope exist for 3 seconds
	Wait(3000)
	-- Delete the rope!
	DeleteRope(rope)
	cleanup_rope_textures()
end)
```
