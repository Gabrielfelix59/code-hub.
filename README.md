local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TitleLabel = Instance.new("TextLabel")
local ToggleFarmButton = Instance.new("TextButton")
local StatusLabel = Instance.new("TextLabel")
local LevelLabel = Instance.new("TextLabel")
local RefreshButton = Instance.new("TextButton")
local IslandSelectionFrame = Instance.new("Frame")
local IslandDropdown = Instance.new("TextButton")
local TeleportButton = Instance.new("TextButton")
local EnemyLabel = Instance.new("TextLabel")
local EnemyCountLabel = Instance.new("TextLabel")
local UpgradeButton = Instance.new("TextButton")

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
MainFrame.Position = UDim2.new(0.1, 0, 0.1, 0)
MainFrame.Size = UDim2.new(0, 300, 0, 400)

TitleLabel.Parent = MainFrame
TitleLabel.BackgroundTransparency = 1
TitleLabel.Size = UDim2.new(0, 300, 0, 30)
TitleLabel.Position = UDim2.new(0, 0, 0, 10)
TitleLabel.Text = "Painel de Controle"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 20
TitleLabel.Font = Enum.Font.SourceSansBold

ToggleFarmButton.Parent = MainFrame
ToggleFarmButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
ToggleFarmButton.Size = UDim2.new(0, 280, 0, 30)
ToggleFarmButton.Position = UDim2.new(0, 10, 0, 50)
ToggleFarmButton.Text = "Iniciar Farm"
ToggleFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleFarmButton.TextSize = 16
ToggleFarmButton.Font = Enum.Font.SourceSansBold

StatusLabel.Parent = MainFrame
StatusLabel.BackgroundTransparency = 1
StatusLabel.Size = UDim2.new(0, 280, 0, 20)
StatusLabel.Position = UDim2.new(0, 10, 0, 90)
StatusLabel.Text = "Status: Inativo"
StatusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
StatusLabel.TextSize = 14

LevelLabel.Parent = MainFrame
LevelLabel.BackgroundTransparency = 1
LevelLabel.Size = UDim2.new(0, 280, 0, 20)
LevelLabel.Position = UDim2.new(0, 10, 0, 110)
LevelLabel.Text = "Nível Atual: "
LevelLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
LevelLabel.TextSize = 14

RefreshButton.Parent = MainFrame
RefreshButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
RefreshButton.Size = UDim2.new(0, 280, 0, 30)
RefreshButton.Position = UDim2.new(0, 10, 0, 140)
RefreshButton.Text = "Atualizar Nível"
RefreshButton.TextColor3 = Color3.fromRGB(255, 255, 255)
RefreshButton.TextSize = 16
RefreshButton.Font = Enum.Font.SourceSans

local islands = {
    {name = "Ilha Inicial", position = Vector3.new(100, 50, 200)},
    {name = "Ilha das Macacos", position = Vector3.new(300, 50, 400)},
    {name = "Ilha do Gorila", position = Vector3.new(500, 50, 600)},
}

IslandSelectionFrame.Parent = ScreenGui
IslandSelectionFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
IslandSelectionFrame.Position = UDim2.new(0.1, 0, 0.5, 0)
IslandSelectionFrame.Size = UDim2.new(0, 300, 0, 150)

IslandDropdown.Parent = IslandSelectionFrame
IslandDropdown.Size = UDim2.new(0, 280, 0, 30)
IslandDropdown.Position = UDim2.new(0, 10, 0, 10)
IslandDropdown.Text = "Selecione uma Ilha"
IslandDropdown.TextColor3 = Color3.fromRGB(255, 255, 255)
IslandDropdown.BackgroundColor3 = Color3.fromRGB(70, 130, 180)

TeleportButton.Parent = IslandSelectionFrame
TeleportButton.Size = UDim2.new(0, 280, 0, 30)
TeleportButton.Position = UDim2.new(0, 10, 0, 50)
TeleportButton.Text = "Teletransportar"
TeleportButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180)

EnemyLabel.Parent = MainFrame
EnemyLabel.BackgroundTransparency = 1
EnemyLabel.Size = UDim2.new(0, 280, 0, 20)
EnemyLabel.Position = UDim2.new(0, 10, 0, 180)
EnemyLabel.Text = "Inimigos Derrotados: "
EnemyLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
EnemyLabel.TextSize = 14

EnemyCountLabel.Parent = MainFrame
EnemyCountLabel.BackgroundTransparency = 1
EnemyCountLabel.Size = UDim2.new(0, 280, 0, 20)
EnemyCountLabel.Position = UDim2.new(0, 10, 0, 200)
EnemyCountLabel.Text = "0"
EnemyCountLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
EnemyCountLabel.TextSize = 14

UpgradeButton.Parent = MainFrame
UpgradeButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
UpgradeButton.Size = UDim2.new(0, 280, 0, 30)
UpgradeButton.Position = UDim2.new(0, 10, 0, 230)
UpgradeButton.Text = "Atualizar Habilidades"
UpgradeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
UpgradeButton.TextSize = 16
UpgradeButton.Font = Enum.Font.SourceSans

local SelectedIsland = nil
local farming = false
local enemyCount = 0

local function moveTo(position)
    local player = game.Players.LocalPlayer
    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = CFrame.new(position)
    end
end

local function attackEnemy(enemy)
    if enemy and enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
        local player = game.Players.LocalPlayer
        if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0, 0, -5)
            wait(0.5)
            local tool = player.Character:FindFirstChildOfClass("Tool")
            if tool then
                tool:Activate()
                enemyCount = enemyCount + 1
                EnemyCountLabel.Text = tostring(enemyCount)
            end
        end
    end
end

local function farmToMaxLevel()
    while farming do
        local player = game.Players.LocalPlayer
        if player and player.Data and player.Data:FindFirstChild("Level") then
            local playerLevel = player.Data.Level.Value

            for _, enemy in pairs(game.Workspace.Enemies:GetChildren()) do
                if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 and 
                   (enemy.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude <= 50 then
                    attackEnemy(enemy)
                    wait(1)
                end
            end

            LevelLabel.Text = "Nível Atual: " .. playerLevel
            wait(2)
        else
            break
        end
    end
end

ToggleFarmButton.MouseButton1Click:Connect(function()
    farming = not farming
    if farming then
        StatusLabel.Text = "Status: Ativo"
        ToggleFarmButton.Text = "Parar Farm"
        enemyCount = 0
        EnemyCountLabel.Text = "0"
        farmToMaxLevel()
    else
        StatusLabel.Text = "Status: Inativo"
        ToggleFarmButton.Text = "Iniciar Farm"
    end
end)

RefreshButton.MouseButton1Click:Connect(function()
    local playerLevel = game.Players.LocalPlayer.Data.Level.Value
    LevelLabel.Text = "Nível Atual: " .. playerLevel
end)

IslandDropdown.MouseButton1Click:Connect(function()
    SelectedIsland = nil
    local islandNames = ""
    for _, island in ipairs(islands) do
        islandNames = islandNames .. island.name .. "\n"
    end
    IslandDropdown.Text = "Selecione uma Ilha\n" .. islandNames
end)

TeleportButton.MouseButton1Click:Connect(function()
    if SelectedIsland then
        for _, island in ipairs(islands) do
            if island.name == SelectedIsland then
                moveTo(island.position)
                break
            end
       
