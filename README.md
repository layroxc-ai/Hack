-- [[ LAYROXC HUB - dc_Layroxc (THE DEFINITIVE VERSION) ]] --
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local MarketplaceService = game:GetService("MarketplaceService")

-- [[ ANA DEĞİŞKENLER ]] --
_G.SpeedValue = 16
_G.JumpValue = 50
_G.Aimbot = false
_G.KillAura = false
_G.MasterESP = false
_G.StealthFarm = false
_G.NoClip = false
_G.AntiFling = false
_G.GrabGun = false
_G.InfJump = false

-- [[ MODERN MERKEZİ MENÜ SİSTEMİ ]] --
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Name = "LayroxcMain"
MainFrame.Size = UDim2.new(0, 380, 0, 350)
MainFrame.Position = UDim2.new(0.5, -190, 0.5, -175) -- Tam Orta
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.BorderSizePixel = 0
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 15)

-- Üst Bar (Sürükleme Alanı)
local TopBar = Instance.new("Frame", MainFrame)
TopBar.Size = UDim2.new(1, 0, 0, 45)
TopBar.BackgroundColor3 = Color3.fromRGB(30, 0, 0) -- Kırmızı/Siyah tema
Instance.new("UICorner", TopBar).CornerRadius = UDim.new(0, 15)

local Title = Instance.new("TextLabel", TopBar)
Title.Text = "  dc_Layroxc HUB - MM2 FULL"
Title.Size = UDim2.new(1, 0, 1, 0); Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.new(1,1,1); Title.Font = Enum.Font.GothamBold; Title.TextSize = 15; Title.TextXAlignment = Enum.TextXAlignment.Left

-- İçerik Kaydırma Alanı
local Content = Instance.new("ScrollingFrame", MainFrame)
Content.Size = UDim2.new(1, -10, 1, -55); Content.Position = UDim2.new(0, 5, 0, 50)
Content.BackgroundTransparency = 1; Content.CanvasSize = UDim2.new(0, 0, 3, 0); Content.ScrollBarThickness = 2
local UIList = Instance.new("UIListLayout", Content); UIList.Padding = UDim.new(0, 8); UIList.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- [[ MOBİL SÜRÜKLEME SİSTEMİ ]] --
local dragging, dragInput, dragStart, startPos
TopBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true; dragStart = input.Position; startPos = MainFrame.Position
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
UserInputService.InputEnded:Connect(function(input) dragging = false end)

-- [[ FONKSİYONLAR ]] --
local function GetRole(v)
    if not v or not v:FindFirstChild("Backpack") then return "Innocent" end
    if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then return "MURDERER" end
    if v.Backpack:FindFirstChild("Gun") or (v.Character and v.Character:FindFirstChild("Gun")) then return "SHERIFF" end
    return "Innocent"
end

local function CreateButton(txt, callback)
    local btn = Instance.new("TextButton", Content)
    btn.Size = UDim2.new(0.9, 0, 0, 40); btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.Text = txt; btn.TextColor3 = Color3.new(1,1,1); btn.Font = Enum.Font.Gotham; btn.TextSize = 14
    Instance.new("UICorner", btn); btn.MouseButton1Click:Connect(callback)
end

local function CreateToggle(txt, var_name)
    local btn = Instance.new("TextButton", Content)
    btn.Size = UDim2.new(0.9, 0, 0, 40); btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.Text = txt .. ": OFF"; btn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(function()
        _G[var_name] = not _G[var_name]
        btn.Text = txt .. ": " .. (_G[var_name] and "ON" or "OFF")
        btn.TextColor3 = _G[var_name] and Color3.fromRGB(255, 0, 0) or Color3.new(1, 1, 1)
    end)
end

local function CreateTextBox(txt, placeholder, callback)
    local box = Instance.new("TextBox", Content)
    box.Size = UDim2.new(0.9, 0, 0, 40); box.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    box.PlaceholderText = placeholder; box.Text = ""; box.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", box); box.FocusLost:Connect(function() callback(box.Text) end)
end

-- [[ MENÜ İÇERİĞİ (TÜM EKSİKLER EKLENDİ) ]] --
CreateToggle("Aimbot (Murderer Focus)", "Aimbot")
CreateToggle("Kill Aura (25m)", "KillAura")
CreateButton("KILL ALL (Knife Required)", function()
    local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
    if k then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                k.Parent = LocalPlayer.Character; LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame; task.wait(0.1); k:Activate()
                firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0); firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
            end
        end
    end
end)
CreateToggle("ESP Master", "MasterESP")
CreateToggle("Magnet Gun", "GrabGun")
CreateToggle("Auto Coin Farm", "StealthFarm")
CreateToggle("NoClip (Walk Through Walls)", "NoClip")
CreateToggle("Anti-Fling", "AntiFling")
CreateToggle("Infinite Jump", "InfJump")
CreateTextBox("WalkSpeed", "Hızınızı Yazın", function(val) _G.SpeedValue = tonumber(val) or 16 end)
CreateTextBox("JumpPower", "Zıplama Gücü", function(val) _G.JumpValue = tonumber(val) or 50 end)
CreateButton("TP Murderer", function()
    for _, v in pairs(Players:GetPlayers()) do if GetRole(v) == "MURDERER" and v.Character then LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame end end
end)
CreateButton("TP Sheriff", function()
    for _, v in pairs(Players:GetPlayers()) do if GetRole(v) == "SHERIFF" and v.Character then LocalPlayer.Character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame end end
end)
CreateButton("Invisible Mode", function()
    local c = LocalPlayer.Character
    if c then for _, v in pairs(c:GetDescendants()) do if v:IsA("BasePart") then v.Transparency = 1 end end end
end)
CreateButton("Get Korblox (80 Robux)", function()
    setclipboard("https://www.roblox.com/tr/game-pass/1812606767/Korblox-FE")
    local h = Instance.new("Hint", game.CoreGui)
    h.Text = "PLEASE RUN THE COPIED LINK IN YOUR BROWSER TO GET KORBLOX"
    task.wait(5); h:Destroy()
end)

-- [[ MOBİL AÇ/KAPAT BUTONU (L) ]] --
local LBtn = Instance.new("TextButton", ScreenGui); LBtn.Size = UDim2.new(0, 50, 0, 50); LBtn.Position = UDim2.new(0, 15, 0.5, -25)
LBtn.Text = "L"; LBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0); LBtn.TextColor3 = Color3.new(1,1,1); LBtn.Draggable = true
Instance.new("UICorner", LBtn).CornerRadius = UDim.new(1, 0)
LBtn.MouseButton1Click:Connect(function() MainFrame.Visible = not MainFrame.Visible end)

-- [[ SİSTEM DÖNGÜLERİ ]] --
RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        for _, v in pairs(Players:GetPlayers()) do
            if GetRole(v) == "MURDERER" and v.Character and v.Character:FindFirstChild("Head") then
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, v.Character.Head.Position)
            end
        end
    end
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                local hl = v.Character:FindFirstChild("LayHL") or Instance.new("Highlight", v.Character)
                hl.Name = "LayHL"; hl.FillColor = GetRole(v) == "MURDERER" and Color3.new(1,0,0) or (GetRole(v) == "SHERIFF" and Color3.new(0,0,1) or Color3.new(0,1,0))
                hl.FillTransparency = 0.5
            end
        end
    end
end)

RunService.Stepped:Connect(function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = _G.SpeedValue
        LocalPlayer.Character.Humanoid.JumpPower = _G.JumpValue
        LocalPlayer.Character.Humanoid.UseJumpPower = true
        for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
            if v:IsA("BasePart") then
                if _G.NoClip then v.CanCollide = false
                elseif _G.AntiFling then v.CanTouch = false end
            end
        end
    end
end)

UserInputService.JumpRequest:Connect(function()
    if _G.InfJump and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

task.spawn(function()
    while task.wait(0.5) do
        pcall(function()
            if _G.StealthFarm then
                for _, v in pairs(workspace:GetDescendants()) do
                    if (v.Name == "Coin" or v.Name == "Candy" or v.Parent.Name == "CoinContainer") and v:IsA("BasePart") then
                        if _G.StealthFarm then LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame task.wait(1.5) end
                    end
                end
            end
            if _G.GrabGun then
                for _, v in pairs(workspace:GetDescendants()) do if v.Name == "GunDrop" then v.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame end end
            end
            if _G.KillAura then
                local k = LocalPlayer.Character:FindFirstChild("Knife") or LocalPlayer.Backpack:FindFirstChild("Knife")
                if k then
                    for _, v in pairs(Players:GetPlayers()) do
                        if v ~= LocalPlayer and v.Character and (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude < 25 then
                            k.Parent = LocalPlayer.Character; k:Activate()
                            firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 0); firetouchinterest(v.Character.HumanoidRootPart, k.Handle, 1)
                        end
                    end
                end
            end
        end)
    end
end)
