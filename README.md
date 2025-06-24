if game.CoreGui:FindFirstChild("JolgueHub") then game.CoreGui.JolgueHub:Destroy() end

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "JolgueHub"
gui.ResetOnSpawn = false

local restoreBtn = Instance.new("TextButton", gui)
restoreBtn.Size = UDim2.new(0, 140, 0, 35)
restoreBtn.Position = UDim2.new(0, 10, 0.5, -20)
restoreBtn.BackgroundColor3 = Color3.fromRGB(30,30,30)
restoreBtn.TextColor3 = Color3.new(1,1,1)
restoreBtn.Font = Enum.Font.GothamBold
restoreBtn.TextSize = 16
restoreBtn.Text = "Abrir Jolgue Hub"
restoreBtn.Visible = false
Instance.new("UICorner", restoreBtn)

local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 320, 0, 400)
mainFrame.Position = UDim2.new(0.5, -160, 0.5, -200)
mainFrame.BackgroundColor3 = Color3.fromRGB(45,45,45)
Instance.new("UICorner", mainFrame)

local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, -50, 0, 40)
title.Position = UDim2.new(0, 10, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Jolgue Hub"
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.TextSize = 24
title.TextXAlignment = Enum.TextXAlignment.Left

local minimize = Instance.new("TextButton", mainFrame)
minimize.Size = UDim2.new(0, 35, 0, 35)
minimize.Position = UDim2.new(1, -40, 0, 5)
minimize.BackgroundColor3 = Color3.fromRGB(70,70,70)
minimize.TextColor3 = Color3.new(1,1,1)
minimize.Font = Enum.Font.GothamBold
minimize.TextSize = 20
minimize.Text = "-"
Instance.new("UICorner", minimize)

minimize.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    restoreBtn.Visible = true
end)
restoreBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    restoreBtn.Visible = false
end)

local function criarBotao(txt, parent, func)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(0, 100, 1, 0)
    btn.Text = txt
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 16
    btn.TextColor3 = Color3.new(1,1,1)
    btn.BackgroundColor3 = Color3.fromRGB(65,65,65)
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(func)
    return btn
end

local navHolder = Instance.new("Frame", mainFrame)
navHolder.Size = UDim2.new(1, -20, 0, 45)
navHolder.Position = UDim2.new(0, 10, 0, 45)
navHolder.BackgroundTransparency = 1

local navScroll = Instance.new("ScrollingFrame", navHolder)
navScroll.Size = UDim2.new(1, 0, 1, 0)
navScroll.CanvasSize = UDim2.new(0, 400, 0, 0)
navScroll.ScrollBarThickness = 4
navScroll.ScrollingDirection = Enum.ScrollingDirection.X
navScroll.BackgroundTransparency = 1
navScroll.BorderSizePixel = 0

local navLayout = Instance.new("UIListLayout", navScroll)
navLayout.FillDirection = Enum.FillDirection.Horizontal
navLayout.Padding = UDim.new(0, 8)
navLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left

local abaPrincipal = Instance.new("Frame", mainFrame)
abaPrincipal.Size = UDim2.new(1, -20, 0, 280)
abaPrincipal.Position = UDim2.new(0, 10, 0, 90)
abaPrincipal.BackgroundTransparency = 1
abaPrincipal.Visible = true
Instance.new("UIListLayout", abaPrincipal).Padding = UDim.new(0, 8)

local abaArmas = Instance.new("Frame", mainFrame)
abaArmas.Size = abaPrincipal.Size
abaArmas.Position = abaPrincipal.Position
abaArmas.BackgroundTransparency = 1
abaArmas.Visible = false
Instance.new("UIListLayout", abaArmas).Padding = UDim.new(0, 8)

criarBotao("Principal", navScroll, function()
    abaPrincipal.Visible = true
    abaArmas.Visible = false
end).BackgroundColor3 = Color3.fromRGB(30, 100, 200)

criarBotao("Armas", navScroll, function()
    abaPrincipal.Visible = false
    abaArmas.Visible = true
end).BackgroundColor3 = Color3.fromRGB(180, 40, 40)

criarBotao("Fechar", navScroll, function()
    gui:Destroy()
end).BackgroundColor3 = Color3.fromRGB(90, 90, 90)

local function criarBotaoAba(texto, parent, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.Text = texto
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 16
    btn.TextColor3 = Color3.new(1,1,1)
    btn.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
    Instance.new("UICorner", btn)
    btn.Parent = parent
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- ✅ Girar 25x por segundo
local girar = false

criarBotaoAba("Girar Player", abaPrincipal, function()
    girar = not girar
    local btn = abaPrincipal:FindFirstChildWhichIsA("TextButton", true)
    btn.Text = girar and "Parar Giro" or "Girar Player"

    if girar then
        task.spawn(function()
            local anglePerFrame = math.rad(9000) * (1/25)
            while girar do
                local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
                local hrp = char:FindFirstChild("HumanoidRootPart")
                if hrp then
                    local pos = CFrame.new(hrp.Position)
                    local rot = hrp.CFrame - hrp.Position
                    hrp.CFrame = pos * rot * CFrame.Angles(0, anglePerFrame, 0)
                end
                task.wait(1/25)
            end
        end)
    end
end)

criarBotaoAba("Infinite Yield", abaPrincipal, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
end)

local function criarTool(nome, cor, onActivate)
    local tool = Instance.new("Tool")
    tool.Name = nome
    tool.RequiresHandle = true

    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(1, 1, 4)
    handle.BrickColor = BrickColor.new(cor)
    handle.Material = Enum.Material.Metal
    handle.CanCollide = false
    handle.Anchored = false
    handle.Parent = tool

    tool.Activated:Connect(onActivate)
    return tool
end

criarBotaoAba("Espada", abaArmas, function()
    local espada = criarTool("EspadaTroll", "Really red", function()
        local char = LocalPlayer.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if hum then hum:TakeDamage(1) end
    end)
    espada.Parent = LocalPlayer.Backpack
end)

criarBotaoAba("Arma", abaArmas, function()
    local arma = criarTool("ArmaFake", "Black", function()
        local origin = LocalPlayer.Character.Head.Position
        local dir = LocalPlayer.Character.Head.CFrame.LookVector

        local bala = Instance.new("Part")
        bala.Size = Vector3.new(0.3, 0.3, 0.3)
        bala.Shape = Enum.PartType.Ball
        bala.Material = Enum.Material.Neon
        bala.BrickColor = BrickColor.new("Bright yellow")
        bala.Position = origin + dir * 2
        bala.CanCollide = false
        bala.Anchored = false
        bala.Parent = workspace

        local bv = Instance.new("BodyVelocity", bala)
        bv.Velocity = dir * 150
        bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)

        local sound = Instance.new("Sound", bala)
        sound.SoundId = "rbxassetid://138186576"
        sound.Volume = 1
        sound:Play()

        bala.Touched:Connect(function(hit)
            local hum = hit.Parent and hit.Parent:FindFirstChildOfClass("Humanoid")
            if hum and hum ~= LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
                hum:TakeDamage(5)
                bala:Destroy()
            end
        end)

        game.Debris:AddItem(bala, 5)
    end)
    arma.Parent = LocalPlayer.Backpack
end)
