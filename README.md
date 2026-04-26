-- [[ LAYROXC HUB v59 - SILENT AIM BUTTON & FULL ENGINE ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- AYARLAR
_G.SpeedValue = 16
_G.JumpValue = 50
_G.SilentAim = false
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.GrabGun = false
_G.StealthFarm = false
_G.NoClip = false

local MyGamepassID = 1812606767
local MyGamepassLink = "https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE"

-- [[ MOBİL AÇ/KAPAT BUTONU ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 15, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ATEŞ ETME BUTONU (SILENT AIM İÇİN) ]] --
local ShootGui = Instance.new("ScreenGui", game.CoreGui)
local ShootBtn = Instance.new("TextButton", ShootGui)
ShootBtn.Size = UDim2.new(0, 70, 0, 70); ShootBtn.Position = UDim2.new(0.8, 0, 0.5, 0)
ShootBtn.Text = "ATEŞ"; ShootBtn.Visible = false; ShootBtn.Draggable = true
ShootBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0); ShootBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", ShootBtn).CornerRadius = UDim.new(1, 0)

-- [[ ROL TESPİT SİSTEMİ ]] --
local function GetMurderer()
    for _, v in pairs(Players:GetPlayers()) do
        if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then
            return v
        end
    end
    return nil
end

-- TABS
local Main = Window:NewTab("Combat (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Movement = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT - SILENT AIM & AIMBOT ]] --
local RageSec = Main:NewSection("Execution Engine")

RageSec:NewToggle("Silent Aim (Tiktok)", "Açınca ekrana tuş gelir", function(state) 
    _G.SilentAim = state 
    ShootBtn.Visible = state -- Tuşu aç/kapat
end)

RageSec:NewToggle("Cam Aimbot", "Kamerayı katile kilitler", function(state) _G.Aimbot = state end)
RageSec:NewToggle("Kill Aura (25m)", "Otomatik bıçak sallar", function(state) _G.KillAura = state end)

-- Ateş Butonu Mantığı
ShootBtn.MouseButton1Click:Connect(function()
    local Gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
    if Gun then
        Gun.Parent = LocalPlayer.Character
        local M = GetMurderer()
        if M and M.Character and M.Character:FindFirstChild("HumanoidRootPart") then
            game:GetService("ReplicatedStorage").Entity.Gun.ShootGun:FireServer(M.Character.HumanoidRootPart.Position)
        end
    end
end)

-- [[ 2. VISUALS - ESP ]] --
local EspSec = Visuals:NewSection("Minimalist Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "İsim & Rolleri Göster", function(state) _G.MasterESP = state end)

-- [[ 3. FARM & MAGNET ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Silahı çeker", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Coin toplar", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT ]] --
local MoveSec = Movement:NewSection("Physics Control")
MoveSec:NewTextBox("WalkSpeed", "Hız", function(txt) _G.SpeedValue = tonumber(txt) or 16 end)
MoveSec:NewTextBox("JumpPower", "Zıplama", function(txt) _G.JumpValue = tonumber(txt) or 50 end)
MoveSec:NewToggle("NoClip", "Duvar Geçme", function(state) _G.NoClip = state end)

-- [[ 5. PRO SEKTÖRÜ (GAMEPASS) ]] --
local ProSec = Pro:NewSection("Support & Korblox")
ProSec:NewButton("Get Korblox (80 Robux)", "Prompt + Link", function()
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
    if setclipboard then setclipboard(MyGamepassLink) end
end)

-- [[ SİSTEM DÖNGÜLERİ ]] --

-- Fizik, NoClip ve Camera Aimbot
RunService.Stepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
            if _G.NoClip then
                for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then v.CanCollide = false end
                end
            end
        end
        if _G.Aimbot then
            local M = GetMurderer()
            if M and M.Character and M.Character:FindFirstChild("HumanoidRootPart") then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, M.Character.HumanoidRootPart.Position)
            end
        end
    end)
end)

-- Farm ve Kill Aura
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if _G.KillAura then
                local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
                local m = GetMurderer()
                if k and m and m.Character and (LocalPlayer.Character.HumanoidRootPart.Position - m.Character.HumanoidRootPart.Position).Magnitude < 25 then
                    k.Parent = LocalPlayer.Character; k:Activate()
                    firetouchinterest(m.Character.HumanoidRootPart, k.Handle, 0)
                    firetouchinterest(m.Character.HumanoidRootPart, k.Handle, 1)
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

-- ESP Rendering
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local isM = v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")
                    local color = isM and Color3.new(1,0,0) or Color3.new(0,1,0)
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.8
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0,100,0,20); bg.ExtentsOffset = Vector3.new(0,2,0)
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg); lb.Name = "TL"
                    lb.Size = UDim2.new(1,0,1,0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.TextSize = 10; lb.Font = Enum.Font.SourceSansBold; lb.Text = v.DisplayName
                end
            end)
        end
    end
end)
