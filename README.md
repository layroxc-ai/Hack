-- [[ LAYROXC HUB v59 - THE ABSOLUTE ENGINE (ULTIMATE COMPLETE EDITION) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - ⚠️BETA⚠️", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- GLOBAL AYARLAR (Ölünce gitmez, her an çalışır)
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

-- SEKMELER
local Main = Window:NewTab("Combat (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Movement = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT SEKTÖRÜ ]] --
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewButton("TP Behind Murderer", "Katilin arkasına ışınlan", function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)
RageSec:NewToggle("Smart Aimbot", "Katile kilitlenir", function(state) _G.Aimbot = state end)
RageSec:NewToggle("Kill Aura (25m)", "Otomatik keser", function(state) _G.KillAura = state end)
RageSec:NewButton("Kill All (Murderer Only)", "Herkesi anında keser", function()
    local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if k then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                k.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,1)
                task.wait(0.1); k:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
            end
        end
    end
end)

-- [[ 2. VISUALS SEKTÖRÜ (GELİŞMİŞ ESP) ]] --
local EspSec = Visuals:NewSection("Minimalist Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "İsimleri ve Rolleri Göster", function(state) _G.MasterESP = state end)

-- [[ 3. FARM SEKTÖRÜ ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Düşen silahı çeker", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Coin/Candy toplar", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT SEKTÖRÜ ]] --
local MoveSec = Movement:NewSection("Physics Control")
MoveSec:NewTextBox("WalkSpeed", "Hız Ayarı", function(txt) _G.SpeedValue = tonumber(txt) or 16 end)
MoveSec:NewTextBox("JumpPower", "Zıplama Ayarı", function(txt) _G.JumpValue = tonumber(txt) or 50 end)
MoveSec:NewToggle("NoClip", "Duvarlardan geç", function(state) _G.NoClip = state end)

-- [[ 5. PRO SEKTÖRÜ (GAMEPASS & LINK) ]] --
local ProSec = Pro:NewSection("Support & Korblox")
ProSec:NewButton("Get Korblox (80 Robux)", "Satın al ve linki kopyala", function()
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
    if setclipboard then setclipboard(MyGamepassLink) end
end)

-- [[ ANA SİSTEM DÖNGÜLERİ ]] --
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
    end)
end)

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

-- [[ ESP RENDER DÖNGÜSÜ ]] --
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0.6,1) or Color3.new(0,1,0))
                    
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.8; hl.OutlineTransparency = 0.5
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0,100,0,20); bg.ExtentsOffset = Vector3.new(0,2.5,0)
                    
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg); lb.Name = "TL"
                    lb.Size = UDim2.new(1,0,1,0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; 
                    lb.TextSize = 10; lb.Font = Enum.Font.SourceSansBold; lb.TextStrokeTransparency = 0.5
                    lb.Text = "[" .. role .. "] " .. v.DisplayName
                end
            end)
        end
    else
        for _, v in pairs(Players:GetPlayers()) do
            if v.Character then
                if v.Character:FindFirstChild("LayHL") then v.Character.LayHL:Destroy() end
                if v.Character.Head:FindFirstChild("LayName") then v.Character.Head.LayName:Destroy() end
            end
        end
    end
end)
