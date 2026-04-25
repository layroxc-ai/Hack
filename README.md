-- [[ LAYROXC HUB v21 - ABSOLUTE EXECUTION & FULL MM2 ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub MM2 - FINAL", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- [[ MOBİL SÜRÜKLENEBİLİR BUTON ]] --
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.5, 0)
OpenButton.Text = "L"
OpenButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
OpenButton.TextColor3 = Color3.fromRGB(0, 255, 0)
OpenButton.Draggable = true 
Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

-- [[ TABLAR ]] --
local Combat = Window:NewTab("Saldırı")
local Farm = Window:NewTab("Farm")
local Visuals = Window:NewTab("ESP")
local Pro = Window:NewTab("Avatar")
local Social = Window:NewTab("Sosyal")

-- [[ 1. KATİLİ ÖLDÜR (CALIŞAN SÜRÜM) ]] --
local CombatSec = Combat:NewSection("Katil İnfaz Sistemi")

CombatSec:NewButton("KATİLİ ŞİMDİ ÖLDÜR", "Katil neredeyse oraya mermi ışınlar", function()
    local Gun = LocalPlayer.Character:FindFirstChild("Gun") or LocalPlayer.Backpack:FindFirstChild("Gun")
    if Gun then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and (v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")) then
                local target = v.Character:FindFirstChild("HumanoidRootPart")
                if target then
                    -- Silahı hazırla
                    Gun.Parent = LocalPlayer.Character
                    task.wait(0.05)
                    
                    -- Kamerayı katile çivile
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
                    
                    -- Ateş Etme Protokolü
                    local shoot = Gun:FindFirstChild("Shoot") or Gun:FindFirstChild("Remote")
                    if shoot then
                        -- Mermiyi doğrudan katilin üzerine ateşler
                        shoot:FireServer(target.Position)
                        Gun:Activate()
                    else
                        -- Alternatif Ateş (Bazı haritalar için)
                        Gun:Activate()
                    end
                end
            end
        end
    else
        print("Silahın yok kanki!")
    end
end)

CombatSec:NewToggle("Magnet Grab Gun", "Silahı saniyede 10 kez tarar ve çeker", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "GunDrop" or (v:IsA("Part") and v:FindFirstChild("TouchTransmitter") and not v.Parent:FindFirstChild("Knife"))) then
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
                end
            end
        end
        task.wait(0.1)
    end
end)

-- [[ 2. AUTO FARM ]] --
local FarmSec = Farm:NewSection("Otomatik Toplayıcı")
FarmSec:NewToggle("Auto Coin/Candy", "Işınlanarak toplar", function(state)
    _G.AutoFarm = state
    while _G.AutoFarm do
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "Coin" or v.Name == "Candy") and v:IsA("BasePart") and _G.AutoFarm then
                LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                task.wait(0.2)
            end
        end
        task.wait()
    end
end)

-- [[ 3. ESP ]] --
local VisSec = Visuals:NewSection("Rol İfşalama")
VisSec:NewToggle("Full Highlight ESP", "Katil=Kırmızı, Şerif=Mavi", function(state)
    _G.MasterESP = state
    while _G.MasterESP do
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character then
                if not v.Character:FindFirstChild("Highlight") then
                    Instance.new("Highlight", v.Character).Name = "Highlight"
                end
                local hl = v.Character.Highlight
                if v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife") then
                    hl.FillColor = Color3.fromRGB(255, 0, 0)
                elseif v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun") then
                    hl.FillColor = Color3.fromRGB(0, 0, 255)
                else
                    hl.FillColor = Color3.fromRGB(0, 255, 0)
                end
            end
        end
        task.wait(0.5)
    end
end)

-- [[ 4. AVATAR HACK ]] --
local ProSec = Pro:NewSection("Bedava Görünüm")
ProSec:NewButton("Headless (Kafasız)", "Kafanı yok eder", function() LocalPlayer.Character.Head.Transparency = 1 end)
ProSec:NewButton("Korblox (Sağ Bacak)", "Bacağını siler", function() if LocalPlayer.Character:FindFirstChild("RightUpperLeg") then LocalPlayer.Character.RightUpperLeg:Destroy() end end)

-- [[ 5. SOSYAL ]] --
Social:NewSection("TikTok: @layroxcderler")
Social:NewButton("Profil Kopyala", "Destek için tıkla kanki!", function() setclipboard("https://www.tiktok.com/@layroxcderler") end)
