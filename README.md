-- Simples ESP com Team Check e Interface
local players = game:GetService("Players")
local localPlayer = players.LocalPlayer

-- Configurações da interface
local gui = Instance.new("ScreenGui", localPlayer:WaitForChild("PlayerGui"))
gui.Name = "ESP_Interface"

-- Função para criar ESP
local function createESP(player)
    if player == localPlayer then return end
    if player.Team == localPlayer.Team then return end  -- TEAM CHECK

    local box = Instance.new("TextLabel")
    box.Name = "ESP_Box"
    box.Text = player.Name
    box.Size = UDim2.new(0, 100, 0, 20)
    box.BackgroundColor3 = Color3.new(1, 0, 0)
    box.BackgroundTransparency = 0.5
    box.TextColor3 = Color3.new(1, 1, 1)
    box.TextScaled = true
    box.Parent = gui

    local function update()
        while player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") do
            local pos = workspace.CurrentCamera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
            box.Position = UDim2.new(0, pos.X - 50, 0, pos.Y - 10)
            box.Visible = pos.Z > 0
            wait()
        end
        box:Destroy()
    end

    coroutine.wrap(update)()
end

-- Monitorar novos jogadores
for _, p in pairs(players:GetPlayers()) do
    createESP(p)
end

players.PlayerAdded:Connect(function(p)
    createESP(p)
end)
