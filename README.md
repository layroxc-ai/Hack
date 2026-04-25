-- [[ LAYROXC HUB v18 - ABSOLUTE GRAB & ALL FEATURES ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub MM2 - FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- [[ MOBİL BUTON ]] --
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

-- [[ TABS ]] --
local Main = Window:NewTab("MM2 Saldırı")
local Farm = Window:NewTab("Auto Farm")
local Visuals = Window:NewTab("Görsel (ESP)")
local Pro = Window:NewTab("Pro & Avatar")
local Social = Window:NewTab("Sosyal")

-- [[ 1. MM2 SALDIRI (GRAB GUN FIXED) ]] --
local CombatSec = Main:NewSection("Aimbot & Magnet Grab")

CombatSec:NewToggle("Magnet Grab Gun (ÇALIŞAN)", "Silahı anında üzerine çeker", function(state)
    _G.GrabGun = state
    task.spawn(function()
        while _G.GrabGun do
            for _, v in pairs(workspace:GetDescendants()) do
                -- İsmi GunDrop olan veya içinde TouchTransmitter olan silah modellerini arar
                if (v.Name == "GunDrop" or (v:IsA("Part") and v:FindFirstChild("TouchTransmitter") and v.Parent:FindFirstChild("Knife") == nil)) then
                    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                        -- Silahın ana parçasını bulup sana ışınlar
                        if v:IsA("Model") then
                            v:MoveTo(LocalPlayer.Character.HumanoidRootPart.Position)
                        else
                            v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
                        end
                    end
                end
            end
            task.wait(0.1)
        end
    end)
end)

CombatSec:NewToggle("Smart Aimbot", "Katile kilitlenir", function(state)
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

CombatSec:NewButton("Kill All (Katilsen)", "Herkesi öldürür", function()
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

-- [[ 2. AUTO FARM ]] --
local FarmSec = Farm:NewSection("Coin Farm")
FarmSec:NewToggle("Auto Coin/Candy", "Paraları toplar", function(state)
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

-- [[ 3. ESP ]] --
local VisSec = Visuals:NewSection("Full ESP")
VisSec:NewToggle("Highlight ESP", "Rolleri duvar arkası gösterir", function(state)
    _G.MasterESP = state
    while _G.MasterESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("Highlight") then
                    Instance.new("Highlight", v.Character).Name = "Highlight"
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

-- [[ 4. AVATAR & PRO ]] --
local ProSec = Pro:NewSection("Avatar Hacks")
ProSec:NewButton("Headless (Kafasız)", "Kafanı siler", function() LocalPlayer.Character.Head.Transparency = 1 end)
ProSec:NewButton("Korblox Leg", "Bacağını siler", function() if LocalPlayer.Character:FindFirstChild("RightUpperLeg") then LocalPlayer.Character.RightUpperLeg:Destroy() end end)
ProSec:NewToggle("Anti-Fling", "Uçmanı önler", function(state)
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
Social:NewButton("Profil Link Kopyala", "Destekle kanki!", function() setclipboard("https://www.tiktok.com/@layroxcderler") end)
