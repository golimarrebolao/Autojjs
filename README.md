-- ============ BOLINHA VERMELHA - VERSÃO MOBILE CORRIGIDA ============
local function criarBolinhaMobile()
    -- Versão Mobile (otimizada para toque)
    local existingGui = player.PlayerGui:FindFirstChild("LG_Toggle")
    if existingGui then existingGui:Destroy() end
    
    local sg = Instance.new("ScreenGui")
    sg.Name = "LG_Toggle"
    sg.ResetOnSpawn = false
    sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    sg.Parent = player.PlayerGui
    
    -- Impedir que o toque passe para outros elementos
    sg.IgnoreGuiInset = true
    
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 80, 0, 80)
    btn.Position = UDim2.new(0, 20, 0, 150)
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
    
    -- IMPEDIR QUE O CLIQUE PASSE PARA OUTROS ELEMENTOS
    btn.Modal = true
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = btn
    
    -- Sombra
    local shadow = Instance.new("ImageLabel")
    shadow.Size = UDim2.new(1.2, 0, 1.2, 0)
    shadow.Position = UDim2.new(-0.1, 0, -0.1, 0)
    shadow.BackgroundTransparency = 1
    shadow.Image = "rbxassetid://1316045965"
    shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
    shadow.ImageTransparency = 0.6
    shadow.ZIndex = 998
    shadow.Parent = btn
    
    -- Sistema de drag para mobile (APENAS NO BOTÃO)
    local dragging = false
    local dragStart = nil
    local startPos = nil
    local wasDragging = false
    
    local function getScreenSize()
        local camera = workspace.CurrentCamera
        if camera then
            return camera.ViewportSize.X, camera.ViewportSize.Y
        end
        return 1920, 1080
    end
    
    -- CAPTURAR APENAS TOQUES NO BOTÃO
    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            -- Verificar se o toque foi no botão
            local position = input.Position
            local btnPos = btn.AbsolutePosition
            local btnSize = btn.AbsoluteSize
            
            if position.X >= btnPos.X and position.X <= btnPos.X + btnSize.X and
               position.Y >= btnPos.Y and position.Y <= btnPos.Y + btnSize.Y then
                dragging = true
                wasDragging = false
                dragStart = input.Position
                startPos = btn.Position
                
                -- IMPEDIR QUE O TOQUE SE PROPAGUE
                input:StopPropagation()
            end
        end
    end)
    
    btn.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            if dragging and not wasDragging then
                -- Só abre a UI se NÃO arrastou
                Library:ToggleUI()
            end
            dragging = false
            -- IMPEDIR PROPAGAÇÃO
            input:StopPropagation()
        end
    end)
    
    -- ATUALIZAR POSIÇÃO APENAS SE ESTIVER ARRASTANDO
    userInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.Touch then
            wasDragging = true
            
            local delta = input.Position - dragStart
            local newX = startPos.X.Offset + delta.X
            local newY = startPos.Y.Offset + delta.Y
            
            local screenW, screenH = getScreenSize()
            local btnW = btn.Size.X.Offset
            local btnH = btn.Size.Y.Offset
            
            newX = math.clamp(newX, 0, screenW - btnW)
            newY = math.clamp(newY, 0, screenH - btnH - 80)
            
            btn.Position = UDim2.new(0, newX, 0, newY)
            
            -- IMPEDIR PROPAGAÇÃO
            input:StopPropagation()
        end
    end)
    
    -- Impedir toque acidental em outros elementos
    sg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            local position = input.Position
            local btnPos = btn.AbsolutePosition
            local btnSize = btn.AbsoluteSize
            
            -- Se o toque NÃO foi no botão, ignorar
            if not (position.X >= btnPos.X and position.X <= btnPos.X + btnSize.X and
                    position.Y >= btnPos.Y and position.Y <= btnPos.Y + btnSize.Y) then
                input:StopPropagation()
            end
        end
    end)
    
    -- Feedback tátil
    btn.TouchTap:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
        waitFunc(0.1)
        btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end)
    
    print("✅ Bolinha Mobile criada com drag limitado ao botão!")
end

-- ============ VERSÃO DESKTOP CORRIGIDA ============
local function criarBolinhaDesktop()
    local existingGui = player.PlayerGui:FindFirstChild("LG_Toggle")
    if existingGui then existingGui:Destroy() end
    
    local sg = Instance.new("ScreenGui")
    sg.Name = "LG_Toggle"
    sg.ResetOnSpawn = false
    sg.Parent = player.PlayerGui
    
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 50, 0, 50)
    btn.Position = UDim2.new(0, 20, 0.5, -25)
    btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    btn.Text = "LG"
    btn.TextScaled = true
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.BackgroundTransparency = 0
    btn.Parent = sg
    btn.ZIndex = 999
    btn.AutoButtonColor = false
    btn.BorderSizePixel = 0
    
    -- IMPEDIR QUE O CLIQUE PASSE PARA OUTROS ELEMENTOS
    btn.Modal = true
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = btn
    
    -- Sistema de drag (APENAS NO BOTÃO)
    local dragging = false
    local dragStart = nil
    local startPos = nil
    local wasDragging = false
    
    local function onInputBegan(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            -- Verificar se o clique foi no botão
            local position = input.Position
            local btnPos = btn.AbsolutePosition
            local btnSize = btn.AbsoluteSize
            
            if position.X >= btnPos.X and position.X <= btnPos.X + btnSize.X and
               position.Y >= btnPos.Y and position.Y <= btnPos.Y + btnSize.Y then
                dragging = true
                wasDragging = false
                dragStart = input.Position
                startPos = btn.Position
                input:StopPropagation()
            end
        end
    end
    
    local function onInputEnded(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            if dragging and not wasDragging then
                Library:ToggleUI()
            end
            dragging = false
            input:StopPropagation()
        end
    end
    
    local function onInputChanged(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            wasDragging = true
            local delta = input.Position - dragStart
            local newX = startPos.X.Offset + delta.X
            local newY = startPos.Y.Offset + delta.Y
            
            local screenSize = game:GetService("GuiService"):GetGuiInset()
            local maxX = screenSize.X - btn.Size.X.Offset
            local maxY = screenSize.Y - btn.Size.Y.Offset
            
            newX = math.clamp(newX, 0, maxX)
            newY = math.clamp(newY, 0, maxY)
            
            btn.Position = UDim2.new(0, newX, 0, newY)
            input:StopPropagation()
        end
    end
    
    btn.InputBegan:Connect(onInputBegan)
    btn.InputEnded:Connect(onInputEnded)
    userInputService.InputChanged:Connect(onInputChanged)
    
    print("✅ Bolinha Desktop criada com drag limitado ao botão!")
end
