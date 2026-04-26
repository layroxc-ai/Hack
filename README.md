-- [[ LAYROXC HUB v59 - MENÜ FIX & FULL ENGINE ]] --

-- Kütüphane Yükleme (Menü Kesin Gelsin Diye Rayfield Kullanıldı)
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Layroxc Hub - v59 FINAL",
   LoadingTitle = "Yükleniyor...",
   LoadingSubtitle = "by Layroxc",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "LayroxcHub",
      FileName = "MM2_Config"
   }
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- AYARLAR
_G.MasterESP = false
_G.SpeedValue = 16
_G.SilentAim = false
_G.Aimbot = false
_G.NoClip = false

local MyGamepassID = 1812606767
local MyGamepassLink = "https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE"

-- [[ ATEŞ ETME BUTONU ]] --
local ShootGui = Instance.new("ScreenGui", game.CoreGui)
local ShootBtn = Instance.new("TextButton", ShootGui)
ShootBtn.Size = UDim2.new(0, 90, 0, 90); ShootBtn.Position = UDim2.new(0.8, 0, 0.4, 0)
ShootBtn.Text = "ÖLDÜR"; ShootBtn.Visible = false; ShootBtn.Draggable = true
ShootBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0); ShootBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", ShootBtn).CornerRadius = UDim.new(1, 0)

-- [[ ROL TESPİTİ ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

-- SEKMELER
local Main = Window:CreateTab("Combat", 4483362458)
local Visuals = Window:CreateTab("Visuals", 4483362458)
local Move = Window:CreateTab("Movement", 4483362458)
local Pro = Window:CreateTab("Pro & Support", 4483362458)

-- [[ COMBAT ]] --
Main:CreateToggle({
   Name = "Silent Aim (Ateş Butonu)",
   CurrentValue = false,
   Callback = function(Value)
      _G.SilentAim = Value
      ShootBtn.Visible = Value
   end,
})

Main:CreateToggle({
   Name = "Cam Aimbot (Lock)",
   CurrentValue = false,
   Callback = function(Value) _G.Aimbot = Value end,
})

Main:CreateButton({
   Name = "KILL ALL (Herkesi Kes)",
   Callback = function()
      local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
      if k then
         k.Parent = LocalPlayer.Character
         for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
               LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,1)
               task.wait(0.1)
               k:Activate()
            end
         end
      end
   end,
})

ShootBtn.MouseButton1Click:Connect(function()
    local gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
    if gun then
        gun.Parent = LocalPlayer.Character
        for _, v in pairs(Players:GetPlayers()) do
            if GetRole(v) == "MURDERER" and v.Character then
                game:GetService("ReplicatedStorage").ShootGun:FireServer(v.Character.HumanoidRootPart.Position)
            end
        end
    end
end)

-- [[ VISUALS ]] --
Visuals:CreateToggle({
   Name = "MASTER ESP (İsim & Rol)",
   CurrentValue = false,
   Callback = function(Value) _G.MasterESP = Value end,
})

-- [[ MOVEMENT ]] --
Move:CreateSlider({
   Name = "Yürüme Hızı",
   Range = {16, 200},
   Increment = 1,
   CurrentValue = 16,
   Callback = function(Value) _G.SpeedValue = Value end,
})

Move:CreateToggle({
   Name = "NoClip (Duvar Geçme)",
   CurrentValue = false,
   Callback = function(Value) _G.NoClip = Value end,
})

-- [[ PRO (KORBLOX & BROWSER UYARISI) ]] --
Pro:CreateButton({
   Name = "Get Korblox (BROWSER'DA AÇ)",
   Callback = function()
      Rayfield:Notify({Title = "BİLGİ", Content = "BROWSER'DA AÇ! Link Kopyalandı.", Duration = 5})
      setclipboard(MyGamepassLink)
      pcall(function() MarketplaceService:PromptGamePassPurchase(LocalPlayer, MyGamepassID) end)
   end,
})

-- [[ ANA MOTOR ]] --
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local role = GetRole(v)
                    local color = role == "MURDERER" and Color3.new(1,0,0) or (role == "SHERIFF" and Color3.new(0,0.5,1) or Color3.new(0,1,0))
                    
                    local bg = v.Character.Head:FindFirstChild("LayName") or Instance.new("BillboardGui", v.Character.Head)
                    bg.Name = "LayName"; bg.AlwaysOnTop = true; bg.Size = UDim2.new(0,150,0,35); bg.ExtentsOffset = Vector3.new(0,3,0)
                    
                    local lb = bg:FindFirstChild("TL") or Instance.new("TextLabel", bg)
                    lb.Name = "TL"; lb.Size = UDim2.new(1,0,1,0); lb.BackgroundTransparency = 1; lb.TextColor3 = color; lb.TextSize = 14; lb.Text = "["..role.."] "..v.DisplayName
                end
            end)
        end
    end

    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if GetRole(v) == "MURDERER" and v.Character then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.HumanoidRootPart.Position)
            end
        end
    end

    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
        if _G.NoClip then
            for _, p in pairs(LocalPlayer.Character:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide = false end
            end
        end
    end
end)
