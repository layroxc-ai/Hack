-- [[ LAYROXC HUB v34 - FULL UNLOCK & KORBLOX ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - FULL V34", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- [[ RAINBOW MOTORU ]] --
local function GetRainbowColor()
    return Color3.fromHSV(tick() % 5 / 5, 1, 1)
end

-- [[ MOBİL BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
RunService.RenderStepped:Connect(function() OpenButton.BackgroundColor3 = GetRainbowColor() end)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local Main = Window:NewTab("Saldırı (Aim)")
local Farm = Window:NewTab("Farm & Magnet")
local Visuals = Window:NewTab("Nitro ESP")
local Pro = Window:NewTab("Korblox & Pro")

-- [[ 1. SALDIRI & AIMBOT ]] --
local MainSec = Main:NewSection("Aimbot Sistemleri")
_G.Aimbot = false
MainSec:NewToggle("Smart Aimbot", "Katile Kilitlenir", function(state) _G.Aimbot = state end)

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
            end
        end
    end
end)

MainSec:NewButton("KATİLİN ARKASINA IŞINLAN", "Hızlı Suikast", function()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

-- [[ 2. FARM & MAGNET ]] --
local FarmSec = Farm:NewSection("Eşya Toplayıcı")
FarmSec:NewToggle("Magnet Grab Gun", "Silahı Sana Getirir", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "GunDrop" or (v:IsA("Part") and v:FindFirstChild("TouchTransmitter") and not v.Parent:FindFirstChild("Knife"))) then
                v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
            end
        end
        task.wait(0.1)
    end
end)

_G.AutoFarm = false
FarmSec:NewToggle("Auto Coin Farm", "Paraları Toplar", function(state)
    _G.AutoFarm = state
    while _G.AutoFarm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy" or v.Name == "CoinVisual") and v:IsA("BasePart") and _G.AutoFarm then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                task.wait(0.15)
            end
        end
        task.wait()
    end
end)

-- [[ 3. NITRO ESP (İSİM & ROL - MESAFESİZ) ]] --
local VisSec = Visuals:NewSection("İfşa Ayarları")
_G.MasterESP = false
VisSec:NewToggle("Nitro ESP Aktif", "Hızlı ve Temiz Görünüm", function(state) _G.MasterESP = state end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                local hl = v.Character:FindFirstChild("Highlight") or Instance.new("Highlight", v.Character)
                hl.OutlineColor = GetRainbowColor()

                local guiName = v.Name .. "_SimpleESP"
                local bgui = game.CoreGui:FindFirstChild(guiName)
                if not bgui then
                    bgui = Instance.new("BillboardGui", game.CoreGui)
                    bgui.Name = guiName
                    bgui.Adornee = v.Character.Head
                    bgui.Size = UDim2.new(0, 150, 0, 40)
                    bgui.AlwaysOnTop = true
                    bgui.ExtentsOffset = Vector3.new(0, 3, 0)
                    local lbl = Instance.new("TextLabel", bgui)
                    lbl.Size = UDim2.new(1, 0, 1, 0)
                    lbl.BackgroundTransparency = 1
                    lbl.TextStrokeTransparency = 0
                    lbl.TextScaled = true
                    lbl.Font = Enum.Font.GothamBold
                end

                local role = "Masum"
                if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then role = "KATİL"
                elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then role = "ŞERİF" end
                
                bgui.TextLabel.Text = "[" .. role .. "] " .. v.Name
                bgui.TextLabel.TextColor3 = GetRainbowColor()
            end
        end
    else
        for _, v in pairs(Players:GetPlayers()) do
            local bgui = game.CoreGui:FindFirstChild(v.Name .. "_SimpleESP")
            if bgui then bgui:Destroy() end
            if v.Character and v.Character:FindFirstChild("Highlight") then v.Character.Highlight:Destroy() end
        end
    end
end)

-- [[ 4. KORBLOX & HEADLESS (80 ROBUX) ]] --
local ProSec = Pro:NewSection("Avatar Geliştirme")

ProSec:NewButton("Korblox Al (80 Robux)", "ID: 1812606767", function()
    MarketplaceService:PromptProductPurchase(LocalPlayer, 1812606767)
end)

ProSec:NewButton("Headless (FE)", "Kafanı Gizle", function() 
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
        LocalPlayer.Character.Head.Transparency = 1
    end
end)

ProSec:NewButton("TikTok Takip", "@layroxcderler", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
