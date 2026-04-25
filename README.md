-- [[ LAYROXC HUB v25 - ZERO ERROR MM2 ]] --
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
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local Combat = Window:NewTab("Saldırı (Silent)")
local Farm = Window:NewTab("Auto Farm")
local Visuals = Window:NewTab("ESP & İfşa")
local Avatar = Window:NewTab("Avatar")

-- [[ 1. MURDER SILENT & KILL ]] --
local CombatSec = Combat:NewSection("Silent Aim & Assassin")

_G.SilentAim = false
CombatSec:NewToggle("Murder Silent (Açık Kalsın)", "Mermiyi Katile Kitler", function(state)
    _G.SilentAim = state
end)

-- SILENT AIM MOTORU (EN STABİL SÜRÜM)
spawn(function()
    RunService.RenderStepped:Connect(function()
        if _G.SilentAim then
            local Gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
            if Gun and Gun:FindFirstChild("Shoot") then
                for _, v in pairs(Players:GetPlayers()) do
                    if v ~= LocalPlayer and v.Character and (v.Character:FindFirstChild("Knife") or v.Backpack:FindFirstChild("Knife")) then
                        local targetPos = v.Character.HumanoidRootPart.Position
                        -- Mermiyi tetikleyen evente katilin yerini gönder
                        Gun.Shoot:FireServer(targetPos)
                    end
                end
            end
        end
    end)
end)

CombatSec:NewButton("KATİLİN ARKASINA IŞINLAN", "Anında suikast pozisyonu", function()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3.5)
        end
    end
end)

CombatSec:NewToggle("Magnet Grab Gun", "Silahı Asla Kaçırmaz", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "GunDrop" or (v:IsA("Part") and v:FindFirstChild("TouchTransmitter") and not v.Parent:FindFirstChild("Knife"))) then
                if LocalPlayer.Character then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
            end
        end
        task.wait(0.1)
    end
end)

-- [[ 2. AUTO FARM ]] --
local FarmSec = Farm:NewSection("Coin Farm")
FarmSec:NewToggle("Auto Coin", "Işınlanarak Toplar", function(state)
    _G.AutoFarm = state
    while _G.AutoFarm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy") and v:IsA("BasePart") and _G.AutoFarm then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                task.wait(0.2)
            end
        end
        task.wait()
    end
end)

-- [[ 3. ESP ]] --
local VisSec = Visuals:NewSection("Rolleri Gör")
VisSec:NewToggle("Master ESP", "Duvar Arkası Görünüm", function(state)
    _G.MasterESP = state
    while _G.MasterESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("Highlight") then Instance.new("Highlight", v.Character).Name = "Highlight" end
                local hl = v.Character.Highlight
                if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then hl.FillColor = Color3.fromRGB(255, 0, 0)
                elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then hl.FillColor = Color3.fromRGB(0, 0, 255)
                else hl.FillColor = Color3.fromRGB(0, 255, 0) end
            end
        end
        task.wait(0.5)
    end
end)

-- [[ 4. AVATAR ]] --
local AvaSec = Avatar:NewSection("Avatar Hacks")
AvaSec:NewButton("Headless", "Kafasız", function() LocalPlayer.Character.Head.Transparency = 1 end)
AvaSec:NewButton("Korblox", "Bacak", function() if LocalPlayer.Character:FindFirstChild("RightUpperLeg") then LocalPlayer.Character.RightUpperLeg:Destroy() end end)
