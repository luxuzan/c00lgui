-- c00lgui - the ultimate exploit suite (all in one)

-- services
local players = game:GetService("players")
local replicatedstorage = game:GetService("replicatedstorage")
local serverscriptservice = game:GetService("serverscriptservice")
local runservice = game:GetService("runservice")
local uis = game:GetService("userinputservice")

local player = players.localplayer

----------------------------------------------------------------
-- remote event setup
----------------------------------------------------------------
local remote = replicatedstorage:FindFirstChild("actionevent")
if not remote then
	remote = Instance.new("remoteevent")
	remote.Name = "actionevent"
	remote.Parent = replicatedstorage
end

----------------------------------------------------------------
-- inject server-side handler (requires exploit with server injection)
----------------------------------------------------------------
local server_code = [[
	local replicatedstorage = game:GetService("replicatedstorage")
	local players = game:GetService("players")
	local remote = replicatedstorage:WaitForChild("actionevent")
	
	remote.OnServerEvent:Connect(function(player, data)
		if data.action == "setname" then
			local newname = data.name
			if typeof(newname) == "string" and newname ~= "" then
				player.displayname = newname
				print(player.name .. "'s display name set to " .. newname)
			end
		elseif data.action == "announce" then
			-- announce to all players: fire back a client event to display announcement UI
			for _, p in ipairs(players:GetPlayers()) do
				remote:FireClient(p, {action = "announceclient", message = "c00lkidd revival!"})
			end
		elseif data.action == "fling" then
			local targetname = data.name
			local target = players:FindFirstChild(targetname)
			if target and target.Character and target.Character:FindFirstChild("humanoidrootpart") then
				local hrp = target.Character.humanoidrootpart
				local newvel = Instance.new("bodyvelocity")
				newvel.Velocity = Vector3.new(math.random(-500,500), math.random(200,500), math.random(-500,500))
				newvel.MaxForce = Vector3.new(10000,10000,10000)
				newvel.P = 5000
				newvel.Parent = hrp
				wait(1)
				newvel:Destroy()
				print("flung " .. targetname)
			else
				print("target not found or invalid")
			end
		end
	end)
]]
local server_script = serverscriptservice:FindFirstChild("c00lgui_server")
if not server_script then
	server_script = Instance.new("script")
	server_script.Name = "c00lgui_server"
	server_script.Source = server_code
	server_script.Parent = serverscriptservice
end

----------------------------------------------------------------
-- create gui
----------------------------------------------------------------
local screen_gui = Instance.new("screengui")
screen_gui.Name = "c00lgui"
screen_gui.ResetOnSpawn = false
screen_gui.Parent = player:WaitForChild("playergui")

local main_frame = Instance.new("frame")
main_frame.Name = "mainframe"
main_frame.Size = UDim2.new(0,300,0,400)
main_frame.Position = UDim2.new(0.5,-150,0.5,-200)
main_frame.BackgroundColor3 = Color3.new(0,0,0)
main_frame.BorderSizePixel = 5 -- important stuff border
main_frame.BorderColor3 = Color3.new(1,1,1)
main_frame.Parent = screen_gui

local title_label = Instance.new("textlabel")
title_label.Name = "title"
title_label.Size = UDim2.new(1,0,0.1,0)
title_label.Position = UDim2.new(0,0,0,0)
title_label.BackgroundTransparency = 1
title_label.Text = "c00lgui"
title_label.Font = Enum.Font["press start 2p"]
title_label.TextSize = 30
title_label.TextColor3 = Color3.new(1,1,1)
title_label.Parent = main_frame

-- utility function to create a button with our style
local function create_button(btn_name, btn_text, posY)
	local btn = Instance.new("textbutton")
	btn.Name = btn_name
	btn.Size = UDim2.new(1,0,0.1,0)
	btn.Position = UDim2.new(0,0,posY,0)
	btn.BackgroundColor3 = Color3.new(1,1,1)
	btn.BorderSizePixel = 3   -- important border for buttons
	btn.BorderColor3 = Color3.new(1,1,1)
	btn.Font = Enum.Font["press start 2p"]
	btn.Text = btn_text
	btn.TextSize = 20
	btn.TextColor3 = Color3.new(0,0,0)
	btn.Parent = main_frame
	return btn
end

-- create buttons
local fly_btn = create_button("fly", "fly", 0.1)
local announce_btn = create_button("announce", "announce", 0.2)
local setname_btn = create_button("setname", "set name", 0.3)
local fling_btn = create_button("fling", "fling username", 0.4)
local xgames_btn = create_button("xgames", "x games mode", 0.5)
local jumppad_btn = create_button("jumppad", "jump pad", 0.6)

-- textboxes for setname and fling
local setname_box = Instance.new("textbox")
setname_box.Name = "nameinput"
setname_box.Size = UDim2.new(1,-20,0.1,0)
setname_box.Position = UDim2.new(0,10,0.7,0)
setname_box.BackgroundColor3 = Color3.new(1,1,1)
setname_box.BorderSizePixel = 3
setname_box.BorderColor3 = Color3.new(1,1,1)
setname_box.Font = Enum.Font["press start 2p"]
setname_box.PlaceholderText = "enter name"
setname_box.Text = ""
setname_box.TextSize = 20
setname_box.TextColor3 = Color3.new(0,0,0)
setname_box.Parent = main_frame

local fling_box = Instance.new("textbox")
fling_box.Name = "flinginput"
fling_box.Size = UDim2.new(1,-20,0.1,0)
fling_box.Position = UDim2.new(0,10,0.8,0)
fling_box.BackgroundColor3 = Color3.new(1,1,1)
fling_box.BorderSizePixel = 3
fling_box.BorderColor3 = Color3.new(1,1,1)
fling_box.Font = Enum.Font["press start 2p"]
fling_box.PlaceholderText = "enter target username"
fling_box.Text = ""
fling_box.TextSize = 20
fling_box.TextColor3 = Color3.new(0,0,0)
fling_box.Parent = main_frame

----------------------------------------------------------------
-- secret activation: key combo "c", "0", "0", "l", "g", "u", "i" in 10 seconds
----------------------------------------------------------------
local secret_combo = {"c","0","0","l","g","u","i"}
local combo_index = 1
local combo_time = 10
local last_key_time = 0

uis.InputBegan:Connect(function(input,gameprocessed)
    if gameprocessed then return end
    if input.UserInputType == Enum.UserInputType.Keyboard then
        local key = input.KeyCode.Name:lower()
        if os.clock() - last_key_time > combo_time then
            combo_index = 1
        end
        last_key_time = os.clock()
        if secret_combo[combo_index] == key then
            combo_index = combo_index + 1
            if combo_index > #secret_combo then
                screen_gui.Enabled = true
                combo_index = 1
            end
        else
            combo_index = 1
        end
    end
end)
screen_gui.Enabled = false

----------------------------------------------------------------
-- button functionality
----------------------------------------------------------------

-- fly: toggles bodyvelocity for flying
local flying = false
fly_btn.MouseButton1Click:Connect(function()
    flying = not flying
    local char = player.Character
    if char and char:FindFirstChild("humanoidrootpart") then
        local hrp = char.humanoidrootpart
        if flying then
            local bv = Instance.new("bodyvelocity")
            bv.Velocity = Vector3.new(0,50,0)
            bv.MaxForce = Vector3.new(400000,400000,400000)
            bv.Parent = hrp
            fly_btn.Text = "fly (on)"
        else
            local old = hrp:FindFirstChildWhichIsA("bodyvelocity")
            if old then old:Destroy() end
            fly_btn.Text = "fly"
        end
    end
end)

-- announce: fire remote for server to broadcast announcement
announce_btn.MouseButton1Click:Connect(function()
    local data = {action="announce"}
    remote:FireServer(data)
end)

-- setname: send name from setname_box to server
setname_btn.MouseButton1Click:Connect(function()
    local newname = setname_box.Text
    if newname ~= "" then
        local data = {action="setname", name=newname}
        remote:FireServer(data)
    end
end)

-- fling: send target username to server
fling_btn.MouseButton1Click:Connect(function()
    local targetname = fling_box.Text
    if targetname ~= "" then
        local data = {action="fling", name=targetname}
        remote:FireServer(data)
    end
end)

-- x games mode: toggle walkspeed and fov on client
local xgames_on = false
xgames_btn.MouseButton1Click:Connect(function()
    xgames_on = not xgames_on
    local char = player.Character
    if char and char:FindFirstChild("humanoid") then
        local hum = char.humanoid
        if xgames_on then
            hum.WalkSpeed = 60
            workspace.currentcamera.FieldOfView = 100
            xgames_btn.Text = "x games mode (on)"
        else
            hum.WalkSpeed = 16
            workspace.currentcamera.FieldOfView = 70
            xgames_btn.Text = "x games mode"
        end
    end
end)

-- jump pad: spawn an invisible platform under the player temporarily
jumppad_btn.MouseButton1Click:Connect(function()
    local char = player.Character
    if char and char:FindFirstChild("humanoidrootpart") then
        local hrp = char.humanoidrootpart
        local plat = Instance.new("part")
        plat.Size = Vector3.new(5,1,5)
        plat.Position = hrp.Position - Vector3.new(0,5,0)
        plat.Anchored = true
        plat.Transparency = 1
        plat.CanCollide = false
        plat.Parent = workspace
        wait(1)
        plat:Destroy()
    end
end)

-- server event handler for announce (client listens to remote events)
remote.OnClientEvent:Connect(function(data)
    if data.action == "announceclient" then
        local ann_frame = Instance.new("frame")
        ann_frame.Size = UDim2.new(1,0,0.2,0)
        ann_frame.Position = UDim2.new(0,0,0,0)
        ann_frame.BackgroundColor3 = Color3.new(0,0,0)
        ann_frame.BorderSizePixel = 5
        ann_frame.Parent = screen_gui

        local ann_label = Instance.new("textlabel")
        ann_label.Size = UDim2.new(1,0,1,0)
        ann_label.BackgroundTransparency = 1
        ann_label.Text = data.message
        ann_label.Font = Enum.Font["press start 2p"]
        ann_label.TextSize = 20
        ann_label.TextColor3 = Color3.new(1,1,1)
        ann_label.Parent = ann_frame

        wait(5)
        ann_frame:Destroy()
    end
end)
-- end of c00lgui
