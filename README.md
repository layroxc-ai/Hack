-- [[ LAYROXC HUB v38 - PRE-GAME DETECTOR & NITRO ]] --
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "Layroxc Hub MM2 - v38", HidePremium = false, SaveConfig = true, ConfigFolder = "LayroxcV38"})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local MarketplaceService = game:GetService("MarketplaceService")

-- [[ ULTRA HIZLI TESPİT MOTORU ]] --
function GetKiller()
    local target = nil
    for _, v in pairs(Players:GetPlayers()) do
        pcall(function()
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                -- Bıçak kontrolü (El + Çanta + Envanter Verisi)
                if v.Character:FindFirstChild("Knife") or v.Backpack:FindFirstChild("Knife") or (v:FindFirstChild("Status") and v.Status:FindFirstChild("Role") and v.Status.Role.Value == "Murderer") then
                    target = v.Character.HumanoidRootPart
                end
            end
        end)
    end
    return target
end

-- [[ AIMBOT - SÜREKLİ AKTİF ]] --
_G.Aimbot = false
RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        local killerPart = GetKiller()
        if killerPart then
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, killerPart.Position)
        end
    end
end)

-- [[ MENÜ TASARIMI ]] --
local Main = Window:MakeTab({Name = "Saldırı (Aim)", Icon = "rbxassetid://4483345998", PremiumOnly = false})

Main:AddToggle({
	Name = "Katili Kilitle (Hızlı)",
	Default = false,
	Callback = function(Value)
		_G.Aimbot = Value
	end    
})

Main:AddButton({
	Name = "Katilin Arkasına Işınlan",
	Callback = function()
        local killerPart = GetKiller()
        if killerPart then
            LocalPlayer.Character.HumanoidRootPart.CFrame = killerPart.CFrame * CFrame.new(0, 0, 3)
        end
	end
})

local Visuals = Window:MakeTab({Name = "Nitro ESP", Icon = "rbxassetid://4483345998", PremiumOnly = false})

_G.MasterESP = false
Visuals:AddToggle({
	Name = "Anlık İfşa ESP (Nitro)",
	Default = false,
	Callback = function(Value)
		_G.MasterESP = Value
	end    
})

-- 1 Salisede Yenilenen ESP
RunService.RenderStepped:Connect(function()
    if _G.MasterESP then
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
                    local bgui = v.Character.Head:FindFirstChild("LayroxcESP") or Instance.new("BillboardGui", v.Character.Head)
                    bgui.Name = "LayroxcESP"
                    bgui.AlwaysOnTop = true
                    bgui.Size = UDim2.new(0, 200, 0, 50)
                    bgui.ExtentsOffset = Vector3.new(0, 3, 0)

                    local lbl = bgui:FindFirstChild("TextLabel") or Instance.new("TextLabel", bgui)
                    lbl.Size = UDim2.new(1, 0, 1, 0)
                    lbl.BackgroundTransparency = 1
                    lbl.TextScaled = true
                    lbl.Font = Enum.Font.GothamBold
                    
                    -- Katil Tespiti
                    if v.Character:FindFirstChild("Knife") or v.Backpack:FindFirstChild("Knife") then
                        lbl.Text = "[KATİL] " .. v.Name
                        lbl.TextColor3 = Color3.fromRGB(255, 0, 0)
                    elseif v.Character:FindFirstChild("Gun") or v.Backpack:FindFirstChild("Gun") then
                        lbl.Text = "[ŞERİF] " .. v.Name
                        lbl.TextColor3 = Color3.fromRGB(0, 150, 255)
                    else
                        lbl.Text = "[MASUM] " .. v.Name
                        lbl.TextColor3 = Color3.fromRGB(0, 255, 0)
                    end
                end
            end)
        end
    else
        for _, v in pairs(Players:GetPlayers()) do
            pcall(function()
                if v.Character and v.Character.Head:FindFirstChild("LayroxcESP") then
                    v.Character.Head.LayroxcESP:Destroy()
                end
            end)
        end
    end
end)

local Pro = Window:MakeTab({Name = "Korblox & Pro", Icon = "rbxassetid://4483345998", PremiumOnly = false})

Pro:AddButton({
	Name = "Korblox Al (80 Robux)",
	Callback = function()
        MarketplaceService:PromptProductPurchase(LocalPlayer, 1812606767)
	end
})

Pro:AddButton({
	Name = "Headless (FE)",
	Callback = function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
            LocalPlayer.Character.Head.Transparency = 1
        end
	end
})

OrionLib:Init()
