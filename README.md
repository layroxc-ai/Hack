-- [[ LAYROXC HUB - dc_Layroxc (FULL & STABLE MM2) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("dc_Layroxc", "DarkTheme")

-- [[ MOBİL MERKEZLEME AYARI ]] --
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

-- GLOBAL DEĞİŞKENLER
_G.SpeedValue = 16
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.GrabGun = false
_G.StealthFarm = false
_G.NoClip = false
_G.AntiFling = false

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
local Teleport = Window:NewTab("Teleport & UI")

-- [[ 1. COMBAT & INVISIBLE ]] --
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewToggle("Ultra Smart Aimbot", "Katile kilitlenir", function(state) _G.Aimbot = state end)
RageSec:NewButton("KILL ALL", "Herkesi anında keser", function()
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
RageSec:NewButton("Invisible", "Görünmez yapar", function()
    local char = LocalPlayer.Character
    if char then
        for _, v in pairs(char:GetDescendants()) do
            if v:IsA("BasePart") or v:IsA("Decal") then v.Transparency = 1 end
        end
    end
end)

-- [[ 2. VISUALS (ESP) ]] --
local EspSec = Visuals:NewSection("Clean Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "Yeşil:Masum, Kırmızı:Katil, Mavi:Sheriff", function(state) _G.MasterESP = state end)

-- [[ 3. MAGNET & FARM (Yavaşlatılmış) ]] --
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN", "Düşen silahı ışınlar", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("STEALTH COIN FARM", "Güvenli ve yavaş toplama", function(state) _G.StealthFarm = state end)

-- [[ 4. MOVEMENT & ANTI-FLING ]] --
local MoveSec = Movement:NewSection("Physics & Protection")
MoveSec:NewTextBox("WalkSpeed", "Hızınızı yazın", function(t) _G.SpeedValue = tonumber(t) or 16 end)
MoveSec:NewToggle("NoClip", "Duvarlardan Geç", function(state) _G.NoClip = state end)
MoveSec:NewToggle("Anti-Fling", "Fırlatılmayı engeller", function(state) _G.AntiFling = state end)

-- [[ 5. TELEPORT UI ]] --
local TPSec = Teleport:NewSection("Teleport Controls")

local TPGui = Instance.new("ScreenGui", game.CoreGui)
local TPFrame = Instance.new("Frame", TPGui)
TPFrame.Size = UDim2.new(0, 130, 0, 160)
TPFrame.Position = UDim2.new(1, -140, 0.5, -80)
TPFrame.Visible = false
TPFrame.BackgroundColor3 = Color3.new(0,0,0)
TPFrame.BackgroundTransparency = 0.4
Instance.new("UICorner", TPFrame)

local function CreateTPBtn(name, pos_y, func)
    local btn = Instance.new("TextButton", TPFrame)
    btn.Size = UDim2.new(0.9, 0, 0, 30); btn.Position = UDim2.new(0.05, 0, 0, pos_y)
    btn.Text = name; btn.BackgroundColor3 = Color3.fromRGB(180, 0, 0); btn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(func)
end

CreateTPBtn("TP Murderer", 10, function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character then LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,3) end
    end
end)
CreateTPBtn("TP Sheriff", 50, function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "SHERIFF" and v.Character then LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,3) end
    end
end)
CreateTPBtn("TP Lobby", 90, function() LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-108, 138, 15) end)

TPSec:NewButton("Toggle TP UI", "Paneli aç/kapat", function() TPFrame.Visible = not TPFrame.Visible end)

-- [[ MOBİL BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 20, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(180, 0, 0); OpenButton.TextColor3 = Color3.new(1,1,1)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ DÖNGÜLER ]] --
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
                    local color = role == "MURDERER" and Color3.new(1, 0, 0) or (role == "SHERIFF" and Color3.new(0, 0, 1) or Color3.new(0, 1, 0))
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.7
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 120, 0, 40); bg.ExtentsOffset = Vector3.new(0, 3, 0)
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg)
                    lb.Name = "TL"; lb.Size = UDim2.new(1, 0, 1, 0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.TextSize = 14; lb.Font = Enum.Font.GothamBold
                    local dist = math.floor((LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude)
                    lb.Text = v.DisplayName .. "\n[" .. role .. "] " .. dist .. "m"
                end
            end)
        end
    end
end)

RunService.Stepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") then
                    if _G.NoClip then v.CanCollide = false
                    elseif _G.AntiFling then v.CanTouch = false end
                end
            end
        end
    end)
end)

task.spawn(function()
    while task.wait(0.7) do
        pcall(function()
            if _G.GrabGun then
                for _, v in pairs(workspace:GetDescendants()) do
                    if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
                end
            end
            if _G.StealthFarm then
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v.Name == "Coin" or v.Name == "Candy") and v:IsA("BasePart") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                        task.wait(1.2)
                    end
                end
            end
        end)
    end
end)
