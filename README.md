-- [[ LAYROXC HUB v59 - THE ABSOLUTE ENGINE (FULL & UNABRIDGED) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- ÖZEL AYARLAR
local MyGamepassID = 1812606767
local MyGamepassLink = "https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE"

_G.SpeedValue = 16
_G.JumpValue = 50
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.GrabGun = false
_G.StealthFarm = false

-- [[ MOBILE TOGGLE BUTTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 15, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ROLE DETECTION ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- TABS
local Main = Window:NewTab("Combat (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Movement = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Fix")

-- [[ 1. COMBAT ]] --
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewButton("TP Behind Murderer", "Instantly teleport", function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)
RageSec:NewToggle("Smart Aimbot", "Lock on Murderer", function(state) _G.Aimbot = state end)
RageSec:NewToggle("Kill Aura (25m)", "Auto Attack", function(state) _G.KillAura = state end)

-- [[ 2. VISUALS ]] --
local EspSec = Visuals:NewSection("Vision Settings")
EspSec:NewToggle("MASTER ESP ACTIVE", "Show Roles & Boxes", function(state) _G.MasterESP = state end)

-- [[ 3. MAGNET & FARM ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Auto pickup gun", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Auto coin collection", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT ]] --
local MoveSec = Movement:NewSection("Speed & Jump")
MoveSec:NewTextBox("WalkSpeed", "Default 16", function(txt) _G.SpeedValue = tonumber(txt) or 16 end)
MoveSec:NewTextBox("JumpPower", "Default 50", function(txt) _G.JumpValue = tonumber(txt) or 50 end)

-- [[ 5. PRO SEKTÖRÜ (GAMEPASS + LINK) ]] --
local ProSec = Pro:NewSection("Special Offer")
ProSec:NewButton("Get Korblox (80 Robux)", "Prompt + Link Copy", function()
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
    if setclipboard then setclipboard(MyGamepassLink) end
    
    local BuyGui = Instance.new("ScreenGui", game.CoreGui); local Fr = Instance.new("Frame", BuyGui)
    Fr.Size = UDim2.new(0,250,0,100); Fr.Position = UDim2.new(0.5,-125,0.4,0); Fr.BackgroundColor3 = Color3.new(0,0,0)
    local Tx = Instance.new("TextLabel", Fr); Tx.Size = UDim2.new(1,0,1,0); Tx.Text = "Satın alma açıldı ve link kopyalandı!"; Tx.TextColor3 = Color3.new(1,1,1)
    task.wait(3); BuyGui:Destroy()
end)

-- [[ ANA DÖNGÜLER ]] --
RunService.Stepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
        end
        if _G.Aimbot then
            for _, v in pairs(Players:GetPlayers()) do
                if GetRole(v) == "MURDERER" and v.Character then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                end
            end
        end
    end)
end)

task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if _G.KillAura then
                local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
                if k then
                    for _, v in pairs(Players:GetPlayers()) do
                        if v ~= LocalPlayer and v.Character and (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            k.Parent = LocalPlayer.Character; k:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
                        end
                    end
                end
            end
            if _G.GrabGun then
                for _, v in pairs(workspace:GetDescendants()) do
                    if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
                end
            end
            if _G.StealthFarm then
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v.Name == "Coin" or v.Name == "Candy") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame; task.wait(0.3)
                    end
                end
            end
        end)
    end
end)

-- ESP LOOP
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.8
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0,80,0,15); bg.ExtentsOffset = Vector3.new(0,2,0)
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg); lb.Name = "TL"
                    lb.Size = UDim2.new(1,0,1,0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.Text = role
                end
            end)
        end
    end
end)
