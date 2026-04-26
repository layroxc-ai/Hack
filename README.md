-- [[ RAYFIELD LIBRARY - PROFESYONEL MENÜ ]] --
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "LAYROXC HUB v3.0 | PREMIUM",
   LoadingTitle = "Layroxc Sistemleri Yükleniyor...",
   LoadingSubtitle = "by dc_Layroxc",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "LayroxcConfigs",
      FileName = "MM2_Master"
   }
})

-- [[ SERVİSLER VE DEĞİŞKENLER ]] --
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UserInputService = game:GetService("UserInputService")

_G.SpeedValue = 16
_G.JumpValue = 50
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.StealthFarm = false
_G.NoClip = false
_G.GrabGun = false
_G.InfJump = false

-- [[ YARDIMCI FONKSİYONLAR ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- [[ SAVAŞ (COMBAT) SEKME ]] --
local CombatTab = Window:CreateTab("Combat", 4483362458)

CombatTab:CreateToggle({
   Name = "Aimbot (Katile Kilitlen)",
   CurrentValue = false,
   Callback = function(Value) _G.Aimbot = Value end,
})

CombatTab:CreateToggle({
   Name = "Kill Aura (Otomatik Vuruş)",
   CurrentValue = false,
   Callback = function(Value) _G.KillAura = Value end,
})

CombatTab:CreateButton({
   Name = "Herkesi Öldür (Kill All)",
   Callback = function()
      local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
      if k then
         k.Parent = LocalPlayer.Character
         for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
               pcall(function()
                  LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                  task.wait(0.1)
                  k:Activate()
                  firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                  firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
               end)
            end
         end
      end
   end,
})

-- [[ GÖRÜNÜM (VISUALS) SEKME ]] --
local VisualsTab = Window:CreateTab("Visuals", 4483362458)

VisualsTab:CreateToggle({
   Name = "Highlight ESP (Rolleri Göster)",
   CurrentValue = false,
   Callback = function(Value) 
       _G.MasterESP = Value 
       if not Value then
           for _, v in pairs(Players:GetPlayers()) do
               if v.Character and v.Character:FindFirstChild("LayHL") then v.Character.LayHL:Destroy() end
           end
       end
   end,
})

VisualsTab:CreateButton({
   Name = "Karakteri Görünmez Yap",
   Callback = function()
      local c = LocalPlayer.Character
      if c then for _, v in pairs(c:GetDescendants()) do if v:IsA("BasePart") or v:IsA("Decal") then v.Transparency = 1 end end end
   end,
})

-- [[ FARM SEKME ]] --
local FarmTab = Window:CreateTab("Farm", 4483362458)

FarmTab:CreateToggle({
   Name = "Otomatik Coin/Candy Topla",
   CurrentValue = false,
   Callback = function(Value) _G.StealthFarm = Value end,
})

FarmTab:CreateToggle({
   Name = "Mıknatıs (Grab Gun)",
   CurrentValue = false,
   Callback = function(Value) _G.GrabGun = Value end,
})

-- [[ HAREKET (MOVEMENT) SEKME ]] --
local MoveTab = Window:CreateTab("Movement", 4483362458)

MoveTab:CreateSlider({
   Name = "Yürüme Hızı",
   Range = {16, 200},
   Increment = 1,
   CurrentValue = 16,
   Callback = function(Value) _G.SpeedValue = Value end,
})

MoveTab:CreateToggle({
   Name = "Duvar Geçme (NoClip)",
   CurrentValue = false,
   Callback = function(Value) _G.NoClip = Value end,
})

MoveTab:CreateToggle({
   Name = "Sonsuz Zıplama",
   CurrentValue = false,
   Callback = function(Value) _G.InfJump = Value end,
})

-- [[ DÖNGÜSEL SİSTEMLER (ARKA PLAN) ]] --
RunService.RenderStepped:Connect(function()
    -- Aimbot Sistemi
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("Head") then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.Head.Position)
            end
        end
    end
    -- ESP Sistemi
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                hl.Name = "LayHL"
                hl.FillColor = GetRole(v) == "MURDERER" and Color3.new(1,0,0) or (GetRole(v) == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
                hl.FillTransparency = 0.5
            end
        end
    end
end)

-- Fizik Döngüsü
RunService.Stepped:Connect(function()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") and _G.NoClip then v.CanCollide = false end
            end
        end
    end)
end)

-- Sonsuz Zıplama
UserInputService.JumpRequest:Connect(function()
    if _G.InfJump and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

-- Farm ve Aura Döngüsü
task.spawn(function()
    while task.wait(0.3) do
        pcall(function()
            -- Farm
            if _G.StealthFarm then
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v.Name == "Coin" or v.Name == "Candy") and v:IsA("BasePart") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                        task.wait(0.5)
                    end
                end
            end
            -- Grab Gun
            if _G.GrabGun then
                local gun = workspace:FindFirstChild("GunDrop") or workspace:FindFirstChild("Gun")
                if gun then gun.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end
            end
            -- Kill Aura
            if _G.KillAura then
                local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
                if k then
                    for _, v in pairs(Players:GetPlayers()) do
                        if v ~= LocalPlayer and v.Character and (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude < 20 then
                            k.Parent = LocalPlayer.Character
                            k:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0)
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
                        end
                    end
                end
            end
        end)
    end
end)

Rayfield:LoadConfiguration()
