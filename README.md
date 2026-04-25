-- [[ LAYROXC HUB v44 - FULL STEALTH & POWER (NO RAINBOW) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v44", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- [[ MOBİL BUTON (SABİT RENK) ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- SEKMELER
local Main = Window:NewTab("Saldırı (Aim)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Farm & Magnet")
local Pro = Window:NewTab("Korblox & Pro")

-- [[ 1. SALDIRI & AIMBOT ]] --
local MainSec = Main:NewSection("Aimbot & Suikast")
_G.Aimbot = false
MainSec:NewToggle("Smart Aimbot (Katil)", "Katile Kilitlenir", function(state) _G.Aimbot = state end)

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

MainSec:NewButton("Katilin Arkasına Işınlan", "Hızlı Kill", function()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and (v.Character:FindFirstChild("Knife") or v.Backpack:FindFirstChild("Knife")) then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

-- [[ 2. FULL ESP (KÜÇÜK İSİM & SKELETON - NO RAINBOW) ]] --
local EspSec = Visuals:NewSection("Görünürlük Ayarları")
_G.MasterESP = false
EspSec:NewToggle("FULL NITRO ESP", "Küçük İsim ve Skeleton", function(state) _G.MasterESP = state end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    -- Rol Tespit ve Renk Belirleme
                    local roleColor = Color3.fromRGB(0, 255, 0) -- Masum (Yeşil)
                    local roleName = "Masum"
                    
                    if v.Character:FindFirstChild("Knife") or v.Backpack:FindFirstChild("Knife") then
                        roleColor = Color3.fromRGB(255, 0, 0) -- Katil (Kırmızı)
                        roleName = "KATİL"
                    elseif v.Character:FindFirstChild("Gun") or v.Backpack:FindFirstChild("Gun") then
                        roleColor = Color3.fromRGB(0, 150, 255) -- Şerif (Mavi)
                        roleName = "ŞERİF"
                    end

                    -- Skeleton Highlight
                    local hl = v.Character:FindFirstChild("LayroxcHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayroxcHL"
                    hl.FillTransparency = 1
                    hl.OutlineColor = roleColor

                    -- Billboard Name ESP (KÜÇÜK)
                    local bgui = v.Character.Head:FindFirstChild("LayroxcName") or Instance.new("BillboardGui", v.Character.Head)
                    bgui.Name = "LayroxcName"
                    bgui.AlwaysOnTop = true
                    bgui.Size = UDim2.new(0, 100, 0, 20)
                    bgui.ExtentsOffset = Vector3.new(0, 2.5, 0)
                    
                    local lbl = bgui:FindFirstChild("TextLabel") or Instance.new("TextLabel", bgui)
                    lbl.Size = UDim2.new(1, 0, 1, 0)
                    lbl.BackgroundTransparency = 1
                    lbl.TextSize = 12 
                    lbl.Font = Enum.Font.GothamBold
                    lbl.TextColor3 = roleColor
                    lbl.Text = "[" .. roleName .. "] " .. v.Name
                end
            end)
        end
    else
        for _, v in pairs(Players:GetPlayers()) do
            if v.Character then
                if v.Character:FindFirstChild("LayroxcHL") then v.Character.LayroxcHL:Destroy() end
                if v.Character.Head:FindFirstChild("LayroxcName") then v.Character.Head.LayroxcName:Destroy() end
            end
        end
    end
end)

-- [[ 3. FARM & MAGNET (EKSİKSİZ) ]] --
local FarmSec = Farm:NewSection("Otomatik Toplama")

_G.GrabGun = false
FarmSec:NewToggle("Magnet Grab Gun", "Silahı Çeker", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "GunDrop" or (v:IsA("Part") and v:FindFirstChild("TouchTransmitter"))) then
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
            if (v.Name == "Coin" or v.Name == "Candy") and _G.AutoFarm then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                task.wait(0.2)
            end
        end
        task.wait(0.1)
    end
end)

-- [[ 4. PRO AVATAR (80 ROBUX GAMEPASS) ]] --
local ProSec = Pro:NewSection("Avatar & Bağış")

ProSec:NewButton("Korblox FE (80 Robux)", "ID: 1812606767", function()
    MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767)
end)

ProSec:NewButton("Headless (FE)", "Kafanı Yok Et", function() 
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
        LocalPlayer.Character.Head.Transparency = 1
    end
end)
