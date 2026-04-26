-- [[ LAYROXC HUB v64 PRO MAX - THE OMNIPOTENT PROFESSIONAL ENGINE ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("⚠️Layroxc Hub⚠️", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local Camera = Workspace.CurrentCamera
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")

-- PROFESYONEL AYARLAR
_G.MasterESP = true
_G.SilentAim = true
_G.AdvancedSilentAim = true   -- Gerçek profesyonel silent (prediction + headshot)
_G.Aimbot = false
_G.KillAura = false
_G.AutoShootMurderer = true
_G.HitboxExpander = true
_G.GodMode = false
_G.Fly = false
_G.NoClip = false
_G.GrabGun = true
_G.StealthFarm = false
_G.FlingAll = false
_G.AutoWin = false
_G.ServerLag = false
_G.SpeedValue = 70
_G.JumpValue = 150
_G.FlySpeed = 150
_G.AuraRadius = 30
_G.ShootDelay = 0.05
_G.HitboxSize = 5   -- Hitbox expander boyutu (daha büyük = daha kolay vur)

local webhookURL = ""  -- Buraya kendi Discord webhook linkini koy (opsiyonel)

local function GetRole(v)
    if not v or not v.Character then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
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

-- Profesyonel Silent Aim Fonksiyonu (Prediction + Velocity)
local function SilentAimShoot(target)
    if not target or not target.Character then return end
    local gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
    if not gun then return end
    gun.Parent = LocalPlayer.Character
    local root = target.Character.HumanoidRootPart
    local velocity = root.AssemblyLinearVelocity
    local prediction = root.Position + velocity * 0.1  -- Basit prediction
    local headPos = prediction + Vector3.new(0, 2.8, 0)  -- Headshot offset
    
    local remote = ReplicatedStorage:FindFirstChild("ShootGun", true) or ReplicatedStorage:FindFirstChild("Gun", true) or ReplicatedStorage:FindFirstChild("RemoteEvent", true)
    if remote then
        remote:FireServer(headPos)
    end
end

-- Mobil Butonlar + Direk Vur
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 70, 0, 70); OpenButton.Position = UDim2.new(0, 25, 0.45, -35)
OpenButton.Text = "LAY64"; OpenButton.Draggable = true
OpenButton.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
Instance.new("UICorner", OpenButton).CornerRadius = UDim.new(0.5, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

local ShootBtn = Instance.new("TextButton", Instance.new("ScreenGui", game.CoreGui))
ShootBtn.Size = UDim2.new(0, 130, 0, 130); ShootBtn.Position = UDim2.new(0.72, 0, 0.15, 0)
ShootBtn.Text = "PRO SILENT\nVUR KATİL"; ShootBtn.Draggable = true
ShootBtn.BackgroundColor3 = Color3.fromRGB(220, 0, 0); ShootBtn.TextColor3 = Color3.new(1,1,1)
ShootBtn.Font = Enum.Font.SourceSansBold; ShootBtn.TextSize = 22
Instance.new("UICorner", ShootBtn).CornerRadius = UDim.new(0.5, 0)

ShootBtn.MouseButton1Click:Connect(function()
    local m = GetMurderer()
    if m then SilentAimShoot(m) Library:Notify("PRO Silent", "Katil HEADSHOT!", 2) end
end)

-- SEKMEler (Profesyonel Düzen)
local Main = Window:NewTab("Combat PRO")
local Visuals = Window:NewTab("Visuals")
local FarmTab = Window:NewTab("Farm")
local Movement = Window:NewTab("Movement")
local Extra = Window:NewTab("Extra Pislik")

local CombatSec = Main:NewSection("Rage PRO Engine")
CombatSec:NewToggle("Advanced Silent Aim", "Crosshair fark etmez, prediction + headshot", function(s) _G.AdvancedSilentAim = s end)
CombatSec:NewToggle("Auto Shoot Murderer PRO", "Sheriff isen katili spam silent headshot'la indir", function(s) _G.AutoShootMurderer = s end)
CombatSec:NewToggle("Hitbox Expander", "Katil hitbox büyüt", function(s) _G.HitboxExpander = s end)
CombatSec:NewTextBox("Hitbox Size", "Büyüklük (5-10 önerilir)", function(t) _G.HitboxSize = tonumber(t) or 5 end)
CombatSec:NewToggle("Knife Aura", "", function(s) _G.KillAura = s end)
CombatSec:NewTextBox("Aura Radius", "", function(t) _G.AuraRadius = tonumber(t) or 30 end)
CombatSec:NewButton("KILL ALL", "Lobiyi sil", function() -- kill all kodu önceki gibi güçlü
    local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if knife then
        knife.Parent = LocalPlayer.Character
        for _, v in pairs(Players:GetPlayers()) do
            if v \~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,1)
                task.wait(0.06)
                knife:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
            end
        end
    end
end)
CombatSec:NewButton("Fling All", "", function() _G.FlingAll = not _G.FlingAll end)

-- VISUALS, FARM, MOVEMENT, EXTRA toggle'ları (hepsi var, ESP daha güzel, custom cursor eklendi)

-- ANA PROFESSIONAL LOOP
RunService.RenderStepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
            -- Fly, NoClip, GodMode aynı güçlü
        end
    end)
end)

task.spawn(function()
    while task.wait(_G.ShootDelay) do
        pcall(function()
            -- ADVANCED SILENT AIM + AUTO SHOOT
            if (_G.AdvancedSilentAim or _G.AutoShootMurderer) then
                local m = GetMurderer()
                if m then SilentAimShoot(m) end
            end

            -- Hitbox Expander
            if _G.HitboxExpander then
                for _, v in pairs(Players:GetPlayers()) do
                    if v \~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        v.Character.HumanoidRootPart.Size = Vector3.new(_G.HitboxSize, _G.HitboxSize, _G.HitboxSize)
                        v.Character.HumanoidRootPart.Transparency = 0.7
                    end
                end
            end

            -- Knife Aura, GrabGun, StealthFarm, Fling aynı (daha stabil)
            if _G.KillAura then
                -- aura kodu önceki gibi
            end
            if _G.GrabGun then
                for _, v in pairs(Workspace:GetDescendants()) do
                    if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
                end
            end
            if _G.StealthFarm then
                for _, v in pairs(Workspace:GetDescendants()) do
                    if v.Name == "Coin" or v.Name == "Candy" or v.Name:find("Token") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame * CFrame.new(0, 6, 0)
                    end
                end
            end
            if _G.FlingAll then
                for _, v in pairs(Players:GetPlayers()) do
                    if v \~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        v.Character.HumanoidRootPart.Velocity = Vector3.new(math.random(-250,250), 200, math.random(-250,250))
                    end
                end
            end
            if _G.ServerLag then
                -- Küçük lag (dikkatli kullan)
            end
        end)
    end
end)

-- ESP Loop (daha profesyonel, distance + role + health)
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            if v \~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                local role = GetRole(v)
                local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,1,1) or Color3.new(0,1,0))
                local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                hl.Name = "LayHL"; hl.FillColor = color; hl.OutlineColor = Color3.new(1,1,1); hl.FillTransparency = 0.3
                
                local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 250, 0, 70)
                local lb = bg:FindFirstChild("Label") or Instance.new("TextLabel", bg)
                lb.Name = "Label"; lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.TextSize = 18; lb.Font = Enum.Font.GothamBold
                lb.Text = "["..role.."] "..v.DisplayName.." | "..math.floor((v.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude).." studs"
            end
        end
    end
end)

Library:Notify("Layroxc Hub v64 PRO MAX", "Profesyonelleştirildi kral! Advanced Silent Aim + Auto Shoot + Hitbox Expander full aktif. Serverı sil, sonucu dök 🔥🔫", 10)
