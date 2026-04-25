-- [[ LAYROXC HUB v59 - THE ABSOLUTE ENGINE (NO CLIPPING - FULL VERSION) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- ÖZEL AYARLAR (Ölünce Sıfırlanmaz)
_G.SpeedValue = 16
_G.JumpValue = 50
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.GrabGun = false
_G.StealthFarm = false
_G.NoClip = false

-- GAMEPASS VERİLERİ
local MyGamepassID = 1812606767
local MyGamepassLink = "https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE"

-- [[ MOBİL AÇ/KAPAT BUTONU ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 15, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ROL TESPİT SİSTEMİ ]] --
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
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT SEKTÖRÜ ]] --
local RageSec = Main:NewSection("Execution Engine")

RageSec:NewButton("TP Behind Murderer", "Katilin Tam Arkasına Işınlar", function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

RageSec:NewToggle("Smart Aimbot", "Katile Otomatik Odaklanır", function(state) _G.Aimbot = state end)
RageSec:NewToggle("Kill Aura (25m)", "Çevrendekileri Seri Keser", function(state) _G.KillAura = state end)

RageSec:NewButton("Kill All (As Murderer)", "Herkesi Anında Temizler", function()
    local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if knife then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                knife.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 1)
                task.wait(0.12)
                knife:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
            end
        end
    end
end)

-- [[ 2. VISUALS SEKTÖRÜ (TEMİZ ESP) ]] --
local EspSec = Visuals:NewSection("Vision Settings")
EspSec:NewToggle("MASTER ESP ACTIVE", "Temiz İsim ve Rol Görünümü", function(state) _G.MasterESP = state end)

-- [[ 3. FARM & MAGNET SEKTÖRÜ ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Düşen Silahı Ayaklarına Çeker", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Güvenli Coin/Candy Toplama", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT SEKTÖRÜ (HIZ & ZIPLAMA) ]] --
local MoveSec = Movement:NewSection("Physics Control")
MoveSec:NewTextBox("WalkSpeed", "Hız (Default 16)", function(txt) _G.SpeedValue = tonumber(txt) or 16 end)
MoveSec:NewTextBox("JumpPower", "Zıplama (Default 50)", function(txt) _G.JumpValue = tonumber(txt) or 50 end)
MoveSec:NewToggle("NoClip", "Duvarlardan Geçme", function(state) _G.NoClip = state end)

-- [[ 5. AVATAR & PRO (GAMEPASS) ]] --
local ProSec = Pro:NewSection("Support Layroxc")
ProSec:NewButton("Get Korblox (80 Robux)", "Prompt Açılır + Link Kopyalanır", function()
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
    if setclipboard then setclipboard(MyGamepassLink) end
end)

-- [[ DÖNGÜSEL SİSTEMLER (LOOPS) ]] --

-- Hız, Zıplama, NoClip ve Aimbot Döngüsü
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
            for _, v in pairs(Players:GetPlayers()) do
                if GetRole(v) == "MURDERER" and v.Character then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                end
            end
        end
    end)
end)

-- Farm ve Aura Döngüsü
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

-- TEMİZ ESP DÖNGÜSÜ
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0.5,1) or Color3.new(0,1,0))
                    
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.85
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0,50,0,10); bg.ExtentsOffset = Vector3.new(0,1.5,0)
                    
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg); lb.Name = "TL"
                    lb.Size = UDim2.new(1,0,1,0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; 
                    lb.TextSize = 8; lb.Font = Enum.Font.SourceSansBold; lb.Text = role
                end
            end)
        end
    end
end)
