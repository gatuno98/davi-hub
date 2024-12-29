local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Função para criar o ESP (nome do jogador acima da cabeça)
local function createESP(player)
    -- Verifica se o personagem do jogador está carregado
    local character = player.Character
    if not character then return end

    -- Espera a parte 'Head' do personagem ser carregada
    local head = character:WaitForChild("Head")

    -- Cria o BillboardGui para exibir o nome acima da cabeça
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Adornee = head  -- O nome será exibido sobre a cabeça
    billboardGui.Size = UDim2.new(0, 200, 0, 50)  -- Define o tamanho do rótulo
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)  -- Ajusta a posição do nome acima da cabeça

    -- Cria o TextLabel para mostrar o nome do jogador
    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = billboardGui
    textLabel.Text = player.Name  -- O texto será o nome do jogador
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)  -- Cor branca para o nome
    textLabel.TextSize = 20  -- Tamanho do texto
    textLabel.BackgroundTransparency = 1  -- Torna o fundo do texto transparente
    textLabel.TextStrokeTransparency = 0.5  -- Adiciona uma borda sutil para dar melhor visibilidade ao texto

    -- Adiciona o BillboardGui à CoreGui para garantir que seja visível para todos
    billboardGui.Parent = game.CoreGui
end

-- Atualiza a lista de jogadores e cria o ESP para cada um
RunService.Heartbeat:Connect(function()
    -- Para cada jogador no jogo
    for _, player in ipairs(Players:GetPlayers()) do
        -- Evita que a ESP seja criada para o próprio jogador
        if player ~= Players.LocalPlayer then
            -- Verifica se o ESP já foi criado para esse jogador
            if not player:FindFirstChild("ESP") then
                -- Cria o ESP para o jogador
                createESP(player)

                -- Marca o jogador com um valor para saber que o ESP foi criado
                local espTag = Instance.new("BoolValue")
                espTag.Name = "ESP"
                espTag.Parent = player
            end
        end
    end
end)
