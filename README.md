-- VTZIN MENU COMPLETO BY GUI_VTZIN

local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local Janela = Rayfield:CreateWindow({
    Name = "VTZIN MENU | Completo Personalizado",
    LoadingTitle = "Carregando Menu...",
    LoadingSubtitle = "by GUI_VTZIN",
    ConfigurationSaving = { Enabled = false }
})

local RS = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local CarSpawner = RS:FindFirstChild("Concessionaria") and RS.Concessionaria.Remote.CarrosSpawner

local CorLinha = Color3.fromRGB(255, 0, 0)
local CorNick = Color3.fromRGB(255, 255, 255)

local function Godmode()
    local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.Health = math.huge
        humanoid.MaxHealth = math.huge
        humanoid:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
        humanoid:GetPropertyChangedSignal("Health"):Connect(function()
            if humanoid.Health <= 0 then humanoid.Health = math.huge end
        end)
        Rayfield:Notify({Title = "Godmode", Content = "Ativado!", Duration = 6})
    end
end

local function AtivarESP()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local char = player.Character

            local highlight = Instance.new("Highlight", char)
            highlight.FillColor = CorNick
            highlight.OutlineColor = Color3.fromRGB(0, 0, 0)
            highlight.FillTransparency = 0.5

            local a0 = Instance.new("Attachment", LocalPlayer.Character.HumanoidRootPart)
            local a1 = Instance.new("Attachment", char.HumanoidRootPart)
            local beam = Instance.new("Beam", a0)
            beam.Attachment0 = a0
            beam.Attachment1 = a1
            beam.FaceCamera = true
            beam.Width0 = 0.15
            beam.Width1 = 0.15
            beam.Color = ColorSequence.new(CorLinha)

            local gui = Instance.new("BillboardGui", char.HumanoidRootPart)
            gui.Size = UDim2.new(0,100,0,40)
            gui.AlwaysOnTop = true

            local texto = Instance.new("TextLabel", gui)
            texto.Size = UDim2.new(1,0,1,0)
            texto.BackgroundTransparency = 1
            texto.TextColor3 = CorNick
            texto.TextStrokeTransparency = 0
            texto.TextScaled = true
            texto.Font = Enum.Font.GothamBold

            game:GetService("RunService").Heartbeat:Connect(function()
                if char and char:FindFirstChild("HumanoidRootPart") then
                    local dist = (LocalPlayer.Character.HumanoidRootPart.Position - char.HumanoidRootPart.Position).Magnitude
                    texto.Text = player.DisplayName.." | "..math.floor(dist).."m"
                    highlight.FillColor = CorNick
                    beam.Color = ColorSequence.new(CorLinha)
                    texto.TextColor3 = CorNick
                end
            end)
        end
    end
    Rayfield:Notify({Title = "ESP", Content = "Ativado com sucesso!", Duration = 6})
end

local ESP_Tab = Janela:CreateTab("ESP", 4483362458)
ESP_Tab:CreateColorPicker({
    Name = "Cor da Linha",
    Color = CorLinha,
    Callback = function(Value)
        CorLinha = Value
        Rayfield:Notify({Title = "Cor Linha", Content = "Cor da linha atualizada!", Duration = 5})
    end
})
ESP_Tab:CreateColorPicker({
    Name = "Cor do Nick",
    Color = CorNick,
    Callback = function(Value)
        CorNick = Value
        Rayfield:Notify({Title = "Cor Nick", Content = "Cor do nick atualizada!", Duration = 5})
    end
})
ESP_Tab:CreateButton({Name = "Ativar ESP Hitbox + Linha + Nick", Callback = AtivarESP})

local Carros = Janela:CreateTab("Carros", 4483362458)
for _, v in ipairs({"Mercedes G63","Ferrari 488","Golf GTI","Civic Type R","Helicoptero"}) do
    Carros:CreateButton({
        Name = v,
        Callback = function()
            if CarSpawner then
                CarSpawner:FireServer(v)
                Rayfield:Notify({Title = "Spawnado", Content = v.." spawnado!", Duration = 5})
            else
                Rayfield:Notify({Title = "Erro", Content = "Spawner não encontrado", Duration = 5})
            end
        end
    })
end

local Motos = Janela:CreateTab("Motos", 4483362458)
for _, v in ipairs({"XJ6","Africa Twin","Titan 160"}) do
    Motos:CreateButton({
        Name = v,
        Callback = function()
            if CarSpawner then
                CarSpawner:FireServer(v)
                Rayfield:Notify({Title = "Spawnado", Content = v.." spawnada!", Duration = 5})
            end
        end
    })
end

local Aimbot = Janela:CreateTab("Aimbot",4483362458)
Aimbot:CreateButton({Name="Aimbot Volcano",Callback=function() loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Volcano-universal-aimbot-36995"))() end})
Aimbot:CreateButton({Name="Aimbot FOV Mobile",Callback=function() loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Aimbot-fov-mobile-7607"))() end})

local Extras = Janela:CreateTab("Extras",4483362458)
Extras:CreateButton({Name="Godmode",Callback=Godmode})
Extras:CreateButton({Name="Revistar",Callback=function() local r=RS:FindFirstChild("RevistarEvent") if r then r:FireServer() end end})
Extras:CreateButton({Name="Roubar",Callback=function() local r=RS:FindFirstChild("RoubarEvent") if r then r:FireServer() end end})

Rayfield:Notify({Title="Menu Completo",Content="Menu carregado com ESP configurável!",Duration=7})
