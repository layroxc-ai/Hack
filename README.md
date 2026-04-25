-- [[ LAYROXC HUB v56 - THE FINAL BEAST (ALL FEATURES RESTORED) ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v56 FINAL", "DarkTheme")

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

-- [[ ÖZEL ROL TESPİT MOTORU ]] --
local function GetPlayerRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Masum" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "KATİL" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "ŞERİF" end
    
    local roleText = ""
    pcall(function()
        if v:FindFirstChild("PlayerGui") and v.PlayerGui:FindFirstChild("MainGui") then
            roleText = v.PlayerGui.MainGui.Game.RoleDesc.Text:lower()
        end
    end)
    
    if roleText:find("murderer") or roleText:find("katil") then return "KATİL" end
    if roleText:find("sheriff") or roleText:find("şerif") or roleText:find("hero") then return "ŞERİF" end
    
    return "Masum"
end

-- SEKMELER
local Main = Window:NewTab("Saldırı (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Farm & Magnet")
local Pro = Window:NewTab("Avatar & Fix")

-- [[ 1. SALDIRI MOTORU (AİMBOT + KILL ALL + TP) ]] --
local RageSec = Main:NewSection("İnfaz ve Aimbot")

_G.Aimbot = false
RageSec:NewToggle("Smart Aimbot", "Katile Otomatik Kilitlenir", function(state) _G.Aimbot = state end)

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if GetPlayerRole(v) == "KATİL" and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
            end
        end
    end
end)

RageSec:NewButton("KILL ALL (KATİLSEN BAS)", "Herkesi Anında Keser", function()
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

_G.KillAura = false
RageSec:NewToggle("Kill Aura", "Yakındakileri Otomatik Keser", function(state)
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

RageSec:NewButton("Katil Işınlanma Tuşunu Sabitle", "Ekrandaki Buton Gitmez", function()
    if game.CoreGui:FindFirstChild("TpGui") then game.CoreGui.TpGui:Destroy() end
    local TpGui = Instance.new("ScreenGui", game.CoreGui)
    TpGui.Name = "TpGui"
    local TpButton = Instance.new("TextButton", TpGui)
    TpButton.Size = UDim2.new(0, 120, 0, 40)
    TpButton.Position = UDim2.new(0.5, -60, 0.8, 0)
    TpButton.Text = "IŞINLAN (TP)"
    TpButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    TpButton.TextColor3 = Color3.new(1, 1, 1)
    TpButton.Font = Enum.Font.GothamBold
    TpButton.Draggable = true
    Instance.new("UICorner", TpButton)
    TpButton.MouseButton1Click:Connect(function()
        for _, v in pairs(Players:GetPlayers()) do
            if GetPlayerRole(v) == "KATİL" and v.Character then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2)
            end
        end
    end)
end)

-- [[ 2. FULL VISUALS (BOX + NAME + ROLE) ]] --
local EspSec = Visuals:NewSection("ESP Ayarları")
_G.MasterESP = false
EspSec:NewToggle("FULL ESP AKTİF", "Kutu ve Küçük İsimler", function(state) _G.MasterESP = state end)

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
                    hl.Name = "LayHL"; hl.FillColor = color; hl.FillTransparency = 0.7; hl.OutlineColor = color
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 80, 0, 20); bg.ExtentsOffset = Vector3.new(0, 3, 0)
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
FarmSec:NewToggle("Magnet Gun", "Düşen Silahı Çeker", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
        end
        task.wait(0.2)
    end
end)

-- [[ 4. AVATAR & SATIN ALMA ]] --
local ProSec = Pro:NewSection("Avatar")
ProSec:NewButton("Korblox (80 Robux)", "ID: 1812606767", function()
    MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767)
end)
