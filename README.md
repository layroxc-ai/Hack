-- [[ LAYROXC ULTRA AUTOMATION - MM2 & MUSCLE LEGENDS ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Pro Hub", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- MOBİL SÜRÜKLENEBİLİR BUTON (L)
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.4, 0)
OpenButton.Text = "L"
OpenButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
OpenButton.TextColor3 = Color3.new(1, 1, 1)
OpenButton.Draggable = true 
local UIKose = Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local MuscleTab = Window:NewTab("Muscle Legends")
local MM2Tab = Window:NewTab("MM2")
local SocialTab = Window:NewTab("Sosyal")

local MuscleSection = MuscleTab:NewSection("Otomatik Gelişim")
local MM2Section = MM2Tab:NewSection("Otomatik Oyun")

-- 1. MUSCLE LEGENDS OTOMASYON
MuscleSection:NewToggle("Ultra Hızlı Kas (Auto)", "Elinizdeki aleti saniyede 100 kere basar", function(state)
    _G.AutoMuscle = state
    while _G.AutoMuscle do
        local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
        if tool then
            tool:Activate()
        end
        task.wait(0.001) -- İnanılmaz hızlı tıklama
    end
end)

MuscleSection:NewToggle("Otomatik Rebirth", "Gücünüz yettiğinde otomatik rebirth atar", function(state)
    _G.AutoRebirth = state
    while _G.AutoRebirth do
        game:GetService("ReplicatedStorage").rEvents.rebirthEvent:FireServer()
        task.wait(2)
    end
end)

-- 2. MM2 OTOMASYON
MM2Section:NewToggle("Otomatik Silah Çek (Bring Gun)", "Silah düştüğü an eline gelir", function(state)
    _G.BringGun = state
    while _G.BringGun do
        local gunDrop = workspace:FindFirstChild("GunDrop")
        if gunDrop and LocalPlayer.Character then
            gunDrop.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
        end
        task.wait(0.1)
    end
end)

MM2Section:NewToggle("Otomatik Katil ESP", "Katili ve Şerifi her zaman gösterir", function(state)
    _G.Visuals = state
    while _G.Visuals do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("BoxHighlight") then
                    local hl = Instance.new("Highlight", v.Character)
                    hl.Name = "BoxHighlight"
                    hl.FillTransparency = 0.5
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

-- 3. SOSYAL
SocialTab:NewSection("TikTok: @layroxcderler")
SocialTab:NewButton("Profil Linkini Kopyala", "TikTok adresine git", function()
    if setclipboard then setclipboard("https://www.tiktok.com/@layroxcderler") end
end)
