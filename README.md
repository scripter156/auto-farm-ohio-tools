-- Limpeza para não acumular GUIs na tela
if game.Players.LocalPlayer.PlayerGui:FindFirstChild("FarmMoneyGui") then
    game.Players.LocalPlayer.PlayerGui.FarmMoneyGui:Destroy()
end

-- Variável Global para controlar o Farm
_G.FarmActive = false

-- Criação da Interface Gráfica (GUI)
local sg = Instance.new("ScreenGui")
sg.Name = "FarmMoneyGui"
sg.Parent = game.Players.LocalPlayer.PlayerGui
sg.ResetOnSpawn = false

local mf = Instance.new("Frame")
mf.Parent = sg
mf.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
mf.Position = UDim2.new(0.05, 0, 0.4, 0)
mf.Size = UDim2.new(0, 180, 0, 100)
mf.Active = true
mf.Draggable = true -- Permite arrastar a GUI

local tt = Instance.new("TextLabel")
tt.Parent = mf
tt.BackgroundTransparency = 1
tt.Size = UDim2.new(1, 0, 0.4, 0)
tt.Font = Enum.Font.SourceSansBold
tt.Text = "Farm Money INSTANT"
tt.TextColor3 = Color3.fromRGB(255, 255, 255)
tt.TextSize = 18

local tb = Instance.new("TextButton")
tb.Parent = mf
tb.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
tb.Position = UDim2.new(0.1, 0, 0.45, 0)
tb.Size = UDim2.new(0.8, 0, 0.45, 0)
tb.Font = Enum.Font.SourceSans
tb.Text = "Status: OFF"
tb.TextColor3 = Color3.fromRGB(255, 255, 255)
tb.TextSize = 16

-- LOOP DE VELOCIDADE EXTREMA (Instantâneo)
-- Usamos Heartbeat para rodar a cada frame físico do jogo, sem delays artificiais.
game:GetService("RunService").Heartbeat:Connect(function()
    if _G.FarmActive == true then
        local plyr = game.Players.LocalPlayer
        if plyr.Character and plyr.Character:FindFirstChild("HumanoidRootPart") then
            -- Busca a part alvo 'TeleportOut' em todo o Workspace
            local part = game.Workspace:FindFirstChild("TeleportOut", true)
            
            if part then
                local prompt = part:FindFirstChildOfClass("ProximityPrompt")
                
                if prompt then
                    -- TELEPORTE: Velocidade máxima, direto para o CFrame da part.
                    plyr.Character.HumanoidRootPart.CFrame = part.CFrame
                    
                    -- INTERAÇÃO: Removemos o tempo de segurar do jogo (E) para ZERO absoluto.
                    prompt.HoldDuration = 0
                    
                    -- Executa a interação no mesmo frame do teleporte
                    if fireproximityprompt then
                        -- Se o executor for bom, usa o método nativo instantâneo
                        fireproximityprompt(prompt)
                    else
                        -- Alternativa nativa instantânea (começa e termina no mesmo frame)
                        prompt:InputHoldBegin()
                        prompt:InputHoldEnd()
                    end
                end
            end
        end
    end
end)

-- Ativador do Botão da GUI
tb.MouseButton1Click:Connect(function()
    _G.FarmActive = not _G.FarmActive
    if _G.FarmActive then
        tb.Text = "Status: ON (RAPIDO)"
        tb.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
    else
        tb.Text = "Status: OFF"
        tb.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    end
end)
