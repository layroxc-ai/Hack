-- [[ LAYROXC HUB v65 - CLEAN & WORKING ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub v65", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

_G.SilentAim = true
_G.AutoShoot = true
_G.Hitbox = true
_G.KillAura = false
_G.GrabGun = true
_G.Farm = false
_G.Speed = 70
_G.Jump = 150
_G.AuraRadius = 25
_G.HitboxSize = 6

local function GetRole(v)
    if not v or not v.Character then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then return "SHERIFF" end
    return "Innocent"
end

local function GetMurderer()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            return v
        end
    end
    return nil
end

local function SilentShoot(target)
    if not target or not target.Character then return end
    local gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
    if not gun then return end
    gun.Parent = LocalPlayer.Character
    local root = target.Character.HumanoidRootPart
    local headPos = root.Position + Vector3.new(0, 2.5, 0)
    local remote = ReplicatedStorage:FindFirstChild("ShootGun", true) or ReplicatedStorage:FindFirstChild("Gun", true)
    if remote then remote:FireServer(headPos) end
end

-- GUI Butonları
local OpenButton = Instance.new("TextButton", game.CoreGui)
OpenButton.Size = UDim2.new(0, 60, 0, 60)
OpenButton.Position = UDim2.new(0, 20, 0.5, -30)
OpenButton.Text = "LAY65"
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.Draggable = true
Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- Sekmeler
local Main = Window:NewTab("Combat")
local Vis = Window:NewTab("Visuals")
local Mov = Window:NewTab("Movement")
local FarmTab = Window:NewTab("Farm")

Main:NewToggle("Silent Aim", "Crosshair fark etmez", function(s) _G.SilentAim = s end)
Main:NewToggle("Auto Shoot Murderer", "Silahla katili otomatik vur", function(s) _G.AutoShoot = s end)
Main:NewToggle("Hitbox Expander", "Katil hitbox büyüt", function(s) _G.Hitbox = s end)
Main:NewToggle("Knife Aura", "", function(s) _G.KillAura = s end)
Main:NewButton("Kill All", "", function()
    local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if knife then
        knife.Parent = LocalPlayer.Character
        for _, v in pairs(Players:GetPlayers()) do
            if v \~= LocalPlayer and v.Character then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,1)
                task.wait(0.07)
                knife:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
            end
        end
    end
end)

Vis:NewToggle("Master ESP", "", function(s) _G.ESP = s end)

Mov:NewTextBox("WalkSpeed", "", function(t) _G.Speed = tonumber(t) or 70 end)
Mov:NewTextBox("JumpPower", "", function(t) _G.Jump = tonumber(t) or 150 end)
Mov:NewToggle("NoClip", "", function(s) _G.NoClip = s end)

FarmTab:NewToggle("Auto Grab Gun", "", function(s) _G.GrabGun = s end)
FarmTab:NewToggle("Stealth Farm", "", function(s) _G.Farm = s end)

-- Ana Loop
RunService.RenderStepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.Speed
            LocalPlayer.Character.Humanoid.JumpPower = _G.Jump
            if _G.NoClip then
                for _, p in pairs(LocalPlayer.Character:GetDescendants()) do
                    if p:IsA("BasePart") then p.CanCollide = false end
                end
            end
        end
    end)
end)

task.spawn(function()
    while task.wait(0.06) do
        pcall(function()
            if (_G.SilentAim or _G.AutoShoot) then
                local m = GetMurderer()
                if m then SilentShoot(m) end
            end

            if _G.Hitbox then
                for _, v in pairs(Players:GetPlayers()) do
                    if v \~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        v.Character.HumanoidRootPart.Size = Vector3.new(_G.HitboxSize, _G.HitboxSize, _G.HitboxSize)
                    end
                end
            end

            if _G.KillAura then
                local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
                if knife then knife.Parent = LocalPlayer.Character end
                for _, v in pairs(Players:GetPlayers()) do
                    if v \~= LocalPlayer and v.Character then
                        local dist = (v.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                        if dist < _G.AuraRadius then
                            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,1)
                            if knife then knife:Activate() end
                            firetouchinterest(v.Character.HumanoidRootPart, knife and knife.Handle, 0)
                            firetouchinterest(v.Character.HumanoidRootPart, knife and knife.Handle, 1)
                        end
                    end
                end
            end

            if _G.GrabGun then
                for _, v in pairs(Workspace:GetDescendants()) do
                    if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
                end
            end

            if _G.Farm then
                for _, v in pairs(Workspace:GetDescendants()) do
                    if v.Name == "Coin" or v.Name == "Candy" then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame * CFrame.new(0,5,0)
                    end
                end
            end
        end)
    end
end)
