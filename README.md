local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Arayüz Ayarları
local ScreenGui = Instance.new("ScreenGui")
local AimButton = Instance.new("TextButton")
local FarmButton = Instance.new("TextButton")
local UICorner = Instance.new("UICorner")
local UICorner2 = Instance.new("UICorner")

ScreenGui.Parent = game.CoreGui

-- AIM BUTONU (Kırmızı)
AimButton.Name = "AimLock"
AimButton.Parent = ScreenGui
AimButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
AimButton.Position = UDim2.new(0.75, 0, 0.45, 0)
AimButton.Size = UDim2.new(0, 70, 0, 70)
AimButton.Text = "AIM"
AimButton.TextColor3 = Color3.new(1, 1, 1)
AimButton.TextScaled = true
UICorner.CornerRadius = UDim.new(0, 50)
UICorner.Parent = AimButton

-- FARM BUTONU (Yeşil)
FarmButton.Name = "CoinFarm"
FarmButton.Parent = ScreenGui
FarmButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
FarmButton.Position = UDim2.new(0.75, 0, 0.58, 0)
FarmButton.Size = UDim2.new(0, 70, 0, 70)
FarmButton.Text = "FARM: OFF"
FarmButton.TextColor3 = Color3.new(1, 1, 1)
FarmButton.TextScaled = true
UICorner2.CornerRadius = UDim.new(0, 50)
UICorner2.Parent = FarmButton

local Holding = false
local Farming = false

AimButton.MouseButton1Down:Connect(function() Holding = true end)
AimButton.MouseButton1Up:Connect(function() Holding = false end)

FarmButton.MouseButton1Click:Connect(function()
    Farming = not Farming
    FarmButton.Text = Farming and "FARM: ON" or "FARM: OFF"
end)

-- Katil Tespiti
local function GetMurderer()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                return v.Character.HumanoidRootPart
            end
        end
    end
    return nil
end

-- Ana Döngü
RunService.RenderStepped:Connect(function()
    -- Aimbot Kısmı
    if Holding then
        local Target = GetMurderer()
        if Target then
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, Target.Position)
        end
    end
    
    -- Coin Farm Kısmı
    if Farming and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        -- Çanta Kontrolü (40 dolunca reset)
        local coinData = LocalPlayer:FindFirstChild("CoinData", true)
        if coinData and coinData:FindFirstChild("CoinContainer") and #coinData.CoinContainer:GetChildren() >= 40 then
            LocalPlayer.Character:BreakJoints() -- Çanta dolunca reset atar
            Farming = false
            FarmButton.Text = "FARM: OFF"
            return
        end

        -- En Yakın Coine Gitme
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj.Name == "Coin" or obj.Name == "Snowflake" or obj.Name == "CandyCorn" then
                if obj:IsA("BasePart") then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = obj.CFrame
                    break -- Tek seferde bir tanesine gider
                end
            end
        end
    end
end)
