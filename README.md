-- [[ LAYROXC HUB v59 - THE OMNIPOTENT ENGINE (FULL & OPTIMIZED) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
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

-- [[ ÖLDÜR BUTONU (SILENT AIM) ]] --
local ShootGui = Instance.new("ScreenGui", game.CoreGui)
local ShootBtn = Instance.new("TextButton", ShootGui)
ShootBtn.Size = UDim2.new(0, 90, 0, 90); ShootBtn.Position = UDim2.new(0.8, 0, 0.4, 0)
ShootBtn.Text = "ÖLDÜR"; ShootBtn.Visible = false; ShootBtn.Draggable = true
ShootBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0); ShootBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", ShootBtn).CornerRadius = UDim.new(1, 0)

-- [[ TEMEL MOTORLAR ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

local function GetMurderer()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character then return v end
    end
    return nil
end

-- [[ STABİL ESP FONKSİYONU ]] --
local function ApplyESP(v)
    if v == LocalPlayer then return end
    local function CreateElements()
        if not v.Character or not v.Character:FindFirstChild("Head") then return end
        if v.Character.Head:FindFirstChild("LayName") then v.Character.Head.LayName:Destroy() end
        if v.Character:FindFirstChild("LayHighlight") then v.Character.LayHighlight:Destroy() end

        local hl = Instance.new("Highlight", v.Character)
        hl.Name = "LayHighlight"; hl.FillTransparency = 0.5; hl.OutlineTransparency = 0

        local bg = Instance.new("BillboardGui", v.Character.Head)
        bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 100, 0, 30); bg.ExtentsOffset = Vector3.new(0, 2.5, 0)

        local lb = Instance.new("TextLabel", bg)
        lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1; lb.TextSize = 11; lb.Font = Enum.Font.SourceSansBold
        
        local connection
        connection = RunService.RenderStepped:Connect(function()
            if not v.Character or not v.Character:FindFirstChild("HumanoidRootPart") or not _G.MasterESP then
                hl.Enabled = false; bg.Enabled = false
                if not _G.MasterESP then connection:Disconnect() end
                return
            end
            local role = GetRole(v)
            local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0.5,1) or Color3.new(0,1,0))
            hl.Enabled = true; hl.FillColor = color; hl.OutlineColor = color
            bg.Enabled = true; lb.TextColor3 = color; lb.Text = "["..role.."]\n"..v.DisplayName
        end)
    end
    v.CharacterAdded:Connect(CreateElements)
    if v.Character then CreateElements() end
end

-- SEKMELER
local Main = Window:NewTab("Combat")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Move = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT ]] --
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewToggle("Silent Aim", "Ateş butonunu açar", function(state) 
    _G.SilentAim = state 
    ShootBtn.Visible = state 
end)
RageSec:NewToggle("Cam Aimbot", "Katili takip eder", function(state) _G.Aimbot = state end)
RageSec:NewToggle("Katili Sürekli Takip Et (TP)", "Katilin dibinde durur", function(state) _G.KatilTakip = state end)
RageSec:NewButton("KILL ALL", "Lobiyi temizler", function()
    local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if k then
        k.Parent = LocalPlayer.Character
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,1)
                task.wait(0.1)
                k:Activate()
            end
        end
    end
end)

ShootBtn.MouseButton1Click:Connect(function()
    local Gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
    local M = GetMurderer()
    if Gun and M then
        Gun.Parent = LocalPlayer.Character
        task.wait(0.05)
        local sr = game:GetService("ReplicatedStorage"):FindFirstChild("ShootGun", true)
        if sr then sr:FireServer(M.Character.HumanoidRootPart.Position) end
    end
end)

-- [[ 2. VISUALS ]] --
local EspSec = Visuals:NewSection("Vision Engine")
EspSec:NewToggle("MASTER ESP ACTIVE", "Box + Skeleton + Role", function(state) 
    _G.MasterESP = state 
    if state then for _, v in pairs(Players:GetPlayers()) do ApplyESP(v) end end
end)

-- [[ 3. MAGNET & FARM ]] --
local FarmSec = Farm:NewSection("Automation")
FarmSec:NewToggle("ULTRA MAGNET GUN", "Silahı çeker", function(s) _G.GrabGun = s end)
FarmSec:NewToggle("STEALTH FARM", "Para toplar", function(s) _G.StealthFarm = s end)

-- [[ 4. MOVEMENT ]] --
local MoveSec = Move:NewSection("Physics")
MoveSec:NewTextBox("WalkSpeed", "Hız", function(t) _G.SpeedValue = tonumber(t) or 16 end)
MoveSec:NewToggle("NoClip", "Duvar Geçme", function(s) _G.NoClip = s end)

-- [[ 5. PRO ]] --
local ProSec = Pro:NewSection("Korblox System")
ProSec:NewButton("Get Korblox (80 Robux)", "Linki Kopyala", function()
    if setclipboard then
        setclipboard("https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE")
        Library:Notify("BAŞARILI", "Link kopyalandı!", 5)
    end
end)

-- [[ ANA DÖNGÜLER ]] --
Players.PlayerAdded:Connect(function(v) if _G.MasterESP then ApplyESP(v) end end)

RunService.RenderStepped:Connect(function()
    pcall(function()
        if _G.Aimbot then
            local m = GetMurderer()
            if m then Camera.CFrame = CFrame.new(Camera.CFrame.Position, m.Character.HumanoidRootPart.Position) end
        end
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
            local m = GetMurderer()
            if _G.KatilTakip and m and m.Character then
                LocalPlayer.Character.HumanoidRootPart.CFrame = m.Character.HumanoidRootPart.CFrame
            end
            if _G.GrabGun then
                for _, v in pairs(workspace:GetDescendants()) do
                    if v.Name == "GunDrop" and v:IsA("BasePart") then
                        v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
                    end
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
