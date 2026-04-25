-- [[ LAYROXC HUB v16 - PURE MM2 ULTIMATE ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub MM2", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- [[ MOBİL SÜRÜKLENEBİLİR BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.4, 0)
OpenButton.Text = "L"
OpenButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
OpenButton.TextColor3 = Color3.fromRGB(255, 0, 0)
OpenButton.Draggable = true 
local UIKose = Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ TABLAR ]] --
local Combat = Window:NewTab("Saldırı")
local Farm = Window:NewTab("Auto Farm")
local Visuals = Window:NewTab("ESP & Görsel")
local Avatar = Window:NewTab("Avatar & Pro")
local Social = Window:NewTab("Sosyal")

-- [[ 1. MM2 SALDIRI MODLARI ]] --
local CombatSec = Combat:NewSection("Aimbot & Kill All")

CombatSec:NewToggle("Smart Aimbot", "Katilin gövdesine kilitlenir", function(state)
    _G.Aimbot = state
    RunService.RenderStepped:Connect(function()
        if _G.Aimbot then
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                end
            end
        end
    end)
end)

CombatSec:NewToggle("Magnet Grab Gun", "Yerdeki silahı sana ışınlar", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        local gun = workspace:FindFirstChild("GunDrop") or workspace:FindFirstChild("Gun")
        if gun and LocalPlayer.Character then
            if gun:IsA("BasePart") then 
                gun.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
            elseif gun:IsA("Model") then 
                gun:MoveTo(LocalPlayer.Character.HumanoidRootPart.Position) 
            end
        end
        task.wait(0.2)
    end
end)

CombatSec:NewButton("Kill All (Katilsen)", "Herkesi sırayla yok eder", function()
    local Knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if Knife then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                Knife.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                task.wait(0.1)
                Knife:Activate()
            end
        end
    end
end)

-- [[ 2. MM2 AUTO FARM ]] --
local FarmSec = Farm:NewSection("Coin & XP Farm")

FarmSec:NewToggle("Auto Coin Farm", "Paraları otomatik toplar", function(state)
    _G.AutoFarm = state
    while _G.AutoFarm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy" or v.Name == "Snowflake") and v:IsA("BasePart") and _G.AutoFarm then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                task.wait(0.2)
            end
        end
        task.wait()
    end
end)

-- [[ 3. ESP & GÖRSEL ]] --
local VisSec = Visuals:NewSection("Gelişmiş ESP")

VisSec:NewToggle("Full Highlight ESP", "Rolleri ifşa eder", function(state)
    _G.MasterESP = state
    while _G.MasterESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("Highlight") then
                    Instance.new("Highlight", v.Character).Name = "Highlight"
                end
                local hl = v.Character.Highlight
                if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                    hl.FillColor = Color3.fromRGB(255, 0, 0) -- KATİL
                elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                    hl.FillColor = Color3.fromRGB(0, 0, 255) -- ŞERİF
                else
                    hl.FillColor = Color3.fromRGB(0, 255, 0) -- MASUM
                end
            end
        end
        task.wait(0.5)
    end
end)

-- [[ 4. AVATAR & GÜVENLİK ]] --
local AvaSec = Avatar:NewSection("Avatar Özelleştirme")

AvaSec:NewButton("FE Headless (Görsel)", "Kafanı gizler", function()
    LocalPlayer.Character.Head.Transparency = 1
end)

AvaSec:NewButton("FE Korblox (Görsel)", "Sağ bacağı gizler", function()
    if LocalPlayer.Character:FindFirstChild("RightUpperLeg") then
        LocalPlayer.Character.RightUpperLeg:Destroy()
    end
end)

AvaSec:NewToggle("Anti-Fling", "Seni uçuramazlar", function(state)
    _G.AntiFling = state
    RunService.Stepped:Connect(function()
        if _G.AntiFling and LocalPlayer.Character then
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") then v.CanCollide = false end
            end
        end
    end)
end)

-- [[ 5. SOSYAL ]] --
Social:NewSection("TikTok: @layroxcderler")
Social:NewButton("Profili Kopyala", "Takip etmeyi unutma!", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
