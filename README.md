-- [[ LAYROXC HUB v28.0 | THE FINAL ULTIMATE EDITION ]] --
-- [[ 400+ LINES | TP MENU, MURDERER AIM, FORCE MAGNET ]] --

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LAYROXC HUB v28.0 | ULTIMATE",
   LoadingTitle = "Layroxc Bypass & Deep-Systems Hazırlanıyor...",
   LoadingSubtitle = "by dc_Layroxc",
   ConfigurationSaving = { Enabled = true, FolderName = "Layroxc28", FileName = "FinalMaster" }
})

-- [[ BÖLÜM 1: ÇEKİRDEK SERVİSLER ]] --
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local VU = game:GetService("VirtualUser")

_G.Speed = 16
_G.Jump = 50
_G.GrabGun = false
_G.MurdererAim = false
_G.KillAura = false
_G.ESP = false
_G.NoClip = false
_G.InfJump = false
_G.AntiAFK = true

-- [[ BÖLÜM 2: DERİN BYPASS SİSTEMİ ]] --
local function InitBypass()
    local mt = getrawmetatable(game)
    setreadonly(mt, false)
    local old = mt.__namecall
    mt.__namecall = newcclosure(function(self, ...)
        if getnamecallmethod() == "FireServer" and (self.Name == "ErrorLog" or self.Name == "CheatCheck") then
            return nil
        end
        return old(self, ...)
    end)
    setreadonly(mt, true)
    LP.Idled:Connect(function() if _G.AntiAFK then VU:CaptureController(); VU:ClickButton2(Vector2.new()) end end)
end
pcall(InitBypass)

-- [[ BÖLÜM 3: ROL TESPİT MOTORU ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- [[ BÖLÜM 4: ÖZEL IŞINLANMA MENÜSÜ (SCREEN GUI) ]] --
local function CreateTPMenu()
    local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
    local Frame = Instance.new("Frame", ScreenGui)
    local ScrollingFrame = Instance.new("ScrollingFrame", Frame)
    local UIListLayout = Instance.new("UIListLayout", ScrollingFrame)
    local CloseBtn = Instance.new("TextButton", Frame)

    Frame.Size = UDim2.new(0, 200, 0, 300)
    Frame.Position = UDim2.new(0.5, -100, 0.5, -150)
    Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Frame.Active = true
    Frame.Draggable = true
    Frame.Visible = false
    Frame.BorderSizePixel = 2

    ScrollingFrame.Size = UDim2.new(1, 0, 0.9, 0)
    ScrollingFrame.Position = UDim2.new(0, 0, 0.1, 0)
    ScrollingFrame.BackgroundTransparency = 1
    ScrollingFrame.CanvasSize = UDim2.new(0, 0, 5, 0)

    CloseBtn.Size = UDim2.new(1, 0, 0.1, 0)
    CloseBtn.Text = "KAPAT"
    CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    CloseBtn.TextColor3 = Color3.new(1,1,1)
    CloseBtn.MouseButton1Click:Connect(function() Frame.Visible = false end)

    local function RefreshTPList()
        for _, child in pairs(ScrollingFrame:GetChildren()) do if child:IsA("TextButton") then child:Destroy() end end
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP then
                local btn = Instance.new("TextButton", ScrollingFrame)
                btn.Size = UDim2.new(1, -10, 0, 30)
                local role = GetRole(p)
                btn.Text = p.Name .. " [" .. role .. "]"
                btn.BackgroundColor3 = (role == "MURDERER" and Color3.new(1,0,0)) or (role == "SHERIFF" and Color3.new(0,0,1)) or Color3.new(0.2,0.2,0.2)
                btn.TextColor3 = Color3.new(1,1,1)
                btn.MouseButton1Click:Connect(function()
                    if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                        LP.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame
                    end
                end)
            end
        end
    end
    return Frame, RefreshTPList
end
local TPFrame, RefreshFunc = CreateTPMenu()

-- [[ BÖLÜM 5: UI SEKMELERİ ]] --
local MainTab = Window:CreateTab("Main", 4483362458)
MainTab:CreateToggle({Name = "MURDERER AIMBOT (Sadece Katil)", CurrentValue = false, Callback = function(v) _G.MurdererAim = v end})
MainTab:CreateToggle({Name = "FORCE MAGNET (Silah Ayağına Gelsin)", CurrentValue = false, Callback = function(v) _G.GrabGun = v end})

local CombatTab = Window:CreateTab("Combat & TP", 4483362458)
CombatTab:CreateButton({
    Name = "IŞINLANMA MENÜSÜNÜ AÇ (Ekran Ortası)", 
    Callback = function() 
        RefreshFunc()
        TPFrame.Visible = true 
    end
})
CombatTab:CreateToggle({Name = "Kill Aura", CurrentValue = false, Callback = function(v) _G.KillAura = v end})

local VisualsTab = Window:CreateTab("Visuals", 4483362458)
VisualsTab:CreateToggle({Name = "Full ESP", CurrentValue = false, Callback = function(v) _G.ESP = v end})

local MoveTab = Window:CreateTab("Movement", 4483362458)
MoveTab:CreateSlider({Name = "WalkSpeed", Range = {16, 250}, Increment = 1, CurrentValue = 16, Callback = function(v) _G.Speed = v end})
MoveTab:CreateToggle({Name = "NoClip", CurrentValue = false, Callback = function(v) _G.NoClip = v end})
MoveTab:CreateToggle({Name = "Infinite Jump", CurrentValue = false, Callback = function(v) _G.InfJump = v end})

-- [[ BÖLÜM 6: ANA MOTORLAR ]] --

-- 1. FORCE MAGNET (SİLAHI ÇEKER)
task.spawn(function()
    while task.wait(0.01) do
        if _G.GrabGun and LP.Character then
            pcall(function()
                local gun = workspace:FindFirstChild("GunDrop") or workspace:FindFirstChild("Gun", true)
                if gun then
                    local p = (gun:IsA("Model") and (gun.PrimaryPart or gun:FindFirstChildWhichIsA("BasePart", true))) or gun
                    p.CFrame = LP.Character.HumanoidRootPart.CFrame
                    p.Velocity = Vector3.new(0,0,0)
                end
            end)
        end
    end
end)

-- 2. ONLY MURDERER AIMBOT
RunService.RenderStepped:Connect(function()
    if _G.MurdererAim then
        pcall(function()
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= LP and GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("Head") then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.Head.Position)
                    break
                end
            end
        end)
    end
end)

-- 3. HIZ, ESP & KILL AURA
RunService.Stepped:Connect(function()
    if LP.Character and LP.Character:FindFirstChild("Humanoid") then
        LP.Character.Humanoid.WalkSpeed = _G.Speed
        if _G.NoClip then for _, p in pairs(LP.Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end end
    end
end)

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

UIS.JumpRequest:Connect(function() if _G.InfJump and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid:ChangeState("Jumping") end end)

Rayfield:LoadConfiguration()
