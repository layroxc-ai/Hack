-- [[ LAYROXC HUB v30 - RAINBOW SKELETON & BOX ESP ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - Rainbow Edition", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- [[ RAINBOW RENDER ]] --
local function GetRainbowColor()
    return Color3.fromHSV(tick() % 5 / 5, 1, 1)
end

-- [[ MOBİL BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 45, 0, 45)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
RunService.RenderStepped:Connect(function() OpenButton.BackgroundColor3 = GetRainbowColor() end)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

local AimTab = Window:NewTab("Aimbot")
local EspTab = Window:NewTab("Visuals (ESP)")

-- [[ 1. AIMBOT AYARLARI ]] --
local AimSec = AimTab:NewSection("Aimbot & FOV")
local FOVring = Drawing.new("Circle")
FOVring.Visible = true
FOVring.Thickness = 1.5
FOVring.Radius = 150
FOVring.Filled = false

_G.AimbotEnabled = false
AimSec:NewToggle("Aimbot Aktif", "Katili hedefler", function(state) _G.AimbotEnabled = state end)
AimSec:NewSlider("Çember Genişliği", "FOV Ayarı", 500, 50, function(s) FOVring.Radius = s end)
AimSec:NewToggle("Çemberi Göster", "Halkayı aç/kapat", function(state) FOVring.Visible = state end)

-- [[ 2. BOX & SKELETON & NAME ESP ]] --
local EspSec = EspTab:NewSection("Görünürlük Ayarları")
_G.EspMaster = false
EspSec:NewToggle("ESP Aktif", "Box, Skeleton ve İsim açar", function(state) _G.EspMaster = state end)

-- Skeleton & Box Çizim Fonksiyonu
local function DrawESP(plr)
    local Box = Drawing.new("Square")
    Box.Visible = false
    Box.Thickness = 1
    Box.Filled = false

    local Name = Drawing.new("Text")
    Name.Visible = false
    Name.Size = 16
    Name.Center = true
    Name.Outline = true

    RunService.RenderStepped:Connect(function()
        if _G.EspMaster and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") and plr ~= LocalPlayer then
            local RootPart = plr.Character.HumanoidRootPart
            local Head = plr.Character:FindFirstChild("Head")
            local Pos, OnScreen = Camera:WorldToViewportPoint(RootPart.Position)

            if OnScreen then
                local Color = GetRainbowColor()
                
                -- Box ESP
                local SizeX = 2000 / Pos.Z
                local SizeY = 3000 / Pos.Z
                Box.Size = Vector2.new(SizeX, SizeY)
                Box.Position = Vector2.new(Pos.X - SizeX / 2, Pos.Y - SizeY / 2)
                Box.Color = Color
                Box.Visible = true

                -- Name ESP
                Name.Position = Vector2.new(Pos.X, Pos.Y - SizeY / 2 - 20)
                Name.Color = Color
                local role = "Masum"
                if plr.Backpack:FindFirstChild("Knife") or plr.Character:FindFirstChild("Knife") then role = "KATİL"
                elseif plr.Backpack:FindFirstChild("Gun") or plr.Character:FindFirstChild("Gun") then role = "ŞERİF" end
                Name.Text = "[" .. role .. "] " .. plr.Name
                Name.Visible = true

                -- Skeleton (Highlight Sürümü - Mobil Stabilite İçin)
                local hl = plr.Character:FindFirstChild("EspHL") or Instance.new("Highlight", plr.Character)
                hl.Name = "EspHL"
                hl.FillTransparency = 1
                hl.OutlineColor = Color
                hl.Visible = true
            else
                Box.Visible = false
                Name.Visible = false
                if plr.Character:FindFirstChild("EspHL") then plr.Character.EspHL.Visible = false end
            end
        else
            Box.Visible = false
            Name.Visible = false
            if plr.Character and plr.Character:FindFirstChild("EspHL") then plr.Character.EspHL:Destroy() end
        end
    end)
end

-- Tüm oyunculara uygula
for _, v in pairs(Players:GetPlayers()) do DrawESP(v) end
Players.PlayerAdded:Connect(DrawESP)

-- Aimbot Loop
RunService.RenderStepped:Connect(function()
    FOVring.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    FOVring.Color = GetRainbowColor()
    if _G.AimbotEnabled then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                local VPos, OnScreen = Camera:WorldToViewportPoint(v.Character.HumanoidRootPart.Position)
                if OnScreen then
                    local MouseDist = (Vector2.new(VPos.X, VPos.Y) - FOVring.Position).Magnitude
                    if MouseDist < FOVring.Radius then
                        Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                    end
                end
            end
        end
    end
end)
