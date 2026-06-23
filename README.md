-- [[ PRODIGIOZX MODS - VERSÃO COMPLETA ]] --
-- Criador: Lucca

local player = game:GetService("Players").LocalPlayer
local mouse = player:GetMouse()

-- ============ CARREGAR KAVO UI ============
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()

local Window = Library.CreateLib("PRODIGIOZX MODS | By Lucca", {
    SchemeColor = Color3.fromRGB(255, 0, 0),
    Background = Color3.fromRGB(20, 20, 20),
    Header = Color3.fromRGB(15, 15, 15),
    TextColor = Color3.fromRGB(255, 255, 255)
})

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

-- ============ TABELAS ============
local p1Parts, p2Parts, p3Parts, p4Parts = {}, {}, {}, {}
local buildingP1, buildingP2, buildingP3, buildingP4 = false, false, false, false

-- ============ AUTO JJS VARIÁVEIS ============
local jjsEvent = nil
pcall(function()
    jjsEvent = game:GetService("ReplicatedStorage"):FindFirstChild("JJsBillboardEvent")
    if not jjsEvent then
        jjsEvent = game:GetService("ReplicatedStorage"):WaitForChild("JJsBillboardEvent", 2)
    end
end)

if not jjsEvent then
    print("⚠️ Evento JJsBillboardEvent NÃO ENCONTRADO! Auto JJs desativado.")
end

local ativoJJS = false
local animTrack = nil
local jjsCount = 0
local delayValue = 0.8
local currentLoop = nil

-- ============ CLONADOR VARIÁVEIS ============
local clones = {}
local modoClique = false
local posicoesFixas = {
    Vector3.new(146.38, 3.04, -855.76),
    Vector3.new(145.41, 3.04, -897.85),
    Vector3.new(398.03, 3.04, -857.27),
    Vector3.new(396.03, 3.04, -907.93)
}

-- ============ MARCADORES VARIÁVEIS ============
local marcadores = {}

-- ============ RÉGUA DE ÂNGULO VARIÁVEIS ============
local anguloUI = nil
local anguloLabel = nil
local anguloConexao = nil

-- ============ TAF VARIÁVEIS ============
local tafParts = {}

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
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char:WaitForChild("Humanoid")
    local anim = Instance.new("Animation")
    anim.AnimationId = placeAtual.animacaoId
    animTrack = hum:LoadAnimation(anim)
    print("✅ Animação carregada: " .. placeAtual.nome)
end

local function enviarJJs(valor)
    if not jjsEvent then return end
    local args = { valor }
    pcall(function()
        jjsEvent:FireServer(unpack(args))
    end)
end

local function tocarAnimacao()
    if animTrack then
        animTrack:Stop()
        animTrack:Play()
    end
end

local function iniciarLoopJJS()
    if currentLoop then return end
    
    currentLoop = task.spawn(function()
        while ativoJJS do
            jjsCount = jjsCount + 1
            tocarAnimacao()
            enviarJJs(jjsCount)
            task.wait(delayValue)
        end
        currentLoop = nil
    end)
end

local function pararLoopJJS()
    ativoJJS = false
    if currentLoop then
        task.cancel(currentLoop)
        currentLoop = nil
    end
    if animTrack then
        animTrack:Stop()
    end
end

-- ============ FUNÇÕES DO CLONADOR ============

local function clonar(posicao, angulo)
    local char = player.Character
    if not char then
        print("❌ Personagem não encontrado!")
        return
    end
    
    angulo = angulo or 0
    
    local clone = char:Clone()
    clone.Name = "Clone_" .. os.time()
    clone.Parent = workspace
    
    local cloneHrp = clone:FindFirstChild("HumanoidRootPart")
    if cloneHrp then
        cloneHrp.CFrame = CFrame.new(posicao) * CFrame.Angles(0, math.rad(angulo), 0)
        cloneHrp.Anchored = true
    end
    
    for _, part in pairs(clone:GetDescendants()) do
        if part:IsA("BasePart") then
            part.Anchored = true
            part.CanCollide = false
        end
    end
    
    local hum = clone:FindFirstChild("Humanoid")
    if hum then
        hum.PlatformStand = true
    end
    
    table.insert(clones, clone)
    print("✅ Clone criado em: " .. tostring(posicao) .. " | Ângulo: " .. angulo .. "°")
end

local function criarClonesFixos()
    local char = player.Character
    if not char then
        print("❌ Personagem não encontrado!")
        return
    end
    
    for _, v in pairs(clones) do
        pcall(function() v:Destroy() end)
    end
    clones = {}
    
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local angulo = hrp and hrp.Orientation.Y or 0
    
    for i, pos in ipairs(posicoesFixas) do
        clonar(pos, angulo)
    end
    
    print("🔹 " .. #posicoesFixas .. " clones fixos criados!")
end

local function removerClones()
    for _, v in pairs(clones) do
        pcall(function() v:Destroy() end)
    end
    clones = {}
    print("🗑️ Clones removidos!")
end

local function copiarPosicoesClones()
    if #clones == 0 then
        print("⚠️ Nenhum clone encontrado!")
        return
    end
    
    local texto = ""
    for i, clone in ipairs(clones) do
        local hrp = clone:FindFirstChild("HumanoidRootPart")
        if hrp then
            local pos = hrp.Position
            texto = texto .. string.format("Clone %d: Vector3.new(%.2f, %.2f, %.2f)\n", i, pos.X, pos.Y, pos.Z)
        end
    end
    
    setclipboard(texto)
    print("📋 Posições de " .. #clones .. " clones copiadas!")
end

-- ============ FUNÇÕES DOS MARCADORES ============

local function criarMarcadores()
    for _, v in pairs(marcadores) do
        pcall(function() v:Destroy() end)
    end
    marcadores = {}
    
    local markers = {
        { position = Vector3.new(145.89, 3.04, -855.14), angle = -89.9 },
        { position = Vector3.new(145.41, 3.04, -897.85), angle = -91.8 },
        { position = Vector3.new(398.03, 3.04, -857.27), angle = 89.0 },
        { position = Vector3.new(396.03, 3.04, -907.93), angle = 110.8 },
    }
    
    local function createMarker(position, angleDegrees, index)
        local name = "Marker_" .. index
        local cframe = CFrame.new(position) * CFrame.Angles(0, math.rad(angleDegrees), 0)
        
        local base = Instance.new("Part")
        base.Name = name
        base.Size = Vector3.new(4, 0.2, 4)
        base.Anchored = true
        base.CanCollide = false
        base.CanQuery = false
        base.CanTouch = false
        base.Material = Enum.Material.Neon
        base.Color = Color3.fromRGB(255, 0, 0)
        base.CFrame = cframe
        base.Parent = workspace
        table.insert(marcadores, base)
        
        local arrow = Instance.new("WedgePart")
        arrow.Name = name .. "_Arrow"
        arrow.Size = Vector3.new(1, 1, 3)
        arrow.Anchored = true
        arrow.CanCollide = false
        arrow.CanQuery = false
        arrow.CanTouch = false
        arrow.Material = Enum.Material.Neon
        arrow.Color = Color3.fromRGB(0, 255, 0)
        arrow.CFrame = cframe * CFrame.new(0, 0.6, -1.5) * CFrame.Angles(0, math.rad(180), 0)
        arrow.Parent = workspace
        table.insert(marcadores, arrow)
        
        local billboard = Instance.new("BillboardGui")
        billboard.Name = name .. "_Label"
        billboard.Size = UDim2.new(0, 150, 0, 80)
        billboard.StudsOffset = Vector3.new(0, 4, 0)
        billboard.AlwaysOnTop = true
        billboard.Adornee = base
        billboard.Parent = base
        
        local angleLabel = Instance.new("TextLabel")
        angleLabel.Size = UDim2.new(1, 0, 0.6, 0)
        angleLabel.Position = UDim2.new(0, 0, 0, 0)
        angleLabel.BackgroundTransparency = 1
        angleLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
        angleLabel.TextStrokeTransparency = 0
        angleLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
        angleLabel.Font = Enum.Font.SourceSansBold
        angleLabel.TextScaled = true
        angleLabel.Text = string.format("%.1f°", angleDegrees)
        angleLabel.Parent = billboard
        
        local coordLabel = Instance.new("TextLabel")
        coordLabel.Size = UDim2.new(1, 0, 0.4, 0)
        coordLabel.Position = UDim2.new(0, 0, 0.6, 0)
        coordLabel.BackgroundTransparency = 1
        coordLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        coordLabel.TextStrokeTransparency = 0
        coordLabel.Font = Enum.Font.Code
        coordLabel.TextScaled = true
        coordLabel.Text = string.format("(%.2f, %.2f, %.2f)", position.X, position.Y, position.Z)
        coordLabel.Parent = billboard
    end
    
    for i, data in ipairs(markers) do
        createMarker(data.position, data.angle, i)
    end
    
    print("✅ " .. #markers .. " marcadores criados!")
end

local function removerMarcadores()
    for _, v in pairs(marcadores) do
        pcall(function() v:Destroy() end)
    end
    marcadores = {}
    print("🗑️ Marcadores removidos!")
end

-- ============ FUNÇÕES DA RÉGUA DE ÂNGULO ============

local function criarAnguloUI()
    if anguloUI then
        pcall(function() anguloUI:Destroy() end)
        anguloUI = nil
    end
    
    if anguloConexao then
        pcall(function() anguloConexao:Disconnect() end)
        anguloConexao = nil
    end
    
    anguloUI = Instance.new("ScreenGui")
    anguloUI.Name = "AnguloUI"
    anguloUI.ResetOnSpawn = false
    anguloUI.Parent = player.PlayerGui
    anguloUI.Enabled = true
    
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 300, 0, 100)
    frame.Position = UDim2.new(0.5, -150, 0.02, 0)
    frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    frame.BackgroundTransparency = 0.4
    frame.BorderSizePixel = 0
    frame.Parent = anguloUI
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = frame
    
    local tituloLabel = Instance.new("TextLabel")
    tituloLabel.Size = UDim2.new(1, 0, 0.2, 0)
    tituloLabel.Position = UDim2.new(0, 0, 0, 0)
    tituloLabel.BackgroundTransparency = 1
    tituloLabel.Text = "🎯 ÂNGULO DO PERSONAGEM"
    tituloLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    tituloLabel.Font = Enum.Font.GothamBold
    tituloLabel.TextSize = 12
    tituloLabel.Parent = frame
    
    anguloLabel = Instance.new("TextLabel")
    anguloLabel.Size = UDim2.new(1, 0, 0.8, 0)
    anguloLabel.Position = UDim2.new(0, 0, 0.2, 0)
    anguloLabel.BackgroundTransparency = 1
    anguloLabel.Text = "0.0°"
    anguloLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
    anguloLabel.Font = Enum.Font.GothamBlack
    anguloLabel.TextSize = 55
    anguloLabel.Parent = frame
    
    local runService = game:GetService("RunService")
    anguloConexao = runService.Heartbeat:Connect(function()
        if not anguloUI or not anguloLabel then
            pcall(function() anguloConexao:Disconnect() end)
            anguloConexao = nil
            return
        end
        
        local char = player.Character
        if not char then return end
        
        local angulo = nil
        
        local hrp = char:FindFirstChild("HumanoidRootPart")
        if hrp then
            angulo = hrp.Orientation.Y
        end
        
        if not angulo then
            local torso = char:FindFirstChild("Torso")
            if torso then
                angulo = torso.Orientation.Y
            end
        end
        
        if not angulo then
            local upperTorso = char:FindFirstChild("UpperTorso")
            if upperTorso then
                angulo = upperTorso.Orientation.Y
            end
        end
        
        if not angulo then
            local hum = char:FindFirstChild("Humanoid")
            if hum and hum.RootPart then
                angulo = hum.RootPart.Orientation.Y
            end
        end
        
        if not angulo then
            for _, part in pairs(char:GetChildren()) do
                if part:IsA("BasePart") and string.find(part.Name, "Root") then
                    angulo = part.Orientation.Y
                    break
                end
            end
        end
        
        if not angulo then
            local camera = workspace.CurrentCamera
            if camera then
                local look = camera.CFrame.LookVector
                angulo = math.deg(math.atan2(look.X, -look.Z))
            end
        end
        
        if angulo then
            angulo = ((angulo + 180) % 360) - 180
            anguloLabel.Text = string.format("%.1f°", angulo)
            
            if angulo >= -5 and angulo <= 5 then
                anguloLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
            elseif angulo > 5 and angulo <= 45 then
                anguloLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
            elseif angulo > 45 and angulo <= 90 then
                anguloLabel.TextColor3 = Color3.fromRGB(255, 165, 0)
            elseif angulo > 90 or angulo < -90 then
                anguloLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
            elseif angulo < -5 and angulo >= -45 then
                anguloLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
            elseif angulo < -45 and angulo >= -90 then
                anguloLabel.TextColor3 = Color3.fromRGB(255, 165, 0)
            end
        end
    end)
    
    print("📐 Régua de ângulo ativada!")
end

local function removerAnguloUI()
    if anguloConexao then
        pcall(function() anguloConexao:Disconnect() end)
        anguloConexao = nil
    end
    if anguloUI then
        pcall(function() anguloUI:Destroy() end)
        anguloUI = nil
    end
    anguloLabel = nil
    print("🗑️ Régua de ângulo removida!")
end

-- ============ FUNÇÕES DO TAF ============

local function criarPartesTaf()
    for _, v in pairs(tafParts) do
        pcall(function() v:Destroy() end)
    end
    tafParts = {}
    
    local partes = {
        {
            Nome = "BFE",
            Posicao = Vector3.new(-188.21, 1.04, -873.57),
            Tamanho = Vector3.new(6, 3, 9.08)
        },
        {
            Nome = "BAC",
            Posicao = Vector3.new(-268.5, 1, -860.35),
            Tamanho = Vector3.new(7, 3, 9.3)
        },
        {
            Nome = "BPE",
            Posicao = Vector3.new(-325.71, 1.54, -860.47),
            Tamanho = Vector3.new(7, 4, 9)
        }
    }
    
    for _, info in ipairs(partes) do
        local p = Instance.new("Part")
        p.Name = info.Nome
        p.Size = info.Tamanho
        p.Position = info.Posicao
        p.Anchored = true
        p.Transparency = 0.5
        p.BrickColor = BrickColor.new("Bright red")
        p.Material = Enum.Material.Neon
        p.Parent = workspace
        table.insert(tafParts, p)
        print("✅ Parte criada: " .. info.Nome)
    end
    
    print("🔹 " .. #partes .. " partes criadas!")
end

local function removerPartesTaf()
    for _, v in pairs(tafParts) do
        pcall(function() v:Destroy() end)
    end
    tafParts = {}
    print("🗑️ Partes removidas!")
end

-- ============ FUNÇÃO PARA CARREGAR TAS ============
local function carregarTAS()
    print("🔄 Carregando TAS...")
    pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/bbssgggv112/skwnqnksnw/refs/heads/main/tas.txt"))()
        print("✅ TAS carregado com sucesso!")
    end)
end

-- ============ FUNÇÃO UPDATE LABEL ============
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

-- ============ ABAS ============

-- ABA: PISTAS
local TabPistas = Window:NewTab("Auxiliares")
local SecPistas = TabPistas:NewSection("Pistas de Treino")

SecPistas:NewToggle("Parkour 1", "Ativa P1 + Pontes", function(s)
    if s then buildP1() else for _,v in pairs(p1Parts) do v:Destroy() end p1Parts = {} end
end)

SecPistas:NewToggle("Parkour 2", "Ativa P2", function(s)
    if s then buildP2() else for _,v in pairs(p2Parts) do v:Destroy() end p2Parts = {} end
end)

SecPistas:NewToggle("Parkour 3", "Ativa P3", function(s)
    if s then buildP3() else for _,v in pairs(p3Parts) do v:Destroy() end p3Parts = {} end
end)

SecPistas:NewToggle("Parkour 4", "Ativa P4", function(s)
    if s then buildP4() else for _,v in pairs(p4Parts) do v:Destroy() end p4Parts = {} end
end)

-- ABA: PLACE
local TabPlace = Window:NewTab("Place")
local SecPlace = TabPlace:NewSection("Selecionar Place")

local placeLabel = SecPlace:NewLabel("📍 Place atual: " .. placeAtual.nome)

SecPlace:NewButton("🏙️ Place Nova", "Usar animação da nova place", function()
    placeAtual = PLACE_CONFIG.NOVA
    updateLabel(placeLabel, "📍 Place atual: " .. placeAtual.nome)
    if animTrack then carregarAnimacao() end
    print("✅ Place alterada para: NOVA")
end)

SecPlace:NewButton("🏚️ Place Antiga", "Usar animação da place antiga", function()
    placeAtual = PLACE_CONFIG.ANTIGA
    updateLabel(placeLabel, "📍 Place atual: " .. placeAtual.nome)
    if animTrack then carregarAnimacao() end
    print("✅ Place alterada para: ANTIGA")
end)

SecPlace:NewLabel("")
SecPlace:NewLabel("📌 INFORMAÇÕES:")
SecPlace:NewLabel("• Nova Place: rbxassetid://122367567937291")
SecPlace:NewLabel("• Antiga Place: rbxassetid://124954987474196")

-- ABA: AUTO JJS
local TabJJS = Window:NewTab("Auto JJs")
local SecJJS = TabJJS:NewSection("Auto Farm JJs")

local countLabel = SecJJS:NewLabel("📊 Total enviado: 0")
local statusLabel = SecJJS:NewLabel("Status: Parado")

SecJJS:NewToggle("🔘 LIGAR/DESLIGAR", "Ativa o auto farm de JJs", function(state)
    if state then
        ativoJJS = true
        if not animTrack then carregarAnimacao() end
        iniciarLoopJJS()
        statusLabel:UpdateLabel("Status: Rodando")
    else
        pararLoopJJS()
        statusLabel:UpdateLabel("Status: Parado")
    end
end)

SecJJS:NewButton("🔄 RESETAR CONTADOR", "Zera o contador", function()
    jjsCount = 0
    countLabel:UpdateLabel("📊 Total enviado: 0")
end)

SecJJS:NewSlider("⚡ VELOCIDADE", "Delay entre animações (0.1s - 2.0s)", 20, 1, function(value)
    delayValue = value / 10
end)

SecJJS:NewButton("🎭 TESTAR ANIMAÇÃO", "Testa a animação atual", function()
    if not animTrack then carregarAnimacao() end
    tocarAnimacao()
    print("🎭 Testando animação: " .. placeAtual.nome)
end)

-- ABA: CLONADOR
local TabClonar = Window:NewTab("Clonador")
local SecClonar = TabClonar:NewSection("Clonar")

local cloneCountLabel = SecClonar:NewLabel("📊 Clones gerados: 0")

SecClonar:NewButton("🧍 CLONAR AQUI", "Gera um clone na sua posição", function()
    local char = player.Character
    if not char then
        print("❌ Personagem não encontrado!")
        return
    end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then
        print("❌ HumanoidRootPart não encontrado!")
        return
    end
    local angulo = hrp.Orientation.Y
    clonar(hrp.Position, angulo)
    updateLabel(cloneCountLabel, "📊 Clones gerados: " .. #clones)
end)

SecClonar:NewToggle("🖱️ MODO CLIQUE", "Clique no chão para clonar", function(state)
    modoClique = state
    print(state and "🖱️ Modo clique ATIVADO" or "🖱️ Modo clique DESATIVADO")
end)

SecClonar:NewButton("📍 CLONES FIXOS", "Gera clones nas posições fixas", function()
    criarClonesFixos()
    updateLabel(cloneCountLabel, "📊 Clones gerados: " .. #clones)
end)

SecClonar:NewButton("🗑️ REMOVER CLONES", "Remove todos os clones", function()
    removerClones()
    updateLabel(cloneCountLabel, "📊 Clones gerados: 0")
end)

-- ABA: POSIÇÕES
local TabPos = Window:NewTab("Posições")
local SecPos = TabPos:NewSection("Copiar Posições")

SecPos:NewButton("📋 COPIAR POSIÇÕES DOS CLONES", "Copia a posição de todos os clones", function()
    copiarPosicoesClones()
end)

SecPos:NewButton("📋 LISTAR CLONES", "Mostra todos os clones no console", function()
    if #clones == 0 then
        print("⚠️ Nenhum clone encontrado!")
        return
    end
    print("===== CLONES =====")
    for i, clone in ipairs(clones) do
        local hrp = clone:FindFirstChild("HumanoidRootPart")
        if hrp then
            local pos = hrp.Position
            print(i .. ". " .. clone.Name .. " - " .. string.format("(%.2f, %.2f, %.2f)", pos.X, pos.Y, pos.Z))
        end
    end
end)

-- ============ ABA: TÁS (NOVA) ============
local TabTas = Window:NewTab("Tás")
local SecTas = TabTas:NewSection("TAS - Ferramentas")

-- Botão para carregar TAS
SecTas:NewButton("🚀 CARREGAR TAS", "Carrega o script TAS", function()
    carregarTAS()
end)

-- Subseção: Marcadores
SecTas:NewLabel("")
SecTas:NewLabel("━━━━━ 📍 MARCADORES ━━━━━")

SecTas:NewButton("📍 GERAR MARCADORES", "Cria marcadores com ângulos nas posições fixas", function()
    criarMarcadores()
end)

SecTas:NewButton("🗑️ REMOVER MARCADORES", "Remove todos os marcadores", function()
    removerMarcadores()
end)

SecTas:NewLabel("📌 POSIÇÕES DOS MARCADORES:")
SecTas:NewLabel("• Marker 1: 145.89, 3.04, -855.14 | -89.9°")
SecTas:NewLabel("• Marker 2: 145.41, 3.04, -897.85 | -91.8°")
SecTas:NewLabel("• Marker 3: 398.03, 3.04, -857.27 | 89.0°")
SecTas:NewLabel("• Marker 4: 396.03, 3.04, -907.93 | 110.8°")

-- Subseção: Régua
SecTas:NewLabel("")
SecTas:NewLabel("━━━━━ 📐 RÉGUA DE ÂNGULO ━━━━━")

SecTas:NewButton("📐 MOSTRAR RÉGUA", "Mostra o ângulo do seu personagem na tela", function()
    criarAnguloUI()
end)

SecTas:NewButton("🗑️ REMOVER RÉGUA", "Remove a régua de ângulo", function()
    removerAnguloUI()
end)

SecTas:NewLabel("📌 INFORMAÇÕES:")
SecTas:NewLabel("• A régua mostra o ângulo em tempo real")
SecTas:NewLabel("• Cores: Verde (frente) | Amarelo (lado)")
SecTas:NewLabel("• Laranja (diagonal) | Vermelho (costas)")
SecTas:NewLabel("• Ângulo normalizado entre -180° e 180°")

-- ============ FIM DA ABA TÁS ============

-- ABA: TAF
local TabTaf = Window:NewTab("Taf")
local SecTaf = TabTaf:NewSection("Partes de Proteção")

SecTaf:NewButton("🟢 CRIAR PARTES", "Cria as partes de proteção do brasão", function()
    criarPartesTaf()
end)

SecTaf:NewButton("🔴 REMOVER PARTES", "Remove todas as partes", function()
    removerPartesTaf()
end)

SecTaf:NewLabel("")
SecTaf:NewLabel("📌 POSIÇÕES DAS PARTES:")
SecTaf:NewLabel("• BFE: -188.21, 1.04, -873.57")
SecTaf:NewLabel("• BAC: -268.5, 1, -860.35")
SecTaf:NewLabel("• BPE: -325.71, 1.54, -860.47")

-- ABA: CRÉDITOS
local CreditTab = Window:NewTab("Créditos")
local CreditSec = CreditTab:NewSection("Criador")

CreditSec:NewLabel("👑 CRIADOR: Lucca")
CreditSec:NewLabel("📖 UI: Kavo Library")
CreditSec:NewLabel("⚡ Executor: Velocity")

-- ABA: CONFIGURAÇÕES
local ConfigTab = Window:NewTab("Configurações")
local ConfigSec = ConfigTab:NewSection("Controles")

ConfigSec:NewKeybind("🔑 TOGGLE UI", "Tecla para mostrar/esconder", Enum.KeyCode.RightControl, function()
    Library:ToggleUI()
end)

-- ============ SISTEMA DE CLIQUE ============
mouse.Button1Down:Connect(function()
    if modoClique then
        local pos = mouse.Hit.p
        clonar(pos, 0)
        updateLabel(cloneCountLabel, "📊 Clones gerados: " .. #clones)
    end
end)

-- ============ ATUALIZAR CONTADOR ============
task.spawn(function()
    while true do
        task.wait(1)
        updateLabel(cloneCountLabel, "📊 Clones gerados: " .. #clones)
    end
end)

-- ============ BOLINHA VERMELHA ============
local sg = Instance.new("ScreenGui", player.PlayerGui)
sg.Name = "LG_Toggle"
sg.ResetOnSpawn = false

local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 50, 0, 50)
btn.Position = UDim2.new(0, 20, 0.5, -25)
btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
btn.Text = "LG"
btn.TextScaled = true
btn.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", btn).CornerRadius = UDim.new(1, 0)

local dragging = false
local dragStart = nil
local startPos = nil
local wasDragging = false
local userInputService = game:GetService("UserInputService")
local camera = workspace.CurrentCamera

btn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        wasDragging = false
        dragStart = input.Position
        startPos = btn.Position
    end
end)

btn.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or 
       input.UserInputType == Enum.UserInputType.Touch then
        if dragging and not wasDragging then
            Library:ToggleUI()
        end
        dragging = false
    end
end)

userInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or 
                     input.UserInputType == Enum.UserInputType.Touch) then
        wasDragging = true
        local delta = input.Position - dragStart
        
        local newX = startPos.X.Offset + delta.X
        local newY = startPos.Y.Offset + delta.Y
        
        local viewport = camera.ViewportSize
        local maxX = viewport.X - btn.Size.X.Offset
        local maxY = viewport.Y - btn.Size.Y.Offset
        
        newX = math.clamp(newX, 0, maxX)
        newY = math.clamp(newY, 0, maxY)
        
        btn.Position = UDim2.new(0, newX, 0, newY)
    end
end)

-- ============ INICIALIZAÇÃO ============

if player.Character then
    carregarAnimacao()
else
    player.CharacterAdded:Connect(carregarAnimacao)
end

player.CharacterAdded:Connect(function()
    task.wait(0.5)
    carregarAnimacao()
    if ativoJJS then
        pararLoopJJS()
    end
end)

print("✅ PRODIGIOZX MODS carregado!")
print("👑 Criador: Lucca")
print("📋 Abas: Auxiliares | Place | Auto JJs | Clonador | Posições | Tás | Taf | Créditos | Configurações")
print("🚀 TAS disponível na aba 'Tás'!")
print("📍 Marcadores e Régua também estão na aba 'Tás'!")
