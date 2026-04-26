-- [[ LAYROXC HUB v59 - THE ABSOLUTE ENGINE (NO MISSING CODE) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v59 FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- TÜM AYARLAR (Ölünce Sıfırlanmaz)
_G.SpeedValue = 16
_G.JumpValue = 50
_G.SilentAim = false
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
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 15, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ROL TESPİT SİSTEMİ ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- TABS
local Main = Window:NewTab("Combat (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Magnet & Farm")
local Movement = Window:NewTab("Movement")
local Pro = Window:NewTab("Avatar & Pro")

-- [[ 1. COMBAT - SILENT AIM DAHİL ]] --
local RageSec = Main:NewSection("Execution Engine")

RageSec:NewToggle("Silent Aim (Tiktok)", "Nereye sıkarsan sık katili vurur", function(state) _G.SilentAim = state end)
RageSec:NewToggle("Kill Aura (25m)", "Otomatik bıçak sallar", function(state) _G.KillAura = state end)

RageSec:NewButton("TP Behind Murderer", "Katilin arkasına ışınlar", function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character then
            LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end)

RageSec:NewButton("KILL ALL (Murderer)", "Herkesi anında yok eder", function()
    local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if k then
        k.Parent = LocalPlayer.Character
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,1)
                task.wait(0.12); k:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
            end
        end
    end
end)

-- [[ 2. VISUALS - TEMİZ ESP ]] --
local EspSec = Visuals:NewSection("Minimalist Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "İsim ve Rolleri gösterir", function(state) _G.MasterESP = state end)

-- [[ 3. FARM & MAGNET ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN (ULTRA)", "Düşen silahı çeker", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH FARM", "Otomatik coin toplar", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT ]] --
local MoveSec = Movement:NewSection("Physics Control")
MoveSec:NewTextBox("WalkSpeed", "Hız", function(txt) _G.SpeedValue = tonumber(txt) or 16 end)
MoveSec:NewTextBox("JumpPower", "Zıplama", function(txt) _G.JumpValue = tonumber(txt) or 50 end)
MoveSec:NewToggle("NoClip", "Duvarlardan geç", function(state) _G.NoClip = state end)

-- [[ 5. PRO SEKTÖRÜ (GAMEPASS & LINK) ]] --
local ProSec = Pro:NewSection("Support & Korblox")
ProSec:NewButton("Get Korblox (80 Robux)", "Satın al ve linki kopyala", function()
    pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
    if setclipboard then setclipboard(MyGamepassLink) end
end)

-- [[ SİSTEM DÖNGÜLERİ - TÜM ÖZELLİKLER BURADA ]] --

-- Silent Aim (Metamethod Hook)
local OldNamecall
OldNamecall = hookmetamethod(game, "__namecall", function(Self, ...)
    local Args = {...}
    local Method = getnamecallmethod()
    if _G.SilentAim and Method == "FireServer" and Self.Name == "ShootGun" then
        for _, v in pairs(Players:GetPlayers()) do
            if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                Args[2] = v.Character.HumanoidRootPart.Position
                return OldNamecall(Self, unpack(Args))
            end
        end
    end
    return OldNamecall(Self, ...)
end)

-- Fizik ve NoClip Döngüsü
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

-- Farm ve Kill Aura Döngüsü
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if _G.KillAura then
                local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
                if k then
                    for _, v in pairs(Players:GetPlayers()) do
                        if v ~= LocalPlayer and v.Character and (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            k.Parent = LocalPlayer.Character; k:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
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
                    if (v.Name == "Coin" or v.Name == "Candy") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame; task.wait(0.3)
                    end
                end
            end
        end)
    end
end)

-- ESP Rendering (Rahatsız Etmeyen)
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0.6,1) or Color3.new(0,1,0))
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.8
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0,100,0,20); bg.ExtentsOffset = Vector3.new(0,2.5,0)
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg); lb.Name = "TL"
                    lb.Size = UDim2.new(1,0,1,0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.TextSize = 10; lb.Font = Enum.Font.SourceSansBold; lb.Text = "["..role.."] "..v.DisplayName
                end
            end)
        end
    end
end)
