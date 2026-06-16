-- [[ PRODIGIOZX MODS + AUTO JJS ]] --
-- Criador: Lucca
-- Versão: 6.0 (MOBILE OTIMIZADO)

-- ============ CONFIGURAÇÕES DE COMPATIBILIDADE ============
local usingTask = pcall(function() return task.wait end)
local waitFunc = usingTask and task.wait or wait
local spawnFunc = usingTask and task.spawn or (coroutine.wrap or spawn)

-- ============ VERIFICAR EXISTÊNCIA DO PLAYER ============
local player = nil
local maxAttempts = 10
local attempt = 0
while not player and attempt < maxAttempts do
    player = game:GetService("Players").LocalPlayer
    if not player then
        waitFunc(0.5)
        attempt = attempt + 1
    end
end

if not player then
    print("❌ Erro: Não foi possível encontrar o jogador!")
    return
end

-- ============ VERIFICAR SE É MOBILE ============
local isMobile = game:GetService("UserInputService").TouchEnabled
local userInputService = game:GetService("UserInputService")

-- ============ VERIFICAR SUPORTE A FUNÇÕES ============
local hasSaveSupport = pcall(function()
    return writefile and readfile and isfile
end)

local hasJSON = pcall(function()
    return game:GetService("HttpService").JSONEncode
end)

-- ============ CARREGAR KAVO UI ============
local Library = nil
local loadSuccess = pcall(function()
    Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
end)

if not loadSuccess or not Library then
    print("❌ Erro ao carregar Kavo UI")
    return
end

-- ============ CONFIGURAÇÕES DE PLACE ============
local PLACE_CONFIG = {
    NOVA = {
        id = "NOVA",
        nome = "🏙️ Place Nova",
        animacaoId = "rbxassetid://122367567937291"
    },
    ANTIGA = {
        id = "ANTIGA",
        nome = "🏚️ Place Antiga",
        animacaoId = "rbxassetid://124954987474196"
    }
}

local placeAtual = PLACE_CONFIG.NOVA

-- ============ VERIFICAR EVENTO JJS ============
local jjsEvent = nil
local eventType = nil
pcall(function()
    local repStorage = game:GetService("ReplicatedStorage")
    jjsEvent = repStorage:FindFirstChild("JJsBillboardEvent")
    if not jjsEvent then
        jjsEvent = repStorage:WaitForChild("JJsBillboardEvent", 3)
    end
    if jjsEvent then
        if jjsEvent:IsA("RemoteEvent") then
            eventType = "RemoteEvent"
        elseif jjsEvent:IsA("RemoteFunction") then
            eventType = "RemoteFunction"
        end
    end
end)

local eventFound = jjsEvent ~= nil

-- ============ SISTEMA DE SAVE/LOAD ============
local config = {
    autoJJsActive = false,
    jjsCount = 0,
    delayValue = 0.8,
    p1Active = false,
    p2Active = false,
    p3Active = false,
    p4Active = false,
    placeAtual = "NOVA"
}

local configFolder = "ProdigiozxConfig"
local configFile = "config.json"

local function saveConfig()
    if not hasSaveSupport or not hasJSON then return false end
    local success = pcall(function()
        if makefolder and not isfolder(configFolder) then
            makefolder(configFolder)
        end
        local data = game:GetService("HttpService"):JSONEncode(config)
        writefile(configFolder .. "/" .. configFile, data)
    end)
    return success
end

local function loadConfig()
    if not hasSaveSupport or not hasJSON then return false end
    local success = pcall(function()
        if isfile(configFolder .. "/" .. configFile) then
            local data = readfile(configFolder .. "/" .. configFile)
            local loaded = game:GetService("HttpService"):JSONDecode(data)
            for k, v in pairs(loaded) do
                config[k] = v
            end
            return true
        end
        return false
    end)
    return success
end

-- ============ JANELA PRINCIPAL ============
local Window = Library.CreateLib("PRODIGIOZX MODS | By Lucca", {
    SchemeColor = Color3.fromRGB(255, 0, 0),
    Background = Color3.fromRGB(20, 20, 20),
    Header = Color3.fromRGB(15, 15, 15),
    TextColor = Color3.fromRGB(255, 255, 255)
})

-- ============ TABELAS ============
local p1Parts, p2Parts, p3Parts, p4Parts = {}, {}, {}, {}
local buildingP1, buildingP2, buildingP3, buildingP4 = false, false, false, false

-- ============ AUTO JJS VARIÁVEIS ============
local ativoJJS = false
local animTrack = nil
local jjsCount = config.jjsCount or 0
local delayValue = config.delayValue or 0.8
local currentLoop = nil
local loopActive = false

if config.placeAtual == "NOVA" then
    placeAtual = PLACE_CONFIG.NOVA
else
    placeAtual = PLACE_CONFIG.ANTIGA
end

-- ============ FUNÇÕES DE UPDATE ============
local function updateLabel(label, text)
    if not label then return end
    pcall(function()
        if label.UpdateLabel then
            label:UpdateLabel(text)
        elseif label.SetText then
            label:SetText(text)
        elseif label.Set then
            label:Set(text)
        end
    end)
end

local function setToggle(toggle, state)
    if not toggle then return end
    pcall(function()
        if toggle.Set then
            toggle:Set(state)
        elseif toggle.SetState then
            toggle:SetState(state)
        elseif toggle.Toggle then
            toggle:Toggle(state)
        end
    end)
end

-- ============ FUNÇÕES DE CONSTRUÇÃO (PISTAS) ============

local function destroyParts(partTable)
    for _, v in pairs(partTable) do
        pcall(function() v:Destroy() end)
    end
    return {}
end

local function buildP1()
    if buildingP1 then return end
    buildingP1 = true
    p1Parts = destroyParts(p1Parts)
    
    local positions = {
        Vector3.new(161,13,-860), Vector3.new(169,13,-853),
        Vector3.new(181,13,-859), Vector3.new(191,13,-854)
    }
    for _, pos in ipairs(positions) do
        local p = Instance.new("Part")
        p.Size = Vector3.new(4,1,4)
        p.Position = pos
        p.Anchored = true
        p.Material = "SmoothPlastic"
        p.Parent = workspace
        table.insert(p1Parts, p)
    end
    
    local b1 = Instance.new("Part")
    b1.Size = Vector3.new(26,1,4)
    b1.Position = Vector3.new(218,11,-850)
    b1.Anchored = true
    b1.Material = "SmoothPlastic"
    b1.Parent = workspace
    table.insert(p1Parts, b1)
    
    local b2 = Instance.new("Part")
    b2.Size = Vector3.new(15,1,4)
    b2.Position = Vector3.new(242.5,11,-857)
    b2.Anchored = true
    b2.Material = "SmoothPlastic"
    b2.Parent = workspace
    table.insert(p1Parts, b2)
    
    buildingP1 = false
end

local function buildP2()
    if buildingP2 then return end
    buildingP2 = true
    p2Parts = destroyParts(p2Parts)
    
    local positions = {
        Vector3.new(167,53,-904), Vector3.new(173,53,-900), Vector3.new(180,53,-893),
        Vector3.new(181,53,-903), Vector3.new(167,53,-891), Vector3.new(198,53,-898),
        Vector3.new(210,53,-898), Vector3.new(223,53,-898), Vector3.new(236,53,-898)
    }
    for _, pos in ipairs(positions) do
        local p = Instance.new("Part")
        p.Size = Vector3.new(6,1,6)
        p.Position = pos
        p.Anchored = true
        p.Material = "SmoothPlastic"
        p.Parent = workspace
        table.insert(p2Parts, p)
    end
    
    buildingP2 = false
end

local function buildP3()
    if buildingP3 then return end
    buildingP3 = true
    p3Parts = destroyParts(p3Parts)
    
    local positions = {
        Vector3.new(389,7,-856), Vector3.new(383,10,-856), Vector3.new(377,12,-856),
        Vector3.new(371,14,-856), Vector3.new(364,16,-856), Vector3.new(352,19,-856),
        Vector3.new(345,20,-856), Vector3.new(339,21,-856), Vector3.new(329,21,-856),
        Vector3.new(318,21,-856), Vector3.new(308,21,-856), Vector3.new(297,21,-856)
    }
    for _, pos in ipairs(positions) do
        local p = Instance.new("Part")
        p.Size = Vector3.new(6,1,6)
        p.Position = pos
        p.Anchored = true
        p.Material = "SmoothPlastic"
        p.Parent = workspace
        table.insert(p3Parts, p)
    end
    
    buildingP3 = false
end

local function buildP4()
    if buildingP4 then return end
    buildingP4 = true
    p4Parts = destroyParts(p4Parts)
    
    local positions = {
        Vector3.new(389,5,-893), Vector3.new(383,7,-893), Vector3.new(378,9,-893),
        Vector3.new(373,11,-893), Vector3.new(367,11,-893), Vector3.new(362,11,-895),
        Vector3.new(358,11,-902), Vector3.new(352,11,-895), Vector3.new(347,11,-900),
        Vector3.new(333,11,-898), Vector3.new(325,11,-898), Vector3.new(318,11,-898),
        Vector3.new(310,11,-899), Vector3.new(303,11,-899), Vector3.new(295,11,-898)
    }
    for _, pos in ipairs(positions) do
        local p = Instance.new("Part")
        p.Size = Vector3.new(6,1,6)
        p.Position = pos
        p.Anchored = true
        p.Material = "SmoothPlastic"
        p.Parent = workspace
        table.insert(p4Parts, p)
    end
    
    buildingP4 = false
end

-- ============ FUNÇÕES AUTO JJS ============

local function carregarAnimacao()
    local char = player.Character
    if not char then return false end
    
    local hum = char:FindFirstChild("Humanoid")
    if not hum then return false end
    
    local animId = placeAtual.animacaoId
    
    return pcall(function()
        if animTrack then
            animTrack:Stop()
            animTrack = nil
        end
        local anim = Instance.new("Animation")
        anim.AnimationId = animId
        animTrack = hum:LoadAnimation(anim)
        print("✅ Animação carregada: " .. animId)
    end)
end

local function enviarJJs(valor)
    if not eventFound or not jjsEvent then return end
    
    pcall(function()
        if eventType == "RemoteEvent" then
            jjsEvent:FireServer(unpack({valor}))
        elseif eventType == "RemoteFunction" then
            jjsEvent:InvokeServer(unpack({valor}))
        else
            pcall(function() jjsEvent:FireServer(unpack({valor})) end)
            pcall(function() jjsEvent:InvokeServer(unpack({valor})) end)
        end
    end)
end

local function tocarAnimacao()
    if not animTrack then
        carregarAnimacao()
        return
    end
    pcall(function()
        animTrack:Stop()
        animTrack:Play()
    end)
end

local function iniciarLoopJJS()
    if loopActive then
        if currentLoop then
            pcall(function() coroutine.close(currentLoop) end)
            currentLoop = nil
        end
        loopActive = false
        waitFunc(0.1)
    end
    if not ativoJJS then return end
    
    loopActive = true
    currentLoop = spawnFunc(function()
        while ativoJJS and loopActive do
            jjsCount = jjsCount + 1
            config.jjsCount = jjsCount
            tocarAnimacao()
            enviarJJs(jjsCount)
            
            if countLabel then
                updateLabel(countLabel, "📊 Total enviado: " .. jjsCount)
            end
            
            waitFunc(delayValue)
        end
        loopActive = false
        currentLoop = nil
    end)
end

local function pararLoopJJS()
    ativoJJS = false
    loopActive = false
    if currentLoop then
        pcall(function() coroutine.close(currentLoop) end)
        currentLoop = nil
    end
    if animTrack then
        pcall(function() animTrack:Stop() end)
    end
end

-- ============ ABAS ============

-- ABA: PISTAS
local TabPistas = Window:NewTab("Auxiliares")
local SecPistas = TabPistas:NewSection("Pistas de Treino")

local p1Toggle, p2Toggle, p3Toggle, p4Toggle

p1Toggle = SecPistas:NewToggle("Parkour 1", "Ativa P1 + Pontes", function(s)
    config.p1Active = s
    if s then
        spawnFunc(buildP1)
    else
        spawnFunc(function() p1Parts = destroyParts(p1Parts) end)
    end
    saveConfig()
end)

p2Toggle = SecPistas:NewToggle("Parkour 2", "Ativa P2", function(s)
    config.p2Active = s
    if s then
        spawnFunc(buildP2)
    else
        spawnFunc(function() p2Parts = destroyParts(p2Parts) end)
    end
    saveConfig()
end)

p3Toggle = SecPistas:NewToggle("Parkour 3", "Ativa P3", function(s)
    config.p3Active = s
    if s then
        spawnFunc(buildP3)
    else
        spawnFunc(function() p3Parts = destroyParts(p3Parts) end)
    end
    saveConfig()
end)

p4Toggle = SecPistas:NewToggle("Parkour 4", "Ativa P4", function(s)
    config.p4Active = s
    if s then
        spawnFunc(buildP4)
    else
        spawnFunc(function() p4Parts = destroyParts(p4Parts) end)
    end
    saveConfig()
end)

-- ABA: PLACE
local TabPlace = Window:NewTab("Place")
local SecPlace = TabPlace:NewSection("Selecionar Place")

local placeLabel = SecPlace:NewLabel("📍 Place atual: " .. placeAtual.nome)

SecPlace:NewButton("🏙️ Place Nova", "Usar animação da nova place", function()
    placeAtual = PLACE_CONFIG.NOVA
    config.placeAtual = "NOVA"
    updateLabel(placeLabel, "📍 Place atual: " .. placeAtual.nome)
    carregarAnimacao()
    saveConfig()
end)

SecPlace:NewButton("🏚️ Place Antiga", "Usar animação da place antiga", function()
    placeAtual = PLACE_CONFIG.ANTIGA
    config.placeAtual = "ANTIGA"
    updateLabel(placeLabel, "📍 Place atual: " .. placeAtual.nome)
    carregarAnimacao()
    saveConfig()
end)

-- ABA: AUTO JJS
local TabJJS = Window:NewTab("Auto JJs")
local SecJJS = TabJJS:NewSection("Auto Farm JJs")

local countLabel = SecJJS:NewLabel("📊 Total enviado: " .. jjsCount)
local statusLabel = SecJJS:NewLabel("Status: Parado")
local autoJJsToggle = nil

if not eventFound then
    updateLabel(statusLabel, "Status: Evento não encontrado!")
end

autoJJsToggle = SecJJS:NewToggle("🔘 LIGAR/DESLIGAR", "Ativa o auto farm de JJs", function(state)
    if not eventFound then
        updateLabel(statusLabel, "Status: Evento não encontrado!")
        setToggle(autoJJsToggle, false)
        return
    end
    
    ativoJJS = state
    config.autoJJsActive = state
    if state then
        if not animTrack then
            carregarAnimacao()
        end
        iniciarLoopJJS()
        updateLabel(statusLabel, "Status: Rodando")
    else
        pararLoopJJS()
        updateLabel(statusLabel, "Status: Parado")
    end
    saveConfig()
end)

SecJJS:NewButton("🔄 RESETAR CONTADOR", "Zera o contador", function()
    jjsCount = 0
    config.jjsCount = 0
    updateLabel(countLabel, "📊 Total enviado: 0")
    saveConfig()
end)

SecJJS:NewSlider("⚡ VELOCIDADE", "Delay entre animações (0.1s - 2.0s)", 20, 1, function(value)
    delayValue = value / 10
    config.delayValue = delayValue
    saveConfig()
end)

local delayDisplay = SecJJS:NewLabel(string.format("⚙️ Delay atual: %.1f segundos", delayValue))
spawnFunc(function()
    while true do
        waitFunc(0.5)
        updateLabel(delayDisplay, string.format("⚙️ Delay atual: %.1f segundos", delayValue))
    end
end)

SecJJS:NewButton("🎭 TESTAR ANIMAÇÃO", "Testa a animação atual", function()
    if not animTrack then
        carregarAnimacao()
        waitFunc(0.3)
    end
    tocarAnimacao()
end)

-- ABA: SAVE/LOAD
local TabSave = Window:NewTab("Save/Load")
local SecSave = TabSave:NewSection("Salvar e Carregar")

local saveStatus = SecSave:NewLabel("💾 Status: Pronto")

SecSave:NewButton("💾 SALVAR", "Salva configurações", function()
    if not hasSaveSupport then
        updateLabel(saveStatus, "💾 Status: Executor sem suporte!")
        waitFunc(2)
        updateLabel(saveStatus, "💾 Status: Pronto")
        return
    end
    config.jjsCount = jjsCount
    config.delayValue = delayValue
    config.autoJJsActive = ativoJJS
    config.placeAtual = placeAtual.id
    saveConfig()
    updateLabel(saveStatus, "💾 Status: Salvo!")
    waitFunc(2)
    updateLabel(saveStatus, "💾 Status: Pronto")
end)

SecSave:NewButton("📂 CARREGAR", "Carrega configurações", function()
    if not hasSaveSupport then
        updateLabel(saveStatus, "💾 Status: Executor sem suporte!")
        waitFunc(2)
        updateLabel(saveStatus, "💾 Status: Pronto")
        return
    end
    if loadConfig() then
        jjsCount = config.jjsCount
        delayValue = config.delayValue
        
        if config.placeAtual == "NOVA" then
            placeAtual = PLACE_CONFIG.NOVA
        else
            placeAtual = PLACE_CONFIG.ANTIGA
        end
        updateLabel(placeLabel, "📍 Place atual: " .. placeAtual.nome)
        carregarAnimacao()
        
        updateLabel(countLabel, "📊 Total enviado: " .. jjsCount)
        
        if config.autoJJsActive then
            ativoJJS = true
            if not animTrack then carregarAnimacao() end
            iniciarLoopJJS()
            updateLabel(statusLabel, "Status: Rodando")
            setToggle(autoJJsToggle, true)
        else
            ativoJJS = false
            pararLoopJJS()
            updateLabel(statusLabel, "Status: Parado")
            setToggle(autoJJsToggle, false)
        end
        
        spawnFunc(function()
            if config.p1Active then buildP1() setToggle(p1Toggle, true) else setToggle(p1Toggle, false) p1Parts = destroyParts(p1Parts) end
            if config.p2Active then buildP2() setToggle(p2Toggle, true) else setToggle(p2Toggle, false) p2Parts = destroyParts(p2Parts) end
            if config.p3Active then buildP3() setToggle(p3Toggle, true) else setToggle(p3Toggle, false) p3Parts = destroyParts(p3Parts) end
            if config.p4Active then buildP4() setToggle(p4Toggle, true) else setToggle(p4Toggle, false) p4Parts = destroyParts(p4Parts) end
        end)
        
        updateLabel(saveStatus, "💾 Status: Carregado!")
    else
        updateLabel(saveStatus, "💾 Status: Sem save!")
    end
    waitFunc(2)
    updateLabel(saveStatus, "💾 Status: Pronto")
end)

SecSave:NewButton("🗑️ RESETAR", "Apaga configurações", function()
    if not hasSaveSupport then
        updateLabel(saveStatus, "💾 Status: Executor sem suporte!")
        waitFunc(2)
        updateLabel(saveStatus, "💾 Status: Pronto")
        return
    end
    pcall(function() delfile(configFolder .. "/" .. configFile) end)
    updateLabel(saveStatus, "💾 Status: Resetado!")
    waitFunc(2)
    updateLabel(saveStatus, "💾 Status: Pronto")
end)

-- ABA: CRÉDITOS
local CreditTab = Window:NewTab("Créditos")
local CreditSec = CreditTab:NewSection("Criador")

CreditSec:NewLabel("👑 CRIADOR: Lucca")
CreditSec:NewLabel("📖 UI: Kavo Library")
CreditSec:NewLabel("⚡ Executor: Velocity")
CreditSec:NewLabel("📱 Suporte Mobile: ✅")
CreditSec:NewLabel("💾 Save/Load: " .. (hasSaveSupport and "✅" or "❌"))
CreditSec:NewLabel("🎯 Auto JJs: " .. (eventFound and "✅" or "❌"))

-- ABA: CONFIGURAÇÕES
local ConfigTab = Window:NewTab("Configurações")
local ConfigSec = ConfigTab:NewSection("Controles")

ConfigSec:NewKeybind("🔑 TOGGLE UI", "Tecla para mostrar/esconder", Enum.KeyCode.RightControl, function()
    Library:ToggleUI()
end)

-- ============ BOLINHA VERMELHA - OTIMIZADA PARA MOBILE ============
print("🔄 Criando bolinha LG para Mobile...")

local function criarBolinha()
    -- Verificar se já existe
    local existingGui = player.PlayerGui:FindFirstChild("LG_Toggle")
    if existingGui then existingGui:Destroy() end
    
    -- Criar ScreenGui
    local sg = Instance.new("ScreenGui")
    sg.Name = "LG_Toggle"
    sg.ResetOnSpawn = false
    sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    sg.Parent = player.PlayerGui
    
    -- Criar botão (maior para mobile)
    local btn = Instance.new("TextButton")
    btn.Size = isMobile and UDim2.new(0, 80, 0, 80) or UDim2.new(0, 55, 0, 55)
    btn.Position = UDim2.new(0, 20, 0, 150) -- Posição fixa em pixels
    btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    btn.Text = "LG"
    btn.TextScaled = true
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.BackgroundTransparency = 0
    btn.Parent = sg
    btn.ZIndex = 999
    btn.AutoButtonColor = false
    btn.BorderSizePixel = 0
    btn.Visible = true
    
    -- Corner arredondado
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = btn
    
    -- Sombra para melhor visibilidade
    local shadow = Instance.new("ImageLabel")
    shadow.Size = UDim2.new(1.2, 0, 1.2, 0)
    shadow.Position = UDim2.new(-0.1, 0, -0.1, 0)
    shadow.BackgroundTransparency = 1
    shadow.Image = "rbxassetid://1316045965"
    shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
    shadow.ImageTransparency = 0.6
    shadow.ZIndex = 998
    shadow.Parent = btn
    
    -- Sistema de drag melhorado para mobile
    local dragging = false
    local dragStart = nil
    local startPos = nil
    local wasDragging = false
    
    -- Função para obter tamanho da tela
    local function getScreenSize()
        local camera = workspace.CurrentCamera
        if camera then
            return camera.ViewportSize.X, camera.ViewportSize.Y
        end
        return 1920, 1080
    end
    
    -- Iniciar drag (clique/toque)
    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or 
           input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            wasDragging = false
            dragStart = input.Position
            startPos = btn.Position
        end
    end)
    
    -- Finalizar drag (soltar)
    btn.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or 
           input.UserInputType == Enum.UserInputType.Touch then
            if dragging and not wasDragging then
                -- Só abre a UI se não arrastou
                Library:ToggleUI()
            end
            dragging = false
        end
    end)
    
    -- Atualizar posição durante drag
    userInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or 
                         input.UserInputType == Enum.UserInputType.Touch) then
            wasDragging = true
            
            local delta = input.Position - dragStart
            local newX = startPos.X.Offset + delta.X
            local newY = startPos.Y.Offset + delta.Y
            
            local screenW, screenH = getScreenSize()
            local btnW = btn.Size.X.Offset
            local btnH = btn.Size.Y.Offset
            
            -- Limites com margem
            newX = math.clamp(newX, 0, screenW - btnW)
            newY = math.clamp(newY, 0, screenH - btnH - 80) -- Margem inferior maior
            
            btn.Position = UDim2.new(0, newX, 0, newY)
        end
    end)
    
    -- Correção quando a tela redimensionar
    local function onViewportChanged()
        local screenW, screenH = getScreenSize()
        local btnW = btn.Size.X.Offset
        local btnH = btn.Size.Y.Offset
        local currentX = btn.Position.X.Offset
        local currentY = btn.Position.Y.Offset
        
        -- Reposicionar se estiver fora dos limites
        if currentX + btnW > screenW then
            btn.Position = UDim2.new(0, math.max(0, screenW - btnW - 10), 0, currentY)
        end
        if currentY + btnH > screenH - 80 then
            btn.Position = UDim2.new(0, currentX, 0, math.max(0, screenH - btnH - 90))
        end
        if currentX < 0 then
            btn.Position = UDim2.new(0, 5, 0, currentY)
        end
        if currentY < 0 then
            btn.Position = UDim2.new(0, currentX, 0, 5)
        end
    end
    
    -- Conectar ao redimensionamento
    local camera = workspace.CurrentCamera
    if camera then
        camera:GetPropertyChangedSignal("ViewportSize"):Connect(onViewportChanged)
    end
    
    -- Efeitos visuais
    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(220, 0, 0)
    end)
    
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end)
    
    -- Para mobile: feedback tátil
    if isMobile then
        btn.TouchTap:Connect(function()
            btn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
            waitFunc(0.1)
            btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        end)
    end
    
    print("✅ Bolinha LG criada com sucesso! (Tamanho: " .. (isMobile and "Mobile" or "Desktop") .. ")")
    return btn
end

-- Criar a bolinha com proteção
local success, err = pcall(criarBolinha)
if not success then
    print("❌ Erro ao criar bolinha: " .. tostring(err))
    
    -- Tentativa de fallback
    pcall(function()
        local sg = Instance.new("ScreenGui")
        sg.Name = "LG_Toggle"
        sg.ResetOnSpawn = false
        sg.Parent = player.PlayerGui
        
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(0, 70, 0, 70)
        btn.Position = UDim2.new(0, 20, 0, 150)
        btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        btn.Text = "LG"
        btn.TextScaled = true
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.Parent = sg
        btn.ZIndex = 999
        
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(1, 0)
        corner.Parent = btn
        
        btn.MouseButton1Click:Connect(function()
            Library:ToggleUI()
        end)
        
        print("✅ Bolinha criada no fallback!")
    end)
end

-- ============ INICIALIZAÇÃO FINAL ============

loadConfig()

if config.placeAtual == "NOVA" then
    placeAtual = PLACE_CONFIG.NOVA
else
    placeAtual = PLACE_CONFIG.ANTIGA
end

jjsCount = config.jjsCount
delayValue = config.delayValue

spawnFunc(function()
    while not player.Character do waitFunc(0.5) end
    waitFunc(0.5)
    carregarAnimacao()
end)

player.CharacterAdded:Connect(function()
    waitFunc(0.5)
    carregarAnimacao()
    if ativoJJS then
        pararLoopJJS()
        waitFunc(0.5)
        if ativoJJS then
            iniciarLoopJJS()
        end
    end
end)

spawnFunc(function()
    waitFunc(1.5)
    if config.p1Active then spawnFunc(buildP1) setToggle(p1Toggle, true) end
    if config.p2Active then spawnFunc(buildP2) setToggle(p2Toggle, true) end
    if config.p3Active then spawnFunc(buildP3) setToggle(p3Toggle, true) end
    if config.p4Active then spawnFunc(buildP4) setToggle(p4Toggle, true) end
end)

print("✅ PRODIGIOZX MODS carregado!")
print("👑 Criador: Lucca")
print("📱 Modo: " .. (isMobile and "MOBILE" or "Desktop"))
print("🏙️ Place: " .. placeAtual.nome)
print("🎯 Auto JJs: " .. (eventFound and "Disponível" or "Indisponível"))
print("🔴 Bolinha LG: " .. (player.PlayerGui:FindFirstChild("LG_Toggle") and "VISÍVEL" or "NÃO ENCONTRADA"))
