local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc MM2", "DarkTheme")

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- AYARLAR
local TargetWalkSpeed = 16
local TikTokLink = "https://www.tiktok.com/@layroxcderler"

-- MOBİL SÜRÜKLENEBİLİR BUTON
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
local Main = Window:NewTab("Saldırı & ESP")
local PlayerTab = Window:NewTab("Karakter")
local FarmTab = Window:NewTab("Farm")
local SocialTab = Window:NewTab("Sosyal")

local MainSection = Main:NewSection("Silah & Görsel")
local SpeedSection = PlayerTab:NewSection("Hız & Hareket")
local FarmSection = FarmTab:NewSection("Otomatik Toplama")
local SocialSection = SocialTab:NewSection("Yapımcı: @layroxcderler")

-- 1. SİLAH BANA IŞINLANSIN (BRING GUN)
MainSection:NewToggle("Silahı Bana Işınla", "Silah yere düştüğünde sana gelir", function(state)
    _G.BringGun = state
    while _G.BringGun do
        local gunDrop = workspace:FindFirstChild("GunDrop")
        if gunDrop and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            -- Silahı senin üzerine ışınlar (Sen hareket etmezsin, silah gelir)
            gunDrop.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
        end
        task.wait(0.1) -- Çok hızlı kontrol
    end
end)

-- 2. BOX & SKELETON ESP (HIGHLIGHT)
MainSection:NewToggle("Box & Skeleton ESP", "Kutu ve Renkli Görünüm", function(state)
    _G.Visuals = state
    while _G.Visuals do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("BoxHighlight") then
                    local hl = Instance.new("Highlight", v.Character)
                    hl.Name = "BoxHighlight"
                    hl.FillTransparency = 0.5
                    -- Rol Kontrolü
                    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                        hl.FillColor = Color3.fromRGB(255, 0, 0) -- KATİL
                    elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                        hl.FillColor = Color3.fromRGB(0, 0, 255) -- SHERIFF
                    else
                        hl.FillColor = Color3.fromRGB(0, 255, 0) -- MASUM
                    end
                end
            end
        end
        task.wait(1)
    end
    if not _G.Visuals then
        for _, v in pairs(Players:GetPlayers()) do
            if v.Character and v.Character:FindFirstChild("BoxHighlight") then
                v.Character.BoxHighlight:Destroy()
            end
        end
    end
end)

-- 3. HIZ SABİTLEYİCİ (ASLA DÜŞMEZ)
SpeedSection:NewSlider("Sabit Hız", "Hızı burdan ayarla", 300, 16, function(s)
    TargetWalkSpeed = s
end)

RunService.Stepped:Connect(function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = TargetWalkSpeed
    end
end)

-- 4. KATİLİ VUR
MainSection:NewButton("Katili Vur", "Anlık arkasına ışınlar", function()
    for _, v in pairs(Players:GetPlayers()) do
        if v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

-- 5. TIKTOK BUTONU
SocialSection:NewButton("TikTok Hesabım: @layroxcderler", "Linki kopyalar", function()
    if setclipboard then 
        setclipboard(TikTokLink)
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Layroxc MM2",
            Text = "TikTok linki başarıyla kopyalandı!",
            Duration = 5
        })
    end
end)

-- 6. AUTO FARM
FarmSection:NewToggle("Auto Coin Farm", "Paraları toplar", function(state)
    _G.Farm = state
    while _G.Farm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy") and v:IsA("BasePart") and _G.Farm then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                task.wait(0.15)
            end
        end
        task.wait()
    end
end)
