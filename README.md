local player = game.Players.LocalPlayer
local camera = game.Workspace.CurrentCamera
local Players = game.Players

-- Função para desenhar as linhas e os nomes
local function drawPlayerInfo()
    -- A cada frame, verifica os outros jogadores
    for _, otherPlayer in pairs(Players:GetPlayers()) do
        -- Ignora o próprio jogador
        if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local otherPlayerRoot = otherPlayer.Character.HumanoidRootPart
            local playerRoot = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            
            if playerRoot then
                -- Cria uma linha entre os jogadores
                local line = Instance.new("Part")
                line.Size = Vector3.new(0.2, 0.2, (playerRoot.Position - otherPlayerRoot.Position).Magnitude)  -- Tamanho da linha
                line.Anchored = true
                line.CanCollide = false
                line.BrickColor = BrickColor.White()  -- Cor branca
                line.Material = Enum.Material.SmoothPlastic
                line.CFrame = CFrame.new(playerRoot.Position, otherPlayerRoot.Position) * CFrame.new(0, 0, -line.Size.Z / 2)
                line.Parent = game.Workspace
                
                -- Adiciona o nome do jogador acima da linha
                local nameLabel = Instance.new("BillboardGui")
                nameLabel.Adornee = otherPlayerRoot
                nameLabel.Size = UDim2.new(0, 100, 0, 50)
                nameLabel.StudsOffset = Vector3.new(0, 3, 0)
                nameLabel.Parent = game.Workspace
                
                local nameText = Instance.new("TextLabel")
                nameText.Size = UDim2.new(1, 0, 1, 0)
                nameText.BackgroundTransparency = 1
                nameText.Text = otherPlayer.Name
                nameText.TextColor3 = Color3.fromRGB(255, 255, 255)
                nameText.TextStrokeTransparency = 0.8
                nameText.Parent = nameLabel
            end
        end
    end
end

-- Chama a função a cada frame
game:GetService("RunService").RenderStepped:Connect(drawPlayerInfo)
