-- [[ LAYROXC HUB v23.0 | APOCALYPSE EDITION - NO SKIPS ]] --
-- [[ DEVELOPED BY LAYROXC - MM2 FINAL BYPASS ]] --

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LAYROXC HUB v23.0 | APOCALYPSE",
   LoadingTitle = "Layroxc Sistemler Ve Bypasslar Hazırlanıyor...",
   LoadingSubtitle = "by dc_Layroxc",
   ConfigurationSaving = { Enabled = true, FolderName = "Layroxc23", FileName = "FinalGodData" }
})

-- [[ BÖLÜM 1: ÇEKİRDEK SERVİSLER VE GLOBAL AYARLAR ]] --
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local TS = game:GetService("TeleportService")
local VU = game:GetService("VirtualUser")
local RS = game:GetService("ReplicatedStorage")

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

-- [[ BÖLÜM 2: ANTI-CHEAT BYPASS & REMOTE PROTECTION ]] --
local function DeepBypass()
    local mt = getrawmetatable(game)
    setreadonly(mt, false)
    local old = mt.__namecall
    mt.__namecall = newcclosure(function(self, ...)
        local method = getnamecallmethod()
        if method == "FireServer" and (self.Name == "ErrorLog" or self.Name == "CheatCheck" or self.Name == "Kick") then
            return nil
        end
        return old(self, ...)
    end)
    setreadonly(mt, true)
end
pcall(DeepBypass)

-- [[ BÖLÜM 3: ROL TESPİT VE LOKASYON MOTORU ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- [[ BÖLÜM 4: UI SEKMELERİ (TÜM FONKSİYONLAR) ]] --

local SocialTab = Window:CreateTab("Socials", 4483362458)
SocialTab:CreateButton({Name = "Roblox: dc_Layroxc", Callback = function() setclipboard("dc_Layroxc") end})
SocialTab:CreateButton({Name = "TikTok: @layroxcderler", Callback = function() setclipboard("layroxcderler") end})
SocialTab:CreateButton({Name = "Instagram: @Layroxc", Callback = function() setclipboard("Layroxc") end})

local CombatTab = Window:CreateTab("Combat", 4483362458)
CombatTab:CreateToggle({Name = "Silent Aim (Lock to Enemies)", CurrentValue = false, Callback = function(v) _G.SilentAim = v end})
CombatTab:CreateToggle({Name = "Kill Aura (Reach 25)", CurrentValue = false, Callback = function(v) _G.KillAura = v end})
CombatTab:CreateButton({Name = "Instant Kill All Players", Callback = function()
    local kn = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife")
    if kn then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LP and v.Character then
                LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                task.wait(0.05); kn.Parent = LP.Character; kn:Activate()
            end
        end
    end
end})

local FarmTab = Window:CreateTab("Farm & Magnet", 4483362458)
FarmTab:CreateToggle({Name = "ULTRA GUN MAGNET (Silah Sana Yapışır)", CurrentValue = false, Callback = function(v) _G.GunMagnet = v end})
FarmTab:CreateToggle({Name = "Auto Coin/Candy Farm", CurrentValue = false, Callback = function(v) _G.Farm = v end})

local VisualsTab = Window:CreateTab("Visuals", 4483362458)
VisualsTab:CreateToggle({Name = "Full ESP (Roles)", CurrentValue = false, Callback = function(v) _G.ESP = v end})
VisualsTab:CreateButton({Name = "Reveal Roles (Chat)", Callback = function()
    for _, v in pairs(Players:GetPlayers()) do
        local r = GetRole(v)
        if r ~= "Innocent" then game.StarterGui:SetCore("ChatMakeSystemMessage", {Text = "[LAYROXC]: " .. v.Name .. " is " .. r, Color = Color3.new(1,0,0)}) end
    end
end})

local TPTab = Window:CreateTab("Teleports", 4483362458)
TPTab:CreateButton({Name = "TP to Murderer (Katil)", Callback = function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character then LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame end
    end
end})
TPTab:CreateButton({Name = "TP to Sheriff (Şerif)", Callback = function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "SHERIFF" and v.Character then LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame end
    end
end})

local MoveTab = Window:CreateTab("Movement", 4483362458)
MoveTab:CreateSlider({Name = "WalkSpeed", Range = {16, 300}, Increment = 1, CurrentValue = 16, Callback = function(v) _G.Speed = v end})
MoveTab:CreateToggle({Name = "NoClip", CurrentValue = false, Callback = function(v) _G.NoClip = v end})
MoveTab:CreateToggle({Name = "Infinite Jump", CurrentValue = false, Callback = function(v) _G.InfJump = v end})

-- [[ BÖLÜM 5: ANA DÖNGÜ VE MOTORLAR (HİÇBİR ŞEY EKSİK DEĞİL) ]] --

-- 1. MAGNET MOTORU (FIXED): Silahı senin karakterine saniyede 100 kez eşitler.
task.spawn(function()
    while task.wait(0.01) do
        if _G.GunMagnet then
            pcall(function()
                local gun = workspace:FindFirstChild("GunDrop") or workspace:FindFirstChild("Gun", true)
                if gun and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                    local p = (gun:IsA("Model") and (gun.PrimaryPart or gun:FindFirstChildWhichIsA("BasePart", true))) or gun
                    p.CFrame = LP.Character.HumanoidRootPart.CFrame
                    p.Velocity = Vector3.new(0,0,0)
                end
            end)
        end
    end
end)

-- 2. SILENT AIM & ESP MOTORU
RunService.RenderStepped:Connect(function()
    pcall(function()
        if _G.SilentAim then
            for _, v in pairs(Players:GetPlayers()) do
                if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("Head") then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.Head.Position)
                end
            end
        end
        if _G.ESP then
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= LP and v.Character then
                    local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    hl.Name = "LayHL"; local r = GetRole(v)
                    hl.FillColor = r == "MURDERER" and Color3.new(1,0,0) or (r == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
                end
            end
        end
    end)
end)

-- 3. SPEED & NOCLIP ENGINE
RunService.Stepped:Connect(function()
    pcall(function()
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            LP.Character.Humanoid.WalkSpeed = _G.Speed
            if _G.NoClip then
                for _, p in pairs(LP.Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end
            end
        end
    end)
end)

-- 4. KILL AURA & AUTO FARM
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if _G.KillAura then
                local kn = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife")
                if kn then
                    for _, p in pairs(Players:GetPlayers()) do
                        if p ~= LP and p.Character and (LP.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            kn.Parent = LP.Character; kn:Activate()
                            firetouchinterest(p.Character.HumanoidRootPart, kn.Handle, 0); firetouchinterest(p.Character.HumanoidRootPart, kn.Handle, 1)
                        end
                    end
                end
            end
            if _G.Farm then
                for _, x in pairs(workspace:GetDescendants()) do
                    if (x.Name == "Coin" or x.Name == "Candy") and x:IsA("BasePart") then
                        LP.Character.HumanoidRootPart.CFrame = x.CFrame; break
                    end
                end
            end
        end)
    end
end)

-- Anti-AFK & Inf Jump
LP.Idled:Connect(function() if _G.AntiAFK then VU:CaptureController(); VU:ClickButton2(Vector2.new()) end end)
UIS.JumpRequest:Connect(function() if _G.InfJump and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid:ChangeState("Jumping") end end)

Rayfield:LoadConfiguration()
