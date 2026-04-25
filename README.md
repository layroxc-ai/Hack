-- [[ LAYROXC HUB v50 - TACTICAL TELEPORT (TOUCH TO TP) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v50", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- [[ MOBİL ANA BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ ROL TESPİT MOTORU ]] --
local function GetKiller()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                return v.Character.HumanoidRootPart
            end
            pcall(function()
                if v.PlayerGui.MainGui.Game.RoleDesc.Text:lower():find("murderer") then
                    return v.Character.HumanoidRootPart
                end
            end)
        end
    end
    return nil
end

-- SEKMELER
local Main = Window:NewTab("Saldırı (Aim/Kill)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Farm & Magnet")
local Pro = Window:NewTab("Avatar & Fix")

-- [[ 1. SALDIRI MOTORU - TUŞLU IŞINLANMA ]] --
local RageSec = Main:NewSection("İnfaz Ayarları")

RageSec:NewButton("Katilin Arkasına Işınlanma Tuşu", "Ekrana Tuş Getirir", function()
    if game.CoreGui:FindFirstChild("TpGui") then game.CoreGui.TpGui:Destroy() end
    
    local TpGui = Instance.new("ScreenGui", game.CoreGui)
    TpGui.Name = "TpGui"
    local TpButton = Instance.new("TextButton", TpGui)
    TpButton.Size = UDim2.new(0, 120, 0, 40)
    TpButton.Position = UDim2.new(0.5, -60, 0.8, 0)
    TpButton.Text = "IŞINLAN (TP)"
    TpButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    TpButton.TextColor3 = Color3.new(1, 1, 1)
    TpButton.Font = Enum.Font.GothamBold
    Instance.new("UICorner", TpButton)
    
    TpButton.MouseButton1Click:Connect(function()
        local target = GetKiller()
        if target then
            LocalPlayer.Character.HumanoidRootPart.CFrame = target.CFrame * CFrame.new(0, 0, 2)
        end
    end)
    
    -- 10 saniye sonra tuş kaybolsun (ekran kirlenmesin)
    task.wait(10)
    if TpGui then TpGui:Destroy() end
end)

_G.Aimbot = false
RageSec:NewToggle("Smart Aimbot", "Katile Kilitlenir", function(state) _G.Aimbot = state end)

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        local target = GetKiller()
        if target then
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
        end
    end
end)

_G.KillAura = false
RageSec:NewToggle("Kill Aura", "Otomatik Keser", function(state)
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

-- [[ 2. NITRO ESP (KÜÇÜK İSİM) ]] --
local EspSec = Visuals:NewSection("İfşa")
_G.MasterESP = false
EspSec:NewToggle("ESP Aktif", "Küçük İsimler", function(state) _G.MasterESP = state end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local color = Color3.fromRGB(0, 255, 0)
                    if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then color = Color3.fromRGB(255, 0, 0) end
                    
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.OutlineColor = color; hl.FillTransparency = 1
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 80, 0, 20); bg.ExtentsOffset = Vector3.new(0, 2.5, 0)
                    local lb = bg:FindFirstChild("TextLabel") or Instance.new("TextLabel", bg)
                    lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1; lb.TextSize = 10; lb.Font = Enum.Font.GothamBold; lb.TextColor3 = color; lb.Text = v.Name
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

-- [[ 4. AVATAR & SATIN ALMA FIX ]] --
local ProSec = Pro:NewSection("Avatar")
ProSec:NewButton("Korblox (80 Robux)", "ID: 1812606767", function()
    MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767)
end)
