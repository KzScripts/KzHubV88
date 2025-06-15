local Players = game:GetService("Players")
local StarterGui = game:GetService("StarterGui")
local Tween = game:GetService("TweenService")
local player = Players.LocalPlayer

-- Notificação inicial
StarterGui:SetCore("SendNotification", {
    Title = "KzHub Key System",
    Text = "Insira sua key para acessar.",
    Duration = 5,
})

local usedKeys = {}

-- GUI principal
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "KeySystemUI"
screenGui.ResetOnSpawn = false

local frame = Instance.new("Frame", screenGui)
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.Position = UDim2.new(0.5,0,0.5,0)
frame.Size = UDim2.new(0, 360, 0, 200)
frame.BackgroundColor3 = Color3.fromRGB(20,20,30)
frame.BorderSizePixel = 0

local uiCorner = Instance.new("UICorner", frame)
uiCorner.CornerRadius = UDim.new(0,8)

-- Título
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1,0,0,40)
title.Position = UDim2.new(0,0,0,0)
title.BackgroundTransparency = 1
title.Text = "KzHub Key"
title.Font = Enum.Font.GothamBold
title.TextSize = 22
title.TextColor3 = Color3.fromRGB(200,200,255)

-- Caixa de input
local textBox = Instance.new("TextBox", frame)
textBox.Position = UDim2.new(0.05,0,0.25,0)
textBox.Size = UDim2.new(0.9,0,0,40)
textBox.BackgroundColor3 = Color3.fromRGB(30,30,45)
textBox.TextColor3 = Color3.fromRGB(230,230,230)
textBox.Font = Enum.Font.Gotham
textBox.PlaceholderText = "SUA-KEY-AKI"
textBox.TextSize = 18
textBox.ClearTextOnFocus = false

local corner2 = Instance.new("UICorner", textBox)
corner2.CornerRadius = UDim.new(0,6)

-- Botões
local function makeButton(text, col)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0.42,0,0,36)
    btn.BackgroundColor3 = col
    btn.Text = text
    btn.Font = Enum.Font.GothamSemibold
    btn.TextSize = 18
    btn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,6)
    return btn
end

local getBtn = makeButton("Get Key", Color3.fromRGB(50,100,200))
getBtn.Position = UDim2.new(0.05,0,0.55,0)

local verifyBtn = makeButton("Verificar", Color3.fromRGB(60,180,130))
verifyBtn.Position = UDim2.new(0.53,0,0.55,0)

-- Label de feedback
local msg = Instance.new("TextLabel", frame)
msg.Position = UDim2.new(0,0,0.85,0)
msg.Size = UDim2.new(1,0,0,24)
msg.BackgroundTransparency = 1
msg.Font = Enum.Font.Gotham
msg.TextSize = 16
msg.TextColor3 = Color3.fromRGB(200,100,100)
msg.Text = ""

-- Funções
getBtn.MouseButton1Click:Connect(function()
    setclipboard("https://vy61yt.mimo.run/index.html")
    msg.Text = "Link copiado!"
end)

local function notify(t, txt)
    StarterGui:SetCore("SendNotification", {Title=t, Text=txt, Duration=4})
end

local function valid(key)
    return key:find("KZFREE_KEY") ~= nil
end

verifyBtn.MouseButton1Click:Connect(function()
    local k = textBox.Text:upper()
    if not valid(k) then
        msg.Text = "Formato inválido"
        return notify("Erro","Formato inválido")
    end
    if usedKeys[k] then
        msg.Text = "Key já usada"
        return notify("Erro","Key repetida")
    end

    usedKeys[k] = true

    -- Animação de fechar corrigida
    local tween = Tween:Create(frame, TweenInfo.new(0.4), {
        Size = UDim2.new(0,0,0,0),
        Position = UDim2.new(0.5,0,0.5,0),
    })
    tween:Play()
    tween.Completed:Wait()

    screenGui:Destroy()
    loadstring(game:HttpGet("https://pastebin.com/raw/nAYXLuAV"))()
end)
