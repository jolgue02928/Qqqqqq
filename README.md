-- Aguarda o jogo carregar totalmente
repeat task.wait() until game:IsLoaded()

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer

-- Função para saber se está no lobby
local function isInLobby()
    local gui = player:FindFirstChild("PlayerGui")
    if not gui then return false end

    -- Verifica se existe o menu de criar partida
    return gui:FindFirstChild("PartyMenu") ~= nil
end

-- Função que cria uma partida no lobby
local function createParty()
    local remote = ReplicatedStorage:WaitForChild("Shared")
        :WaitForChild("Network")
        :WaitForChild("RemoteEvent")
        :WaitForChild("CreateParty")

    local args = {
        [1] = {
            ["trainId"] = "default",
            ["maxMembers"] = 1,
            ["gameMode"] = "Normal"
        }
    }

    remote:FireServer(unpack(args))
end

-- Função que executa o ciclo de autofarm dentro da partida
local function startFarm()
    -- Aguarda personagem
    repeat task.wait() until player.Character and player.Character:FindFirstChild("HumanoidRootPart")

    -- Executa o Script 1 (que inicia todo o ciclo)
    loadstring(game:HttpGet("https://raw.githubusercontent.com/jolgue02928/Kdkdnd-D/refs/heads/main/README.md"))()
end

-- Decide com base no estado atual
if isInLobby() then
    print("[AUTOEXEC] No lobby, criando nova partida...")
    createParty()
else
    print("[AUTOEXEC] Dentro de uma partida, iniciando autofarm...")
    startFarm()
end
