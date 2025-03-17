--[[
    Script para Mini City RP (Roblox) - Menu de Armas (Adaptado para PC)
    Criado por: [Seu Nome/Nickname]

    Instruções:
    1. Cole este script no StarterPlayer > StarterPlayerScripts
    2. Ajuste as IDs das armas (item_id) na tabela 'armas' de acordo com o seu jogo.
    3. Teste o script no seu jogo Mini City RP.

    Observações:
    - Este script é uma base e pode precisar de ajustes dependendo da estrutura do seu servidor.
    - Certifique-se de que o servidor permite adicionar itens ao inventário via script.
    - Adapte as IDs dos itens (armas) aos IDs corretos do seu jogo.
    - Este script foi adaptado especificamente para PC.
--]]

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local ContextActionService = game:GetService("ContextActionService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Definição das armas e seus IDs (SUBSTITUA OS IDs PELOS CORRETOS DO SEU JOGO)
local armas = {
    Uzi = 123456789,
    AK47 = 987654321,
    PARAFAL = 112233445,
    G3 = 554433221,
    IA2 = 998877665,
    ["AR-15"] = 567890123
}

-- Variáveis de estado
local menuAtivo = false
local armasAtivas = false
local teclaMenu = Enum.KeyCode.F -- Tecla para abrir o menu no PC

-- Interface Gráfica
local menuPrincipal = Instance.new("ScreenGui")
menuPrincipal.Name = "MenuPrincipalArmas"
menuPrincipal.Parent = playerGui
menuPrincipal.ResetOnSpawn = false
menuPrincipal.DisplayOrder = 1 -- Garante que o menu fique na frente de outros elementos

-- Frame Principal do Menu
local frameMenu = Instance.new("Frame")
frameMenu.Name = "FrameMenu"
frameMenu.Parent = menuPrincipal
frameMenu.Size = UDim2.new(0.4, 0, 0.6, 0) -- Reduzi o tamanho do frame
frameMenu.Position = UDim2.new(0.3, 0, 0.2, 0) -- Centralizado na tela
frameMenu.BackgroundColor3 = Color3.new(0, 0, 0) -- Preto
frameMenu.BorderColor3 = Color3.new(0, 0, 0)
frameMenu.BorderSizePixel = 0
frameMenu.Visible = false -- Inicialmente invisível

-- Título do Menu
local titulo = Instance.new("TextLabel")
titulo.Name = "Titulo"
titulo.Parent = frameMenu
titulo.Size = UDim2.new(1, 0, 0.15, 0)
titulo.Position = UDim2.new(0, 0, 0, 0)
titulo.BackgroundColor3 = Color3.new(0, 0, 0)
titulo.TextColor3 = Color3.new(1, 1, 1)
titulo.Text = "MENU DE ARMAS"
titulo.Font = Enum.Font.SourceSansBold
titulo.TextScaled = true
titulo.BorderColor3 = Color3.new(0, 0, 0)
titulo.BorderSizePixel = 0

-- Botão para ativar/desativar o menu (no canto inferior direito, mais discreto)
local botaoMenu = Instance.new("TextButton")
botaoMenu.Name = "BotaoMenu"
botaoMenu.Parent = menuPrincipal
botaoMenu.Size = UDim2.new(0.15, 0, 0.05, 0)
botaoMenu.Position = UDim2.new(0.84, 0, 0.94, 0) -- canto inferior direito
botaoMenu.BackgroundColor3 = Color3.new(0, 0, 0)
botaoMenu.TextColor3 = Color3.new(1, 1, 1)
botaoMenu.Text = "MENU"
botaoMenu.Font = Enum.Font.SourceSansBold
botaoMenu.TextScaled = true
botaoMenu.BorderColor3 = Color3.new(0, 0, 0)
botaoMenu.BorderSizePixel = 0

-- Botão "Puxar Armas"
local botaoPuxarArmas = Instance.new("TextButton")
botaoPuxarArmas.Name = "BotaoPuxarArmas"
botaoPuxarArmas.Parent = frameMenu
botaoPuxarArmas.Size = UDim2.new(0.4, 0, 0.15, 0)
botaoPuxarArmas.Position = UDim2.new(0.3, 0, 0.2, 0)
botaoPuxarArmas.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
botaoPuxarArmas.TextColor3 = Color3.new(1, 1, 1)
botaoPuxarArmas.Text = "PUXAR ARMAS"
botaoPuxarArmas.Font = Enum.Font.SourceSansBold
botaoPuxarArmas.TextScaled = true
botaoPuxarArmas.BorderColor3 = Color3.new(0, 0, 0)
botaoPuxarArmas.BorderSizePixel = 0

local listaArmasFrame = nil

-- Função para alternar a visibilidade do menu
local function toggleMenu()
    menuAtivo = not menuAtivo
    frameMenu.Visible = menuAtivo

    if menuAtivo then
        botaoMenu.Text = "FECHAR"
    else
        botaoMenu.Text = "MENU"
        -- Desativa a lista de armas se o menu for fechado
        armasAtivas = false
        if listaArmasFrame then
            listaArmasFrame:Destroy()
            listaArmasFrame = nil
        end
    end
end

-- Conectar o botão ao toggleMenu
botaoMenu.MouseButton1Click:Connect(toggleMenu)

-- Função para lidar com a tecla no PC
local function handleMenuInput(actionName, inputState, gameProcessedEvent)
    if actionName == "ToggleMenu" and inputState == Enum.UserInputState.Begin then
        toggleMenu()
    end
end

-- Registrar a ação da tecla
ContextActionService:BindAction("ToggleMenu", handleMenuInput, false, teclaMenu)
ContextActionService:SetTitle("ToggleMenu", "Menu Armas")
ContextActionService:SetImage("ToggleMenu", "rbxassetid://4947388903") -- Replace with an actual icon asset ID

botaoPuxarArmas.MouseButton1Click:Connect(function()
    armasAtivas = not armasAtivas

    if armasAtivas then
        botaoPuxarArmas.Text = "ESCONDER ARMAS"

        -- Criar a lista de armas
        listaArmasFrame = Instance.new("ScrollingFrame") -- Use ScrollingFrame para permitir scroll em mobile
        listaArmasFrame.Name = "ListaArmasFrame"
        listaArmasFrame.Parent = frameMenu -- Anexar ao frameMenu
        listaArmasFrame.Size = UDim2.new(0.9, 0, 0.6, 0) -- Ajustar o tamanho
        listaArmasFrame.Position = UDim2.new(0.05, 0, 0.35, 0) -- Posicionar abaixo do botão "Puxar Armas"
        listaArmasFrame.BackgroundColor3 = Color3.new(0, 0, 0)
        listaArmasFrame.BackgroundTransparency = 0.8
        listaArmasFrame.BorderColor3 = Color3.new(0, 0, 0)
        listaArmasFrame.BorderSizePixel = 0
        listaArmasFrame.ClipsDescendants = true
        listaArmasFrame.ScrollBarThickness = 12 -- Aumenta a espessura da barra de rolagem para facilitar o uso em mobile
        listaArmasFrame.CanvasSize = UDim2.new(0,0,0,0) -- Inicializa com tamanho zero, o layout ajustará automaticamente

        -- Layout para organizar os botões verticalmente
        local layout = Instance.new("UIListLayout")
        layout.Parent = listaArmasFrame
        layout.SortOrder = Enum.SortOrder.LayoutOrder
        layout.Padding = UDim.new(0, 5)

        -- Criar botões para cada arma
        local i = 1
        for nomeArma, item_id in pairs(armas) do
            local botaoArma = Instance.new("TextButton")
            botaoArma.Name = "Botao" .. nomeArma
            botaoArma.Parent = listaArmasFrame
            botaoArma.Size = UDim2.new(1, 0, 0.15, 0)
            botaoArma.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
            botaoArma.TextColor3 = Color3.new(1, 1, 1)
            botaoArma.Text = nomeArma
            botaoArma.Font = Enum.Font.SourceSansBold
            botaoArma.TextScaled = true
            botaoArma.BorderColor3 = Color3.new(0, 0, 0)
            botaoArma.BorderSizePixel = 0
            botaoArma.LayoutOrder = i

            botaoArma.MouseButton1Click:Connect(function()
                -- Função para dar a arma ao jogador (ADAPTAR AO SEU SISTEMA DE INVENTÁRIO)
                print("Dando arma: " .. nomeArma)
                darArma(item_id)
            end)

            i = i + 1
        end

        -- Ajusta o CanvasSize após adicionar todos os botões
        listaArmasFrame.CanvasSize = UDim2.new(0,0,0, (i - 1) * (0.15 + layout.Padding.Offset))

    else
        botaoPuxarArmas.Text = "PUXAR ARMAS"
        -- Destrói a lista de armas
        if listaArmasFrame then
            listaArmasFrame:Destroy()
            listaArmasFrame = nil
        end
    end
end)

-- Função para dar a arma ao jogador (ADAPTAR AO SEU SISTEMA DE INVENTÁRIO)
local function darArma(item_id)
    -- *** ADAPTE ESTA FUNÇÃO AO SISTEMA DE INVENTÁRIO DO SEU SERVIDOR ***

    -- Exemplo: Se o servidor tiver um evento remoto para dar itens:
    -- ReplicatedStorage:WaitForChild("DarItem"):FireServer(item_id)

    -- Ou, se você puder acessar o inventário localmente (menos comum em RP):
    -- player.Character:FindFirstChild("Inventory"):AddItem(item_id)

	-- Este é apenas um exemplo! Adapte de acordo com o seu servidor.
	print("Tentando dar o item com ID: ", item_id)

	-- Adicionando feedback visual (temporário, substitua pelo sistema do servidor)
	local feedback = Instance.new("BillboardGui")
	feedback.Size = UDim2.new(2, 0, 1, 0)
	feedback.AlwaysOnTop = true
	feedback.ExtentsOffset = Vector3.new(0,2,0) -- Ajusta a posição acima da cabeça

	local textLabel = Instance.new("TextLabel")
	textLabel.Size = UDim2.new(1, 0, 1, 0)
	textLabel.BackgroundTransparency = 1
	textLabel.TextColor3 = Color3.new(1,1,0) -- Amarelo
	textLabel.TextScaled = true
	textLabel.Font = Enum.Font.SourceSansBold
	textLabel.Text = "Arma recebida (simulação)!"
	textLabel.Parent = feedback

	feedback.Parent = player.Character:WaitForChild("Head")

	game.Debris:AddItem(feedback, 3) -- Destrói o feedback após 3 segundos
end

--[[
    Considerações Finais:

    - Este script é um ponto de partida.  Você precisará adaptá-lo e testá-lo
      cuidadosamente no contexto do seu servidor Mini City RP.

    - Segurança: NUNCA confie totalmente no cliente (este script).
      Sempre valide e sanitize as requisições no servidor para evitar exploits.

    - Sistema de Inventário: A parte mais importante é integrar corretamente
      com o sistema de inventário do seu servidor.  Se o servidor tiver um
      sistema complexo, você provavelmente precisará de um RemoteFunction ou
      RemoteEvent para se comunicar com o servidor e adicionar os itens
      de forma segura.

    - Otimização: Se você tiver muitas armas ou itens, considere usar
      caching e otimizar a forma como os botões são criados para evitar
      problemas de performance, especialmente em dispositivos mobile.
--]]
