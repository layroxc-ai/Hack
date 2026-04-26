-- [[ LAYROXC HUB v59 - THE GOD MODE (Aimbot & Korblox Fix) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - ⚠️🌌", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- TÜM AYARLAR (EKSİKSİZ)
_G.MasterESP = false
_G.SpeedValue = 16
_G.JumpValue = 50
_G.SilentAim = false
_G.Aimbot = false
_G.NoClip = false
_G.GrabGun = false

local MyGamepassID = 1812606767
local MyGamepassLink = "https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE"

-- [[ MOBİL MENÜ BUTONU ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 15, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ATEŞ ETME BUTONU ]] --
local ShootGui = Instance.new("ScreenGui", game.CoreGui)
local ShootBtn = Instance.new("TextButton", ShootGui)
ShootBtn.Size = UDim2.new(0, 90, 0, 90); ShootBtn.Position = UDim2.new(0.8, 0, 0.4, 0)
ShootBtn.Text = "ÖLDÜR"; ShootBtn.Visible = false; ShootBtn.Draggable = true
ShootBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0); ShootBtn.TextColor3 = Color3.new(1,1,1)
ShootBtn.Font = Enum.Font.SourceSansBold; ShootBtn.TextSize = 20; Instance.new("UICorner", ShootBtn).CornerRadius = UDim.new(1, 0)

-- [[ ROL VE KATİL TESPİT MOTORU ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
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

-- SEKMELER
local Main = Window:NewTab("Combat")
local Visuals = Window:NewTab("Visuals (ESP)")
local Move = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT & AIMBOT ]] --
local RageSec = Main:NewSection("Combat Engine")

RageSec:NewToggle("Silent Aim", "Ateş butonunu açar", function(state) 
    _G.SilentAim = state 
    ShootBtn.Visible = state 
end)

RageSec:NewToggle("Cam Aimbot (Lock)", "Kamerayı Katil'e sabitler", function(state) 
    _G.Aimbot = state 
end)

RageSec:NewButton("Kill All (Everyone)", "Bıçakla herkesi keser", function()
    local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if k then
        k.Parent = LocalPlayer.Character
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,1)
                task.wait(0.1)
                k:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
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
        local shootRem = game:GetService("ReplicatedStorage"):FindFirstChild("ShootGun", true)
        if shootRem then shootRem:FireServer(M.Character.HumanoidRootPart.Position) end
    end
end)

-- [[ 2. VISUALS (EKSİKSİZ ESP) ]] --
local EspSec = Visuals:NewSection("Vision Engine")
EspSec:NewToggle("MASTER ESP ACTIVE", "İsim & Rolleri Göster", function(state) _G.MasterESP = state end)

-- [[ 3. MOVEMENT ]] --
local MoveSec = Move:NewSection("Physics")
MoveSec:NewTextBox("WalkSpeed", "Hız Ayarı", function(t) _G.SpeedValue = tonumber(t) or 16 end)
MoveSec:NewToggle("NoClip", "Duvar Geçme", function(state) _G.NoClip = state end)

-- [[ 4. PRO (KORBLOX & UYARI FIX) ]] --
local ProSec = Pro:NewSection("Korblox System")
ProSec:NewButton("Get Korblox (80 Robux)", "BROWSER'DA AÇ", function()
    -- GÖRSEL UYARI
    Library:Notify("BİLGİ", "BROWSER'DA AÇ! Link kopyalandı ve mağaza açılıyor.", 5)
    
    -- KOPYALAMA VE İŞLEM
    setclipboard(MyGamepassLink)
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
end)

-- [[ ANA MOTOR (HİÇBİR ŞEY EKSİK DEĞİL) ]] --
RunService.RenderStepped:Connect(function()
    -- ESP MOTORU
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0.5,1) or Color3.new(0,1,0))
                    
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.7
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0,150,0,35); bg.ExtentsOffset = Vector3.new(0,3,0)
                    
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg)
                    lb.Name = "TL"; lb.Size = UDim2.new(1,0,1,0); lb.BackgroundTransparency = 1; lb.TextColor3 = color
                    lb.TextSize = 14; lb.Font = Enum.Font.SourceSansBold; lb.Text = "["..role.."] "..v.DisplayName
                end
            end)
        end
    end

    -- AIMBOT VE FİZİK MOTORU
    pcall(function()
        if _G.Aimbot then
            local target = GetMurderer()
            if target and target.Character then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Character.HumanoidRootPart.Position)
            end
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
