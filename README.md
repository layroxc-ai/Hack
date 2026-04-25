-- [[ LAYROXC HUB v17 - HYPER GRAB GUN FIX ]] --
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Layroxc Hub MM2", "DarkTheme")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- BUTON (L)
local OpenGui = Instance.new("ScreenGui", game.CoreGui)
local OpenButton = Instance.new("TextButton", OpenGui)
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0, 10, 0.4, 0)
OpenButton.Text = "L"
OpenButton.Draggable = true 
local UIKose = Instance.new("UICorner", OpenButton)
OpenButton.MouseButton1Click:Connect(function() Library:ToggleUI() end)

local Combat = Window:NewTab("Saldırı")
local CombatSec = Combat:NewSection("Silah Çekici (Fixed)")

-- EN GÜÇLÜ GRAB GUN KODU
CombatSec:NewToggle("Hyper Magnet Grab Gun", "Silah düştüğü an sana ışınlanır", function(state)
    _G.GrabGun = state
    while _G.GrabGun do
        -- Sadece isme bakmaz, objenin içindeki 'Gun' özelliğini de arar
        for _, v in pairs(workspace:GetDescendants()) do
            if (v.Name == "GunDrop" or v.Name == "Gun" or v:IsA("TouchTransmitter")) and v.Parent:IsA("Model") then
                if v.Parent:FindFirstChild("Handle") then
                    v.Parent.Handle.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
                end
            end
        end
        task.wait(0.1) -- Tarama hızını artırdım
    end
end)

CombatSec:NewButton("Katili İfşala & Vur", "Aimbot + ESP Aktif Eder", function()
    -- ESP ve Aimbot'u tek tuşla açar
    _G.Aimbot = true
    _G.MasterESP = true
end)

-- SOSYAL
local Social = Window:NewTab("Sosyal")
Social:NewSection("TikTok: @layroxcderler")
Social:NewButton("Profil Kopyala", "Takip et kanki!", function()
    setclipboard("https://www.tiktok.com/@layroxcderler")
end)
