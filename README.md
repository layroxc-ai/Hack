local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc MM2", "DarkTheme")

-- Servisler
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Değişkenler
local AimbotEnabled = false
local FarmEnabled = false
local ESPEnabled = false

-- TABLAR
local Main = Window:NewTab("Main")
local MainSection = Main:NewSection("Combat & Visuals")
local FarmTab = Window:NewTab("Auto Farm")
local FarmSection = FarmTab:NewSection("Coin Farm")

-- 1. KİM KİM KONTROLÜ (ESP)
MainSection:NewToggle("Player ESP", "Oyuncularin rollerini gor", function(state)
    ESPEnabled = state
    while ESPEnabled do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                if not v.Character:FindFirstChild("Highlight") then
                    local hl = Instance.new("Highlight", v.Character)
                    hl.FillTransparency = 0.5
                    
                    -- Rol Belirleme
                    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                        hl.FillColor = Color3.fromRGB(255, 0, 0) -- KATİL (Kırmızı)
                    elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                        hl.FillColor = Color3.fromRGB(0, 0, 255) -- SHERIFF (Mavi)
                    else
                        hl.FillColor = Color3.fromRGB(0, 255, 0) -- MASUM (Yeşil)
                    end
                end
            end
        end
        task.wait(2)
        if not ESPEnabled then
            for _, v in pairs(Players:GetPlayers()) do
                if v.Character and v.Character:FindFirstChild("Highlight") then
                    v.Character.Highlight:Destroy()
                end
            end
        end
    end
end)

-- 2. AIMBOT
MainSection:NewToggle("Silent Aim", "Katili otomatik hedefler", function(state)
    AimbotEnabled = state
    RunService.RenderStepped:Connect(function()
        if AimbotEnabled then
            for _, v in pairs(Players:GetPlayers()) do
                if v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                    workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                end
            end
        end
    end)
end)

-- 3. KATİLİ VUR (KILL MURDERER)
MainSection:NewButton("Kill Murderer", "Katili aninda vurur", function()
    local target = nil
    for _, v in pairs(Players:GetPlayers()) do
        if v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
            target = v.Character.HumanoidRootPart
        end
    end
    
    if target and LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = target.CFrame * CFrame.new(0, 0, 3) -- Arkasına ışınlan
        -- Ateş etme tetikleyicisi buraya eklenebilir
    end
end)

-- 4. OTOMATİK COIN FARM
FarmSection:NewToggle("Auto Coin Farm", "Paralari otomatik toplar", function(state)
    FarmEnabled = state
    while FarmEnabled do
        for _, v in pairs(workspace:GetDescendants()) do
            if v.Name == "Coin" or v.Name == "Candy" or v.Name == "Snowflake" then
                if v:IsA("BasePart") and FarmEnabled then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                    task.wait(0.1) -- Ban yememek için çok az bekleme
                end
            end
        end
        task.wait()
    end
end)

FarmSection:NewButton("Reset when Full", "Canta dolunca reset atar", function()
    LocalPlayer.Character.Humanoid:BreakJoints()
end)
