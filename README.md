-- [[ LAYROXC HUB v60 - THE ULTIMATE ENGINE (FULL & CENTERED) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()

-- MENÜYÜ OLUŞTUR
local Window = Library.CreateLib("Layroxc Hub - dc_Layroxc", "DarkTheme")

-- [[ MOBİL İÇİN MENÜYÜ TAM ORTAYA SABİTLEME (ZORLA) ]] --
local CoreGui = game:GetService("CoreGui")
task.spawn(function()
    local LibraryGui = CoreGui:WaitForChild("Library", 10)
    if LibraryGui then
        local MainFrame = LibraryGui:FindFirstChild("Main")
        if MainFrame then
            MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
            MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
        end
    end
end)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- GLOBAL AYARLAR (Ölünce karakter yenilense bile bu değerler silinmez)
_G.SpeedValue = 16
_G.JumpValue = 50
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.GrabGun = false
_G.StealthFarm = false
_G.NoClip = false

local MyGamepassID = 1812606767
local MyGamepassLink = "https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE"

-- [[ MOBİL AÇ/KAPAT BUTONU ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 20, 0.5, -25)
OpenButton.Text = "L"
OpenButton.Draggable = true
Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
OpenButton.TextColor3 = Color3.new(1,1,1)
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
local Avatar = Window:NewTab("Avatar & Fix")

-- [[ 1. COMBAT SEKTÖRÜ ]] --
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewToggle("Ultra Smart Aimbot", "Katile kilitlenir", function(state) _G.Aimbot = state end)
RageSec:NewButton("KILL ALL (CLEANUP)", "Herkesi anında keser", function()
    local knife = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if knife then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                knife.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 1)
                task.wait(0.1); knife:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, knife.Handle, 1)
            end
        end
    end
end)
RageSec:NewToggle("Kill Aura (25m)", "Otomatik keser", function(state) _G.KillAura = state end)
RageSec:NewButton("TP Behind Murderer", "Katilin arkasına ışınlan", function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

-- [[ 2. VISUALS (ESP - ÖZEL RENKLER) ]] --
local EspSec = Visuals:NewSection("Clean Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "Yeşil:Masum, Kırmızı:Katil, Mavi:Sheriff", function(state) _G.MasterESP = state end)

-- [[ 3. MAGNET & FARM ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN", "Düşen silahı ışınlar", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Otomatik Coin/Candy toplar", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT ]] --
local MoveSec = Movement:NewSection("Physics Control")
MoveSec:NewTextBox("WalkSpeed", "Hızınızı buraya yazın", function(t) _G.SpeedValue = tonumber(t) or 16 end)
MoveSec:NewTextBox("JumpPower", "Zıplama gücünü yazın", function(t) _G.JumpValue = tonumber(t) or 50 end)
MoveSec:NewToggle("NoClip", "Duvarlardan geç", function(state) _G.NoClip = state end)

-- [[ 5. AVATAR & FIX ]] --
local AvaSec = Avatar:NewSection("Support & Korblox")
AvaSec:NewButton("Get Korblox (80 Robux)", "Al ve linki kopyala", function()
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
    if setclipboard then setclipboard(MyGamepassLink) end
    local Hint = Instance.new("Hint", game.CoreGui)
    Hint.Text = "PLEASE OPEN THE COPIED LINK IN YOUR BROWSER TO GET KORBLOX"
    task.wait(4); Hint:Destroy()
end)

-- [[ ANA RENDER DÖNGÜSÜ (Aimbot & ESP) ]] --
RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("Head") then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.Head.Position)
            end
        end
    end

    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1, 0, 0) 
                                 or (role == "SHERIFF" and Color3.new(0, 0, 1) 
                                 or Color3.new(0, 1, 0))
                    
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.7; hl.OutlineTransparency = 0.3
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 120, 0, 40); bg.ExtentsOffset = Vector3.new(0, 3, 0)
                    
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg)
                    lb.Name = "TL"; lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.TextSize = 14; lb.Font = Enum.Font.GothamBold; lb.TextStrokeTransparency = 0
                    
                    local dist = math.floor((LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude)
                    lb.Text = v.DisplayName .. "\n[" .. role .. "] " .. dist .. "m"
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

-- [[ FİZİKSEL DÖNGÜ (Ölünce karakter hızını korur) ]] --
RunService.Stepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
            if _G.NoClip then
                for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then v.CanCollide = false end
                end
            end
        end
    end)
end)

-- [[ ARKA PLAN OTOMASYONLARI ]] --
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if _G.KillAura then
                local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
                if k then
                    for _, v in pairs(Players:GetPlayers()) do
                        if v ~= LocalPlayer and v.Character and (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            k.Parent = LocalPlayer.Character; k:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0); firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
                        end
                    end
                end
            end
            if _G.GrabGun then
                for _, v in pairs(workspace:GetDescendants()) do
                    if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
                end
            end
            if _G.StealthFarm then
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v.Name == "Coin" or v.Name == "Candy") and v:IsA("BasePart") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame; task.wait(0.2)
                    end
                end
            end
        end)
    end
end)
