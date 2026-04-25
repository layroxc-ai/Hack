-- [[ LAYROXC HUB v58 - STEALTH FARM & ANTI-KICK ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v58", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local TweenService = game:GetService("TweenService")
local MarketplaceService = game:GetService("MarketplaceService")

-- [[ MOBİL ANA BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ROL TESPİT MOTORU ]] --
local function GetPlayerRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Masum" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "KATİL" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "ŞERİF" end
    pcall(function()
        local roleText = v.PlayerGui.MainGui.Game.RoleDesc.Text:lower()
        if roleText:find("murderer") then return "KATİL" end
        if roleText:find("sheriff") then return "ŞERİF" end
    end)
    return "Masum"
end

-- SEKMELER
local Main = Window:NewTab("Saldırı (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Pro = Window:NewTab("Avatar & Fix")

-- [[ 1. SALDIRI MOTORU ]] --
local RageSec = Main:NewSection("İnfaz Ayarları")

RageSec:NewButton("KILL ALL", "Anında Temizler", function()
    local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if knife then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                knife.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 1)
                task.wait(0.1)
                knife:Activate()
            end
        end
    end
end)

_G.Aimbot = false
RageSec:NewToggle("Smart Aimbot", "Katile Kilitlenir", function(state) _G.Aimbot = state end)

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if GetPlayerRole(v) == "KATİL" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
            end
        end
    end
end)

RageSec:NewButton("Işınlanma Tuşu (Sabit)", "Katilin Ensesine Uçar", function()
    if game.CoreGui:FindFirstChild("TpGui") then game.CoreGui.TpGui:Destroy() end
    local TpGui = Instance.new("ScreenGui", game.CoreGui)
    local TpButton = Instance.new("TextButton", TpGui)
    TpButton.Size = UDim2.new(0, 120, 0, 40)
    TpButton.Position = UDim2.new(0.5, -60, 0.8, 0)
    TpButton.Text = "IŞINLAN (TP)"
    TpButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    TpButton.TextColor3 = Color3.new(1, 1, 1)
    TpButton.Draggable = true
    Instance.new("UICorner", TpButton)
    TpButton.MouseButton1Click:Connect(function()
        for _, v in pairs(Players:GetPlayers()) do
            if GetPlayerRole(v) == "KATİL" and v.Character then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2)
            end
        end
    end)
end)

-- [[ 2. FULL ESP ]] --
local EspSec = Visuals:NewSection("Deşifre")
_G.MasterESP = false
EspSec:NewToggle("FULL ESP AKTİF", "Kutu ve İsimler", function(state) _G.MasterESP = state end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetPlayerRole(v)
                    local color = Color3.fromRGB(0, 255, 0)
                    if role == "KATİL" then color = Color3.fromRGB(255, 0, 0)
                    elseif role == "ŞERİF" then color = Color3.fromRGB(0, 150, 255) end
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.FillColor = color; hl.FillTransparency = 0.7; hl.OutlineColor = color
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 80, 0, 20); bg.ExtentsOffset = Vector3.new(0, 3, 0)
                    local lb = bg:FindFirstChild("TextLabel") or Instance.new("TextLabel", bg)
                    lb.TextColor3 = color; lb.TextSize = 10; lb.Text = "["..role.."] "..v.Name
                end
            end)
        end
    end
end)

-- [[ 3. MAGNET & STEALTH FARM (KICK SAVAR) ]] --
local FarmSec = Farm:NewSection("Toplama")

_G.GrabGun = false
FarmSec:NewToggle("Magnet Gun (Fixed)", "Silahı Sana Getirir", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        pcall(function()
            for _, v in pairs(workspace:GetDescendants()) do
                if v.Name == "GunDrop" then 
                    v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
                    v.Velocity = Vector3.new(0,0,0)
                end
            end
        end)
        task.wait(0.1)
    end
end)

_G.StealthFarm = false
FarmSec:NewToggle("GÜVENLİ FARM (NO KICK)", "Tween ile Yavaşça Toplar", function(state)
    _G.StealthFarm = state
    while _G.StealthFarm do
        pcall(function()
            for _, v in pairs(workspace:GetDescendants()) do
                if (v.Name == "Coin" or v.Name == "Candy") and _G.StealthFarm then
                    local info = TweenInfo.new(0.5, Enum.EasingStyle.Linear) -- Hız ayarı
                    local tween = TweenService:Create(LocalPlayer.Character.HumanoidRootPart, info, {CFrame = v.CFrame})
                    tween:Play()
                    tween.Completed:Wait()
                end
            end
        end)
        task.wait(0.1)
    end
end)

-- [[ 4. AVATAR ]] --
local ProSec = Pro:NewSection("Avatar")
ProSec:NewButton("Korblox (80 Robux)", "ID: 1812606767", function()
    MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767)
end)
