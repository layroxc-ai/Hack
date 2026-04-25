-- [[ LAYROXC HUB v59 - THE ABSOLUTE ENGINE (ENGLISH VERSION) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local TweenService = game:GetService("TweenService")
local MarketplaceService = game:GetService("MarketplaceService")

-- [[ MOBILE TOGGLE BUTTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ADVANCED ROLE DETECTION ]] --
local function GetPlayerRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    
    local role = "Innocent"
    pcall(function()
        local gui = v.PlayerGui.MainGui.Game.RoleDesc.Text:lower()
        if gui:find("murderer") then role = "MURDERER"
        elseif gui:find("sheriff") or gui:find("hero") then role = "SHERIFF" end
    end)
    return role
end

-- TABS
local Main = Window:NewTab("Combat (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Pro = Window:NewTab("Avatar & Fix")

-- [[ 1. COMBAT - KILL ALL / AIMBOT / TP ]] --
local RageSec = Main:NewSection("Execution Engine")

RageSec:NewButton("KILL ALL (CLEANUP)", "Instantly kills everyone if you are Murderer", function()
    local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if knife then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                knife.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 1)
                task.wait(0.12)
                knife:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
            end
        end
    end
end)

_G.Aimbot = false
RageSec:NewToggle("Smart Aimbot", "Locks onto the Murderer", function(state) _G.Aimbot = state end)

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if GetPlayerRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
            end
        end
    end
end)

_G.KillAura = false
RageSec:NewToggle("Kill Aura (25m)", "Automatically slices nearby players", function(state)
    _G.KillAura = state
    while _G.KillAura do
        pcall(function()
            local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
            if knife then
                for _, v in pairs(Players:GetPlayers()) do
                    if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        if (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            knife.Parent = LocalPlayer.Character
                            knife:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                            firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
                        end
                    end
                end
            end
        end)
        task.wait(0.1)
    end
end)

-- [[ 2. VISUALS - FULL BOX ESP ]] --
local EspSec = Visuals:NewSection("Vision Settings")
_G.MasterESP = false
EspSec:NewToggle("FULL BOX ESP ACTIVE", "Box + Name + Role", function(state) _G.MasterESP = state end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetPlayerRole(v)
                    local color = Color3.fromRGB(0, 255, 0)
                    if role == "MURDERER" then color = Color3.fromRGB(255, 0, 0)
                    elseif role == "SHERIFF" then color = Color3.fromRGB(0, 150, 255) end
                    
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.7; hl.OutlineColor = color
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 100, 0, 20); bg.ExtentsOffset = Vector3.new(0, 3, 0)
                    local lb = bg:FindFirstChild("TextLabel") or Instance.new("TextLabel", bg)
                    lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.TextSize = 10; lb.Text = "["..role.."] "..v.Name
                end
            end)
        end
    end
end)

-- [[ 3. MAGNET & STEALTH FARM ]] --
local FarmSec = Farm:NewSection("Item Collection")

_G.GrabGun = false
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Never miss the dropped gun", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        pcall(function()
            for _, v in pairs(workspace:GetDescendants()) do
                if v.Name == "GunDrop" then 
                    v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
                    if not v:FindFirstChild("NoVelocity") then
                        local bv = Instance.new("BodyVelocity", v); bv.Name = "NoVelocity"; bv.MaxForce = Vector3.new(9e9, 9e9, 9e9); bv.Velocity = Vector3.new(0,0,0)
                    end
                end
            end
        end)
        task.wait(0.1)
    end
end)

-- [[ 4. PRO / AVATAR ]] --
local ProSec = Pro:NewSection("Support")

ProSec:NewButton("Korblox (80 robux)", "Click to Purchase!", function()
    -- Copy attempt
    if setclipboard then setclipboard("https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE") end
    
    -- INFO WINDOW
    if game.CoreGui:FindFirstChild("DonateGui") then game.CoreGui.DonateGui:Destroy() end
    local DonateGui = Instance.new("ScreenGui", game.CoreGui); DonateGui.Name = "DonateGui"
    local Frame = Instance.new("Frame", DonateGui)
    Frame.Size = UDim2.new(0, 300, 0, 150); Frame.Position = UDim2.new(0.5, -150, 0.4, 0)
    Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30); Frame.BorderSizePixel = 0
    Instance.new("UICorner", Frame)
    
    local Title = Instance.new("TextLabel", Frame)
    Title.Text = "TO PURCHASE KORBLOX"; Title.Size = UDim2.new(1, 0, 0, 40)
    Title.TextColor3 = Color3.fromRGB(255, 0, 0); Title.BackgroundTransparency = 1; Title.TextSize = 18
    
    local LinkBox = Instance.new("TextBox", Frame)
    LinkBox.Text = "roblox.com/tr/game-pass/1812606767"; LinkBox.Size = UDim2.new(0.9, 0, 0, 40)
    LinkBox.Position = UDim2.new(0.05, 0, 0.35, 0); LinkBox.TextEditable = false
    LinkBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50); LinkBox.TextColor3 = Color3.new(1, 1, 1)
    LinkBox.TextScaled = true; Instance.new("UICorner", LinkBox)
    
    local CloseBtn = Instance.new("TextButton", Frame)
    CloseBtn.Text = "Close"; CloseBtn.Size = UDim2.new(0.4, 0, 0, 30); CloseBtn.Position = UDim2.new(0.3, 0, 0.75, 0)
    CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0); CloseBtn.TextColor3 = Color3.new(1, 1, 1)
    Instance.new("UICorner", CloseBtn)
    CloseBtn.MouseButton1Click:Connect(function() DonateGui:Destroy() end)
    
    -- Official Purchase Prompt
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767) end)
end)
