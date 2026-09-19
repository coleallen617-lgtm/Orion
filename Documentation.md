# Orion Library
This documentation is for the stable release of Orion Library.

## Booting the Library
```lua
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/jensonhirst/Orion/main/source')))()
```



## Creating a Window
local Window = OrionLib:MakeWindow({
   Name = "Just Monika OS - MM2 Edition", 
   HidePremium = true, 
   SaveConfig = true, 
   ConfigFolder = "MonikaScriptSettings",
   IntroEnabled = true,
   IntroText = "Loading Monika.chr...",
   IntroIcon = "rbxassetid://116075205517234"
})

local Tab = Window:MakeTab({
   Name = "MM2 Utilities",
   Icon = "rbxassetid://116075205517234",
   PremiumOnly = false
})



## Creating a Tab
local Tab = Window:MakeTab({
	Name = "Monika.chr Premium",
	Icon = "rbxassetid://116075205517234",
	PremiumOnly = true
})

## Creating a Section
```lua
local Section = Tab:AddSection({
	Name = "Monika's Visuals"
})

Tab:AddToggle({
	Name = "Reveal Player Roles (ESP)",
	Default = false,
	Callback = function(Value)
		print(Value)
	end
})

```

## Notifying the user
```lua
OrionLib:MakeNotification({
	Name = "💚 monika.chr",
	Content = "Oh, hello there, " .. game.Players.LocalPlayer.Name .. "! Let me execute the script for you... 😉",
	Image = "rbxassetid://116075205517234",
	Time = 5
})



task.spawn(function()
    local GunNotified = false
    while true do
        task.wait(1)
        local GunDrop = game.Workspace:FindFirstChild("GunDrop")
        if GunDrop and not GunNotified then
            GunNotified = true
            OrionLib:MakeNotification({
               Name = "💚 monika.chr",
               Content = "Look! The gun dropped. Go pick it up! 👉",
               Image = "rbxassetid://75235649339045",
               Time = 6
            })
        elseif not GunDrop then
            GunNotified = false
        end
    end
end)



## Creating a Checkbox toggle
```lua
local AntiFlingActive = false
Tab:AddToggle({
	Name = "Anti-Fling Protection",
	Default = false,
	Callback = function(Value)
		AntiFlingActive = Value
		if AntiFlingActive then
			task.spawn(function()
				while AntiFlingActive and task.wait(0.1) do
					for _, p in ipairs(game.Players:GetPlayers()) do
						if p ~= game.Players.LocalPlayer and p.Character then
							for _, part in ipairs(p.Character:GetDescendants()) do
								if part:IsA("BasePart") then
									part.CanCollide = false
									part.Velocity = Vector3.new(0, 0, 0)
									part.RotVelocity = Vector3.new(0, 0, 0)
								end
							end
						end
					end
				end
			end)
		else
			for _, p in ipairs(game.Players:GetPlayers()) do
				if p.Character then
					for _, part in ipairs(p.Character:GetDescendants()) do
						if part:IsA("BasePart") then
							part.CanCollide = true
						end
					end
				end
			end
		end
	end    
})
local CoinFarmActive = false
Tab:AddToggle({
	Name = "Automated Coin Farm",
	Default = false,
	Callback = function(Value)
		CoinFarmActive = Value
		if CoinFarmActive then
			task.spawn(function()
				while CoinFarmActive and task.wait(0.3) do
					local MainContainer = game.Workspace:FindFirstChild("Normal") or game.Workspace:FindFirstChild("Map")
					if MainContainer then
						for _, obj in ipairs(MainContainer:GetDescendants()) do
							if CoinFarmActive and (obj.Name == "Coin_Sub" or obj.Name == "CoinContainer" or (obj:IsA("TouchTransmitter") and obj.Parent.Name == "Coin")) then
								local CoinPart = obj.Parent
								local LocalChar = game.Players.LocalPlayer.Character
								if CoinPart and LocalChar and LocalChar:FindFirstChild("HumanoidRootPart") and LocalChar:FindFirstChildOfClass("Humanoid") and LocalChar:FindFirstChildOfClass("Humanoid").Health > 0 then
									LocalChar.HumanoidRootPart.CFrame = CoinPart.CFrame
									task.wait(0.2)
								end
							end
						end
					end
				end
			end)
		end
	end    
})
local XRayActive = false
Tab:AddToggle({
	Name = "Glitch Reality (X-Ray / Noclip)",
	Default = false,
	Callback = function(Value)
		XRayActive = Value
		if XRayActive then
			task.spawn(function()
				while XRayActive and task.wait(0.1) do
					local Character = game.Players.LocalPlayer.Character
					if Character then
						for _, part in ipairs(Character:GetDescendants()) do
							if part:IsA("BasePart") and part.CanCollide then
								part.CanCollide = false
							end
						end
					end
				end
			end)
		else
			local Character = game.Players.LocalPlayer.Character
			if Character then
				for _, part in ipairs(Character:GetDescendants()) do
					if part.Name == "HumanoidRootPart" or part.Name == "UpperTorso" or part.Name == "LowerTorso" then
						part.CanCollide = true
					end
				end
			end
		end
	end    
})
local FlyActive = false
local FlySpeed = 50
local BodyGyro, BodyVelocity

Tab:AddToggle({
	Name = "Glitch Flight Control (Fly)",
	Default = false,
	Callback = function(Value)
		FlyActive = Value
		local LocalPlayer = game.Players.LocalPlayer
		local Camera = game.Workspace.CurrentCamera
		
		if FlyActive then
			local Character = LocalPlayer.Character
			if Character and Character:FindFirstChild("HumanoidRootPart") then
				BodyGyro = Instance.new("BodyGyro")
				BodyGyro.P = 9e4
				BodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
				BodyGyro.cframe = Character.HumanoidRootPart.CFrame
				BodyGyro.Parent = Character.HumanoidRootPart
				
				BodyVelocity = Instance.new("BodyVelocity")
				BodyVelocity.velocity = Vector3.new(0, 0.1, 0)
				BodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)
				BodyVelocity.Parent = Character.HumanoidRootPart
				
				task.spawn(function()
					local Humanoid = Character:FindFirstChildOfClass("Humanoid")
					if Humanoid then
						Humanoid.PlatformStand = true
					end
					
					while FlyActive and task.wait(0.01) do
						if Character and Character:FindFirstChild("HumanoidRootPart") and Humanoid and Humanoid.Health > 0 then
							local Direction = Vector3.new(0, 0, 0)
							local MoveDirection = Humanoid.MoveDirection
							
							if MoveDirection.Magnitude > 0 then
								Direction = MoveDirection * FlySpeed
							end
							
							BodyGyro.cframe = Camera.CFrame
							BodyVelocity.velocity = Direction
						else
							break
						end
					end
					
					if Humanoid then
						Humanoid.PlatformStand = false
					end
					if BodyGyro then BodyGyro:Destroy() end
					if BodyVelocity then BodyVelocity:Destroy() end
				end)
			end
		else
			local Character = LocalPlayer.Character
			if Character and Character:FindFirstChildOfClass("Humanoid") then
				Character:FindFirstChildOfClass("Humanoid").PlatformStand = false
			end
			if BodyGyro then BodyGyro:Destroy() end
			if BodyVelocity then BodyVelocity:Destroy() end
		end
	end    
})
local SilentAimActive = false
local Camera = game.Workspace.CurrentCamera

local function GetClosestTarget()
    local Closest, MaxDist = nil, math.huge
    for _, p in ipairs(game.Players:GetPlayers()) do
        if p ~= game.Players.LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            local ScreenPos, OnScreen = Camera:WorldToViewportPoint(p.Character.HumanoidRootPart.Position)
            if OnScreen then
                local Dist = (Vector2.new(ScreenPos.X, ScreenPos.Y) - Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)).Magnitude
                if Dist < MaxDist then
                    Closest = p.Character.HumanoidRootPart
                    MaxDist = Dist
                end
            end
        end
    end
    return Closest
end

local Hook
Hook = hookmetamethod(game, "__index", function(Self, Index)
    if SilentAimActive and tostring(Self) == "Mouse" and Index == "Hit" then
        local Target = GetClosestTarget()
        if Target then
            return Target.CFrame
        end
    end
    return Hook(Self, Index)
end)

Tab:AddToggle({
	Name = "Silent Aim (Auto-Assist)",
	Default = false,
	Callback = function(Value)
		SilentAimActive = Value
	end    
})
local InfiniteJumpActive = false
local JumpConnection

Tab:AddToggle({
	Name = "Infinite Jump Override",
	Default = false,
	Callback = function(Value)
		InfiniteJumpActive = Value
		if InfiniteJumpActive then
			JumpConnection = game:GetService("UserInputService").JumpRequest:Connect(function()
				local Char = game.Players.LocalPlayer.Character
				if Char and Char:FindFirstChildOfClass("Humanoid") then
					Char:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
				end
			end)
		else
			if JumpConnection then
				JumpConnection:Disconnect()
			end
		end
	end    
})
local SpectateDropdown = Tab:AddDropdown({
	Name = "Spectate Targeted Player",
	Default = "None",
	Options = {"None"},
	Callback = function(SelectedPlayerName)
		local Camera = game.Workspace.CurrentCamera
		if SelectedPlayerName == "None" then
			Camera.CameraSubject = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
		else
			local TargetPlayer = game.Players:FindFirstChild(SelectedPlayerName)
			if TargetPlayer and TargetPlayer.Character and TargetPlayer.Character:FindFirstChildOfClass("Humanoid") then
				Camera.CameraSubject = TargetPlayer.Character:FindFirstChildOfClass("Humanoid")
			end
		end
	end    
})

task.spawn(function()
	while task.wait(5) do
		local CurrentPlayers = {"None"}
		for _, p in ipairs(game.Players:GetPlayers()) do
			if p ~= game.Players.LocalPlayer then
				table.insert(CurrentPlayers, p.Name)
			end
		end
		SpectateDropdown:Refresh(CurrentPlayers, true)
	end
end)

--[[
Name = <string> - The name of the toggle.
Default = <bool> - The default value of the toggle.
Callback = <function> - The function of the toggle.
]]
```

### Changing the value of an existing Toggle
```lua
CoolToggle:Set(true)

```



## Creating a Color Picker
```lua
Tab:AddColorpicker({
	Name = "UI Glow Theme Changer",
	Default = Color3.fromRGB(85, 255, 120),
	Callback = function(Value)
		OrionLib.Color = Value
		OrionLib:Init()
	end	  
})

```

### Setting the color picker's value
```lua
 InnocentColorPicker:Set(Color3.fromRGB(85, 255, 120))
```


## Creating a Slider
```lua
Tab:AddSlider({
	Name = "Override Server Speed",
	Min = 16,
	Max = 200,
	Default = 16,
	Color = Color3.fromRGB(85, 255, 120),
	Increment = 1,
	ValueName = "Speed",
	Callback = function(Value)
		game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
	end    
})
```

### Change Slider Value
```lua
Slider:Set(2)
```


## Creating a Label
```lua
Tab:AddLabel("Just Monika OS v1.0 — System Fully Operational 💚")

```

### Changing the value of an existing label
```lua
local StatusLabel = Tab:AddLabel("System Status: Idle 🟢")

Tab:AddButton({
   Name = "Activate Overrides",
   Callback = function()
       StatusLabel:Set("System Status: Injecting Monika.chr... ⚡")
   end
})

```


## Creating a Paragraph
```lua
Tab:AddParagraph("Just Monika OS v1.0 — System Logs", "Welcome back, " .. game.Players.LocalPlayer.Name .. "! Every component is loaded. sayori.chr, yuri.chr, and natsuki.chr have been safely moved out of your way. Enjoy your match! 💚")

```

### Changing an existing paragraph
```lua
local HelpParagraph = Tab:AddParagraph("Monika Instruction Mode", "Current State: Waiting for Match to Start...")

task.spawn(function()
    while true do
        task.wait(1)
        local GunDrop = game.Workspace:FindFirstChild("GunDrop")
        if GunDrop then
            HelpParagraph:Set("⚠️ MONIKA SYSTEM ALERT ⚠️", "The Sheriff has been eliminated! The gun is currently dropped on the floor. Grab it immediately!")
        else
            HelpParagraph:Set("Monika Instruction Mode", "Current State: Sheriff is currently alive. Play safely and blend in with the innocents.")
        end
    end
end)

```


## Creating an Adaptive Input
```lua
Tab:AddTextbox({
	Name = "Glitch-Teleport to Player",
	Default = "Type Username Here",
	TextDisappear = true,
	Callback = function(Value)
		local TargetPlayer = game.Players:FindFirstChild(Value)
		if TargetPlayer and TargetPlayer.Character and TargetPlayer.Character:FindFirstChild("HumanoidRootPart") then
			game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = TargetPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0)
		else
			OrionLib:MakeNotification({
				Name = "💚 monika.chr",
				Content = "I couldn't find a player named " .. Value .. " in this match... 🔍",
				Image = "rbxassetid://116075205517234",
				Time = 4
			})
		end
	end	  
})

```


## Creating a Keybind
```lua
Tab:AddBind({
	Name = "Toggle Menu Visibility",
	Default = Enum.KeyCode.RightControl,
	Hold = false,
	Callback = function()
		local TargetGUI = game:GetService("CoreGui"):FindFirstChild("Orion") or game.Players.LocalPlayer:FindFirstChild("PlayerGui"):FindFirstChild("Orion")
		if TargetGUI then
			TargetGUI.Enabled = not TargetGUI.Enabled
		end
	end    
})

```

### Chaning the value of a bind
```lua
Bind:Set(Enum.KeyCode.E)
```


## Creating a Dropdown menu
```lua
Tab:AddDropdown({
	Name = "Glitch-Teleport to Zone",
	Default = "Lobby",
	Options = {"Lobby", "Map Center", "Sheriff Gun Spawn"},
	Callback = function(Value)
		local TargetCFrame
		if Value == "Lobby" then
			TargetCFrame = CFrame.new(-109, 138, -10)
		elseif Value == "Map Center" then
			local MainContainer = game.Workspace:FindFirstChild("Normal") or game.Workspace:FindFirstChild("Map")
			if MainContainer and MainContainer:FindFirstChild("Spawns") then
				TargetCFrame = MainContainer.Spawns:GetChildren()[1].CFrame + Vector3.new(0, 3, 0)
			end
		elseif Value == "Sheriff Gun Spawn" then
			local GunDrop = game.Workspace:FindFirstChild("GunDrop")
			if GunDrop then
				TargetCFrame = GunDrop.CFrame + Vector3.new(0, 3, 0)
			end
		end
		
		if TargetCFrame then
			game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = TargetCFrame
		else
			OrionLib:MakeNotification({
				Name = "💚 monika.chr",
				Content = "I couldn't locate that zone array right now... 🔍",
				Image = "rbxassetid://116075205517234",
				Time = 3
			})
		end
	end    
})
```

### Adding a set of new Dropdown buttons to an existing menu
```lua
local TargetPlayerDropdown = Tab:AddDropdown({
	Name = "Select Target Player",
	Default = "None",
	Options = {"None"},
	Callback = function(Value)
		SelectedTargetPlayer = Value
	end    
})

task.spawn(function()
	while true do
		task.wait(5)
		local ActiveServerPlayers = {"None"}
		for _, p in ipairs(game.Players:GetPlayers()) do
			if p ~= game.Players.LocalPlayer then
				table.insert(ActiveServerPlayers, p.Name)
			end
		end
		TargetPlayerDropdown:Refresh(ActiveServerPlayers, true)
	end
end)

```

The above boolean value "true" is whether or not the current buttons will be deleted.
### Selecting a dropdown option
```lua
local TeleportDropdown = Tab:AddDropdown({
	Name = "Glitch-Teleport to Zone",
	Default = "Lobby",
	Options = {"Lobby", "Map Center", "Sheriff Gun Spawn"},
	Callback = function(Value)
		print("Selected: " .. Value)
	end    
})

Tab:AddButton({
   Name = "Panic Warp: Back to Lobby",
   Callback = function()
       TeleportDropdown:Set("Lobby")
   end
})

```

# Finishing your script (REQUIRED)
The below function needs to be added at the end of your code.
```lua
OrionLib:Init()
```

### How flags work.
The flags feature in the ui may be confusing for some people. It serves the purpose of being the ID of an element in the config file, and makes accessing the value of an element anywhere in the code possible.
Below in an example of using flags.
```lua
Tab:AddToggle({
    Name = "Automated Murderer Proximity Alert",
    Default = false,
    Save = true,
    Flag = "MurdererAlertToggle"
})

task.spawn(function()
    while task.wait(1) do
        if OrionLib.Flags["MurdererAlertToggle"] and OrionLib.Flags["MurdererAlertToggle"].Value then
            for _, p in ipairs(game.Players:GetPlayers()) do
                if p ~= game.Players.LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    if p.Backpack:FindFirstChild("Knife") or p.Character:FindFirstChild("Knife") then
                        local Distance = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude
                        if Distance < 40 then
                            OrionLib:MakeNotification({
                                Name = "⚠️ DANGER ⚠️",
                                Content = p.Name .. " (Murderer) is closing in! Distance: " .. math.floor(Distance) .. " studs!",
                                Image = "rbxassetid://75235649339045",
                                Time = 2
                            })
                        end
                    end
                end
            end
        end
    end
end)

```
Flags only work with the toggle, slider, dropdown, bind, and colorpicker.

### Making your interface work with configs.
In order to make your interface use the configs function you first need to add the `SaveConfig` and `ConfigFolder` arguments to your window function. The explanation of these arguments in above.
Then you need to add the `Flag` and `Save` values to every toggle, slider, dropdown, bind, and colorpicker you want to include in the config file.
The `Flag = <string>` argument is the ID of an element in the config file.
The `Save = <bool>` argument includes the element in the config file.
Config files are made for every game the library is launched in.

## Destroying the Interface
```lua
OrionLib:Destroy()
```
