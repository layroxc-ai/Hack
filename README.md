-- [[ LAYROXC HUB - FULL AUTO & ESP FIX ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub MM2", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- MOBİL BUTON
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.4, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
local UIKose = Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local Main = Window:NewTab("Otomasyon")
local Visuals = Window:NewTab("Görsel")
local Social = Window:NewTab("Sosyal")

-- 1. OTOMASYON (AUTO SHOOT & GRAB)
local AutoSec = Main:NewSection("Saldırı ve Toplama")

AutoSec:NewToggle("Auto Grab Gun", "Silah yere düşerse anında eline gelir", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        local gun = workspace:FindFirstChild("GunDrop")
        if gun and LocalPlayer.Character then
            gun.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
        end
        task.wait(0.1)
    end
end)

AutoSec:NewToggle("Auto Shoot Murderer", "Katili görünce otomatik ateş eder", function(state)
    _G.AutoShoot = state
    RunService.RenderStepped:Connect(function()
        if _G.AutoShoot then
            local gun = LocalPlayer.Character:FindFirstChild("Gun")
            if gun then
                for _, v in pairs(Players:GetPlayers()) do
                    if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                        -- Katil menzildeyse ateş et
                        gun:Activate() 
                        workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                    end
                end
            end
        end
    end)
end)

-- 2. GÖRSEL (ESP FIX)
local VisSec = Visuals:NewSection("ESP (Katil/Şerif İfşa)")

VisSec:NewToggle("Fixlenmiş ESP", "Asla kapanmayan ve rolleri gösteren ESP", function(state)
    _G.MasterESP = state
    while _G.MasterESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                -- Eski Highlight'ları temizle ve yenile (Fixleme kısmı)
                if not v.Character:FindFirstChild("Highlight") then
                    local hl = Instance.new("Highlight", v.Character)
                    hl.Name = "Highlight"
                    hl.FillTransparency = 0.4
                end
                
                local hl = v.Character.Highlight
                if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                    hl.FillColor = Color3.fromRGB(255, 0, 0) -- KATİL
                elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                    hl.FillColor = Color3.fromRGB(0, 0, 255) -- ŞERİF
                else
                    hl.FillColor = Color3.fromRGB(0, 255, 0) -- MASUM
                end
            end
        end
        task.wait(0.5)
    end
end)

-- 3. SOSYAL
Social:NewSection("TikTok: @layroxcderler")
Social:NewButton("Profilimi Kopyala", "Destek için takip et!", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
