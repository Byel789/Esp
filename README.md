-- Configurações do ESP
local ESPColor = Color3.fromRGB(255, 0, 0) -- Cor da caixa (vermelho)
local ESPThickness = 2 -- Espessura da linha da caixa

-- Função para criar a caixa ESP
local function createESP(player)
    local Box = Drawing.new("Square")
    Box.Visible = false
    Box.Color = ESPColor
    Box.Thickness = ESPThickness
    Box.Filled = false

    local function update()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local rootPart = player.Character.HumanoidRootPart
            local vector, onScreen = workspace.CurrentCamera:WorldToViewportPoint(rootPart.Position)
            if onScreen then
                Box.Size = Vector2.new(2000 / vector.Z, 3000 / vector.Z)
                Box.Position = Vector2.new(vector.X - Box.Size.X / 2, vector.Y - Box.Size.Y / 2)
                Box.Visible = true
            else
                Box.Visible = false
            end
        else
            Box.Visible = false
        end
    end

    game:GetService("RunService").RenderStepped:Connect(update)
end

-- Aplicar ESP para todos os jogadores
for _, player in pairs(game:GetService("Players"):GetPlayers()) do
    if player ~= game.Players.LocalPlayer then
        createESP(player)
    end
end

-- Aplicar ESP para novos jogadores
game:GetService("Players").PlayerAdded:Connect(function(player)
    if player ~= game.Players.LocalPlayer then
        createESP(player)
    end
end)
