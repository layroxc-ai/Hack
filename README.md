-- [[ LAYROXC HUB v12 - BEST FEATURES EDITION ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Pro Hub", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- MOBİL BUTON (L)
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.4, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
local UIKose = Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- TABLAR
local FunTab = Window:NewTab("Eğlence & Trol")
local GodTab = Window:NewTab("God Modu")
local Social = Window:NewTab("Sosyal")

-- 1. EĞLENCE VE TROL (EN İYİLER)
local FunSec = FunTab:NewSection("Trol Özellikleri")

FunSec:NewButton("Görünmezlik (Invisible)", "Seni kimse göremez (Sadece sende)", function()
    local char = LocalPlayer.Character
    for _, v in pairs(char:GetDescendants()) do
        if v:IsA("BasePart") or v:IsA("Decal") then
            v.Transparency = 1
        end
    end
end)

FunSec:NewToggle("Fly (Uçma Modu)", "Haritada özgürce uçun", function(state)
    _G.Fly = state
    local BodyVel = Instance.new("BodyVelocity")
    BodyVel.Velocity = Vector3.new(0, 0, 0)
    BodyVel.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    
    if _G.Fly then
        BodyVel.Parent = LocalPlayer.Character.HumanoidRootPart
        while _G.Fly do
            BodyVel.Velocity = workspace.CurrentCamera.CFrame.LookVector * 50
            task.wait()
        end
    else
        for _, v in pairs(LocalPlayer.Character.HumanoidRootPart:GetChildren()) do
            if v:IsA("BodyVelocity") then v:Destroy() end
        end
    end
end)

FunSec:NewButton("Serverı Dans Ettir (FE)", "Herkesin ekranında garip hareketler yaparsınız", function()
    loadstring(game:HttpGet("https://pastebin.com/raw/766v9UvU"))() -- FE Animation Script
end)

-- 2. GOD MODU (DURDURULAMAZ ÖZELLİKLER)
local GodSec = GodTab:NewSection("Durdurulamaz Güç")

GodSec:NewToggle("Auto Dodge (Bıçaktan Kaç)", "Katil yaklaştığında otomatik ışınlanarak kaçar", function(state)
    _G.AutoDodge = state
    while _G.AutoDodge do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                local dist = (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude
                if dist < 15 then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 50, 0)
                end
            end
        end
        task.wait(0.1)
    end
end)

GodSec:NewButton("X-Ray (Duvarları Kaldır)", "Haritadaki tüm engelleri şeffaf yapar", function()
    for _, v in pairs(workspace:GetDescendants()) do
        if v:IsA("BasePart") and not v.Parent:FindFirstChild("Humanoid") then
            v.Transparency = 0.5
        end
    end
end)

GodSec:NewToggle("Infinity Jump", "Sınırsız zıplama", function(state)
    _G.InfJump = state
    game:GetService("UserInputService").JumpRequest:Connect(function()
        if _G.InfJump then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
        end
    end)
end)

-- 3. SOSYAL
Social:NewSection("TikTok: @layroxcderler")
Social:NewButton("Profilimi Kopyala", "Takip et, yeni hileleri kaçırma!", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
