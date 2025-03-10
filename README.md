local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

local thankYouLabel = Instance.new("TextLabel")
thankYouLabel.Parent = screenGui
thankYouLabel.Size = UDim2.new(0, 300, 0, 50)
thankYouLabel.Position = UDim2.new(0.5, -150, 0.1, 0)
thankYouLabel.BackgroundTransparency = 0.8
thankYouLabel.BackgroundColor3 = Color3.new(0, 0, 0)
thankYouLabel.TextColor3 = Color3.new(1, 1, 1)
thankYouLabel.Text = "Obrigado por usar Gusta Hub"
thankYouLabel.TextScaled = true
thankYouLabel.Visible = true

game:GetService("TweenService"):Create(thankYouLabel, TweenInfo.new(3), {TextTransparency = 1}):Play()
game:GetService("TweenService"):Create(thankYouLabel, TweenInfo.new(3), {BackgroundTransparency = 1}):Play()
game:GetService("TweenService"):Create(thankYouLabel, TweenInfo.new(3), {Size = UDim2.new(0, 0, 0, 0)}):Play()

wait(3)
thankYouLabel:Destroy()

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 250, 0, 490)
frame.Position = UDim2.new(0.05, 0, 0.1, 0)
frame.Parent = screenGui

local hubLabel = Instance.new("TextLabel")
hubLabel.Parent = frame
hubLabel.Size = UDim2.new(1, 0, 0, 30)
hubLabel.Position = UDim2.new(0, 0, 0, 0)
hubLabel.BackgroundColor3 = Color3.new(0, 0, 0)
hubLabel.BackgroundTransparency = 0.8
hubLabel.TextColor3 = Color3.new(1, 1, 1)
hubLabel.Text = "Gusta Hub"
hubLabel.TextScaled = true

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 120, 0, 40)
toggleButton.Position = UDim2.new(0.05, 0, 0.05, 0)
toggleButton.Text = "Menu"
toggleButton.Parent = screenGui

local menuOpen = true

local function toggleMenu()
    menuOpen = not menuOpen
    frame.Visible = menuOpen
end

toggleButton.MouseButton1Click:Connect(toggleMenu)

local playerTextBox = Instance.new("TextBox")
playerTextBox.Size = UDim2.new(0, 230, 0, 40)
playerTextBox.Position = UDim2.new(0, 10, 0, 30)
playerTextBox.PlaceholderText = "Nome do Jogador"
playerTextBox.Parent = frame

local teleportButton = Instance.new("TextButton")
teleportButton.Size = UDim2.new(0, 230, 0, 40)
teleportButton.Position = UDim2.new(0, 10, 0, 70)
teleportButton.Text = "Teleportar"
teleportButton.Parent = frame

local function teleportToPlayer()
    local playerName = playerTextBox.Text
    if playerName == "" then
        print("Digite um nome válido.")
        return
    end
    
    local targetPlayer = game.Players:FindFirstChild(playerName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
    else
        print("Jogador não encontrado ou sem personagem.")
    end
end

teleportButton.MouseButton1Click:Connect(teleportToPlayer)

local cloneButton = Instance.new("TextButton")
cloneButton.Size = UDim2.new(0, 230, 0, 40)
cloneButton.Position = UDim2.new(0, 10, 0, 110)
cloneButton.Text = "Clonar Roupas"
cloneButton.Parent = frame

local function cloneClothes()
    local playerName = playerTextBox.Text
    if playerName == "" then
        print("Digite um nome válido.")
        return
    end
    
    local targetPlayer = game.Players:FindFirstChild(playerName)
    local localCharacter = game.Players.LocalPlayer.Character

    if targetPlayer and targetPlayer.Character and localCharacter then
        for _, accessory in pairs(targetPlayer.Character:GetChildren()) do
            if accessory:IsA("Accessory") or accessory:IsA("Shirt") or accessory:IsA("Pants") then
                local clone = accessory:Clone()
                clone.Parent = localCharacter
            end
        end
    else
        print("Jogador não encontrado ou sem personagem.")
    end
end

cloneButton.MouseButton1Click:Connect(cloneClothes)

local flyCarButton = Instance.new("TextButton")
flyCarButton.Size = UDim2.new(0, 230, 0, 40)
flyCarButton.Position = UDim2.new(0, 10, 0, 190)
flyCarButton.Text = "Fly Car"
flyCarButton.Parent = frame

local flyCarEnabled = false
local currentCar = nil

local function toggleFlyCar()
    flyCarEnabled = not flyCarEnabled
    print("Fly Car toggled: " .. tostring(flyCarEnabled))

    if flyCarEnabled then
        local character = game.Players.LocalPlayer.Character
        if character then
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid and humanoid.SeatPart then
                local seat = humanoid.SeatPart
                if seat.Parent and seat.Parent:IsA("VehicleSeat") then
                    currentCar = seat.Parent.Parent
                    if currentCar then
                        currentCar.PrimaryPart.Anchored = true
                        currentCar.PrimaryPart.Velocity = Vector3.new(0, 5, 0)
                    end
                else
                    flyCarEnabled = false
                    print("Não está dirigindo um carro.")
                end
            else
                flyCarEnabled = false
                print("Não está dirigindo um carro.")
            end
        else
            flyCarEnabled = false
            print("Não está dirigindo um carro.")
        end
    else
        if currentCar and currentCar.PrimaryPart then
            currentCar.PrimaryPart.Anchored =
