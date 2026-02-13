-- BY. BRIAN'S LAB --
Qualquer divulgação do script sem permissão resultará em DMCA + Ban.

-- COLOQUE ESSE SCRIPT EM SERVER SCRIPT SERVICE --

local Players = game:GetService("Players")

-- CONFIG
local TEMPO_PAGAMENTO = 300 -- 300 segundos (5 minutos)
local VALOR_SALARIO = 500

-- equipes que NÃO recebem
local EQUIPE_BLOQUEADA = "Civil"

local function criarLeaderstats(player)
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local dinheiro = Instance.new("IntValue")
    dinheiro.Name = "Dinheiro"
    dinheiro.Value = 0
    dinheiro.Parent = leaderstats
end

local function podeReceber(player)
    if not player.Team then return false end
    return player.Team.Name ~= EQUIPE_BLOQUEADA
end

local function enviarNotificacao(player)
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://6026984224" -- som de notificação Roblox
    sound.Volume = 1
    sound.Parent = player:WaitForChild("PlayerGui")
    sound:Play()

    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "Salário",
        Text = "Você recebeu 500 Reais pelo seus serviços.",
        Duration = 5
    })
end

local function pagarSalario(player)
    if not podeReceber(player) then return end

    local dinheiro = player:FindFirstChild("leaderstats") and player.leaderstats:FindFirstChild("Dinheiro")
    if dinheiro then
        dinheiro.Value += VALOR_SALARIO
        enviarNotificacao(player)
    end
end

Players.PlayerAdded:Connect(function(player)
    criarLeaderstats(player)

    while player.Parent do
        task.wait(TEMPO_PAGAMENTO)
        pagarSalario(player)
    end
end)
