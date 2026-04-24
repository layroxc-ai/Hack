-- [[ LAYROXC HUB - ANTI-FLING & SILENT AIM ]] --
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
OpenButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
OpenButton.TextColor3 = Color3.fromRGB(255, 255, 0) -- Sarı buton
OpenButton.Draggable = true 
local UIKose = Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local Main = Window:NewTab("Saldırı")
local Visuals = Window:NewTab("Görsel")
local Security = Window:NewTab("Güvenlik")
local Social = Window:NewTab("Sosyal")

-- 1. SALDIRI (AIMBOT)
local MainSec = Main:NewSection("Aimbot Ayarları")

MainSec:NewToggle("Silent Aim (Kilitlen)", "Mermiler otomatik olarak katile gider", function(state)
    _G.SilentAim = state
    RunService.RenderStepped:Connect(function()
        if _G.SilentAim then
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                    -- Aimbot Kamerayı odaklar
                    workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                end
            end
        end
    end)
end)

-- 2. GÖRSEL (KATİL İFŞA)
local VisSec = Visuals:NewSection("Katil & Şerif ESP")

VisSec:NewToggle("Katili Göster (Full ESP)", "Katili duvar arkasından gösterir", function(state)
    _G.KillerESP = state
    while _G.KillerESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("Highlight") then
                    local hl = Instance.new("Highlight", v.Character)
                    hl.Name = "Highlight"
                    -- Envanter Kontrolü (Bıçak varsa kırmızı yap)
                    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                        hl.FillColor = Color3.fromRGB(255, 0, 0)
                        hl.OutlineColor = Color3.fromRGB(255, 255, 255)
                    elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                        hl.FillColor = Color3.fromRGB(0, 0, 255)
                    else
                        hl.FillColor = Color3.fromRGB(0, 255, 0)
                    end
                end
            end
        end
        task.wait(0.5)
    end
end)

-- 3. GÜVENLİK (ANTI-FLING)
local SecSec = Security:NewSection("Koruma")

SecSec:NewToggle("Anti-Fling", "Birinin sizi uçurmasını engeller", function(state)
    _G.AntiFling = state
    if state then
        RunService.Stepped:Connect(function()
            if _G.AntiFling and LocalPlayer.Character then
                for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then
                        v.CanCollide = false -- Çarpışmayı kapatarak uçmayı engeller
                    end
                end
            end
        end)
    end
end)

-- 4. SOSYAL
Social:NewSection("TikTok: @layroxcderler")
Social:NewButton("Profil Linkini Kopyala", "Destek için takip et!", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
