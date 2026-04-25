-- [[ LAYROXC HUB v28 - TACTICAL ESP & NITRO SPEED ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub MM2 - TACTICAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- [[ MOBİL SÜRÜKLENEBİLİR BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local Main = Window:NewTab("Saldırı")
local Farm = Window:NewTab("Farm & Magnet")
local Visuals = Window:NewTab("Tactical ESP")
local Pro = Window:NewTab("Avatar")

-- [[ 1. TACTICAL NAME & HIGHLIGHT ESP ]] --
local VisSec = Visuals:NewSection("İsim ve Rol ESP")

_G.MasterESP = false
VisSec:NewToggle("Nitro Full ESP", "İsim + Rol + Uzaklık (0ms)", function(state)
    _G.MasterESP = state
end)

-- ESP FONKSİYONU (İsim Etiketi Oluşturma)
local function CreateESP(player)
    local bgui = Instance.new("BillboardGui", game.CoreGui)
    bgui.Name = player.Name .. "_ESP"
    bgui.Adornee = player.Character:WaitForChild("Head")
    bgui.Size = UDim2.new(0, 200, 0, 50)
    bgui.AlwaysOnTop = true
    bgui.ExtentsOffset = Vector3.new(0, 3, 0)

    local nametag = Instance.new("TextLabel", bgui)
    nametag.Size = UDim2.new(1, 0, 1, 0)
    nametag.BackgroundTransparency = 1
    nametag.TextStrokeTransparency = 0
    nametag.TextScaled = true
    nametag.Font = Enum.Font.SpecialElite
    return bgui, nametag
end

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                -- Highlight (Vücut Parlaması)
                local hl = v.Character:FindFirstChild("Highlight") or Instance.new("Highlight", v.Character)
                hl.Name = "Highlight"
                
                -- İsim Etiketi (Name ESP)
                local guiName = v.Name .. "_ESP"
                local bgui = game.CoreGui:FindFirstChild(guiName)
                if not bgui then bgui, _ = CreateESP(v) end
                local nametag = bgui.TextLabel
                
                local dist = math.floor((LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).magnitude)
                
                -- ROL KONTROLÜ VE RENKLENDİRME
                if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                    hl.FillColor = Color3.fromRGB(255, 0, 0)
                    nametag.TextColor3 = Color3.fromRGB(255, 0, 0)
                    nametag.Text = "[KATİL] " .. v.Name .. " [" .. dist .. "m]"
                elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                    hl.FillColor = Color3.fromRGB(0, 0, 255)
                    nametag.TextColor3 = Color3.fromRGB(0, 0, 255)
                    nametag.Text = "[ŞERİF] " .. v.Name .. " [" .. dist .. "m]"
                else
                    hl.FillColor = Color3.fromRGB(0, 255, 0)
                    nametag.TextColor3 = Color3.fromRGB(0, 255, 0)
                    nametag.Text = "[MASUM] " .. v.Name .. " [" .. dist .. "m]"
                end
            end
        end
    else
        -- Temizlik
        for _, v in pairs(Players:GetPlayers()) do
            if v.Character and v.Character:FindFirstChild("Highlight") then v.Character.Highlight:Destroy() end
            local bgui = game.CoreGui:FindFirstChild(v.Name .. "_ESP")
            if bgui then bgui:Destroy() end
        end
    end
end)

-- [[ 2. SALDIRI & AIMBOT ]] --
local MainSec = Main:NewSection("Aimbot & Silent")
_G.Aimbot = false
MainSec:NewToggle("Ultra Aimbot", "Katile Kilitlen", function(state) _G.Aimbot = state end)

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
            end
        end
    end
end)

_G.SilentAim = false
MainSec:NewToggle("Murder Silent", "Silent Aim Aktif", function(state) _G.SilentAim = state end)

-- [[ 3. FARM & MAGNET ]] --
local FarmSec = Farm:NewSection("Hızlı Toplama")
_G.AutoFarm = false
FarmSec:NewToggle("Smart Coin Farm", "Paralara Işınlan", function(state)
    _G.AutoFarm = state
    while _G.AutoFarm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy" or v.Name == "CoinVisual") and v:IsA("BasePart") then
                if _G.AutoFarm and LocalPlayer.Character then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                    task.wait(0.12)
                end
            end
        end
        task.wait()
    end
end)

FarmSec:NewToggle("Magnet Grab Gun", "Silahı Çek", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "GunDrop" or (v:IsA("Part") and v:FindFirstChild("TouchTransmitter") and not v.Parent:FindFirstChild("Knife"))) then
                if LocalPlayer.Character then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
            end
        end
        task.wait(0.05)
    end
end)

-- [[ 4. AVATAR ]] --
local AvaSec = Pro:NewSection("Avatar")
AvaSec:NewButton("FE Headless", "Kafasız", function() LocalPlayer.Character.Head.Transparency = 1 end)
AvaSec:NewButton("FE Korblox", "Bacak Hilesi", function() if LocalPlayer.Character:FindFirstChild("RightUpperLeg") then LocalPlayer.Character.RightUpperLeg:Destroy() end end)
