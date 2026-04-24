local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc MM2", "DarkTheme")

-- Servisler
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- MOBİL AÇMA/KAPAMA BUTONU
local OpenGui = Instance.new("ScreenGui")
local OpenButton = Instance.new("TextButton")
local UICorner = Instance.new("UICorner")

OpenGui.Parent = game.CoreGui
OpenButton.Parent = OpenGui
OpenButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
OpenButton.Position = UDim2.new(0.02, 0, 0.4, 0)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Text = "L"
OpenButton.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenButton.TextSize = 25
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = OpenButton

OpenButton.MouseButton1Click:Connect(function()
    Library:ToggleUI()
end)

-- TABLAR
local Main = Window:NewTab("Saldırı & Görsel")
local PlayerTab = Window:NewTab("Karakter")
local TargetTab = Window:NewTab("Hedef")
local FarmTab = Window:NewTab("Otomatik Farm")

local MainSection = Main:NewSection("Ana Özellikler")
local PlayerSection = PlayerTab:NewSection("Hareket")
local TargetSection = TargetTab:NewSection("Oyuncu Listesi")
local FarmSection = FarmTab:NewSection("Coin Farm")

-- 1. KATİLİ OTOMATİK VUR (KILL MURDERER)
MainSection:NewButton("Katili Otomatik Vur", "Katilin arkasına ışınlanır", function()
    local target = nil
    for _, v in pairs(Players:GetPlayers()) do
        if v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
            target = v.Character.HumanoidRootPart
        end
    end
    if target then
        LocalPlayer.Character.HumanoidRootPart.CFrame = target.CFrame * CFrame.new(0, 0, 3)
    end
end)

-- 2. AIMBOT (RAINBOW LINE)
local AimLine = Drawing.new("Line")
AimLine.Thickness = 2
local AimbotEnabled = false

MainSection:NewToggle("Aimbot & Rainbow Çizgi", "Katili hedefler", function(state)
    AimbotEnabled = state
    while AimbotEnabled do
        local target = nil
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                target = v.Character.HumanoidRootPart
            end
        end
        if target then
            local vector, onScreen = Camera:WorldToViewportPoint(target.Position)
            if onScreen then
                workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, target.Position)
                AimLine.Color = Color3.fromHSV(tick() % 5 / 5, 1, 1)
                AimLine.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
                AimLine.To = Vector2.new(vector.X, vector.Y)
                AimLine.Visible = true
            else AimLine.Visible = false end
        else AimLine.Visible = false end
        task.wait()
    end
    AimLine.Visible = false
end)

-- 3. İSİM ESP & X-RAY
MainSection:NewToggle("Oyuncu ESP", "Rolleri gösterir", function(state)
    _G.ESP = state
    while _G.ESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                if not v.Character.Head:FindFirstChild("Nametag") then
                    local bb = Instance.new("BillboardGui", v.Character.Head)
                    bb.Name = "Nametag"
                    bb.Size = UDim2.new(0, 200, 0, 50)
                    bb.AlwaysOnTop = true
                    local lbl = Instance.new("TextLabel", bb)
                    lbl.Size = UDim2.new(1, 0, 1, 0)
                    lbl.BackgroundTransparency = 1
                    lbl.TextScaled = true
                    local role = "Masum"; local col = Color3.fromRGB(0, 255, 0)
                    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then role = "KATIL"; col = Color3.fromRGB(255, 0, 0)
                    elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then role = "SHERIFF"; col = Color3.fromRGB(0, 0, 255) end
                    lbl.Text = v.Name .. " [" .. role .. "]"; lbl.TextColor3 = col
                end
            end
        end
        task.wait(1)
    end
end)

PlayerSection:NewButton("X-Ray Aç/Kapat", "Duvarları şeffaf yapar", function()
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsDescendantOf(LocalPlayer.Character) then
            obj.Transparency = (obj.Transparency == 0 and 0.5 or 0)
        end
    end
end)

-- 4. HIZ & ZIPLAMA
PlayerSection:NewSlider("Hız", "Walkspeed", 300, 16, function(s) LocalPlayer.Character.Humanoid.WalkSpeed = s end)
PlayerSection:NewSlider("Zıplama", "JumpPower", 300, 50, function(s) LocalPlayer.Character.Humanoid.JumpPower = s end)

-- 5. TARGET & FLING (OYUNCU LİSTESİ)
local SelectedPlayer = nil
local PlayerNames = {}
for _, v in pairs(Players:GetPlayers()) do if v ~= LocalPlayer then table.insert(PlayerNames, v.Name) end end
local Drop = TargetSection:NewDropdown("Hedef Seç", "Seçili kişiyi uçurur", PlayerNames, function(t) SelectedPlayer = t end)

TargetSection:NewButton("Fling (Uçur)", "Kişiyi fırlatır", function()
    local t = Players:FindFirstChild(SelectedPlayer)
    if t and t.Character then
        LocalPlayer.Character.HumanoidRootPart.CFrame = t.Character.HumanoidRootPart.CFrame
        local bv = Instance.new("BodyAngularVelocity", LocalPlayer.Character.HumanoidRootPart)
        bv.AngularVelocity = Vector3.new(0, 99999, 0)
        bv.MaxTorque = Vector3.new(0, 99999, 0)
        task.wait(0.5); bv:Destroy()
    end
end)

-- 6. COIN FARM (GÜÇLENDİRİLMİŞ)
FarmSection:NewToggle("Auto Coin Farm", "Paraları otomatik toplar", function(state)
    _G.Farm = state
    while _G.Farm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy" or v.Name == "Snowflake") and v:IsA("BasePart") and _G.Farm then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                task.wait(0.15)
            end
        end
        task.wait()
    end
end)
