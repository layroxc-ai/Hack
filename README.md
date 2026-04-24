# Hack
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Arayüz Oluşturma (Mobil Buton)
local ScreenGui = Instance.new("ScreenGui")
local AimButton = Instance.new("TextButton")
local UICorner = Instance.new("UICorner")

ScreenGui.Parent = game.CoreGui
AimButton.Name = "AimLock"
AimButton.Parent = ScreenGui
AimButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
AimButton.Position = UDim2.new(0.75, 0, 0.45, 0) -- Ekranın sağ orta kısmı
AimButton.Size = UDim2.new(0, 70, 0, 70)
AimButton.Text = "AIM"
AimButton.TextColor3 = Color3.new(1, 1, 1)
AimButton.TextScaled = true

UICorner.CornerRadius = UDim.new(0, 50) -- Yuvarlak buton
UICorner.Parent = AimButton

local Holding = false
AimButton.MouseButton1Down:Connect(function() Holding = true end)
AimButton.MouseButton1Up:Connect(function() Holding = false end)

-- Katili Bul (Eşya Kontrolü)
local function GetMurderer()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            -- Bıçak elinde mi veya çantasında mı?
            if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                return v.Character.HumanoidRootPart
            end
        end
    end
    return nil
end

-- Aimbot Çalışma Döngüsü
RunService.RenderStepped:Connect(function()
    if Holding then
        local Target = GetMurderer()
        if Target then
            -- Mobil Kamera Odaklama
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, Target.Position)
        end
    end
end)

print("Mobil MM2 Script Aktif - Butona Basili Tut!")

