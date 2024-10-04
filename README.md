-- Inicialização
local widget = require("widget")
local physics = require("physics")
physics.start()

-- Função para criar o botão
local function createButton()
    local button = widget.newButton({
        left = display.contentCenterX - 50,
        top = display.contentCenterY - 25,
        width = 100,
        height = 50,
        label = "Aimbot",
        onRelease = function()
            activateAimbot()
        end
    })
end

-- Função para ativar o aimbot
local function activateAimbot()
    local function findClosestPlayer()
        local closestPlayer = nil
        local minDistance = math.huge
        for i, player in ipairs(players) do
            local distance = math.sqrt((player.x - myPlayer.x)^2 + (player.y - myPlayer.y)^2)
            if distance < minDistance then
                minDistance = distance
                closestPlayer = player
            end
        end
        return closestPlayer
    end

    local function aimAtPlayer(player)
        if player then
            -- Código para mirar na cabeça do jogador
            -- Exemplo: player.head é a posição da cabeça do jogador
            myPlayer:aimAt(player.head)
        end
    end

    local function updateAura(player)
        if player then
            -- Código para adicionar uma aura em volta do jogador
            -- Exemplo: player.aura é o objeto da aura
            player.aura.isVisible = true
            player.distanceText.text = string.format("Distância: %.2f", distance)
            player.nameText.text = player.name
        end
    end

    local closestPlayer = findClosestPlayer()
    aimAtPlayer(closestPlayer)
    updateAura(closestPlayer)

    -- Atualizar alvo quando o jogador morrer
    Runtime:addEventListener("enterFrame", function()
        if closestPlayer and closestPlayer.isDead then
            closestPlayer = findClosestPlayer()
            aimAtPlayer(closestPlayer)
            updateAura(closestPlayer)
        end
    end)
end

-- Criar o botão na tela
createButton()
