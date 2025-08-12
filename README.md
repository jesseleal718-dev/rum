# rum
-- GUI Setup
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local ToggleButton = Instance.new("TextButton", ScreenGui)
ToggleButton.Size = UDim2.new(0, 120, 0, 40)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.Text = "Coleta: OFF"
ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 20

-- Variáveis
local ativo = false
local lp = game.Players.LocalPlayer
local rs = game:GetService("ReplicatedStorage")
local frutas = {"Fruit", "Banana", "Apple", "Gomu", "Mera"}

-- Função de coleta
local function coletar(fruta)
    if rs:FindFirstChild("CollectFruit") then
        rs.CollectFruit:FireServer(fruta)
    else
        fruta:Destroy()
    end
end

-- Loop de coleta
task.spawn(function()
    while true do
        task.wait(1.5)
        if not ativo then continue end
        local hrp = lp.Character and lp.Character:FindFirstChild("HumanoidRootPart")
        if not hrp then continue end
        for _, v in ipairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") and table.find(frutas, v.Name) then
                if (v.Position - hrp.Position).Magnitude < 100 then
                    coletar(v)
                end
            end
        end
    end
end)

-- Botão de ativação
ToggleButton.MouseButton1Click:Connect(function()
    ativo = not ativo
    ToggleButton.Text = ativo and "Coleta: ON" or "Coleta: OFF"
    ToggleButton.BackgroundColor3 = ativo and Color3.fromRGB(80, 255, 80) or Color3.fromRGB(255, 80, 80)
end)
