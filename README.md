-- Script para permitir voo no Roblox ao segurar a barra de espaço

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local flying = false
local speed = 50

-- Função para iniciar o voo
local function startFlying()
    local character = player.Character
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    local bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Velocity = Vector3.new(0, speed, 0)
    bodyVelocity.Parent = character.PrimaryPart
    flying = true

    -- Manter o voo enquanto a barra de espaço estiver pressionada
    humanoid.Jump = true
    humanoid.Changed:Connect(function(property)
        if property == "Jump" and not humanoid.Jump then
            stopFlying()
        end
    end)
end

-- Função para parar o voo
local function stopFlying()
    local character = player.Character
    if character and character.PrimaryPart then
        for _, v in pairs(character.PrimaryPart:GetChildren()) do
            if v:IsA("BodyVelocity") then
                v:Destroy()
            end
        end
    end
    flying = false
end

-- Alternar voo com a barra de espaço
mouse.KeyDown:Connect(function(key)
    if key == " " then
        if not flying then
            startFlying()
        end
    end
end)

mouse.KeyUp:Connect(function(key)
    if key == " " then
        stopFlying()
    end
end)
