-- [[ LAYROXC HUB v47 - THE EVERYTHING VERSION (NO MISSING CODE) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v47 FINAL", "DarkTheme")

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
OpenButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ÖZEL ROL TESPİT MOTORU (SAYIMDA ÇALIŞIR) ]] --
local function GetPlayerRole(v)
    -- Envanter kontrolü
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "KATİL" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "ŞERİF" end
    
    -- Sayım sırasında deşifre (GUI Kontrolü)
    pcall(function()
        local mainGui = v:FindFirstChild("PlayerGui") and v.PlayerGui:FindFirstChild("MainGui")
        local roleDesc = mainGui and mainGui:FindFirstChild("Game") and mainGui.Game:FindFirstChild("RoleDesc")
        if roleDesc then
            if roleDesc.Text:lower():find("murderer") or roleDesc.Text:lower():find("katil") then return "KATİL" end
            if roleDesc.Text:lower():find("sheriff") or roleDesc.Text:lower():find("şerif") then return "ŞERİF" end
        end
    end)
    return "Masum"
end

-- SEKMELER
local Rage = Window:NewTab("Saldırı (Kill)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Farm & Magnet")
local Pro = Window:NewTab("Korblox & Pro")

-- [[ 1. SALDIRI MOTORU (KILL AURA & KILL ALL) ]] --
local RageSec = Rage:NewSection("İnfaz Ayarları")

_G.KillAura = false
RageSec:NewToggle("Kill Aura", "Yakındakileri Keser", function(state)
    _G.KillAura = state
    while _G.KillAura do
        pcall(function()
            local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
            if knife then
                for _, v in pairs(Players:GetPlayers()) do
                    if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        local dist = (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude
                        if dist < 18 then
                            knife.Parent = LocalPlayer.Character
                            knife:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                            firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
                        end
                    end
                end
            end
        end)
        task.wait(0.1)
    end
end)

RageSec:NewButton("Kill All (Katilsen Bas)", "Saniyeler İçinde Herkesi Keser", function()
    local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if knife then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                knife.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 1)
                task.wait(0.1)
                knife:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
            end
        end
    end
end)

_G.Aimbot = false
RageSec:NewToggle("Smart Aimbot", "Katile Otomatik Odaklanır", function(state) _G.Aimbot = state end)

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if GetPlayerRole(v) == "KATİL" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
            end
        end
    end
end)

-- [[ 2. FULL ESP (KÜÇÜK İSİM & SKELETON) ]] --
local EspSec = Visuals:NewSection("İfşa Sistemi")
_G.MasterESP = false
EspSec:NewToggle("ESP Aktif (Sayımda Gösterir)", "Küçük İsimler ve Skeleton", function(state) _G.MasterESP = state end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetPlayerRole(v)
                    local color = Color3.fromRGB(0, 255, 0)
                    if role == "KATİL" then color = Color3.fromRGB(255, 0, 0)
                    elseif role == "ŞERİF" then color = Color3.fromRGB(0, 150, 255) end
                    
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillTransparency = 1; hl.OutlineColor = color
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 80, 0, 20); bg.ExtentsOffset = Vector3.new(0, 2.5, 0)
                    
                    local lb = bg:FindFirstChild("TextLabel") or Instance.new("TextLabel", bg)
                    lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1; lb.TextSize = 10; lb.Font = Enum.Font.GothamBold; lb.TextColor3 = color; lb.Text = "["..role.."] "..v.Name
                end
            end)
        end
    end
end)

-- [[ 3. FARM & MAGNET ]] --
local FarmSec = Farm:NewSection("Otomatik Toplama")
_G.GrabGun = false
FarmSec:NewToggle("Magnet Grab Gun", "Düşen Silahı Sana Getirir", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
        end
        task.wait(0.2)
    end
end)

_G.AutoFarm = false
FarmSec:NewToggle("Auto Coin Farm", "Paraları Otomatik Toplar", function(state)
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

-- [[ 4. KORBLOX (80 ROBUX GAMEPASS) ]] --
local ProSec = Pro:NewSection("Avatar")
ProSec:NewButton("Korblox (80 Robux)", "Link: 1812606767", function()
    MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767)
end)

ProSec:NewButton("Headless (FE)", "Kafanı Gizle", function() 
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
        LocalPlayer.Character.Head.Transparency = 1
    end
end)
