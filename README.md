-- [[ LAYROXC HUB v14.0 - THE FINAL BEAST | 250+ LINES ]] --
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LAYROXC HUB| LAYROXC",
   LoadingTitle = "Layroxc Ultimate Sistemler Yükleniyor...",
   LoadingSubtitle = "by dc_Layroxc",
   ConfigurationSaving = { Enabled = true, FolderName = "Layroxc14", FileName = "FinalMaster" }
})

-- [[ SERVİSLER VE ÇEKİRDEK AYARLAR ]] --
local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local TS = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")

-- [[ GLOBAL DURUMLAR ]] --
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

-- [[ ANTI-AFK SİSTEMİ (Oyunun Atmasını Engeller) ]] --
task.spawn(function()
    local VirtualUser = game:GetService("VirtualUser")
    LP.Idled:Connect(function()
        VirtualUser:CaptureController()
        VirtualUser:ClickButton2(Vector2.new())
    end)
end)

-- [[ ROL TESPİT MOTORU ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then
        return "MURDERER"
    elseif v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then
        return "SHERIFF"
    end
    return "Innocent"
end

-- [[ SOSYAL HESAPLAR SEKME ]] --
local SocialTab = Window:CreateTab("Socials", 4483362458)
SocialTab:CreateSection("Yapımcı Sosyal Medya")
SocialTab:CreateButton({Name = "Roblox: dc_Layroxc", Callback = function() setclipboard("dc_Layroxc") end})
SocialTab:CreateButton({Name = "TikTok: @layroxcderler", Callback = function() setclipboard("layroxcderler") end})
SocialTab:CreateButton({Name = "Instagram: @Layroxc", Callback = function() setclipboard("Layroxc") end})

-- [[ COMBAT SEKME ]] --
local CombatTab = Window:CreateTab("Combat", 4483362458)
CombatTab:CreateToggle({Name = "Aimbot (Katili Takip Et)", CurrentValue = false, Callback = function(v) _G.Aimbot = v end})
CombatTab:CreateToggle({Name = "Kill Aura (25 Metre Reach)", CurrentValue = false, Callback = function(v) _G.KillAura = v end})
CombatTab:CreateButton({
   Name = "Kill All Players (Anında)",
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
FarmTab:CreateToggle({Name = "Auto Coin & Candy Farm", CurrentValue = false, Callback = function(v) _G.Farm = v end})
FarmTab:CreateToggle({Name = "Grab Gun (Düşen Silahı Al)", CurrentValue = false, Callback = function(v) _G.GrabGun = v end})

-- [[ VISUALS SEKME ]] --
local VisualsTab = Window:CreateTab("Visuals", 4483362458)
VisualsTab:CreateToggle({Name = "Master ESP (Highlight)", CurrentValue = false, Callback = function(v) _G.ESP = v end})
VisualsTab:CreateButton({
   Name = "Rolleri Chat'te Göster",
   Callback = function()
      for _, v in pairs(Players:GetPlayers()) do
         local role = GetRole(v)
         if role ~= "Innocent" then
            game.StarterGui:SetCore("ChatMakeSystemMessage", {Text = "[LAYROXC HUB]: " .. v.Name .. " -> " .. role, Color = Color3.new(1,0.5,0)})
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
MoveTab:CreateSlider({Name = "JumpPower", Range = {50, 300}, Increment = 1, CurrentValue = 50, Callback = function(v) _G.Jump = v end})
MoveTab:CreateToggle({Name = "Fly Mode", CurrentValue = false, Callback = function(v) _G.Fly = v end})
MoveTab:CreateToggle({Name = "NoClip", CurrentValue = false, Callback = function(v) _G.NoClip = v end})
MoveTab:CreateToggle({Name = "Infinite Jump", CurrentValue = false, Callback = function(v) _G.InfJump = v end})

-- [[ TELEPORTS & SERVER ]] --
local TPTab = Window:CreateTab("Settings", 4483362458)
TPTab:CreateButton({
   Name = "Katile Işınlanma Butonu",
   Callback = function()
      local SG = Instance.new("ScreenGui", game.CoreGui)
      local B = Instance.new("TextButton", SG)
      B.Size = UDim2.new(0, 140, 0, 50); B.Position = UDim2.new(0.5, 0, 0.1, 0)
      B.Text = "KATİLE GİT"; B.BackgroundColor3 = Color3.fromRGB(200, 0, 0); B.Draggable = true
      Instance.new("UICorner", B)
      B.MouseButton1Click:Connect(function()
         for _, p in pairs(Players:GetPlayers()) do
            if GetRole(p) == "MURDERER" and p.Character then
               LP.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
            end
         end
      end)
   end,
})
TPTab:CreateButton({Name = "Server Hop (Yeni Sunucu)", Callback = function()
    local x = HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100"))
    for _, s in pairs(x.data) do if s.playing < s.maxPlayers then TS:TeleportToPlaceInstance(game.PlaceId, s.id) end end
end})

-- [[ BACKEND OPERASYONLARI (ANA MOTOR) ]] --
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
                local h = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                h.Name = "LayHL"
                h.FillColor = GetRole(v) == "MURDERER" and Color3.new(1,0,0) or (GetRole(v) == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
            end
        end
    end
end)

RunService.Stepped:Connect(function()
    pcall(function()
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            LP.Character.Humanoid.WalkSpeed = _G.Speed
            LP.Character.Humanoid.JumpPower = _G.Jump
            if _G.NoClip then
                for _, p in pairs(LP.Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end
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

task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            if _G.GrabGun then
                for _, x in pairs(workspace:GetDescendants()) do
                    if x.Name == "GunDrop" or (x:IsA("Model") and x.Name == "Gun") then
                        LP.Character.HumanoidRootPart.CFrame = x.PrimaryPart and x.PrimaryPart.CFrame or x.CFrame
                    end
                end
            end
            if _G.Farm then
                for _, x in pairs(workspace:GetDescendants()) do
                    if (x.Name == "Coin" or x.Name == "Candy") and x:IsA("BasePart") then
                        LP.Character.HumanoidRootPart.CFrame = x.CFrame; task.wait(0.3)
                    end
                end
            end
            if _G.KillAura then
                local k = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife")
                if k then
                    for _, p in pairs(Players:GetPlayers()) do
                        if p ~= LP and p.Character and (LP.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            k.Parent = LP.Character; k:Activate()
                            firetouchinterest(p.Character.HumanoidRootPart, k.Handle, 0)
                            firetouchinterest(p.Character.HumanoidRootPart, k.Handle, 1)
                        end
                    end
                end
            end
        end)
    end
end)

Rayfield:LoadConfiguration()
