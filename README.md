-- [[ LAYROXC HUB v53 - DEEP SCAN & PERMANENT TP ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub - v53", "DarkTheme")

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

-- [[ DERİN ROL TESPİT MOTORU ]] --
local function GetPlayerRole(v)
    -- 1. Yöntem: Envanter Kontrolü (Bıçak veya Silah Çektiği An)
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "KATİL" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "ŞERİF" end
    
    -- 2. Yöntem: ReplicatedStorage Üzerinden Rol Taraması (Sayım Esnası)
    pcall(function()
        local RoleData = game:GetService("ReplicatedStorage"):FindFirstChild("RoleData", true)
        if RoleData then
            if RoleData:FindFirstChild(v.Name) and RoleData[v.Name].Value == "Murderer" then return "KATİL" end
            if RoleData:FindFirstChild(v.Name) and RoleData[v.Name].Value == "Sheriff" then return "ŞERİF" end
        end
    end)

    -- 3. Yöntem: GUI Text Taraması
    pcall(function()
        local roleLabel = v.PlayerGui.MainGui.Game.RoleDesc.Text:lower()
        if roleLabel:find("murderer") or roleLabel:find("katil") then return "KATİL" end
        if roleLabel:find("sheriff") or roleLabel:find("şerif") or roleLabel:find("hero") then return "ŞERİF" end
    end)
    
    return "Masum"
end

-- SEKMELER
local Main = Window:NewTab("Saldırı (Rage)")
local Visuals = Window:NewTab("Visuals (ESP)")
local Farm = Window:NewTab("Farm & Magnet")
local Pro = Window:NewTab("Avatar & Fix")

-- [[ 1. SALDIRI MOTORU - SABİT TP BUTONU ]] --
local RageSec = Main:NewSection("Kill & Teleport")

RageSec:NewButton("TP Butonunu Ekrana Sabitle", "Buton Gitmez", function()
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
    TpButton.Draggable = true
    Instance.new("UICorner", TpButton)
    
    TpButton.MouseButton1Click:Connect(function()
        for _, v in pairs(Players:GetPlayers()) do
            if GetPlayerRole(v) == "KATİL" and v.Character then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2)
            end
        end
    end)
    
    local CloseX = Instance.new("TextButton", TpButton)
    CloseX.Size = UDim2.new(0, 20, 0, 20)
    CloseX.Position = UDim2.new(1, -20, 0, 0)
    CloseX.Text = "X"
    CloseX.BackgroundColor3 = Color3.new(0,0,0)
    CloseX.TextColor3 = Color3.new(1,1,1)
    CloseX.MouseButton1Click:Connect(function() TpGui:Destroy() end)
end)

_G.Aimbot = false
RageSec:NewToggle("Smart Aimbot", "Katile Odaklanır", function(state) _G.Aimbot = state end)

_G.KillAura = false
RageSec:NewToggle("Kill Aura", "Otomatik Kesme", function(state)
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

-- [[ 2. FULL ESP (BOX + NAME + SKELETON) ]] --
local EspSec = Visuals:NewSection("Anlık İfşa")
_G.MasterESP = false
EspSec:NewToggle("FULL ESP AKTİF", "Kutu ve İsimler", function(state) _G.MasterESP = state end)

RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetPlayerRole(v)
                    local color = Color3.fromRGB(0, 255, 0) -- Masum
                    if role == "KATİL" then color = Color3.fromRGB(255, 0, 0)
                    elseif role == "ŞERİF" then color = Color3.fromRGB(0, 150, 255) end
                    
                    -- Box / Highlight
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"
                    hl.FillColor = color
                    hl.FillTransparency = 0.7
                    hl.OutlineColor = color
                    hl.OutlineTransparency = 0
                    
                    -- Küçük İsim & Rol
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0, 100, 0, 25); bg.ExtentsOffset = Vector3.new(0, 3, 0)
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
FarmSec:NewToggle("Magnet Grab Gun", "Silah Çeker", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
        end
        task.wait(0.2)
    end
end)

-- [[ 4. AVATAR ]] --
local ProSec = Pro:NewSection("Avatar")
ProSec:NewButton("Korblox (80 Robux)", "ID: 1812606767", function()
    MarketplaceService:PromptGamePassPurchase(LocalPlayer, 1812606767)
end)
