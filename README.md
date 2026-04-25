-- [[ LAYROXC HUB v59 - THE ABSOLUTE ENGINE (FULL VERSION) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local TweenService = game:GetService("TweenService")
local MarketplaceService = game:GetService("MarketplaceService")

-- Global Ayarlar (Ölünce Sıfırlanmaz)
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
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

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

-- [[ 1. COMBAT SEKTÖRÜ ]] --
local RageSec = Main:NewSection("Execution Engine")

RageSec:NewButton("TP Behind Murderer", "Teleport behind the killer", function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetPlayerRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

RageSec:NewButton("KILL ALL (CLEANUP)", "Instant kill everyone", function()
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

RageSec:NewButton("Murderer TP Button (HUD)", "Screen Button for TP", function()
    if game.CoreGui:FindFirstChild("TpGui") then game.CoreGui.TpGui:Destroy() end
    local TpGui = Instance.new("ScreenGui", game.CoreGui); local TpBtn = Instance.new("TextButton", TpGui)
    TpBtn.Size = UDim2.new(0, 120, 0, 40); TpBtn.Position = UDim2.new(0.5, -60, 0.8, 0)
    TpBtn.Text = "TP TO MURDERER"; TpBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0); TpBtn.Draggable = true
    TpBtn.MouseButton1Click:Connect(function()
        for _, v in pairs(Players:GetPlayers()) do
            if GetPlayerRole(v) == "MURDERER" and v.Character then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2)
            end
        end
    end)
end)

-- [[ 2. VISUALS SEKTÖRÜ ]] --
local EspSec = Visuals:NewSection("Minimal Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "Small Box + Mini Name", function(state) _G.MasterESP = state end)

-- [[ 3. FARM & MAGNET SEKTÖRÜ ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Auto gun pickup", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Safe coin collection", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT SEKTÖRÜ ]] --
local MoveSec = Movement:NewSection("Speed & Jump")
MoveSec:NewTextBox("WalkSpeed", "Default 16", function(txt) _G.SpeedValue = tonumber(txt) or 16 end)
MoveSec:NewTextBox("JumpPower", "Default 50", function(txt) _G.JumpValue = tonumber(txt) or 50 end)

-- [[ 5. AVATAR & PRO SEKTÖRÜ ]] --
local ProSec = Pro:NewSection("Styles & Support")

ProSec:NewButton("FE Headless (Everyone Sees)", "Invisible Head", function()
    pcall(function()
        LocalPlayer.Character.Head.Transparency = 1
        local headMove = true
        RunService.RenderStepped:Connect(function()
            if headMove and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
                LocalPlayer.Character.Head.CFrame = CFrame.new(0, 500000, 0)
            end
        end)
    end)
end)

ProSec:NewButton("Korblox (80 robux)", "Click to Purchase!", function()
    if setclipboard then setclipboard("https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE") end
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767) end)
end)

-- [[ DÖNGÜSEL SİSTEMLER (LOOP) ]] --
RunService.Stepped:Connect(function()
    pcall(function()
        -- Movement Fix
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
        end
        -- Aimbot
        if _G.Aimbot then
            for _, v in pairs(Players:GetPlayers()) do
                if GetPlayerRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                end
            end
        end
    end)
end)

task.spawn(function()
    while task.wait(0.1) do
        -- Kill Aura
        if _G.KillAura then
            local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
            if knife then
                for _, v in pairs(Players:GetPlayers()) do
                    if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        if (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            knife.Parent = LocalPlayer.Character; knife:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                            firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
                        end
                    end
                end
            end
        end
        -- Magnet & Stealth Farm
        if _G.GrabGun then
            for _, v in pairs(workspace:GetDescendants()) do
                if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
            end
        end
        if _G.StealthFarm then
            for _, v in pairs(workspace:GetDescendants()) do
                if (v.Name == "Coin" or v.Name == "Candy") then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                    task.wait(0.2)
                end
            end
        end
    end
end)

-- ESP Loop
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetPlayerRole(v)
                    local color = role == "MURDERER" and Color3.fromRGB(255, 0, 0) or (role == "SHERIFF" and Color3.fromRGB(0, 150, 255) or Color3.fromRGB(0, 255, 0))
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.8; hl.OutlineColor = color
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 80, 0, 15); bg.ExtentsOffset = Vector3.new(0, 2.5, 0)
                    local lb = bg:FindFirstChild("TextLabel") or Instance.new("TextLabel", bg)
                    lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.TextSize = 7; lb.Text = role .. " | " .. v.Name:sub(1,10)
                end
            end)
        end
    end
end)
