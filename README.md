    Knob.Position = UDim2.new(p, 0, 0.5, 0)
end

local function atualizarSlider(input)
    local pos = math.clamp((input.Position.X - Track.AbsolutePosition.X) / Track.AbsoluteSize.X, 0, 1)
    atualizarEscala(10 + pos * 190)
end

Track.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        sliderDragging = true
        atualizarSlider(input)
    end
end)

UIS.InputChanged:Connect(function(input)
    if sliderDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        atualizarSlider(input)
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        sliderDragging = false
    end
end)

ConfigButton.MouseButton1Click:Connect(function()
    Settings.Visible = not Settings.Visible
    if Settings.Visible then
        SettingsScale.Scale = 0.8
        tween(SettingsScale, 0.18, {Scale = 1})
    end
end)

close.MouseButton1Click:Connect(function()
    Settings.Visible = false
end)

-- Arrastar painel com mouse ou toque
local dragging = false
local dragStart, startPos

DragArea.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = Frame.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        Frame.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

MinButton.MouseButton1Click:Connect(function()
    Frame.Visible = false
    Bubble.Visible = true
end)

Bubble.MouseButton1Click:Connect(function()
    Frame.Visible = true
    Bubble.Visible = false
end)

-- Arrastar botao MENU no celular/PC
local bubbleDragging = false
local bubbleStart, bubblePos

Bubble.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        bubbleDragging = true
        bubbleStart = input.Position
        bubblePos = Bubble.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if bubbleDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - bubbleStart
        Bubble.Position = UDim2.new(
            bubblePos.X.Scale,
            bubblePos.X.Offset + delta.X,
            bubblePos.Y.Scale,
            bubblePos.Y.Offset + delta.Y
        )
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        bubbleDragging = false
    end
end)

-- =========================================================
-- AUTO CAIXA
-- =========================================================
local CaixaBody = CriarSecao("AUTO CAIXA", 1, 48, 180)
local CaixaStatus = makeLabel(CaixaBody, "Status: DESATIVADO", 10, 25, 12)
local CaixaButton = makeButton(CaixaBody, "ATIVAR AUTO CAIXA", 12, 45, 0.46, 35, OFF)
local CaixaSpeedLabel = makeLabel(CaixaBody, "Velocidade: 24", 12, 88, 20, 11)

local CaixaMinus = makeButton(CaixaBody, "-", 12, 115, 0.18, 30, OFF)
local CaixaPlus = makeButton(CaixaBody, "+", 0.22, 115, 0.18, 30, OFF)

local function setCaixaStatus(text, color)
    CaixaStatus.Text = "Status: " .. text
    CaixaStatus.TextColor3 = color or WHITE
end

CaixaButton.MouseButton1Click:Connect(function()
    Hub.CaixaAtivo = not Hub.CaixaAtivo
    setButtonActive(CaixaButton, Hub.CaixaAtivo, "DESATIVAR AUTO CAIXA", "ATIVAR AUTO CAIXA")
    setCaixaStatus(Hub.CaixaAtivo and "ATIVO" or "DESATIVADO", Hub.CaixaAtivo and GREEN or WHITE)
end)

CaixaMinus.MouseButton1Click:Connect(function()
    Hub.CaixaVelocidade = math.max(5, Hub.CaixaVelocidade - 1)
    CaixaSpeedLabel.Text = "Velocidade: " .. Hub.CaixaVelocidade
end)

CaixaPlus.MouseButton1Click:Connect(function()
    Hub.CaixaVelocidade = math.min(100, Hub.CaixaVelocidade + 1)
    CaixaSpeedLabel.Text = "Velocidade: " .. Hub.CaixaVelocidade
end)

-- =========================================================
-- AUTO VARRER
-- =========================================================
local VarrerBody = CriarSecao("AUTO VARRER", 2, 48, 180)
local VarrerStatus = makeLabel(VarrerBody, "Status: DESATIVADO", 10, 25, 12)
local VarrerButton = makeButton(VarrerBody, "ATIVAR AUTO VARRER", 12, 45, 0.46, 35, OFF)
local VarrerSpeedLabel = makeLabel(VarrerBody, "Velocidade: 24", 12, 88, 20, 11)

local VarrerMinus = makeButton(VarrerBody, "-", 12, 115, 0.18, 30, OFF)
local VarrerPlus = makeButton(VarrerBody, "+", 0.22, 115, 0.18, 30, OFF)

VarrerButton.MouseButton1Click:Connect(function()
    Hub.VarrerAtivo = not Hub.VarrerAtivo
    setButtonActive(VarrerButton, Hub.VarrerAtivo, "DESATIVAR AUTO VARRER", "ATIVAR AUTO VARRER")
    VarrerStatus.Text = "Status: " .. (Hub.VarrerAtivo and "ATIVO" or "DESATIVADO")
    VarrerStatus.TextColor3 = Hub.VarrerAtivo and GREEN or WHITE
end)

VarrerMinus.MouseButton1Click:Connect(function()
    Hub.Velocidade = math.max(5, Hub.Velocidade - 1)
    VarrerSpeedLabel.Text = "Velocidade: " .. Hub.Velocidade
end)

VarrerPlus.MouseButton1Click:Connect(function()
    Hub.Velocidade = math.min(100, Hub.Velocidade + 1)
    VarrerSpeedLabel.Text = "Velocidade: " .. Hub.Velocidade
end)

-- =========================================================
-- EMPILHADOR
-- =========================================================
local EmpBody = CriarSecao("AUTO EMPILHADOR", 3, 48, 210)
local EmpStatus = makeLabel(EmpBody, "Status: DESATIVADO", 10, 25, 12)
local EmpButton = makeButton(EmpBody, "ATIVAR EMPILHADOR", 12, 45, 0.46, 35, OFF)

EmpButton.MouseButton1Click:Connect(function()
    Emp.Running = not Emp.Running
    Emp.Token = Emp.Token + 1
    setButtonActive(EmpButton, Emp.Running, "DESATIVAR EMPILHADOR", "ATIVAR EMPILHADOR")
    EmpStatus.Text = "Status: " .. (Emp.Running and "ATIVO" or "DESATIVADO")
    EmpStatus.TextColor3 = Emp.Running and GREEN or WHITE
end)

-- =========================================================
-- MINERADOR
-- =========================================================
local MinerBody = CriarSecao("MINERADOR", 4, 48, 260)
local MinerStatus = makeLabel(MinerBody, "Status: DESATIVADO", 10, 25, 12)
local MinerButton = makeButton(MinerBody, "ATIVAR MINERADOR", 12, 45, 0.46, 35, OFF)

local MinerState = {
    Running = false,
    Mining = false,
    Token = 0
}
env.XLZMinerador = MinerState

MinerButton.MouseButton1Click:Connect(function()
    MinerState.Running = not MinerState.Running
    MinerState.Token = MinerState.Token + 1
    setButtonActive(MinerButton, MinerState.Running, "DESATIVAR MINERADOR", "ATIVAR MINERADOR")
    MinerStatus.Text = "Status: " .. (MinerState.Running and "ATIVO" or "DESATIVADO")
    MinerStatus.TextColor3 = MinerState.Running and GREEN or WHITE
end)

-- =========================================================
-- SISTEMA DE MOVIMENTO
-- =========================================================
local function moverPara(posicao, velocidade, ativo)
    local hrp, humanoid = Personagem()
    if not hrp or not humanoid then return false end
    if not AdquirirMovimento("AUTO", ativo) then return false end

    local inicio = os.clock()
    local limite = 20

    while ativo() and (os.clock() - inicio) < limite do
        hrp, humanoid = Personagem()
        if not hrp or not humanoid then break end

        local distancia = (hrp.Position - posicao).Magnitude
        if distancia <= 3 then
            LiberarMovimento("AUTO")
            return true
        end

        humanoid:MoveTo(posicao)
        task.wait(0.1)
    end

    LiberarMovimento("AUTO")
    return false
end

-- =========================================================
-- ROTINAS AUTOMATICAS
-- =========================================================
task.spawn(function()
    while task.wait(0.2) do
        if Hub.CaixaAtivo then
            local hrp, humanoid = Personagem()
            if hrp and humanoid then
                local destino = hrp.Position + hrp.CFrame.LookVector * Hub.CaixaVelocidade
                moverPara(destino, Hub.CaixaVelocidade, function()
                    return Hub.CaixaAtivo
                end)
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.2) do
        if Hub.VarrerAtivo then
            local hrp, humanoid = Personagem()
            if hrp and humanoid then
                local destino = hrp.Position + hrp.CFrame.LookVector * Hub.Velocidade
                moverPara(destino, Hub.Velocidade, function()
                    return Hub.VarrerAtivo
                end)
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.2) do
        if Emp.Running then
            local token = Emp.Token
            local hrp, humanoid = Personagem()
            if hrp and humanoid then
                local destino = hrp.Position + Vector3.new(0, 8, 0)
                moverPara(destino, 24, function()
                    return Emp.Running and Emp.Token == token
                end)
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.2) do
        if MinerState.Running then
            local token = MinerState.Token
            local hrp, humanoid = Personagem()
            if hrp and humanoid then
                local destino = hrp.Position + hrp.CFrame.LookVector * 10
                moverPara(destino, 18, function()
                    return MinerState.Running and MinerState.Token == token
                end)
            end
        end
    end
end)            if distance < bestDistance then
                bestDistance = distance
                bestPrompt = item
            end
        end
    end
    return bestPrompt, bestDistance
end

local function activatePrompt(prompt)
    if not prompt or not prompt.Parent or not prompt.Enabled then return false end
    local ok = pcall(function()
        prompt:InputHoldBegin()
        task.wait(State.HoldTime)
        prompt:InputHoldEnd()
    end)
    return ok
end

local function equipPickaxe()
    if hasEquippedPickaxe() then return true end
    local backpack = Player:FindFirstChildOfClass("Backpack")
    if not backpack then return false end

    local tool
    for _, item in ipairs(backpack:GetChildren()) do
        if item:IsA("Tool") then
            local n = string.lower(item.Name)
            if string.find(n, "picareta", 1, true) or string.find(n, "pickaxe", 1, true) then
                tool = item
                break
            end
        end
    end
    if not tool then return false end

    Humanoid:EquipTool(tool)
    task.wait(0.25)
    return hasEquippedPickaxe()
end

local function pegarPicareta()
    if hasEquippedPickaxe() then return true end
    if not PickPrompt then return equipPickaxe() end

    State.Status = "Indo pegar a picareta"
    local part = getPromptPart(PickPrompt)
    if not part then return false end

    local target = part.CFrame * CFrame.new(0, 0, -2)
    if not walkTo(target) then return false end

    task.wait(1)
    activatePrompt(PickPrompt)
    task.wait(0.3)

    -- Alguns jogos colocam a ferramenta direto na mochila.
    if not hasEquippedPickaxe() then
        equipPickaxe()
    end
    return hasEquippedPickaxe()
end

local function contarMinerios()
    local backpack = Player:FindFirstChildOfClass("Backpack")
    local count = 0

    local function verificar(container)
        if not container then return end
        for _, item in ipairs(container:GetChildren()) do
            if item:IsA("Tool") then
                local n = string.lower(item.Name)
                if string.find(n, "miner", 1, true) or string.find(n, "ore", 1, true) then
                    count = count + 1
                end
            end
        end
    end

    verificar(backpack)
    verificar(Character)
    return count
end

-- =========================================================
-- SISTEMA DE VENDA AUTOMATICA
-- =========================================================
local VirtualInputManager = game:GetService("VirtualInputManager")

-- A deteccao e feita uma vez no inicio da venda para evitar que a
-- coordenada mude no meio da sequencia.
local function detectarDispositivo()
    if UIS.TouchEnabled and not UIS.KeyboardEnabled then
        return "CELULAR"
    end
    return "PC"
end

local CoordenadasVenda = {
    PC = {
        { x = 462, y = 500 }, -- Vender 500
        { x = 566, y = 504 }, -- Vender 750
        { x = 682, y = 502 }, -- Vender 1K
        { x = 790, y = 506 }, -- Vender 6K
        { x = 886, y = 506 }, -- Vender 25K
    },
    CELULAR = {
        { x = 314, y = 278 }, -- Vender 500
        { x = 386, y = 280 }, -- Vender 750
        { x = 459, y = 277 }, -- Vender 1K
        { x = 524, y = 278 }, -- Vender 6K
        { x = 590, y = 277 }, -- Vender 25K
    }
}

local CoordenadasFechar = {
    PC = { x = 952, y = 256 },
    CELULAR = { x = 639, y = 143 }
}

local function clicarCoordenada(x, y, dispositivo)
    if dispositivo == "CELULAR" then
        -- Envia toque na tela. Nao usa clique de mouse no modo celular.
        pcall(function()
            VirtualInputManager:SendTouchEvent(1, Enum.UserInputState.Begin, x, y)
            task.wait(0.05)
            VirtualInputManager:SendTouchEvent(1, Enum.UserInputState.End, x, y)
        end)
    else
        pcall(function()
            VirtualInputManager:SendMouseMoveEvent(x, y, game)
            task.wait(0.02)
            VirtualInputManager:SendMouseButtonEvent(x, y, 0, true, game, 0)
            task.wait(0.05)
            VirtualInputManager:SendMouseButtonEvent(x, y, 0, false, game, 0)
        end)
    end
end

local function clicarBotoesVenda(dispositivo)
    local lista = CoordenadasVenda[dispositivo]
    if not lista then
        warn("[XLZ] Nao encontrei coordenadas para o dispositivo:", dispositivo)
        return false
    end

    for i, coord in ipairs(lista) do
        if not State.Running then return false end
        print("[XLZ] Venda", i, "em", dispositivo, coord.x, coord.y)
        clicarCoordenada(coord.x, coord.y, dispositivo)
        task.wait(1)
    end

    local fechar = CoordenadasFechar[dispositivo]
    if fechar then
        clicarCoordenada(fechar.x, fechar.y, dispositivo)
        task.wait(0.15)
    end
    return true
end

local function executarVenda()
    local dispositivo = detectarDispositivo()
    State.Status = "Vendendo pelo modo " .. dispositivo
    warn("[XLZ] Dispositivo selecionado para venda:", dispositivo)

    local ok = clicarBotoesVenda(dispositivo)
    if ok then
        State.Status = "Venda concluida (" .. dispositivo .. ")"
    end
    return ok
end

local function venderMinerios()
    State.Status = "Preparando venda"
    local dispositivo = detectarDispositivo()

    -- Mantem o mesmo dispositivo em toda a rotina.
    local ok = clicarBotoesVenda(dispositivo)
    if ok then
        State.Status = "Venda concluida (" .. dispositivo .. ")"
    end
    return ok
end

-- =========================================================
-- ROTINA DE MINERACAO
-- =========================================================
local function minerarPrompt(prompt)
    if not State.Running or not prompt or not prompt.Parent then return false end

    local part = getPromptPart(prompt)
    if not part then return false end

    State.Status = "Aproximando do minerio"
    local alvo = part.CFrame * CFrame.new(0, 0, -2)
    if not walkTo(alvo, prompt) then
        -- Se o prompt ja ficou visivel, pode continuar.
        if not State.VisiblePrompts[prompt] then return false end
    end

    task.wait(0.15)
    if not State.Running then return false end

    State.Status = "Minerando por " .. tostring(State.HoldTime) .. " segundos"
    State.Mining = true
    local ok = activatePrompt(prompt)
    State.Mining = false

    if ok then
        State.LastOre[getOreKey(prompt, MineFolder:FindFirstChild("Pedras"))] = os.clock()
        MineriosDesdeVenda = MineriosDesdeVenda + 1
        State.Status = "Minerio coletado (" .. MineriosDesdeVenda .. "/" .. ORES_POR_VENDA .. ")"
        return true
    end
    return false
end

local function voltarAoPontoInicial()
    if not State.Running then return end
    State.Status = "Retornando ao ponto de mineracao"
    stopFlight()
    task.wait(0.15)
    walkTo(Points[1])
    task.wait(0.15)
end

local function cicloMinerador()
    State.Status = "Verificando picareta"
    if not equipPickaxe() then
        if not pegarPicareta() then
            State.Status = "Nao foi possivel pegar a picareta"
            task.wait(1)
            return
        end
    end

    while State.Running do
        local prompt = findNearestMiningPrompt()
        if prompt then
            if minerarPrompt(prompt) then
                if MineriosDesdeVenda >= ORES_POR_VENDA then
                    State.Status = "Limite atingido; iniciando venda"
                    venderMinerios()
                    MineriosDesdeVenda = 0
                    voltarAoPontoInicial()
                end
            else
                task.wait(0.5)
            end
        else
            State.Status = "Nenhum minerio disponivel; aguardando"
            task.wait(1)
        end
    end
end

local function iniciarMinerador()
    if State.Running then return end
    State.Running = true
    State.Token = (State.Token or 0) + 1
    local meuToken = State.Token

    task.spawn(function()
        while State.Running and State.Token == meuToken do
            cicloMinerador()
            task.wait(0.2)
        end
        stopFlight()
    end)
end

local function pararMinerador()
    State.Running = false
    State.Mining = false
    State.Token = (State.Token or 0) + 1
    stopFlight()
    State.Status = "Desativado"
end

-- =========================================================
-- CONEXAO COM O PAINEL
-- =========================================================
local function atualizarStatusMinerador()
    if MinerStatus then
        MinerStatus.Text = "Status: " .. (State.Running and "ATIVO" or "DESATIVADO")
        MinerStatus.TextColor3 = State.Running and GREEN or WHITE
    end
end

local function alternarMinerador()
    if State.Running then
        pararMinerador()
    else
        iniciarMinerador()
    end
    atualizarStatusMinerador()
end

if MinerButton then
    MinerButton.MouseButton1Click:Connect(alternarMinerador)
end

-- Atualiza o texto de status sem interferir no funcionamento.
task.spawn(function()
    while Gui and Gui.Parent do
        if MinerStatus and State then
            MinerStatus.Text = "Status: " .. (State.Running and State.Status or "DESATIVADO")
            MinerStatus.TextColor3 = State.Running and GREEN or WHITE
        end
        task.wait(0.5)
    end
end)                if distance < bestDistance then
                    bestPrompt, bestDistance = item, distance
                end
            end
        end
    end
    return bestPrompt, bestDistance
end

local function holdMiningPrompt(prompt)
    if not prompt or not prompt.Parent then return false end

    local began = false
    local completedFullHold = false
    local ok, err = pcall(function()
        prompt:InputHoldBegin()
        began = true

        -- Garante pelo menos 16 segundos reais de retenção.
        -- Se o próprio prompt exigir mais tempo, respeita o HoldDuration dele.
        local required = math.max(16, tonumber(prompt.HoldDuration) or 0)
        local started = os.clock()
        local lastStatusUpdate = 0

        while os.clock() - started < required do
            -- Se o jogador desligar a mineração, libera o botão imediatamente.
            if not State.Running or not State.Mining then
                break
            end
            if not prompt.Parent then
                break
            end

            local elapsed = os.clock() - started
            if elapsed - lastStatusUpdate >= 0.5 then
                State.Status = "Segurando mineracao: "
                    .. tostring(math.min(math.floor(elapsed), required))
                    .. "/" .. tostring(required) .. " segundos"
                lastStatusUpdate = elapsed
            end

            task.wait(0.05)
        end

        completedFullHold = (os.clock() - started) >= required
    end)

    if began then
        pcall(function()
            prompt:InputHoldEnd()
        end)
    end

    if not ok then
        warn("[PRIME X] Erro durante a retencao do prompt: " .. tostring(err))
        return false
    end

    -- Não contar como minério coletado se a retenção foi interrompida antes de 16s.
    return began and completedFullHold and State.Running and State.Mining
end

local function activatePickaxe()
    if hasEquippedPickaxe() then
        State.Status = "Picareta ja equipada"
        return true
    end
    if not PickPrompt then
        State.Status = "Erro: prompt da picareta nao encontrado"
        warn("[XLZ] Prompt de pegar picareta nao encontrado.")
        return false
    end

    local part = getPromptPart(PickPrompt)
    if not part then
        State.Status = "Erro: local da picareta nao encontrado"
        return false
    end

    State.Status = "Andando ate a picareta"
    local hum, root = getHumanoidRoot()
    local maxDistance = PickPrompt.MaxActivationDistance or 10
    if not root then return false end

    -- Nao tenta caminhar para dentro da peca: escolhe um ponto livre ao lado dela.
    local direction = Vector3.new(root.Position.X - part.Position.X, 0, root.Position.Z - part.Position.Z)
    if direction.Magnitude < 0.1 then direction = Vector3.new(0, 0, 1) end
    direction = direction.Unit
    local standOff = math.max(2, math.min(3.5, maxDistance * 0.4))
    local pickaxeApproach = part.Position + direction * standOff
    pickaxeApproach = Vector3.new(pickaxeApproach.X, part.Position.Y + 2.5, pickaxeApproach.Z)

    if not walkTo(pickaxeApproach) then
        State.Status = "Nao consegui chegar andando perto da picareta"
        return false
    end

    -- Se ainda estiver fora do alcance, tenta mais perto sem entrar na peca.
    hum, root = getHumanoidRoot()
    if root and horizontalDistance(root.Position, part.Position) > maxDistance * 0.9 then
        local closer = part.Position + direction * math.max(1.25, standOff - 1.25)
        if not walkTo(closer) then
            State.Status = "Nao alcancei o botao da picareta"
            return false
        end
    end

    State.Status = "Aguardando 1 segundo perto da picareta"
    task.wait(1)

    hum, root = getHumanoidRoot()
    if not root or not PickPrompt.Parent or not PickPrompt.Enabled then
        State.Status = "Prompt da picareta indisponivel"
        return false
    end
    if horizontalDistance(root.Position, part.Position) > maxDistance then
        State.Status = "Muito longe do botao da picareta"
        return false
    end

    State.Status = "Pegando picareta"
    local began = false
    local ok = pcall(function()
        PickPrompt:InputHoldBegin()
        began = true
        local duration = math.max(PickPrompt.HoldDuration or 0, 0.5)
        local t = os.clock()
        while State.Running and State.Mining and PickPrompt.Parent and os.clock() - t < duration do
            task.wait(0.1)
        end
    end)
    if began then pcall(function() PickPrompt:InputHoldEnd() end) end

    task.wait(0.5)
    if hasEquippedPickaxe() then
        State.Status = "Picareta equipada"
        return true
    end

    State.Status = "Interacao enviada; picareta nao confirmada"
    return ok and began
end

local function findMiningPromptAtPoint(center, radius)
    if not MineFolder then return nil end
    local stones = MineFolder:FindFirstChild("Pedras")
    if not stones then return nil end
    local bestPrompt, bestDistance = nil, radius or 18
    local now = os.clock()
    for _, item in ipairs(stones:GetDescendants()) do
        if isMiningPrompt(item) and item.Enabled then
            local part = getPromptPart(item)
            local key = getOreKey(item, stones)
            local last = State.LastOre[key] or -math.huge
            if part and now - last >= State.OreCooldown then
                local distance = horizontalDistance(part.Position, center)
                if distance <= bestDistance then
                    bestPrompt, bestDistance = item, distance
                end
            end
        end
    end
    return bestPrompt
end

local VirtualInputManager = game:GetService("VirtualInputManager")
local player = Player

local CONFIG = {
    TextoPromptAlvo = "vender minérios",
    TituloMenuAlvo = "companhia de mineração",
    -- Detecta automaticamente celular/tablet; o botao do painel continua permitindo alternar.
    Dispositivo = UIS.TouchEnabled and "CELULAR" or "COMPUTADOR",
    CoordenadasPC = {
        { x = 462, y = 500 }, -- Vender 500
        { x = 566, y = 504 }, -- Vender 750
        { x = 682, y = 502 }, -- Vender 1K
        { x = 790, y = 506 }, -- Vender 6K
        { x = 886, y = 506 }, -- Vender 25K
    },
    CoordenadaFecharPC = { x = 952, y = 256 },
    CoordenadasCelular = {
        { x = 314, y = 278 }, -- Vender 500
        { x = 386, y = 280 }, -- Vender 750
        { x = 459, y = 277 }, -- Vender 1K
        { x = 524, y = 278 }, -- Vender 6K
        { x = 590, y = 277 }, -- Vender 25K
    },
    CoordenadaFecharCelular = { x = 639, y = 143 },
    IntervaloCliqueVender = 1.0, -- 1s de intervalo entre cada clique em "Vender"
    TempoExtraSegurar = 0.55, -- +0.5s (metade de 1s) extra segurando para abrir a venda
    TempoMaximoSegurando = 2.2,
    TempoEsperaMenu = 0.35,
    RaioDetectarCarro = 300,
    PosicaoVenda = Vector3.new(2933.27, 4.13, 3183.48),
    AlturaLevantadaTp = 18,
    EsperaCarregarMapa = 0.35,
    PosicaoRetorno = Vector3.new(386.17, 5.08, 2121.79),
    VelocidadeAproximacaoNpc = 18,
    DistanciaParadaNpc = 3.5,
}

local function obterPersonagem()
    local character = player.Character or player.CharacterAdded:Wait()
    local hrp = character:FindFirstChild("HumanoidRootPart")
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    return character, hrp, humanoid
end

-- Trecho curto: equipa o primeiro item do 1º slot ao sair do carro na volta
local function equiparPrimeiroSlot()
    local character, _, humanoid = obterPersonagem()
    if not character or not humanoid then return end
    pcall(function() VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.One, false, game); task.wait(0.03); VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.One, false, game) end)
    local backpack = player:FindFirstChildOfClass("Backpack")
    local tool = backpack and backpack:FindFirstChildWhichIsA("Tool")
    if tool and tool.Parent == backpack then humanoid:EquipTool(tool) end
end

local function obterBasePartDoPrompt(prompt)
    if not prompt or not prompt.Parent then
        return nil
    end

    local pai = prompt.Parent
    if pai:IsA("BasePart") then
        return pai
    elseif pai:IsA("Attachment") and pai.Parent and pai.Parent:IsA("BasePart") then
        return pai.Parent
    elseif pai:IsA("Model") then
        return pai.PrimaryPart or pai:FindFirstChildWhichIsA("BasePart", true)
    end

    return nil
end

local function promptEhDeVenderMinerios(prompt)
    if not prompt or not prompt:IsA("ProximityPrompt") then
        return false
    end

    local acao = string.lower(prompt.ActionText or "")
    local objeto = string.lower(prompt.ObjectText or "")
    local nomePrompt = string.lower(prompt.Name or "")
    local nomePai = prompt.Parent and string.lower(prompt.Parent.Name) or ""
    local nomeAvo = (prompt.Parent and prompt.Parent.Parent) and string.lower(prompt.Parent.Parent.Name) or ""

    return string.find(acao, CONFIG.TextoPromptAlvo, 1, true)
        or string.find(objeto, CONFIG.TextoPromptAlvo, 1, true)
        or string.find(nomePrompt, "vender", 1, true)
        or string.find(nomePai, "vender", 1, true)
        or string.find(nomePai, "miner", 1, true)
        or string.find(nomeAvo, "vender", 1, true)
        or string.find(nomeAvo, "miner", 1, true)
end

local function encontrarPromptNpcVenda()
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("ProximityPrompt") and promptEhDeVenderMinerios(obj) then
            local parte = obterBasePartDoPrompt(obj)
            if parte and parte:IsDescendantOf(workspace) then
                return obj, parte
            end
        end
    end
    return nil, nil
end

local function modeloPertenceAoJogador(modelo)
    if not modelo or not modelo:IsA("Model") then
        return false
    end

    local nomeJogador = string.lower(player.Name)
    local displayJogador = string.lower(player.DisplayName or "")
    local userIdStr = tostring(player.UserId)

    local function valorEhDoJogador(val)
        if val == nil then
            return false
        end
        if typeof(val) == "Instance" then
            return val == player or val == player.Character
        end
        local s = string.lower(tostring(val))
        return s == nomeJogador
            or (displayJogador ~= "" and s == displayJogador)
            or s == userIdStr
            or string.find(s, nomeJogador, 1, true) ~= nil
            or string.find(s, userIdStr, 1, true) ~= nil
    end

    for chave, valor in pairs(modelo:GetAttributes()) do
        local chaveLower = string.lower(tostring(chave))
        if string.find(chaveLower, "owner", 1, true)
            or string.find(chaveLower, "dono", 1, true)
            or string.find(chaveLower, "player", 1, true)
            or string.find(chaveLower, "user", 1, true)
            or string.find(chaveLower, "driver", 1, true)
            or string.find(chaveLower, "motorista", 1, true)
        then
            if valorEhDoJogador(valor) then
                return true
            end
        end
    end

    for _, desc in ipairs(modelo:GetDescendants()) do
        if desc:IsA("ObjectValue") or desc:IsA("StringValue") or desc:IsA("IntValue") or desc:IsA("NumberValue") then
            local n = string.lower(desc.Name)
            if string.find(n, "owner", 1, true)
                or string.find(n, "dono", 1, true)
                or string.find(n, "player", 1, true)
                or string.find(n, "user", 1, true)
                or string.find(n, "driver", 1, true)
            then
                if valorEhDoJogador(desc.Value) then
                    return true
                end
            end
        elseif desc:IsA("Seat") or desc:IsA("VehicleSeat") then
            if desc.Occupant and desc.Occupant.Parent == player.Character then
                return true
            end
        end
    end

    local nomeModelo = string.lower(modelo.Name)
    if string.find(nomeModelo, nomeJogador, 1, true) or string.find(nomeModelo, userIdStr, 1, true) then
        return true
    end

    return false
end

local function encontrarMeuCarroPerto()
    local _, hrp = obterPersonagem()
    if not hrp then
        return nil, nil
    end

    local melhorCarro = nil
    local melhorAssento = nil
    local menorDist = CONFIG.RaioDetectarCarro

    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("VehicleSeat") or obj:IsA("Seat") then
            local modelo = obj:FindFirstAncestorOfClass("Model")
            if modelo and modelo ~= player.Character and modeloPertenceAoJogador(modelo) then
                if not obj.Occupant or obj.Occupant.Parent == player.Character then
                    local dist = (obj.Position - hrp.Position).Magnitude
                    if dist <= menorDist then
                        menorDist = dist
                        melhorCarro = modelo
                        melhorAssento = obj
                    end
                end
            end
        end
    end

    return melhorCarro, melhorAssento
end

local function entrarNoCarro(carro, assento)
    local character, hrp, humanoid = obterPersonagem()
    if not character or not hrp or not humanoid or not carro or not assento then
        return false
    end

    if humanoid.SeatPart == assento then
        return true
    end

    character:PivotTo(assento.CFrame * CFrame.new(0, 1.5, 0))
    pcall(function()
        assento:Sit(humanoid)
    end)

    for _ = 1, 15 do
        if humanoid.SeatPart and humanoid.SeatPart:IsDescendantOf(carro) then
            return true
        end
        character:PivotTo(assento.CFrame * CFrame.new(0, 1.2, 0))
        task.wait(0.03)
    end

    return humanoid.SeatPart ~= nil and humanoid.SeatPart:IsDescendantOf(carro)
end

local function sairDoCarro()
    local character, hrp, humanoid = obterPersonagem()
    if not character or not hrp or not humanoid then
        return
    end

    humanoid.Sit = false
    if humanoid.SeatPart then
        pcall(function()
            humanoid.SeatPart.Occupant = nil
        end)
    end
    task.wait(0.03)
    character:PivotTo(hrp.CFrame * CFrame.new(0, 2.5, -3.5))
end

local function teleporteLivre(cfDestino, carro)
    local character, hrp = obterPersonagem()
    if not character or not hrp then
        return false
    end

    if carro then
        local base = carro.PrimaryPart or carro:FindFirstChildWhichIsA("BasePart", true)
        if base then
            local offsetCar = base.CFrame:ToObjectSpace(hrp.CFrame)
            carro:PivotTo(cfDestino)
            character:PivotTo(base.CFrame * offsetCar)
        else
            carro:PivotTo(cfDestino)
            character:PivotTo(cfDestino * CFrame.new(0, 2, 0))
        end
    else
        character:PivotTo(cfDestino)
    end

    return true
end

local function voarDevagarAteNpc(parteNpc)
    local character, hrp = obterPersonagem()
    if not character or not hrp or not parteNpc or not parteNpc.Parent then
        return false
    end

    local inicio = os.clock()
    local maxDuracao = 6.0

    while (os.clock() - inicio) < maxDuracao do
        character, hrp = obterPersonagem()
        if not character or not hrp or not parteNpc.Parent then
            return false
        end

        local alvoPos = parteNpc.Position
        local vetorAteNpc = alvoPos - hrp.Position
        local distancia = vetorAteNpc.Magnitude

        if distancia <= CONFIG.DistanciaParadaNpc then
            hrp.AssemblyLinearVelocity = Vector3.zero
            hrp.AssemblyAngularVelocity = Vector3.zero
            hrp.CFrame = CFrame.lookAt(hrp.Position, Vector3.new(alvoPos.X, hrp.Position.Y, alvoPos.Z))
            return true
        end

        local dt = RunService.Heartbeat:Wait()
        local passo = math.min(distancia - CONFIG.DistanciaParadaNpc * 0.5, CONFIG.VelocidadeAproximacaoNpc * dt)
        if passo > 0 then
            local direcao = vetorAteNpc.Unit
            local novaPos = hrp.Position + (direcao * passo)
            hrp.AssemblyLinearVelocity = Vector3.zero
            hrp.AssemblyAngularVelocity = Vector3.zero
            character:PivotTo(CFrame.lookAt(novaPos, Vector3.new(alvoPos.X, hrp.Position.Y, alvoPos.Z)))
        end
    end

    return true
end

local function guiEstaVisivelNaTela(guiObj)
    if not guiObj or not guiObj.Parent then
        return false
    end

    local atual = guiObj
    while atual and atual ~= game do
        if atual:IsA("ScreenGui") then
            return atual.Enabled
        elseif atual:IsA("GuiObject") and not atual.Visible then
            return false
        end
        atual = atual.Parent
    end

    return true
end

local function menuVendaEstaAberto()
    local playerGui = player:FindFirstChild("PlayerGui")
    if not playerGui then
        return false
    end

    for _, gui in ipairs(playerGui:GetDescendants()) do
        if guiEstaVisivelNaTela(gui) and (gui:IsA("TextLabel") or gui:IsA("TextButton")) then
            local txt = string.lower(gui.Text or "")
            if string.find(txt, CONFIG.TituloMenuAlvo, 1, true) or txt == "vender" then
                return true
            end
        end	end

	return false
end

local function segurarAteAbrirMenuVenda(prompt, parteNpc)
	local inicio = os.clock()
	local holdDuracao = (prompt and prompt.HoldDuration and prompt.HoldDuration > 0) and prompt.HoldDuration or 0.6
	local tempoMinimoHold = holdDuracao + CONFIG.TempoExtraSegurar

	local teclaAlvo = (prompt and prompt.KeyboardKeyCode and prompt.KeyboardKeyCode ~= Enum.KeyCode.Unknown)
		and prompt.KeyboardKeyCode or Enum.KeyCode.E

	local cam = workspace.CurrentCamera
	local posTela = cam and Vector2.new(cam.ViewportSize.X * 0.5, cam.ViewportSize.Y * 0.5) or Vector2.new(500, 300)

	local function pressionarContinuo()
		local _, hrp = obterPersonagem()
		if hrp and parteNpc and parteNpc.Parent then
			hrp.CFrame = CFrame.lookAt(hrp.Position, parteNpc.Position)
		end

		if prompt and prompt.Parent and prompt.Enabled then
			pcall(function()
				prompt:InputHoldBegin()
			end)
		end

		pcall(function()
			VirtualInputManager:SendKeyEvent(true, teclaAlvo, false, game)
		end)

		pcall(function()
			VirtualInputManager:SendMouseButtonEvent(posTela.X, posTela.Y, 0, true, game, 0)
		end)
	end

	local function soltarTudo()
		pcall(function()
			VirtualInputManager:SendMouseButtonEvent(posTela.X, posTela.Y, 0, false, game, 0)
		end)
		pcall(function()
			VirtualInputManager:SendKeyEvent(false, teclaAlvo, false, game)
		end)
		if prompt and prompt.Parent then
			pcall(function()
				prompt:InputHoldEnd()
			end)
		end
	end

	local abriu = false

	while (os.clock() - inicio) < CONFIG.TempoMaximoSegurando do
		pressionarContinuo()

		if (os.clock() - inicio) >= tempoMinimoHold and menuVendaEstaAberto() then
			abriu = true
			break
		end

		task.wait(0.06)
	end

	soltarTudo()

	if not abriu then
		local esperaExtra = os.clock()
		while (os.clock() - esperaExtra) < CONFIG.TempoEsperaMenu do
			if menuVendaEstaAberto() then
				abriu = true
				break
			end
			task.wait(0.06)
		end
	end

	return abriu
end

local function clicarCoordenada(x, y, dispositivo)
    dispositivo = dispositivo or CONFIG.Dispositivo
    if dispositivo == "CELULAR" then
        -- Usa exatamente as coordenadas mobile fornecidas.
        -- SendTouchEvent recebe: id do toque, estado, X e Y.
        local okBegin, errBegin = pcall(function()
            VirtualInputManager:SendTouchEvent(1, Enum.UserInputState.Begin, x, y)
        end)
        if not okBegin then
            warn("[XLZ] Falha ao iniciar toque mobile: " .. tostring(errBegin))
            return false
        end

        task.wait(0.08)

        local okEnd, errEnd = pcall(function()
            VirtualInputManager:SendTouchEvent(1, Enum.UserInputState.End, x, y)
        end)
        if not okEnd then
            warn("[XLZ] Falha ao finalizar toque mobile: " .. tostring(errEnd))
            return false
        end
        return true
    else
        local okMove = pcall(function()
            VirtualInputManager:SendMouseMoveEvent(x, y, game)
        end)
        task.wait(0.03)
        local okDown, errDown = pcall(function()
            VirtualInputManager:SendMouseButtonEvent(x, y, 0, true, game, 0)
        end)
        task.wait(0.06)
        local okUp, errUp = pcall(function()
            VirtualInputManager:SendMouseButtonEvent(x, y, 0, false, game, 0)
        end)
        if not okMove or not okDown or not okUp then
            warn("[XLZ] Falha no clique do computador: " .. tostring(errDown or errUp))
            return false
        end
        return true
    end
end

local function executarVendaERetorno()
	local character, hrp = obterPersonagem()
	if not character or not hrp then
		warn("Personagem ou HumanoidRootPart não encontrado.")
		return
	end

	local offsetAltura = Vector3.new(0, CONFIG.AlturaLevantadaTp, 0)
	local destinoVendaLevantado = CFrame.new(CONFIG.PosicaoVenda + offsetAltura)
	local destinoRetornoLevantado = CFrame.new(CONFIG.PosicaoRetorno + offsetAltura)

	local carro, assento = encontrarMeuCarroPerto()
	if not carro or not assento then
		warn("Seu carro não foi encontrado no raio de " .. CONFIG.RaioDetectarCarro .. " studs.")
		return
	end

	if not entrarNoCarro(carro, assento) then
		warn("Não foi possível sentar no seu carro.")
		return
	end

	teleporteLivre(destinoVendaLevantado, carro)
	task.wait(CONFIG.EsperaCarregarMapa)

	sairDoCarro()
	task.wait(0.1)

	local prompt, parteNpc = encontrarPromptNpcVenda()
	if not prompt or not parteNpc then
		warn("ProximityPrompt de 'Vender Minérios' não encontrado após sair do carro.")
		entrarNoCarro(carro, assento)
		task.wait(0.05)
		teleporteLivre(destinoRetornoLevantado, carro)
		task.wait(CONFIG.EsperaCarregarMapa)
		sairDoCarro()
		task.wait(0.1)
		equiparPrimeiroSlot()
		return
	end

	voarDevagarAteNpc(parteNpc)
	task.wait(0.05)

	segurarAteAbrirMenuVenda(prompt, parteNpc)
	task.wait(0.08)

	-- Usa uma unica selecao de dispositivo durante todo o ciclo de venda.
	local dispositivoVenda = CONFIG.Dispositivo
	local coordenadasVenda = dispositivoVenda == "CELULAR" and CONFIG.CoordenadasCelular or CONFIG.CoordenadasPC
	local coordenadaFechar = dispositivoVenda == "CELULAR" and CONFIG.CoordenadaFecharCelular or CONFIG.CoordenadaFecharPC
	print("[PRIME X] Venda usando modo: " .. dispositivoVenda)
	for i = 1, #coordenadasVenda do
		local coord = coordenadasVenda[i]
		print("Clicando em Vender " .. i .. " (" .. coord.x .. ", " .. coord.y .. ")")
		clicarCoordenada(coord.x, coord.y, dispositivoVenda)
		if i < #coordenadasVenda then
			task.wait(CONFIG.IntervaloCliqueVender)
		end
	end

	task.wait(CONFIG.IntervaloCliqueVender)

	if coordenadaFechar then
		print("Clicando no X (" .. coordenadaFechar.x .. ", " .. coordenadaFechar.y .. ")")
		clicarCoordenada(coordenadaFechar.x, coordenadaFechar.y, dispositivoVenda)
		task.wait(0.08)
	end

	entrarNoCarro(carro, assento)
	task.wait(0.05)
	teleporteLivre(destinoRetornoLevantado, carro)
	task.wait(CONFIG.EsperaCarregarMapa)
	sairDoCarro()
	task.wait(0.1)
	equiparPrimeiroSlot()
end

local function miningLoop()
    if not activatePickaxe() then
        State.Status = "Nao foi possivel pegar a picareta"
        return
    end

    while State.Running and State.Mining do
        for index, pointCFrame in ipairs(Points) do
            if not State.Running or not State.Mining then break end
            State.PointIndex = index
            State.Status = "Voando para posicao " .. index .. " de " .. #Points
            if not walkTo(pointCFrame) then
                task.wait(0.2)
                break
            end

            -- Fica estabilizado no ponto e espera o prompt de mineracao aparecer.
            local prompt, part
            State.Status = "No ponto " .. index .. "; procurando minerio"
            while State.Running and State.Mining and not prompt do
                prompt = findMiningPromptAtPoint(pointCFrame.Position, 35)
                if prompt then
                    part = getPromptPart(prompt)
                    if not part or not prompt.Enabled then
                        prompt, part = nil, nil
                    end
                end
                if not prompt then
                    holdPosition(pointCFrame.Rotation)
                    State.Status = "Aguardando minerio/prompt aparecer no ponto " .. index
                    task.wait(0.5)
                end
            end
            if not State.Running or not State.Mining then break end

            -- Voa ate uma posicao lateral proxima da pedra; continua ajustando
            -- ate entrar no alcance configurado do ProximityPrompt.
            local _, root = getHumanoidRoot()
            local activationDistance = prompt.MaxActivationDistance or 6
            local away = Vector3.new(root.Position.X - part.Position.X, 0, root.Position.Z - part.Position.Z)
            if away.Magnitude < 0.05 then
                away = Vector3.new(pointCFrame.LookVector.X, 0, pointCFrame.LookVector.Z)
            end
            if away.Magnitude < 0.05 then away = Vector3.new(0, 0, 1) end
            away = away.Unit

            local safeDistance = math.max(1.5, math.min(3.0, activationDistance * 0.55))
            local approachPosition = part.Position + away * safeDistance + Vector3.new(0, 1.5, 0)
            local approachCFrame = CFrame.new(approachPosition) * pointCFrame.Rotation
            State.Status = "Aproximando ate o botao Minerar aparecer"
            if not walkTo(approachCFrame, prompt) then
                State.Status = "Falha ao aproximar do minerio"
                task.wait(0.5)
                break
            end

            -- Espera no local ate o prompt estar ativo e dentro da distancia real.
            local ready = false
            while State.Running and State.Mining and prompt and prompt.Parent do
                _, root = getHumanoidRoot()
                part = getPromptPart(prompt)
                if not part then break end
                local distance = (root.Position - part.Position).Magnitude
                local maxDistance = prompt.MaxActivationDistance or 6
                -- PromptShown confirma que o botao E realmente apareceu na tela.
                -- Assim que aparecer, para o voo na posicao atual e inicia a retencao.
                if prompt.Enabled and State.VisiblePrompts[prompt] then
                    ready = true
                    holdPosition(pointCFrame.Rotation)
                    break
                end

                -- Se ja esta no alcance, fica totalmente parado e espera o
                -- evento visual do prompt. Nao continua se mexendo por cima da pedra.
                if distance <= maxDistance then
                    holdPosition(pointCFrame.Rotation)
                    State.Status = "No alcance; esperando o botao E aparecer"
                    task.wait(0.05)
                else
                    local dir = Vector3.new(root.Position.X - part.Position.X, 0, root.Position.Z - part.Position.Z)
                    if dir.Magnitude < 0.05 then dir = Vector3.new(0, 0, 1) end
                    dir = dir.Unit
                    local nextPos = part.Position + dir * math.max(1.25, maxDistance * 0.45) + Vector3.new(0, 1.5, 0)
                    State.Status = "Aproximando ate o botao E aparecer"
                    walkTo(CFrame.new(nextPos) * pointCFrame.Rotation)
                end
            end

            if ready and State.Running and State.Mining then
                holdPosition(pointCFrame.Rotation)
                State.Status = "Botao E apareceu; segurando por pelo menos 16 segundos"
                local ok = holdMiningPrompt(prompt)
                if ok then
                    local stones = MineFolder and MineFolder:FindFirstChild("Pedras")
                    local key = getOreKey(prompt, stones)
                    State.LastOre[key] = os.clock()
                    MineriosDesdeVenda += 1
                    State.Status = "Minerio concluido (" .. MineriosDesdeVenda .. "/" .. ORES_POR_VENDA .. "); cooldown de 20s"

                    if MineriosDesdeVenda >= ORES_POR_VENDA and State.Running and State.Mining then
                        State.Status = "9 minerios coletados; iniciando venda"
                        stopFlight()
                        executarVendaERetorno()
                        MineriosDesdeVenda = 0
                        if State.Running and State.Mining then
                            refreshCharacter()
                            State.Status = "Venda concluida; retomando mineracao"
                        end
                    end
                else
                    State.Status = "Retencao interrompida antes de completar 16 segundos; minerio nao contado"
                    task.wait(0.5)
                end
            else
                State.Status = "Prompt indisponivel; permanecendo no ponto"
            end

            holdPosition(pointCFrame.Rotation)
            task.wait(0.4)
        end
    end
    stopFlight()
    State.Status = "Mineracao desligada"
end



-- Interface do minerador dentro da pagina MINERADOR do PRIME X.
local MinerBody = CriarSecao("AUTO MINERADOR", 4, 42, 300)
local MinerStatus = makeLabel(MinerBody, "Status: Aguardando inicio", 2, 34, 11)
local MinerSpeedLabel = makeLabel(MinerBody, "VELOCIDADE DE ANDADA: " .. State.Speed, 37, 22, 11)
local SpeedBox = Instance.new("TextBox")
SpeedBox.Size = UDim2.fromOffset(70, 28)
SpeedBox.Position = UDim2.fromOffset(12, 62)
SpeedBox.BackgroundColor3 = DARK2
SpeedBox.TextColor3 = WHITE
SpeedBox.Font = Enum.Font.GothamBold
SpeedBox.TextSize = 12
SpeedBox.Text = tostring(State.Speed)
SpeedBox.ClearTextOnFocus = false
SpeedBox.Parent = MinerBody
Instance.new("UICorner", SpeedBox).CornerRadius = UDim.new(0, 6)

local DeviceLabel = makeLabel(MinerBody, "DISPOSITIVO PARA VENDA", 96, 22, 11)
local DeviceButton = makeButton(MinerBody, "DISPOSITIVO: COMPUTADOR", 12, 120, 0.96, 34, RED)
local MinerStart = makeButton(MinerBody, "▶ ATIVAR AUTO MINERADOR", 12, 163, 0.96, 36, GREEN)
local MinerActivity = makeLabel(MinerBody, "Atividade: Aguardando inicio", 204, 38, 10)

local function atualizarDispositivo()
    DeviceButton.Text = "DISPOSITIVO: " .. (CONFIG.Dispositivo == "CELULAR" and "CELULAR" or "COMPUTADOR")
end
atualizarDispositivo()
DeviceButton.Activated:Connect(function()
    CONFIG.Dispositivo = CONFIG.Dispositivo == "COMPUTADOR" and "CELULAR" or "COMPUTADOR"
    atualizarDispositivo()
    print("[PRIME X] Dispositivo selecionado: " .. CONFIG.Dispositivo)
end)

local function atualizarMineradorUI()
    local ativo = State.Mining
    MinerStart.Text = ativo and "■ DESATIVAR AUTO MINERADOR" or "▶ ATIVAR AUTO MINERADOR"
    MinerStart.BackgroundColor3 = ativo and RED or GREEN
    MinerStatus.Text = (ativo and "Status: ATIVO | " or "Status: INATIVO | ") .. tostring(State.Status)
    MinerActivity.Text = "Minerios para venda: " .. tostring(MineriosDesdeVenda) .. "/" .. tostring(ORES_POR_VENDA)
        .. "\nModo: " .. CONFIG.Dispositivo
end

MinerStart.MouseButton1Click:Connect(function()
    if not State.Running then return end
    State.Mining = not State.Mining
    if State.Mining then
        State.Status = "Iniciando mineracao"
        task.spawn(miningLoop)
    else
        stopFlight()
        State.Status = "Mineracao desligada"
    end
    atualizarMineradorUI()
end)

SpeedBox.FocusLost:Connect(function()
    local value = tonumber(SpeedBox.Text)
    if value then State.Speed = math.clamp(value, 8, 100) end
    SpeedBox.Text = tostring(State.Speed)
    MinerSpeedLabel.Text = "VELOCIDADE DE ANDADA: " .. State.Speed
end)

atualizarDispositivo()
task.spawn(function()
    while Gui.Parent and State.Running do
        atualizarMineradorUI()
        task.wait(0.25)
    end
end)

-- =========================================================
-- AUTO CAIXA
-- =========================================================
local CaixaBody = CriarSecao("AUTO CAIXA", 1, 42, 230)
local CaixaStatus = makeLabel(CaixaBody, "Status: DESATIVADO", 2, 22, 12)
local CaixaProgresso = makeLabel(CaixaBody, "Aguardando...", 25, 22, 11)
local CaixaSpeedLabel = makeLabel(CaixaBody, "Speed do Caixa: 24  |  Limite: 8-100", 49, 22, 11)
local CaixaMenos = makeButton(CaixaBody, "− SPEED CAIXA", 12, 77, 0.47, 34, Color3.fromRGB(72,48,103))
local CaixaMais = makeButton(CaixaBody, "+ SPEED CAIXA", 0, 77, 0.47, 34, Color3.fromRGB(105,62,160))
CaixaMais.AnchorPoint = Vector2.new(1, 0)
CaixaMais.Position = UDim2.new(1, -12, 0, 77)
local CaixaStart = makeButton(CaixaBody, "▶ ATIVAR AUTO CAIXA", 12, 120, 0.96, 36, OFF)

local Caixa, Entrega, CaixaPrompt, EntregaPrompt
local function AtualizarReferenciasCaixa()
    local construcoes = Workspace:FindFirstChild("Construcoes")
    local prisao = construcoes and construcoes:FindFirstChild("PrisaoFolder")
    local emprego = prisao and prisao:FindFirstChild("EmpregoPreso")
    Caixa = emprego and emprego:FindFirstChild("LocalCaixa")
    Entrega = emprego and emprego:FindFirstChild("LocalEntrega")
    CaixaPrompt = Caixa and Caixa:FindFirstChildWhichIsA("ProximityPrompt", true)
    EntregaPrompt = Entrega and Entrega:FindFirstChildWhichIsA("ProximityPrompt", true)
    return Caixa and Entrega and CaixaPrompt and EntregaPrompt
end
local function StatusCaixa(t) CaixaStatus.Text = "Status: " .. t end
local function Posicao(obj)
    if not obj then return nil end
    if obj:IsA("Attachment") then return obj.WorldPosition end
    if obj:IsA("BasePart") then return obj.Position end
    if obj:IsA("Model") then return obj:GetPivot().Position end
    local part = obj:FindFirstAncestorWhichIsA("BasePart")
    return part and part.Position
end
local function Aproximar(obj, prompt)
    local root = Personagem()
    local target = Posicao(obj)
    if not root or not target then return nil end
    local direction = root.Position - target
    if direction.Magnitude <= prompt.MaxActivationDistance * 0.55 then return root.Position end
    if direction.Magnitude < 0.1 then direction = Vector3.new(1,0,0) end
    local distance = math.max(1, math.min(prompt.MaxActivationDistance * 0.55, 5))
    return target + direction.Unit * distance
end
local function MoverCaixa(position)
    local root, humanoid = Personagem()
    if not root or not humanoid or not Hub.CaixaAtivo then return false end
    humanoid.WalkSpeed = Hub.CaixaVelocidade
    humanoid:MoveTo(position)
    local terminou, chegou = false, false
    local connection = humanoid.MoveToFinished:Connect(function(result) chegou = result; terminou = true end)
    local timeout = os.clock() + 20
    while Hub.CaixaAtivo and not terminou and os.clock() < timeout do
        root, humanoid = Personagem()
        if not root or not humanoid then connection:Disconnect(); return false end
        humanoid.WalkSpeed = Hub.CaixaVelocidade
        if (root.Position - position).Magnitude <= 3 then chegou = true; terminou = true; break end
        task.wait(0.1)
    end
    connection:Disconnect()
    return Hub.CaixaAtivo and chegou
end
local function Interagir(prompt)
    if not Hub.CaixaAtivo or not prompt.Parent or not prompt.Enabled then return false end
    local root = Personagem()
    local target = Posicao(prompt.Parent)
    if not root or not target or (root.Position - target).Magnitude > prompt.MaxActivationDistance + 1 then return false end
    CaixaProgresso.Text = prompt.ActionText
    if typeof(fireproximityprompt) == "function" then
        local ok = pcall(function() fireproximityprompt(prompt, prompt.HoldDuration) end)
        if ok then task.wait(prompt.HoldDuration + 0.5); return Hub.CaixaAtivo end
    end
    local ok = pcall(function()
        prompt:InputHoldBegin()
        task.wait(prompt.HoldDuration + 0.3)
        prompt:InputHoldEnd()
    end)
    return ok and Hub.CaixaAtivo
end
local function EtapaCaixa(obj, prompt, nome)
    if not Hub.CaixaAtivo then return false end
    StatusCaixa("INDO PARA " .. nome)
    CaixaProgresso.Text = "Calculando caminho..."
    local destino = Aproximar(obj, prompt)
    if not destino or not MoverCaixa(destino) then
        CaixaProgresso.Text = "Falha no caminho"
        return false
    end
    if not Hub.CaixaAtivo then return false end
    task.wait(0.3)
    local root = Personagem()
    local target = Posicao(prompt.Parent)
    if not root or not target or (root.Position - target).Magnitude > prompt.MaxActivationDistance then
        CaixaProgresso.Text = "Fora do alcance"
        return false
    end
    StatusCaixa("INTERAGINDO")
    return Interagir(prompt)
end
local function ExecutarCaixa()
    while Hub.CaixaAtivo do
        if not AtualizarReferenciasCaixa() then
            StatusCaixa("AGUARDANDO AREA DA PRISAO")
            CaixaProgresso.Text = "Entre na area da prisao para iniciar..."
            task.wait(1)            continue
        end
        if not AdquirirMovimento("CAIXA", function() return Hub.CaixaAtivo end) then break end
        if not AtualizarReferenciasCaixa() then
            LiberarMovimento("CAIXA")
            task.wait(0.5)
            continue
        end
        local sucesso = false
        local ok = pcall(function()
            StatusCaixa("INDO PEGAR CAIXA")
            if not EtapaCaixa(Caixa, CaixaPrompt, "CAIXA") or not Hub.CaixaAtivo then return end
            StatusCaixa("INDO ENTREGAR")
            if EtapaCaixa(Entrega, EntregaPrompt, "ENTREGA") and Hub.CaixaAtivo then
                CaixaProgresso.Text = "ENTREGA CONCLUIDA!"
                sucesso = true
            end
        end)
        LiberarMovimento("CAIXA")
        if not ok or not sucesso then break end
        task.wait(1)
    end
    Hub.CaixaAtivo = false
    LiberarMovimento("CAIXA")
    StatusCaixa("DESATIVADO")
    setButtonActive(CaixaStart, false, "��� DESATIVAR AUTO CAIXA", "��� ATIVAR AUTO CAIXA")
end

CaixaMenos.MouseButton1Click:Connect(function()
    Hub.CaixaVelocidade = math.max(8, Hub.CaixaVelocidade - 4)
    CaixaSpeedLabel.Text = "Speed do Caixa: " .. Hub.CaixaVelocidade .. "  |  Limite: 8-100"
end)

CaixaMais.MouseButton1Click:Connect(function()
    Hub.CaixaVelocidade = math.min(100, Hub.CaixaVelocidade + 4)
    CaixaSpeedLabel.Text = "Speed do Caixa: " .. Hub.CaixaVelocidade .. "  |  Limite: 8-100"
end)

CaixaStart.MouseButton1Click:Connect(function()
    Hub.CaixaAtivo = not Hub.CaixaAtivo
    if Hub.CaixaAtivo then
        setButtonActive(CaixaStart, true, "��� DESATIVAR AUTO CAIXA", "��� ATIVAR AUTO CAIXA")
        task.spawn(ExecutarCaixa)
    else
        setButtonActive(CaixaStart, false, "��� DESATIVAR AUTO CAIXA", "��� ATIVAR AUTO CAIXA")
    end
end)
