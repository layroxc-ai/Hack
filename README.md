-- [[ LAYROXC HUB - ULTIMATE MM2 v10 ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub MM2", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- MOBİL SÜRÜKLENEBİLİR BUTON (L)
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.4, 0)
OpenButton.Text = "L"
OpenButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
OpenButton.TextColor3 = Color3.fromRGB(255, 255, 0)
OpenButton.Draggable = true 
local UIKose = Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local Main = Window:NewTab("Saldırı")
local FarmTab = Window:NewTab("Farm")
local Visuals = Window:NewTab("Görsel")
local Social = Window:NewTab("Sosyal")

-- 1. SALDIRI (KILL ALL & GRAB GUN)
local CombatSec = Main:NewSection("Katil & Şerif Modları")

CombatSec:NewButton("Kill All (Katilsen)", "Herkesi saniyeler içinde öldürür", function()
    local Knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if Knife then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                -- Bıçağı ele al ve ışınlanıp vur
                Knife.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                task.wait(0.1)
                Knife:Activate()
            end
        end
    end
end)

CombatSec:NewToggle("Magnet Grab Gun (Mıknatıs)", "Yerdeki silahı otomatik üzerine çeker", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        -- Oyun içinde silahı farklı isimlerle tarar (Gelişmiş Grab)
        for _, v in pairs(workspace:GetChildren()) do
            if v.Name == "GunDrop" or v:FindFirstChild("Gun") then
                v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
            end
        end
        task.wait(0.2)
    end
end)

-- 2. FARM (AUTO COIN)
local FarmSec = FarmTab:NewSection("Otomatik Toplama")

FarmSec:NewToggle("Auto Coin Farm", "Haritadaki tüm paraları ışınlanarak toplar", function(state)
    _G.AutoFarm = state
    while _G.AutoFarm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy" or v.Name == "Snowflake") and v:IsA("BasePart") then
                if _G.AutoFarm then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                    task.wait(0.2) -- Ban yememek için küçük bir bekleme
                end
            end
        end
        task.wait()
    end
end)

-- 3. GÖRSEL (ESP FIX)
local VisSec = Visuals:NewSection("ESP (İfşa)")

VisSec:NewToggle("Full ESP (Gelişmiş)", "Katili Kırmızı, Şerifi Mavi gösterir", function(state)
    _G.MasterESP = state
    while _G.MasterESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("Highlight") then
                    local hl = Instance.new("Highlight", v.Character)
                    hl.Name = "Highlight"
                    hl.FillTransparency = 0.5
                end
                
                local hl = v.Character.Highlight
                if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                    hl.FillColor = Color3.fromRGB(255, 0, 0)
                elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                    hl.FillColor = Color3.fromRGB(0, 0, 255)
                else
                    hl.FillColor = Color3.fromRGB(0, 255, 0)
                end
            end
        end
        task.wait(0.5)
    end
end)

-- 4. SOSYAL
Social:NewSection("TikTok: @layroxcderler")
Social:NewButton("Profil Kopyala", "Takip etmeyi unutma kanki!", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
