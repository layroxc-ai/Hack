-- [[ LAYROXC HUB - WALLBANG & SILENT AIM ]] --
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
OpenButton.TextColor3 = Color3.fromRGB(255, 0, 0)
OpenButton.Draggable = true 
local UIKose = Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local Combat = Window:NewTab("Combat")
local Visuals = Window:NewTab("Visuals")
local Social = Window:NewTab("Social")

local CombatSec = Combat:NewSection("Saldırı Modları")
local VisSec = Visuals:NewSection("Görsel Hileler")

-- 1. DUVAR ARKASI VURMA (WALLBANG / SILENT AIM)
CombatSec:NewToggle("Wallbang (Duvar Arkası Vur)", "Mermiler engelleri geçer ve katile gider", function(state)
    _G.Wallbang = state
    RunService.RenderStepped:Connect(function()
        if _G.Wallbang then
            local gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
            if gun then
                -- Katili Bul
                for _, v in pairs(Players:GetPlayers()) do
                    if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                        local targetPos = v.Character.HumanoidRootPart.Position
                        -- Mermiyi katilin üzerine yönlendirir (Duvarları yok sayar)
                        local shootRemote = gun:FindFirstChild("Shoot") or gun:FindFirstChild("Remote")
                        if shootRemote and shootRemote:IsA("RemoteEvent") then
                            shootRemote:FireServer(targetPos)
                        end
                    end
                end
            end
        end
    end)
end)

-- 2. KATİLİ VUR (TELEPORT)
CombatSec:NewButton("Katilin Arkasına Işınlan", "Hızlı suikast için", function()
    for _, v in pairs(Players:GetPlayers()) do
        if v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

-- 3. MASTER ESP (ISIM + KUTU + ROL)
VisSec:NewToggle("Duvar Arkası Görme (ESP)", "Katili kırmızı, Şerifi mavi gösterir", function(state)
    _G.ESP = state
    while _G.ESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("Highlight") then
                    local hl = Instance.new("Highlight", v.Character)
                    hl.Name = "Highlight"
                    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                        hl.FillColor = Color3.fromRGB(255, 0, 0)
                    elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                        hl.FillColor = Color3.fromRGB(0, 0, 255)
                    else
                        hl.FillColor = Color3.fromRGB(0, 255, 0)
                    end
                end
            end
        end
        task.wait(1)
    end
end)

-- 4. SOSYAL (TIKTOK)
Social:NewSection("TikTok: @layroxcderler")
Social:NewButton("Profil Linkini Kopyala", "Kopyalamak için tıkla", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
