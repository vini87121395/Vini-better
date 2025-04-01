-- Carregar a biblioteca de UI
local UI = loadstring(game:HttpGet("https://raw.githubusercontent.com/XYZ/UILib/main.lua"))()

-- Criar a interface com o nome "Vini 1.0"
local Janela = UI:CreateWindow("Vini 1.0")

-- Definir o jogador e os serviços necessários
local p, rs, ws = game.Players.LocalPlayer, game:GetService("ReplicatedStorage"), game:GetService("Workspace")

-- Função Anti-Ban (Proteção contra o banimento)
function AntiBan()
    p.Idled:Connect(function() 
        game:GetService("VirtualUser"):Button2Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
        wait(1)
        game:GetService("VirtualUser"):Button2Up(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
    end)
end

-- Função para entrar em servidor vazio (Para evitar servidores lotados)
function EntrarVazio()
    local success, result = pcall(function() return game:GetService("GameJoinService"):GetGameInstances(game.PlaceId) end)
    if success and result then
        for _, server in pairs(result) do
            if server.NumPlayers == 0 then
                game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, server.Id)
            end
        end
    end
end

-- Usar Códigos Automáticos (Para ganhar benefícios no jogo)
function UsarCodigos()
    local codigos = {"SUB2GAMERROBOT_EXP1", "THANKYOUFOR100K", "GAMERROBOT_YOUTUBE"}
    for _, code in ipairs(codigos) do
        pcall(function() rs.Remotes.Code:FireServer(code) end)
    end
end

-- Função Auto Sea Beast (Atacar Sea Beast automaticamente)
function AutoSeaBeast()
    while wait(3) do
        for _, seaBeast in pairs(ws:GetChildren()) do
            if seaBeast.Name == "Sea Beast" and seaBeast:FindFirstChild("HumanoidRootPart") then
                rs.Remotes.FruitAttack:FireServer(seaBeast)  -- Atacar o Sea Beast
            end
        end
    end
end

-- Função Auto Factory (Completar missões da fábrica)
function AutoFactory()
    while wait(5) do
        for _, factory in pairs(ws:GetChildren()) do
            if factory.Name:match("Factory") then
                p.Character.HumanoidRootPart.CFrame = factory.TeleportLocation.CFrame
                rs.Remotes.StartFactoryMission:FireServer()
                rs.Remotes.CompleteFactoryMission:FireServer()
                break
            end
        end
    end
end

-- Função para notificar quando uma fruta spawna
function NotificarFruta()
    while wait(2) do
        for _, item in pairs(ws:GetChildren()) do
            if item.Name:match("Fruit") and item:FindFirstChild("HumanoidRootPart") then
                game:GetService("StarterGui"):SetCore("SendNotification", {
                    Title = "Fruta Spawnada!", 
                    Text = item.Name, 
                    Duration = 5
                }) 
                break
            end
        end
    end
end

-- Adicionando funções na UI
Janela:CreateTab("Funções Básicas")
Janela:GetTab("Funções Básicas"):CreateButton("Ativar Anti-Ban", AntiBan)
Janela:GetTab("Funções Básicas"):CreateButton("Entrar em Servidor Vazio", EntrarVazio)
Janela:GetTab("Funções Básicas"):CreateButton("Usar Códigos", UsarCodigos)

Janela:CreateTab("Funções Avançadas")
Janela:GetTab("Funções Avançadas"):CreateButton("Auto Sea Beast", AutoSeaBeast)
Janela:GetTab("Funções Avançadas"):CreateButton("Auto Factory", AutoFactory)
Janela:GetTab("Funções Avançadas"):CreateButton("Notificar Fruta", NotificarFruta)

-- Notificação de carregamento
game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "Vini 1.0",
    Text = "Script Carregado com Sucesso!",
    Duration = 5
})

print("Script Vini 1.0 carregado com sucesso!")
