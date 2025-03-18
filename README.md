--[[
    Script para equipar arma específica no América PvP e Mini City RP (Roblox)
    Adaptado para PC
    INSTRUÇÕES:
    1.  Edite a variável "armaDesejada" com o nome EXATO da arma que você quer.
        (Copie e cole o nome da arma do inventário para evitar erros.)
    2.  Execute o script.
    
    IMPORTANTE:
    * Este script assume que a arma está no seu inventário.
    * A detecção da mochila/inventário pode variar entre os jogos.  O script tenta
      detectar os padrões mais comuns.  Se não funcionar, você precisará inspecionar
      a estrutura da sua mochila/inventário e ajustar as linhas de código que a acessam.
    * Alguns jogos têm proteção contra scripts de auto-equip.  Este script pode não funcionar
      se houver proteções em vigor.
    * Use este script por sua conta e risco.
]]

-- //////////////////////////////////////////////////////////////
-- CONFIGURAÇÃO
-- //////////////////////////////////////////////////////////////

local armaDesejada = "Nome da Arma Aqui"  -- <---- COLOQUE O NOME DA ARMA AQUI!

-- //////////////////////////////////////////////////////////////
-- FUNÇÕES AUXILIARES
-- //////////////////////////////////////////////////////////////

local function encontrarArmaNoInventario(playerName)
    local player = game.Players:FindFirstChild(playerName)
    if not player then
        print("Jogador '" .. playerName .. "' não encontrado.")
        return nil
    end
    
    local backpack = player:FindFirstChild("Backpack") or player:FindFirstChild("StarterGear")
    
    if not backpack then
       -- Tentando encontrar a mochila dentro do PlayerGui (comum em alguns jogos)
       local playerGui = player:FindFirstChild("PlayerGui")
       if playerGui then
           backpack = playerGui:FindFirstChild("Backpack") 
       end
       
       if not backpack then
           print("Mochila/Inventário não encontrado para '" .. playerName .. "'.")
           return nil
       end
    end

    for _, item in pairs(backpack:GetChildren()) do
        if item:IsA("Tool") and item.Name == armaDesejada then
            return item
        end
    end

    print("Arma '" .. armaDesejada .. "' não encontrada no inventário de '" .. playerName .. "'.")
    return nil
end


local function equiparArma(arma)
    if arma then
        arma.Parent = game.Players.LocalPlayer.Character
        print("Arma '" .. armaDesejada .. "' equipada.")
    else
        print("Falha ao equipar arma. Arma não encontrada ou inválida.")
    end
end

-- //////////////////////////////////////////////////////////////
-- SCRIPT PRINCIPAL
-- //////////////////////////////////////////////////////////////

local jogador = game.Players.LocalPlayer

-- Espera o personagem carregar
jogador.CharacterAdded:Wait()

-- Espera um pouco para garantir que o inventário esteja carregado
wait(2)

local armaEncontrada = encontrarArmaNoInventario(jogador.Name)

if armaEncontrada then
    equiparArma(armaEncontrada)
else
    print("Não foi possível encontrar a arma '" .. armaDesejada .. "' no seu inventário.")
end
