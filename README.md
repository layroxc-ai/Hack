-- [[ LAYROXC HUB v48 - TELEPORT & PURCHASE FIXED ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v48", "DarkTheme")

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

-- [[ ROL BULUCU MOTOR ]] --
local function GetKiller()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                return v.Character.HumanoidRootPart
            end
        end
    end
    return nil
end

-- SEKMELER
local Main = Window:NewTab("Saldırı (Kill)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Farm & Magnet")
local Pro = Window:NewTab("Korblox & Pro")

-- [[ 1. SALDIRI - IŞINLANMA EKLENDİ ]] --
local RageSec = Main:NewSection("İnfaz Ayarları")

RageSec:NewButton("Katilin Arkasına Işınlan", "Anında Suikast", function()
    local target = GetKiller()
    if target then
        LocalPlayer.Character.HumanoidRootPart.CFrame = target.CFrame * CFrame.new(0, 0, 2)
    else
        -- Katil henüz bıçak çekmemişse bile bulmaya çalışır
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v.PlayerGui.MainGui.Game.RoleDesc.Text:lower():find("murderer") then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2)
                end
            end)
        end
    end
end)

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

-- [[ 2. ESP (SAYIMDA ÇALIŞAN & KÜÇÜK İSİM) ]] --
local EspSec = Visuals:NewSection("İfşa")
_G.MasterESP = false
EspSec:NewToggle("Nitro ESP (Sayımda Gösterir)", "Küçük İsimler", function(state) _G.MasterESP = state end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local color = Color3.fromRGB(0, 255, 0)
                    local role = "Masum"
                    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then 
                        color = Color3.fromRGB(255, 0, 0); role = "KATİL"
                    elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then 
                        color = Color3.fromRGB(0, 150, 255); role = "ŞERİF"
                    end
                    
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
local FarmSec = Farm:NewSection("Toplama")
_G.GrabGun = false
FarmSec:NewToggle("Magnet Grab Gun", "Silahı Çeker", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
        end
        task.wait(0.2)
    end
end)

-- [[ 4. SATIN ALMA DÜZELTİLDİ (80 ROBUX) ]] --
local ProSec = Pro:NewSection("Avatar")
ProSec:NewButton("Korblox Satın Al (80 Robux)", "ID: 1812606767", function()
    -- Gamepass satın alma ekranını zorla açar
    MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767)
end)
