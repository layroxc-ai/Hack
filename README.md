-- [[ LAYROXC HUB v59 - THE ABSOLUTE ENGINE (FULL VERSION) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - ⚠️BETA⚠️", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- GLOBAL DEĞERLER (Ölünce sıfırlanmaz)
_G.SpeedValue = 16
_G.JumpValue = 50
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.GrabGun = false
_G.StealthFarm = false
_G.NoClip = false

-- GAMEPASS VERİSİ
local MyGamepassID = 1812606767
local MyGamepassLink = "https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE"

-- [[ MOBİL AÇ/KAPAT BUTONU ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 55, 0, 55); OpenButton.Position = UDim2.new(0, 20, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(180, 0, 0); OpenButton.TextColor3 = Color3.new(1,1,1)
OpenButton.TextSize = 25; OpenButton.Font = Enum.Font.GothamBold
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ROL TESPİT SİSTEMİ ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- SEKMELER
local Main = Window:NewTab("Combat (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Movement = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT SEKTÖRÜ ]] --
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewButton("TP Behind Murderer", "Katilin arkasına ışınlanır", function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

RageSec:NewButton("Kill All (Murderer Only)", "Bıçak elindeyse herkesi keser", function()
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

RageSec:NewToggle("Smart Kill Aura (25m)", "Yakındakileri otomatik keser", function(state) _G.KillAura = state end)

-- [[ 2. VISUALS SEKTÖRÜ (GELİŞMİŞ ESP) ]] --
local EspSec = Visuals:NewSection("Clean Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "Nickname, Rol ve Mesafe gösterir", function(state) _G.MasterESP = state end)

-- [[ 3. FARM SEKTÖRÜ ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Düşen silahı ışınlar", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Otomatik Coin toplar", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT SEKTÖRÜ ]] --
local MoveSec = Movement:NewSection("Physics Control")
MoveSec:NewTextBox("WalkSpeed", "Hızınızı buraya yazın", function(txt) _G.SpeedValue = tonumber(txt) or 16 end)
MoveSec:NewTextBox("JumpPower", "Zıplama gücünü yazın", function(txt) _G.JumpValue = tonumber(txt) or 50 end)
MoveSec:NewToggle("NoClip", "Duvarlardan geçmenizi sağlar", function(state) _G.NoClip = state end)

-- [[ 5. PRO SEKTÖRÜ ]] --
local ProSec = Pro:NewSection("Support & Korblox")
ProSec:NewButton("Get Korblox (80 Robux)", "Satın al ve linki kopyala", function()
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
    if setclipboard then setclipboard(MyGamepassLink) end
end)

-- [[ ANA DÖNGÜ: Hız, Zıplama ve NoClip (Ölsen bile devam eder) ]] --
RunService.Stepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
            -- Karakter öldüğünde hızın düşmemesi için zorla uygulama
            if LocalPlayer.Character.Humanoid.Health > 0 then
                LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            end
            if _G.NoClip then
                for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then v.CanCollide = false end
                end
            end
        end
    end)
end)

-- [[ OTOMATİK İŞLEMLER (Aura, Magnet, Farm) ]] --
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if _G.KillAura then
                local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
                if k then
                    for _, v in pairs(Players:GetPlayers()) do
                        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                            local dist = (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude
                            if dist < 25 then
                                k.Parent = LocalPlayer.Character; k:Activate()
                                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
                            end
                        end
                    end
                end
            end
            if _G.GrabGun then
                for _, v in pairs(workspace:GetDescendants()) do
                    if v.Name == "GunDrop" and v:IsA("BasePart") then 
                        v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame 
                    end
                end
            end
            if _G.StealthFarm then
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v.Name == "Coin" or v.Name == "Candy") and v:IsA("BasePart") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame; task.wait(0.3)
                    end
                end
            end
        end)
    end
end)

-- [[ TEMİZ ESP DÖNGÜSÜ ]] --
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.fromRGB(255, 50, 50) 
                                 or (role == "SHERIFF" and Color3.fromRGB(50, 150, 255) 
                                 or Color3.fromRGB(255, 255, 255))
                    
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.85; hl.OutlineTransparency = 0.5
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 150, 0, 50); bg.ExtentsOffset = Vector3.new(0, 3.5, 0)
                    
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg)
                    lb.Name = "TL"; lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1
                    lb.TextColor3 = color; lb.TextSize = 13; lb.Font = Enum.Font.GothamBold
                    lb.TextStrokeTransparency = 0.2; lb.TextStrokeColor3 = Color3.new(0, 0, 0)
                    
                    local dist = math.floor((LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude)
                    lb.Text = v.DisplayName .. "\n[" .. role .. "]\n" .. dist .. "m"
                end
            end)
        end
    else
        for _, v in pairs(Players:GetPlayers()) do
            if v.Character then
                if v.Character:FindFirstChild("LayHL") then v.Character.LayHL:Destroy() end
                if v.Character.Head:FindFirstChild("LayName") then v.Character.Head.LayName:Destroy() end
            end
        end
    end
end)
