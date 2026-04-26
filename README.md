-- [[ LAYROXC HUB v22.0 | THE ULTIMATE EDITION ]] --
-- [[ TÜM SİSTEMLER VE ULTRA MAGNET DAHİL ]] --

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LAYROXC HUB v22.0 | ULTIMATE",
   LoadingTitle = "Layroxc Sistemler Ve Bypasslar Yükleniyor...",
   LoadingSubtitle = "by dc_Layroxc",
   ConfigurationSaving = { Enabled = true, FolderName = "Layroxc22", FileName = "FullConfig" }
})

-- [[ SERVİSLER ]] --
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local TS = game:GetService("TeleportService")
local VU = game:GetService("VirtualUser")

-- [[ GLOBAL DURUMLAR ]] --
_G.Speed = 16
_G.JumpPower = 50
_G.GrabMagnet = false
_G.KillAura = false
_G.Aimbot = false
_G.ESP = false
_G.Farm = false
_G.NoClip = false
_G.InfJump = false
_G.AntiAFK = true

-- [[ 1. BÖLÜM: EKSİKSİZ BYPASS VE ANTI-AFK ]] --
local function SecurityBypass()
    local mt = getrawmetatable(game)
    setreadonly(mt, false)
    local oldNamecall = mt.__namecall
    mt.__namecall = newcclosure(function(self, ...)
        local method = getnamecallmethod()
        if method == "FireServer" and (self.Name == "ErrorLog" or self.Name == "CheatCheck") then
            return nil
        end
        return oldNamecall(self, ...)
    end)
    setreadonly(mt, true)
end
pcall(SecurityBypass)

if _G.AntiAFK then
    LP.Idled:Connect(function()
        VU:CaptureController()
        VU:ClickButton2(Vector2.new())
    end)
end

-- [[ ROL TESPİT FONKSİYONU ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- [[ SOSYAL HESAPLAR ]] --
local SocialTab = Window:CreateTab("Socials", 4483362458)
SocialTab:CreateSection("Yapımcı Hesapları")
SocialTab:CreateButton({Name = "Roblox: dc_Layroxc", Callback = function() setclipboard("dc_Layroxc") end})
SocialTab:CreateButton({Name = "TikTok: @layroxcderler", Callback = function() setclipboard("layroxcderler") end})
SocialTab:CreateButton({Name = "Instagram: @Layroxc", Callback = function() setclipboard("Layroxc") end})

-- [[ COMBAT SEKME ]] --
local CombatTab = Window:CreateTab("Combat", 4483362458)
CombatTab:CreateToggle({Name = "Aimbot (Lock Killer)", CurrentValue = false, Callback = function(v) _G.Aimbot = v end})
CombatTab:CreateToggle({Name = "Kill Aura (Reach)", CurrentValue = false, Callback = function(v) _G.KillAura = v end})
CombatTab:CreateButton({Name = "Kill All Players", Callback = function()
    local k = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife")
    if k then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LP and v.Character then
                LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                task.wait(0.05); k.Parent = LP.Character; k:Activate()
            end
        end
    end
end})

-- [[ FARM VE ULTRA MAGNET (SİLAH SANA GELİR) ]] --
local FarmTab = Window:CreateTab("Farm & Magnet", 4483362458)
FarmTab:CreateToggle({
    Name = "ULTRA MAGNET (Silah Elime Gelsin)", 
    CurrentValue = false, 
    Callback = function(v) _G.GrabMagnet = v end
})
FarmTab:CreateToggle({Name = "Auto Coin/Candy Farm", CurrentValue = false, Callback = function(v) _G.Farm = v end})

-- [[ VISUALS SEKME ]] --
local VisualsTab = Window:CreateTab("Visuals", 4483362458)
VisualsTab:CreateToggle({Name = "Master ESP", CurrentValue = false, Callback = function(v) _G.ESP = v end})
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

-- [[ MOVEMENT SEKME ]] --
local MoveTab = Window:CreateTab("Movement", 4483362458)
MoveTab:CreateSlider({Name = "Speed", Range = {16, 300}, Increment = 1, CurrentValue = 16, Callback = function(v) _G.Speed = v end})
MoveTab:CreateToggle({Name = "NoClip", CurrentValue = false, Callback = function(v) _G.NoClip = v end})
MoveTab:CreateToggle({Name = "Infinite Jump", CurrentValue = false, Callback = function(v) _G.InfJump = v end})

-- [[ 2. BÖLÜM: ANA MOTORLAR (ARKA PLAN KODLARI) ]] --

-- ULTRA MAGNET MOTORU (FIXED)
task.spawn(function()
    while task.wait(0.01) do
        if _G.GrabMagnet then
            pcall(function()
                local gun = workspace:FindFirstChild("GunDrop") or workspace:FindFirstChild("Gun", true)
                if gun and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                    if gun:IsA("Model") then
                        local p = gun.PrimaryPart or gun:FindFirstChildWhichIsA("BasePart", true)
                        if p then p.CFrame = LP.Character.HumanoidRootPart.CFrame; p.Velocity = Vector3.new(0,0,0) end
                    else
                        gun.CFrame = LP.Character.HumanoidRootPart.CFrame; gun.Velocity = Vector3.new(0,0,0)
                    end
                end
            end)
        end
    end
end)

-- KILL AURA VE ESP MOTORU
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
            if _G.ESP then
                for _, v in pairs(Players:GetPlayers()) do
                    if v ~= LP and v.Character then
                        local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                        hl.Name = "LayHL"
                        local r = GetRole(v)
                        hl.FillColor = r == "MURDERER" and Color3.new(1,0,0) or (r == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
                    end
                end
            end
        end)
    end
end)

-- HIZ VE NOCLIP
RunService.Stepped:Connect(function()
    if LP.Character and LP.Character:FindFirstChild("Humanoid") then
        LP.Character.Humanoid.WalkSpeed = _G.Speed
        if _G.NoClip then
            for _, p in pairs(LP.Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end
        end
    end
end)

-- AUTO FARM
task.spawn(function()
    while task.wait(0.3) do
        if _G.Farm and LP.Character then
            pcall(function()
                for _, x in pairs(workspace:GetDescendants()) do
                    if (x.Name == "Coin" or x.Name == "Candy") and x:IsA("BasePart") then
                        LP.Character.HumanoidRootPart.CFrame = x.CFrame; break
                    end
                end
            end)
        end
    end
end)

-- INF JUMP
UIS.JumpRequest:Connect(function()
    if _G.InfJump and LP.Character:FindFirstChildOfClass("Humanoid") then
        LP.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

Rayfield:LoadConfiguration()
