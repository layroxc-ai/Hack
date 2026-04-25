-- [[ LAYROXC HUB v40 - THE EVERYTHING VERSION ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub MM2 - v40 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- [[ MOBİL BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- SEKMELER
local Main = Window:NewTab("Saldırı (Aim)")
local Farm = Window:NewTab("Farm & Magnet")
local Visuals = Window:NewTab("Nitro ESP")
local Pro = Window:NewTab("Korblox & Pro")

-- [[ 1. SALDIRI MOTORU ]] --
local MainSec = Main:NewSection("Katil İnfaz Sistemi")
_G.Aimbot = false

MainSec:NewToggle("Katili Kilitle (Aimbot)", "Kamerayı Katile Sabitler", function(state)
    _G.Aimbot = state
end)

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                if v.Character:FindFirstChild("Knife") or v.Backpack:FindFirstChild("Knife") then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
                end
            end
        end
    end
end)

MainSec:NewButton("Katilin Arkasına Işınlan", "Hızlı Suikast", function()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and (v.Character:FindFirstChild("Knife") or v.Backpack:FindFirstChild("Knife")) then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

-- [[ 2. FARM & MAGNET ]] --
local FarmSec = Farm:NewSection("Toplayıcılar")

_G.GrabGun = false
FarmSec:NewToggle("Magnet Grab Gun", "Düşen Silahı Sana Işınlar", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "GunDrop" or (v:IsA("Part") and v:FindFirstChild("TouchTransmitter") and not v.Parent:FindFirstChild("Knife"))) then
                v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
            end
        end
        task.wait(0.2)
    end
end)

_G.AutoFarm = false
FarmSec:NewToggle("Auto Coin Farm", "Paraları Toplar", function(state)
    _G.AutoFarm = state
    while _G.AutoFarm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy" or v.Name == "CoinVisual") and v:IsA("BasePart") and _G.AutoFarm then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                task.wait(0.2)
            end
        end
        task.wait(0.1)
    end
end)

-- [[ 3. NITRO ESP (MESAFESİZ & NET) ]] --
local VisSec = Visuals:NewSection("Anlık Rol İfşa")
_G.MasterESP = false

VisSec:NewToggle("İsim & Rol ESP (Nitro)", "Mesafesiz Sabit Renkler", function(state)
    _G.MasterESP = state
end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local bgui = v.Character.Head:FindFirstChild("LayroxcESP") or Instance.new("BillboardGui", v.Character.Head)
                    bgui.Name = "LayroxcESP"
                    bgui.AlwaysOnTop = true
                    bgui.Size = UDim2.new(0, 200, 0, 50)
                    bgui.ExtentsOffset = Vector3.new(0, 3, 0)

                    local lbl = bgui:FindFirstChild("TextLabel") or Instance.new("TextLabel", bgui)
                    lbl.Size = UDim2.new(1, 0, 1, 0)
                    lbl.BackgroundTransparency = 1
                    lbl.TextScaled = true
                    lbl.Font = Enum.Font.GothamBold
                    
                    if v.Character:FindFirstChild("Knife") or v.Backpack:FindFirstChild("Knife") then
                        lbl.Text = "[KATİL] " .. v.Name
                        lbl.TextColor3 = Color3.fromRGB(255, 0, 0)
                    elseif v.Character:FindFirstChild("Gun") or v.Backpack:FindFirstChild("Gun") then
                        lbl.Text = "[ŞERİF] " .. v.Name
                        lbl.TextColor3 = Color3.fromRGB(0, 150, 255)
                    else
                        lbl.Text = "[MASUM] " .. v.Name
                        lbl.TextColor3 = Color3.fromRGB(0, 255, 0)
                    end
                end
            end)
        end
    else
        for _, v in pairs(Players:GetPlayers()) do
            if v.Character and v.Character.Head:FindFirstChild("LayroxcESP") then
                v.Character.Head.LayroxcESP:Destroy()
            end
        end
    end
end)

-- [[ 4. PRO AVATAR (80 ROBUX KORBLOX) ]] --
local ProSec = Pro:NewSection("Avatar Geliştirme")

ProSec:NewButton("Korblox Al (80 Robux)", "ID: 1812606767", function()
    MarketplaceService:PromptProductPurchase(LocalPlayer, 1812606767)
end)

ProSec:NewButton("Headless (FE)", "Kafasız Görünüm", function() 
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
        LocalPlayer.Character.Head.Transparency = 1
    end
end)

ProSec:NewButton("TikTok: @layroxcderler", "Kopyala", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
