-- [[ LAYROXC HUB v59 - KILL CONFIRMED & FULL ENGINE ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- AYARLAR
_G.MasterESP = false
_G.SpeedValue = 16
_G.JumpValue = 50
_G.SilentAim = false
_G.Aimbot = false
_G.KillAura = false
_G.GrabGun = false
_G.NoClip = false

local MyGamepassID = 1812606767
local MyGamepassLink = "https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE"

-- [[ MOBİL BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 15, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ATEŞ ETME BUTONU (FIXED KILL) ]] --
local ShootGui = Instance.new("ScreenGui", game.CoreGui)
local ShootBtn = Instance.new("TextButton", ShootGui)
ShootBtn.Size = UDim2.new(0, 90, 0, 90); ShootBtn.Position = UDim2.new(0.8, 0, 0.4, 0)
ShootBtn.Text = "ÖLDÜR"; ShootBtn.Visible = false; ShootBtn.Draggable = true
ShootBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0); ShootBtn.TextColor3 = Color3.new(1,1,1)
ShootBtn.Font = Enum.Font.SourceSansBold; ShootBtn.TextSize = 20
Instance.new("UICorner", ShootBtn).CornerRadius = UDim.new(1, 0)

-- [[ ROL VE KATİL TESPİTİ ]] --
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

-- TABS
local Main = Window:NewTab("Combat")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Move = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT & ATEŞ SİSTEMİ ]] --
local RageSec = Main:NewSection("Killing Machine")

RageSec:NewToggle("Silent Aim (Auto Kill)", "Basınca katili yok eder", function(state) 
    _G.SilentAim = state 
    ShootBtn.Visible = state 
end)

RageSec:NewToggle("Cam Aimbot", "Kamerayı katile kitler", function(state) _G.Aimbot = state end)

RageSec:NewButton("TP Behind Murderer", "Katilin arkasına ışınlan", function()
    local m = GetMurderer()
    if m then LocalPlayer.Character.HumanoidRootPart.CFrame = m.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3) end
end)

-- ASIL ATEŞ ETME VE ÖLDÜRME MOTORU
ShootBtn.MouseButton1Click:Connect(function()
    local Gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
    local Murderer = GetMurderer()
    
    if Gun and Murderer then
        Gun.Parent = LocalPlayer.Character -- Silahı ele al
        task.wait(0.05) -- Küçük bir gecikme (server için)
        
        local ShootEvent = game:GetService("ReplicatedStorage"):FindFirstChild("ShootGun", true)
        if ShootEvent then
            -- Mermiyi katilin göğsüne (HumanoidRootPart) gönderiyoruz
            local targetPos = Murderer.Character.HumanoidRootPart.Position
            ShootEvent:FireServer(targetPos) 
        end
    else
        Library:Notify("HATA", "Silahın yok veya Katil bulunamadı!", 3)
    end
end)

-- [[ 2. VISUALS (ESP) ]] --
local EspSec = Visuals:NewSection("Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "Saliselik Yenileme", function(state) _G.MasterESP = state end)

-- [[ 3. MAGNET & FARM ]] --
local FarmSec = Farm:NewSection("Collection")
FarmSec:NewToggle("MAGNET GUN", "Silahı çeker", function(s) _G.GrabGun = s end)
FarmSec:NewToggle("STEALTH FARM", "Coin toplar", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT ]] --
local MoveSec = Move:NewSection("Physics")
MoveSec:NewTextBox("WalkSpeed", "Hız", function(t) _G.SpeedValue = tonumber(t) or 16 end)
MoveSec:NewTextBox("JumpPower", "Zıplama", function(t) _G.JumpValue = tonumber(t) or 50 end)
MoveSec:NewToggle("NoClip", "Duvar Geçme", function(state) _G.NoClip = state end)

-- [[ 5. PRO (KORBLOX & UYARI) ]] --
local ProSec = Pro:NewSection("Support")
ProSec:NewButton("Get Korblox (80 Robux)", "OPEN YOUR BROWSER", function()
    Library:Notify("BİLGİ", "OPEN YOUR BROWSER - Link kopyalandı ve mağaza açıldı.", 5)
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
    if setclipboard then setclipboard(MyGamepassLink) end
end)

-- [[ SİSTEM DÖNGÜLERİ ]] --
RunService.RenderStepped:Connect(function()
    -- ESP
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0.5,1) or Color3.new(0,1,0))
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.7
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0,100,0,20); bg.ExtentsOffset = Vector3.new(0,3,0)
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg)
                    lb.Name = "TL"; lb.TextColor3 = color; lb.TextSize = 12; lb.Font = Enum.Font.SourceSansBold; lb.BackgroundTransparency = 1
                    lb.Text = "["..role.."] "..v.DisplayName
                end
            end)
        end
    end

    -- Physics & Camera
    pcall(function()
        if _G.Aimbot then
            local m = GetMurderer()
            if m then Camera.CFrame = CFrame.new(Camera.CFrame.Position, m.Character.HumanoidRootPart.Position) end
        end
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
            if _G.NoClip then
                for _, p in pairs(LocalPlayer.Character:GetDescendants()) do
                    if p:IsA("BasePart") then p.CanCollide = false end
                end
            end
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
        if _G.StealthFarm then
            for _, v in pairs(workspace:GetDescendants()) do
                if v.Name == "Coin" or v.Name == "Candy" then LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame end
            end
        end
    end
end)
