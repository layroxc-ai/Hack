-- [[ LAYROXC HUB v20.0 | OMNIPOTENT EDITION - THE 300+ LINE SCRIPT ]] --
-- [[ CREATED BY LAYROXC - MM2 ULTIMATE BYPASS ]] --

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LAYROXC HUB v20.0 | OMNIPOTENT",
   LoadingTitle = "Bypass Motorları ve Sistemler Yükleniyor...",
   LoadingSubtitle = "by dc_Layroxc",
   ConfigurationSaving = { Enabled = true, FolderName = "Layroxc20", FileName = "MasterData" }
})

-- [[ 1. BÖLÜM: CORE SERVİSLER VE DEĞİŞKENLER ]] --
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local TS = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local RS = game:GetService("ReplicatedStorage")
local VU = game:GetService("VirtualUser")

_G.Speed = 16
_G.JumpPower = 50
_G.GunMagnet = false
_G.KillAura = false
_G.SilentAim = false
_G.ESP = false
_G.Farm = false
_G.NoClip = false
_G.InfJump = false
_G.AntiAFK = true
_G.Invisible = false
_G.AutoRejoin = true

-- [[ 2. BÖLÜM: HARDCORE BYPASS & METATABLE HOOKS ]] --
local function InitBypass()
    local g = getrawmetatable(game)
    setreadonly(g, false)
    local old = g.__namecall
    g.__namecall = newcclosure(function(self, ...)
        local m = getnamecallmethod()
        if m == "FireServer" then
            if self.Name == "ErrorLog" or self.Name == "CheatCheck" or self.Name == "Kick" then
                return nil
            end
        end
        return old(self, ...)
    end)
    setreadonly(g, true)
    
    -- Anti-AFK Aktifleştirme
    LP.Idled:Connect(function()
        if _G.AntiAFK then
            VU:CaptureController()
            VU:ClickButton2(Vector2.new())
        end
    end)
end
pcall(InitBypass)

-- [[ 3. BÖLÜM: FONKSİYONLAR ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

local function ServerHop()
    local x = HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100"))
    for _, s in pairs(x.data) do
        if s.playing < s.maxPlayers then
            TS:TeleportToPlaceInstance(game.PlaceId, s.id)
        end
    end
end

-- [[ 4. BÖLÜM: UI SEKMELERİ ]] --
local SocialTab = Window:CreateTab("Socials", 4483362458)
SocialTab:CreateSection("Yapımcı")
SocialTab:CreateButton({Name = "Roblox: dc_Layroxc", Callback = function() setclipboard("dc_Layroxc") end})
SocialTab:CreateButton({Name = "TikTok: @layroxcderler", Callback = function() setclipboard("layroxcderler") end})
SocialTab:CreateButton({Name = "Instagram: @Layroxc", Callback = function() setclipboard("Layroxc") end})

local CombatTab = Window:CreateTab("Combat", 4483362458)
CombatTab:CreateToggle({Name = "Silent Aimbot (Face Killer)", CurrentValue = false, Callback = function(v) _G.SilentAim = v end})
CombatTab:CreateToggle({Name = "Kill Aura (Legit-Reach)", CurrentValue = false, Callback = function(v) _G.KillAura = v end})
CombatTab:CreateButton({Name = "Instant Kill All (Danger)", Callback = function()
    local k = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife")
    if k then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LP and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                task.wait(0.05)
                k.Parent = LP.Character; k:Activate()
            end
        end
    end
end})

local FarmTab = Window:CreateTab("Farm & Magnet", 4483362458)
FarmTab:CreateToggle({Name = "GUN MAGNET (Silah Sana Gelsin)", CurrentValue = false, Callback = function(v) _G.GunMagnet = v end})
FarmTab:CreateToggle({Name = "Auto Coin/Candy Farm", CurrentValue = false, Callback = function(v) _G.Farm = v end})

local VisualsTab = Window:CreateTab("Visuals", 4483362458)
VisualsTab:CreateToggle({Name = "Highlight ESP", CurrentValue = false, Callback = function(v) _G.ESP = v end})
VisualsTab:CreateButton({Name = "Reveal Roles (Chat)", Callback = function()
    for _, v in pairs(Players:GetPlayers()) do
        local r = GetRole(v)
        if r ~= "Innocent" then
            game.StarterGui:SetCore("ChatMakeSystemMessage", {Text = "[SYSTEM]: " .. v.Name .. " is " .. r, Color = Color3.new(1,0,0)})
        end
    end
end})
VisualsTab:CreateButton({
   Name = "KORBLOX SATIN AL",
   Callback = function()
      setclipboard("https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE")
      Rayfield:Notify({Title = "LINK COPIED!", Content = "OPEN IN BROWSER", Duration = 5})
   end,
})

local MoveTab = Window:CreateTab("Movement", 4483362458)
MoveTab:CreateSlider({Name = "WalkSpeed", Range = {16, 300}, Increment = 1, CurrentValue = 16, Callback = function(v) _G.Speed = v end})
MoveTab:CreateSlider({Name = "JumpPower", Range = {50, 300}, Increment = 1, CurrentValue = 50, Callback = function(v) _G.JumpPower = v end})
MoveTab:CreateToggle({Name = "NoClip", CurrentValue = false, Callback = function(v) _G.NoClip = v end})
MoveTab:CreateToggle({Name = "Infinite Jump", CurrentValue = false, Callback = function(v) _G.InfJump = v end})

local ServerTab = Window:CreateTab("Server", 4483362458)
ServerTab:CreateButton({Name = "Server Hop", Callback = ServerHop})
ServerTab:CreateButton({Name = "Rejoin Server", Callback = function() TS:Teleport(game.PlaceId, LP) end})

-- [[ 5. BÖLÜM: BACKEND ENGINE (ANA MOTORLAR) ]] --

-- Magnet & Farm Motoru
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if _G.GunMagnet and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                local gun = workspace:FindFirstChild("GunDrop") or workspace:FindFirstChild("Gun", true)
                if gun then
                    if gun:IsA("Model") then
                        gun:SetPrimaryPartCFrame(LP.Character.HumanoidRootPart.CFrame)
                    else
                        gun.CFrame = LP.Character.HumanoidRootPart.CFrame
                    end
                end
            end
            
            if _G.Farm and LP.Character then
                for _, x in pairs(workspace:GetDescendants()) do
                    if (x.Name == "Coin" or x.Name == "Candy") and x:IsA("BasePart") then
                        LP.Character.HumanoidRootPart.CFrame = x.CFrame
                        task.wait(0.3)
                    end
                end
            end
        end)
    end
end)

-- Kill Aura, ESP & Silent Aim Motoru
RunService.RenderStepped:Connect(function()
    pcall(function()
        if _G.ESP then
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= LP and v.Character then
                    local h = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    h.Name = "LayHL"
                    local r = GetRole(v)
                    h.FillColor = r == "MURDERER" and Color3.new(1,0,0) or (r == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
                end
            end
        end
        
        if _G.SilentAim then
            for _, v in pairs(Players:GetPlayers()) do
                if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("Head") then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.Head.Position)
                end
            end
        end
    end)
end)

-- Stepped Motoru (Hız & NoClip)
RunService.Stepped:Connect(function()
    pcall(function()
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            LP.Character.Humanoid.WalkSpeed = _G.Speed
            LP.Character.Humanoid.JumpPower = _G.JumpPower
            if _G.NoClip then
                for _, p in pairs(LP.Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end
            end
        end
    end)
end)

-- Kill Aura Backend
task.spawn(function()
    while task.wait(0.1) do
        if _G.KillAura then
            pcall(function()
                local kn = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife")
                if kn then
                    for _, p in pairs(Players:GetPlayers()) do
                        if p ~= LP and p.Character and (LP.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            kn.Parent = LP.Character; kn:Activate()
                            firetouchinterest(p.Character.HumanoidRootPart, kn.Handle, 0)
                            firetouchinterest(p.Character.HumanoidRootPart, kn.Handle, 1)
                        end
                    end
                end
            end)
        end
    end
end)

-- Inf Jump Listener
UIS.JumpRequest:Connect(function()
    if _G.InfJump and LP.Character:FindFirstChildOfClass("Humanoid") then
        LP.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

Rayfield:LoadConfiguration()
