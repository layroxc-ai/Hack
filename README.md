-- [[ LAYROXC HUB v59 - THE OMNIPOTENT ENGINE (FULL & FINAL) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL PRO", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Camera = workspace.CurrentCamera

-- TÜM DEĞİŞKENLER
_G.MasterESP = false
_G.SpeedValue = 16
_G.SilentAim = false
_G.Aimbot = false
_G.NoClip = false
_G.GrabGun = false
_G.StealthFarm = false
_G.KatilTakip = false

-- [[ MOBİL AÇ/KAPAT ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 15, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ S.AIM ATEŞ BUTONU ]] --
local ShootGui = Instance.new("ScreenGui", game.CoreGui)
local ShootBtn = Instance.new("TextButton", ShootGui)
ShootBtn.Size = UDim2.new(0, 90, 0, 90); ShootBtn.Position = UDim2.new(0.8, 0, 0.4, 0)
ShootBtn.Text = "S. AIM\nFIRE"; ShootBtn.Visible = false; ShootBtn.Draggable = true
ShootBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0); ShootBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", ShootBtn).CornerRadius = UDim.new(1, 0)

-- [[ ROL TESPİT MOTORU ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- [[ KALICI VE KÜÇÜK İSİMLİ ESP ]] --
local function ApplyPermanentESP(v)
    if v == LocalPlayer then return end
    local function CreateESP()
        local char = v.Character or v.CharacterAdded:Wait()
        local head = char:WaitForChild("Head", 5)
        if not head then return end
        if char:FindFirstChild("LayHighlight") then char.LayHighlight:Destroy() end
        if head:FindFirstChild("LayName") then head.LayName:Destroy() end
        local hl = Instance.new("Highlight", char)
        hl.Name = "LayHighlight"; hl.FillTransparency = 0.5; hl.OutlineTransparency = 0
        local bg = Instance.new("BillboardGui", head)
        bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 100, 0, 30); bg.ExtentsOffset = Vector3.new(0, 2.5, 0)
        local lb = Instance.new("TextLabel", bg)
        lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1; lb.TextSize = 11; lb.Font = Enum.Font.SourceSansBold
        spawn(function()
            while char.Parent ~= nil do
                if _G.MasterESP then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0.5,1) or Color3.new(0,1,0))
                    hl.Enabled = true; hl.FillColor = color; hl.OutlineColor = color
                    bg.Enabled = true; lb.TextColor3 = color; lb.Text = "["..role.."]\n"..v.DisplayName
                else
                    hl.Enabled = false; bg.Enabled = false
                end
                task.wait(0.4)
            end
        end)
    end
    v.CharacterAdded:Connect(CreateESP); if v.Character then CreateESP() end
end

-- SEKMELER
local Main = Window:NewTab("Combat")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Move = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT ]] --
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewToggle("Silent Aim", "Butonu açar", function(state) _G.SilentAim = state; ShootBtn.Visible = state end)
RageSec:NewButton("KILL ALL", "Bıçakla herkesi öldürür", function()
    local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if k then
        k.Parent = LocalPlayer.Character
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                task.wait(0.1)
                k:Activate()
            end
        end
    end
end)

ShootBtn.MouseButton1Click:Connect(function()
    if not _G.SilentAim then return end
    local gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
    local murderer = nil
    for _, v in pairs(Players:GetPlayers()) do if GetRole(v) == "MURDERER" then murderer = v break end end
    if gun and murderer then
        gun.Parent = LocalPlayer.Character
        task.wait(0.05)
        local event = ReplicatedStorage:FindFirstChild("ShootGun", true)
        if event then event:FireServer(murderer.Character.HumanoidRootPart.Position) end
    end
end)

-- [[ 2. VISUALS ]] --
local EspSec = Visuals:NewSection("Vision Engine")
EspSec:NewToggle("MASTER ESP ACTIVE", "Kalıcı Takip", function(state) 
    _G.MasterESP = state 
    if state then for _, v in pairs(Players:GetPlayers()) do ApplyPermanentESP(v) end end
end)

-- [[ 3. MAGNET & FARM ]] --
local FarmSec = Farm:NewSection("Automation")
FarmSec:NewToggle("ULTRA MAGNET GUN", "Yerdeki silahı çeker", function(s) _G.GrabGun = s end)
FarmSec:NewToggle("STEALTH FARM", "Para/Şeker toplar", function(s) _G.StealthFarm = s end)

-- [[ 4. MOVEMENT ]] --
local MoveSec = Move:NewSection("Physics")
MoveSec:NewTextBox("WalkSpeed", "Hız", function(t) _G.SpeedValue = tonumber(t) or 16 end)
MoveSec:NewToggle("NoClip", "Duvar Geçme", function(s) _G.NoClip = s end)

-- [[ 5. PRO (LİNK + ÖZEL UYARI) ]] --
local ProSec = Pro:NewSection("Korblox System")
ProSec:NewButton("Get Korblox (80 Robux)", "Kopyalar ve Bildirim Verir", function()
    if setclipboard then
        setclipboard("https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE")
        Library:Notify("SİSTEM", "OPEN YOUR BROWSER AND PASTE THE LINK!", 7)
    end
end)

-- [[ ANA MOTOR VE DÖNGÜLER ]] --
Players.PlayerAdded:Connect(function(v) ApplyPermanentESP(v) end)

RunService.RenderStepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            if _G.NoClip then
                for _, p in pairs(LocalPlayer.Character:GetDescendants()) do
                    if p:IsA("BasePart") then p.CanCollide = false end
                end
            end
        end
    end)
end)

task.spawn(function()
    while task.wait() do
        pcall(function()
            if _G.GrabGun then
                for _, v in pairs(workspace:GetDescendants()) do
                    if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
                end
            end
            if _G.StealthFarm then
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v.Name == "Coin" or v.Name == "Candy") and v:IsA("BasePart") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                        task.wait(0.1)
                    end
                end
            end
        end)
    end
end)
