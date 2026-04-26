-- [[ LAYROXC HUB - dc_Layroxc (ULTIMATE MOBILE REBUILD) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc HUB●dc_Layroxc", "DarkTheme")

-- [[ YENİ NESİL MOBİL SÜRÜKLEME VE MERKEZLEME ]] --
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

task.spawn(function()
    local MainFrame = CoreGui:WaitForChild("Library", 15):WaitForChild("Main")
    MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
    MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)

    local dragging, dragInput, dragStart, startPos

    local function update(input)
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end

    MainFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = MainFrame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    MainFrame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)
end)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- AYARLAR
_G.SpeedValue = 16
_G.JumpValue = 50
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.GrabGun = false
_G.StealthFarm = false
_G.NoClip = false
_G.AntiFling = false

-- ROL BULUCU
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
local Movement = Window:NewTab("Movement & Prot")
local Teleport = Window:NewTab("Teleport & UI")
local Avatar = Window:NewTab("Avatar & Support")

-- COMBAT
local RageSec = Main:NewSection("Execution Engine")
RageSec:NewToggle("Ultra Smart Aimbot", "Katile kilitlenir", function(state) _G.Aimbot = state end)
RageSec:NewToggle("Kill Aura (25m)", "Otomatik bıçak", function(state) _G.KillAura = state end)
RageSec:NewButton("KILL ALL", "Anında temizler", function()
    local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if k then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                k.Parent = LocalPlayer.Character
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 1)
                task.wait(0.1); k:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0); firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
            end
        end
    end
end)
RageSec:NewButton("Invisible", "Karakteri gizler", function()
    local char = LocalPlayer.Character
    if char then for _, v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.Transparency = 1 end end end
end)

-- ESP
local EspSec = Visuals:NewSection("Clean Vision")
EspSec:NewToggle("MASTER ESP ACTIVE", "Mesafe ve Renkli ESP", function(state) _G.MasterESP = state end)

-- FARM (TAMİR EDİLDİ)
local FarmSec = Farm:NewSection("Item Collection")
FarmSec:NewToggle("MAGNET GUN", "Düşen silahı çeker", function(state) _G.GrabGun = state end)
FarmSec:NewToggle("FIXED COIN FARM", "MM2 Coin & Candy Farm", function(state) _G.StealthFarm = state end)

-- MOVEMENT
local MoveSec = Movement:NewSection("Physics")
MoveSec:NewTextBox("WalkSpeed", "Hız", function(t) _G.SpeedValue = tonumber(t) or 16 end)
MoveSec:NewTextBox("JumpPower", "Zıplama", function(t) _G.JumpValue = tonumber(t) or 50 end)
MoveSec:NewToggle("NoClip", "Duvar Geçişi", function(state) _G.NoClip = state end)
MoveSec:NewToggle("Anti-Fling", "Fırlatılma Engeli", function(state) _G.AntiFling = state end)

-- TELEPORT UI
local TPSec = Teleport:NewSection("Controls")
local TPGui = Instance.new("ScreenGui", game.CoreGui)
local TPFrame = Instance.new("Frame", TPGui)
TPFrame.Size = UDim2.new(0, 130, 0, 160); TPFrame.Position = UDim2.new(1, -140, 0.5, -80)
TPFrame.Visible = false; TPFrame.BackgroundColor3 = Color3.new(0,0,0); TPFrame.BackgroundTransparency = 0.4
Instance.new("UICorner", TPFrame)

local function CreateTPBtn(name, pos_y, func)
    local btn = Instance.new("TextButton", TPFrame)
    btn.Size = UDim2.new(0.9, 0, 0, 30); btn.Position = UDim2.new(0.05, 0, 0, pos_y)
    btn.Text = name; btn.BackgroundColor3 = Color3.fromRGB(180, 0, 0); btn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", btn); btn.MouseButton1Click:Connect(func)
end

CreateTPBtn("TP Murderer", 10, function()
    for _, v in pairs(Players:GetPlayers()) do if GetRole(v) == "MURDERER" and v.Character then LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,3) end end
end)
CreateTPBtn("TP Sheriff", 50, function()
    for _, v in pairs(Players:GetPlayers()) do if GetRole(v) == "SHERIFF" and v.Character then LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,3) end end
end)
CreateTPBtn("TP Lobby", 90, function() LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-108, 138, 15) end)

TPSec:NewButton("Toggle TP UI", "TP Panelini açar", function() TPFrame.Visible = not TPFrame.Visible end)

-- KORBLOX & LINK
local AvaSec = Avatar:NewSection("Support")
AvaSec:NewButton("Get Korblox (80 Robux)", "Kopyala & Uyarı Ver", function()
    pcall(function() game:GetService("MarketplaceService"):PromptGamePassPurchase(LocalPlayer, 1812606767) end)
    setclipboard("https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE")
    local h = Instance.new("Hint", game.CoreGui)
    h.Text = "PLEASE RUN THE COPIED LINK IN YOUR BROWSER TO GET KORBLOX"
    task.wait(5); h:Destroy()
end)

-- MOBİL BUTON
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50); OpenButton.Position = UDim2.new(0, 20, 0.5, -25)
OpenButton.Text = "L"; OpenButton.Draggable = true; Instance.new("UICorner", OpenButton)
OpenButton.BackgroundColor3 = Color3.fromRGB(180, 0, 0); OpenButton.TextColor3 = Color3.new(1,1,1)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- ANA DÖNGÜ SİSTEMİ
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
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.5
                end
            end)
        end
    end
end)

RunService.Stepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
            LocalPlayer.Character.Humanoid.UseJumpPower = true
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") then
                    if _G.NoClip then v.CanCollide = false
                    elseif _G.AntiFling then v.CanTouch = false end
                end
            end
        end
    end)
end)

-- OTOMATİK İŞLEMLER
task.spawn(function()
    while task.wait(0.5) do
        pcall(function()
            if _G.GrabGun then
                for _, v in pairs(workspace:GetDescendants()) do
                    if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
                end
            end
            if _G.StealthFarm then
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v.Name == "Coin" or v.Name == "Candy" or v.Parent.Name == "CoinContainer") and v:IsA("BasePart") then
                        if _G.StealthFarm then
                            LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                            task.wait(1.3)
                        end
                    end
                end
            end
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
        end)
    end
end)
