-- Carregar a biblioteca da interface (Kavo UI)
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()

-- Criar a janela principal
local Window = Library.CreateLib("PEGAX_LEVEL", "DarkTheme")

-- Criar uma aba chamada "Script"
local Tab = Window:NewTab("Script")

-- Criar uma seção dentro da aba
local Section = Tab:NewSection("Script")

-- Adicionar um botão de Auto Farm
Section:NewButton("AUTO FARM", "Clique para ativar o Auto Farm", function()
    repeat wait() until game:IsLoaded()
    local player = game.Players.LocalPlayer
    local char = player.Character or player.CharacterAdded:Wait()
    local hrp = char:FindFirstChild("HumanoidRootPart")

    -- Configurações do Auto Farm
    local farmRange = 50 -- Distância máxima dos inimigos
    local attackDelay = 0.3 -- Tempo entre ataques

    -- Função para encontrar inimigos próximos
    function getEnemies()
        local enemies = {}
        for _, v in pairs(game.Workspace.Enemies:GetChildren()) do
            if v:IsA("Model") and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") then
                if v.Humanoid.Health > 0 then
                    table.insert(enemies, v)
                end
            end
        end
        return enemies
    end

    -- Função para atacar inimigos
    function attackEnemy(enemy)
        if enemy and enemy:FindFirstChild("HumanoidRootPart") then
            hrp.CFrame = enemy.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0) -- Move o jogador para o inimigo
            wait(attackDelay)
            game:GetService("VirtualUser"):CaptureController()
            game:GetService("VirtualUser"):Button1Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
        end
    end

    -- Loop do Auto Farm
    while true do
        wait(0.1)
        local enemies = getEnemies()
        for _, enemy in pairs(enemies) do
            if (enemy.HumanoidRootPart.Position - hrp.Position).magnitude <= farmRange then
                attackEnemy(enemy)
            end
        end
    end
end)
