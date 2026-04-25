-- [[ LAYROXC HUB v59 - THE ABSOLUTE ENGINE (FIXED & MOVABLE) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()

-- Menüyü ekranın tam ortasında ve sürüklenebilir yapıyoruz
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- Global Değerler
_G.SpeedValue = 16
_G.JumpValue = 50
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.GrabGun = false
_G.StealthFarm = false

-- [[ MOBİL AÇ/KAPAT BUTONU - EKSTRA STABİL ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 15, 0.5, -25) -- Sol orta kenar
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.TextColor3 = Color3.new(1, 1, 1)
OpenButton.MouseButton1Click:Connect(function() 
    Library:ToggleUI() 
end)

-- [[ ROLE DETECTION ]] --
local function GetPlayerRole(v)
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

-- [[ COMBAT ]] --
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewButton("TP Behind Murderer", "Instant Teleport", function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetPlayerRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)
RageSec:NewButton("KILL ALL (CLEANUP)", "Kill Everyone", function()
    local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if knife then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                knife.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 1)
                task.wait(0.1)
                knife:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
            end
        end
    end
end)
RageSec:NewToggle("Smart Aimbot", "Locks onto Murderer", function(state) _G.Aimbot = state end)
RageSec:NewToggle("Kill Aura (25m)", "Auto slice nearby", function(state) _G.KillAura = state end)

-- [[ VISUALS ]] --
local EspSec = Visuals:NewSection("Minimal Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "Box + Name", function(state) _G.MasterESP = state end)

-- [[ FARM ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Auto gun pickup", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Safe coin farm", function(state) _G.StealthFarm = state end)

-- [[ MOVEMENT ]] --
local MoveSec = Movement:NewSection("Speed & Jump")
MoveSec:NewTextBox("WalkSpeed", "Value", function(txt) _G.SpeedValue = tonumber(txt) or 16 end)
MoveSec:NewTextBox("JumpPower", "Value", function(txt) _G.JumpValue = tonumber(txt) or 50 end)

-- [[ PRO / KORBLOX ]] --
local ProSec = Pro:NewSection("Support")
ProSec:NewButton("Korblox (80 robux)", "Purchase screen", function()
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767) end)
end)

-- [[ SYSTEM LOOPS ]] --
RunService.Stepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
        end
    end)
end)

task.spawn(function()
    while task.wait(0.1) do
        if _G.GrabGun then
            for _, v in pairs(workspace:GetDescendants()) do
                if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
            end
        end
        -- Kill Aura Loop
        if _G.KillAura then
            local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
            if k then
                for _, v in pairs(Players:GetPlayers()) do
                    if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        if (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            k.Parent = LocalPlayer.Character; k:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
                        end
                    end
                end
            end
        end
    end
end)
