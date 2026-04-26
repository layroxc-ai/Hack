-- [[ LAYROXC HUB v25.0 | THE FINAL JUDGMENT - FULL 300+ LINES ]] --
-- [[ WARNING: HEAVY SCRIPT - BYPASSES ACTIVE ]] --

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LAYROXC HUB v25.0 | THE JUDGMENT",
   LoadingTitle = "Layroxc Deep-Bypass & God-Mode Hazırlanıyor...",
   LoadingSubtitle = "by dc_Layroxc",
   ConfigurationSaving = { Enabled = true, FolderName = "LayroxcFinal", FileName = "GodModeConfig" }
})

-- [[ BÖLÜM 1: SİSTEM SERVİSLERİ VE GLOBAL DEĞİŞKENLER ]] --
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local TS = game:GetService("TeleportService")
local RS = game:GetService("ReplicatedStorage")
local VU = game:GetService("VirtualUser")

_G.Speed = 16
_G.Jump = 50
_G.GunMagnet = false
_G.KillAura = false
_G.SilentAim = false
_G.ESP = false
_G.NoClip = false
_G.InfJump = false
_G.AntiAFK = true
_G.AutoGetGun = true

-- [[ BÖLÜM 2: DERİN BYPASS VE REMOTE KORUMASI (EKSİKSİZ) ]] --
local function InitializeBypass()
    local mt = getrawmetatable(game)
    setreadonly(mt, false)
    local oldNamecall = mt.__namecall
    
    mt.__namecall = newcclosure(function(self, ...)
        local method = getnamecallmethod()
        local args = {...}
        
        -- MM2'nin güvenlik eventlerini tamamen susturur
        if method == "FireServer" and (self.Name == "ErrorLog" or self.Name == "CheatCheck" or self.Name == "Kick") then
            return nil
        end
        return oldNamecall(self, ...)
    end)
    setreadonly(mt, true)
    
    -- Anti-AFK Motoru
    LP.Idled:Connect(function()
        if _G.AntiAFK then
            VU:CaptureController()
            VU:ClickButton2(Vector2.new())
        end
    end)
end
pcall(InitializeBypass)

-- [[ BÖLÜM 3: ROL TESPİT ALGORİTMASI ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then
        return "MURDERER"
    elseif v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then
        return "SHERIFF"
    end
    return "Innocent"
end

-- [[ BÖLÜM 4: UI SEKMELERİ VE TÜM HİLELER ]] --

local SocialTab = Window:CreateTab("Socials", 4483362458)
SocialTab:CreateButton({Name = "Roblox: dc_Layroxc", Callback = function() setclipboard("dc_Layroxc") end})
SocialTab:CreateButton({Name = "TikTok: @layroxcderler", Callback = function() setclipboard("layroxcderler") end})

local MainTab = Window:CreateTab("Combat & Aim", 4483362458)
MainTab:CreateToggle({Name = "Silent Aimbot (Lock on Killer)", CurrentValue = false, Callback = function(v) _G.SilentAim = v end})
MainTab:CreateToggle({Name = "Kill Aura (Legit 25 Reach)", CurrentValue = false, Callback = function(v) _G.KillAura = v end})
MainTab:CreateButton({
    Name = "Kill All (Anında Herkese Işınlan)",
    Callback = function()
        local kn = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife")
        if kn then
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= LP and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                    LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                    task.wait(0.05); kn.Parent = LP.Character; kn:Activate()
                end
            end
        end
    end
})

local MagnetTab = Window:CreateTab("Gun Magnet", 4483362458)
MagnetTab:CreateSection("Silah Mıknatısı")
MagnetTab:CreateToggle({
    Name = "SİLAH MIKNATISI (Silah Ayağına Gelsin)",
    CurrentValue = false,
    Callback = function(v) 
        _G.GunMagnet = v 
        if v then Rayfield:Notify({Title = "ULTRA MAGNET", Content = "Silah artık sana ışınlanacak!", Duration = 3}) end
    end
})

local TPTab = Window:CreateTab("Teleports", 4483362458)
TPTab:CreateButton({Name = "Katile Işınlan (Arkasında Bit)", Callback = function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "MURDERER" and v.Character then
            LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
        end
    end
end})
TPTab:CreateButton({Name = "Şerife Işınlan", Callback = function()
    for _, v in pairs(Players:GetPlayers()) do
        if GetRole(v) == "SHERIFF" and v.Character then
            LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
        end
    end
end})

local VisualsTab = Window:CreateTab("Visuals", 4483362458)
VisualsTab:CreateToggle({Name = "Full ESP (Highlight)", CurrentValue = false, Callback = function(v) _G.ESP = v end})
VisualsTab:CreateButton({Name = "Rolleri Chat'te Göster", Callback = function()
    for _, v in pairs(Players:GetPlayers()) do
        local r = GetRole(v)
        if r ~= "Innocent" then 
            game.StarterGui:SetCore("ChatMakeSystemMessage", {Text = "[LAYROXC]: " .. v.Name .. " is " .. r, Color = Color3.new(1,0,0)})
        end
    end
end})

local MoveTab = Window:CreateTab("Movement", 4483362458)
MoveTab:CreateSlider({Name = "Hız (WalkSpeed)", Range = {16, 250}, Increment = 1, CurrentValue = 16, Callback = function(v) _G.Speed = v end})
MoveTab:CreateToggle({Name = "NoClip (Duvarlardan Geç)", CurrentValue = false, Callback = function(v) _G.NoClip = v end})
MoveTab:CreateToggle({Name = "Infinite Jump", CurrentValue = false, Callback = function(v) _G.InfJump = v end})

-- [[ BÖLÜM 5: ANA MOTORLAR (ARKA PLAN - ASIL GÜÇ BURADA) ]] --

-- 1. SİLAH MIKNATISI MOTORU: Silahı sana getiren asıl kod
task.spawn(function()
    while task.wait(0.01) do -- 0.01sn ile her an tarar
        if _G.GunMagnet and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
            pcall(function()
                local drop = workspace:FindFirstChild("GunDrop") or workspace:FindFirstChild("Gun", true)
                if drop then
                    local target = (drop:IsA("Model") and (drop.PrimaryPart or drop:FindFirstChildWhichIsA("BasePart", true))) or drop
                    if target then
                        -- Silahı haritadan alıp senin üstüne çakar
                        target.CFrame = LP.Character.HumanoidRootPart.CFrame
                        target.Velocity = Vector3.new(0,0,0)
                        target.RotVelocity = Vector3.new(0,0,0)
                    end
                end
            end)
        end
    end
end)

-- 2. AIMBOT VE ESP MOTORU
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
                    local h = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                    h.Name = "LayHL"
                    local r = GetRole(v)
                    h.FillColor = r == "MURDERER" and Color3.new(1,0,0) or (r == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
                end
            end
        end
    end)
end)

-- 3. HIZ, NOCLIP VE JUMP MOTORU
RunService.Stepped:Connect(function()
    if LP.Character and LP.Character:FindFirstChild("Humanoid") then
        LP.Character.Humanoid.WalkSpeed = _G.Speed
        if _G.NoClip then
            for _, p in pairs(LP.Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end
        end
    end
end)

UIS.JumpRequest:Connect(function()
    if _G.InfJump and LP.Character:FindFirstChildOfClass("Humanoid") then
        LP.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

-- 4. KILL AURA MOTORU
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

Rayfield:LoadConfiguration()
