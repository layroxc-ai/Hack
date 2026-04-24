local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc MM2", "DarkTheme")

-- Servisler
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- TABLAR
local Main = Window:NewTab("Main")
local MainSection = Main:NewSection("Combat & Visuals")
local VisualTab = Window:NewTab("Visuals")
local VisualSection = VisualTab:NewSection("Lines & Effects")
local TargetTab = Window:NewTab("Target")
local TargetSection = TargetTab:NewSection("Player List")

-- RAINBOW RENK DÖNGÜSÜ
local Hue = 0
RunService.RenderStepped:Connect(function()
    Hue = Hue + 0.01
    if Hue > 1 then Hue = 0 end
end)

-- 1. RAINBOW AIMBOT LINE
local AimLine = Drawing.new("Line")
AimLine.Thickness = 2
AimLine.Visible = false

MainSection:NewToggle("Rainbow Aim Line", "Katili cizgiyle takip et", function(state)
    _G.AimLine = state
    while _G.AimLine do
        local target = nil
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                target = v.Character.HumanoidRootPart
            end
        end

        if target then
            local vector, onScreen = Camera:WorldToViewportPoint(target.Position)
            if onScreen then
                AimLine.Color = Color3.fromHSV(Hue, 1, 1)
                AimLine.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
                AimLine.To = Vector2.new(vector.X, vector.Y)
                AimLine.Visible = true
            else
                AimLine.Visible = false
            end
        else
            AimLine.Visible = false
        end
        task.wait()
    end
    AimLine.Visible = false
end)

-- 2. TRACES (İZLEYİCİ ÇİZGİLER)
local Traces = {}
VisualSection:NewToggle("Player Traces", "Oyunculara giden izler", function(state)
    _G.Traces = state
    if not state then
        for _, line in pairs(Traces) do line:Remove() end
        Traces = {}
    end
end)

RunService.RenderStepped:Connect(function()
    if _G.Traces then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                local line = Traces[v] or Drawing.new("Line")
                line.Thickness = 1
                line.Transparency = 0.8
                
                local vector, onScreen = Camera:WorldToViewportPoint(v.Character.HumanoidRootPart.Position)
                if onScreen then
                    -- Rol Rengi
                    local color = Color3.fromRGB(0, 255, 0)
                    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then color = Color3.fromRGB(255, 0, 0)
                    elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then color = Color3.fromRGB(0, 0, 255) end
                    
                    line.Color = color
                    line.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y) -- Ekranın altından çıkar
                    line.To = Vector2.new(vector.X, vector.Y)
                    line.Visible = true
                else
                    line.Visible = false
                end
                Traces[v] = line
            end
        end
    end
end)

-- [DİĞER ÖZELLİKLER (SPEED, FLING, FARM) BURAYA EKLENEBİLİR]
-- Önceki kodlardaki bölümleri buraya ekleyerek menüyü tamamlayabilirsin.
