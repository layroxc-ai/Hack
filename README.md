-- [[ LAYROXC HUB v13.0 - FULL & LONG VERSION ]] --
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LAYROXC HUB v13.0 | 250+ LINES",
   LoadingTitle = "Layroxc Ultimate Sistemler Yükleniyor...",
   LoadingSubtitle = "by dc_Layroxc",
   ConfigurationSaving = { Enabled = true, FolderName = "Layroxc13", FileName = "MasterConfig" }
})

-- [[ SERVİSLER ]] --
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local TS = game:GetService("TeleportService")

-- [[ DEĞİŞKENLER ]] --
_G.Speed = 16
_G.Jump = 50
_G.Aimbot = false
_G.KillAura = false
_G.ESP = false
_G.Farm = false
_G.GrabGun = false
_G.NoClip = false
_G.Fly = false
_G.InfJump = false
_G.AntiAFK = true

-- [[ ROL BULUCU FONKSİYON ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- [[ ANTI-AFK SİSTEMİ ]] --
if _G.AntiAFK then
    local VirtualUser = game:GetService("VirtualUser")
    LP.Idled:Connect(function()
        VirtualUser:CaptureController()
        VirtualUser:ClickButton2(Vector2.new())
    end)
end

-- [[ SOSYAL HESAPLAR ]] --
local SocialTab = Window:CreateTab("Socials", 4483362458)
SocialTab:CreateSection("Beni Takip Et")
SocialTab:CreateButton({Name = "Roblox: dc_Layroxc", Callback = function() setclipboard("dc_Layroxc") end})
SocialTab:CreateButton({Name = "TikTok: @layroxcderler", Callback = function() setclipboard("layroxcderler") end})
SocialTab:CreateButton({Name = "Instagram: @Layroxc", Callback = function() setclipboard("Layroxc") end})

-- [[ COMBAT SEKME ]] --
local CombatTab = Window:CreateTab("Combat", 4483362458)
CombatTab:CreateToggle({Name = "Aimbot (Lock Killer)", CurrentValue = false, Callback = function(v) _G.Aimbot = v end})
CombatTab:CreateToggle({Name = "Kill Aura (Reach 25)", CurrentValue = false, Callback = function(v) _G.KillAura = v end})
CombatTab:CreateButton({
   Name = "Kill All Players (Instant)",
   Callback = function()
      local kn = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife")
      if kn then
         for _, v in pairs(Players:GetPlayers()) do
            if v ~= LP and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
               LP.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
               task.wait(0.05)
               kn.Parent = LP.Character; kn:Activate()
            end
         end
      end
   end,
})

-- [[ FARM SEKME ]] --
local FarmTab = Window:CreateTab("Farm", 4483362458)
FarmTab:CreateToggle({Name = "Auto Coin/Candy Farm", CurrentValue = false, Callback = function(v) _G.Farm = v end})
FarmTab:CreateToggle({Name = "Grab Gun (Mıknatıs)", CurrentValue = false, Callback = function(v) _G.GrabGun = v end})

-- [[ VISUALS SEKME ]] --
local VisualsTab = Window:CreateTab("Visuals", 4483362458)
VisualsTab:CreateToggle({Name = "Master ESP (Highlights)", CurrentValue = false, Callback = function(v) _G.ESP = v end})
VisualsTab:CreateButton({
   Name = "Reveal Roles in Chat",
   Callback = function()
      for _, v in pairs(Players:GetPlayers()) do
         local role = GetRole(v)
         if role ~= "Innocent" then
            game.StarterGui:SetCore("ChatMakeSystemMessage", {Text = "[LAYROXC]: " .. v.Name .. " is " .. role, Color = Color3.new(1,0,0)})
         end
      end
   end,
})
VisualsTab:CreateButton({
   Name = "KORBLOX SATIN AL",
   Callback = function()
      setclipboard("https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE")
      Rayfield:Notify({Title = "LINK COPIED!", Content = "OPEN IN BROWSER", Duration = 5})
   end,
})

-- [[ MOVEMENT SEKME ]] --
local MoveTab = Window:CreateTab("Movement", 4483362458)
MoveTab:CreateSlider({Name = "WalkSpeed", Range = {16, 300}, Increment = 1, CurrentValue = 16, Callback = function(v) _G.Speed = v end})
MoveTab:CreateToggle({Name = "Fly Mode", CurrentValue = false, Callback = function(v) _G.Fly = v end})
MoveTab:CreateToggle({Name = "NoClip", CurrentValue = false, Callback = function(v) _G.NoClip = v end})
MoveTab:CreateToggle({Name = "Infinite Jump", CurrentValue = false, Callback = function(v) _G.InfJump = v end})

-- [[ TELEPORTS SEKME ]] --
local TPTab = Window:CreateTab("Teleports", 4483362458)
TPTab:CreateButton({
   Name = "Katile Işınlanma Butonu (Aç)",
   Callback = function()
      local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
      local Btn = Instance.new("TextButton", ScreenGui)
      Btn.Size = UDim2.new(0, 130, 0, 45); Btn.Position = UDim2.new(0.5, 0, 0.2, 0)
      Btn.Text = "TP TO MURD"; Btn.BackgroundColor3 = Color3.fromRGB(150,0,0); Btn.Draggable = true
      Instance.new("UICorner", Btn)
      Btn.MouseButton1Click:Connect(function()
         for _, p in pairs(Players:GetPlayers()) do
            if GetRole(p) == "MURDERER" and p.Character then
               LP.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
            end
         end
      end)
   end,
})

-- [[ ANA DÖNGÜLER ]] --
RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
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
                hl.Name = "LayHL"
                hl.FillColor = GetRole(v) == "MURDERER" and Color3.new(1,0,0) or (GetRole(v) == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
            end
        end
    end
end)

RunService.Stepped:Connect(function()
    pcall(function()
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            LP.Character.Humanoid.WalkSpeed = _G.Speed
            if _G.NoClip then
                for _, part in pairs(LP.Character:GetDescendants()) do
                    if part:IsA("BasePart") then part.CanCollide = false end
                end
            end
            if _G.Fly then LP.Character.HumanoidRootPart.Velocity = Vector3.new(0, 3, 0) end
        end
    end)
end)

UIS.JumpRequest:Connect(function()
    if _G.InfJump and LP.Character:FindFirstChildOfClass("Humanoid") then
        LP.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

-- FARM & KILL AURA MOTORU
task.spawn(function()
    while task.wait(0.15) do
        pcall(function()
            if _G.GrabGun then
                local gun = workspace:FindFirstChild("GunDrop") or workspace:FindFirstChild("Gun", true)
                if gun then LP.Character.HumanoidRootPart.CFrame = gun.CFrame end
            end
            if _G.Farm then
                for _, obj in pairs(workspace:GetDescendants()) do
                    if (obj.Name == "Coin" or obj.Name == "Candy") and obj:IsA("BasePart") then
                        LP.Character.HumanoidRootPart.CFrame = obj.CFrame; task.wait(0.35)
                    end
                end
            end
            if _G.KillAura then
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
            end
        end)
    end
end)

Rayfield:LoadConfiguration()
