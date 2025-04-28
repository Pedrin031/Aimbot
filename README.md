local player = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local camera = workspace.CurrentCamera

local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "PedrinHub"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 250, 0, 180)
Frame.Position = UDim2.new(0.05, 0, 0.1, 0)
Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.Text = "Pedrin Hub"
Title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Title.BorderSizePixel = 0
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20

local CopyButton = Instance.new("TextButton", Frame)
CopyButton.Size = UDim2.new(0.8, 0, 0, 30)
CopyButton.Position = UDim2.new(0.1, 0, 0, 40)
CopyButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
CopyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CopyButton.Font = Enum.Font.Gotham
CopyButton.TextSize = 16
CopyButton.Text = "Copiar Discord"
CopyButton.MouseButton1Click:Connect(function()
    setclipboard("https://discord.gg/jfKVrrMx")
end)

local SliderLabel = Instance.new("TextLabel", Frame)
SliderLabel.Size = UDim2.new(1, -20, 0, 30)
SliderLabel.Position = UDim2.new(0, 10, 0, 80)
SliderLabel.BackgroundTransparency = 1
SliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
SliderLabel.Font = Enum.Font.Gotham
SliderLabel.TextSize = 16
SliderLabel.Text = "Tamanho da Bola: 150"

local AimbotActive = false
local AimbotRadius = 150
local Circle = Drawing.new("Circle")
Circle.Radius = AimbotRadius
Circle.Thickness = 2
Circle.Color = Color3.fromRGB(255, 0, 0)
Circle.Transparency = 0.7
Circle.Filled = false
Circle.Visible = true

local SliderButton = Instance.new("TextButton", Frame)
SliderButton.Size = UDim2.new(0.8, 0, 0, 30)
SliderButton.Position = UDim2.new(0.1, 0, 0, 120)
SliderButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
SliderButton.TextColor3 = Color3.fromRGB(255, 255, 255)
SliderButton.Font = Enum.Font.Gotham
SliderButton.TextSize = 16
SliderButton.Text = "Aumentar Tamanho"
SliderButton.MouseButton1Click:Connect(function()
    AimbotRadius = AimbotRadius + 25
    if AimbotRadius > 500 then
        AimbotRadius = 50
    end
    Circle.Radius = AimbotRadius
    SliderLabel.Text = "Tamanho da Bola: "..AimbotRadius
end)

function GetClosestPlayer()
    local closestPlayer = nil
    local shortestDistance = math.huge
    for _,v in pairs(game.Players:GetPlayers()) do
        if v ~= player and v.Character and v.Character:FindFirstChild("Head") and v.Character:FindFirstChildOfClass("Humanoid").Health > 0 then
            local pos, onScreen = camera:WorldToViewportPoint(v.Character.Head.Position)
            if onScreen then
                local mousePos = uis:GetMouseLocation()
                local distance = (Vector2.new(pos.X, pos.Y) - Vector2.new(mousePos.X, mousePos.Y)).Magnitude
                if distance < shortestDistance and distance <= AimbotRadius then
                    closestPlayer = v
                    shortestDistance = distance
                end
            end
        end
    end
    return closestPlayer
end

RunService.RenderStepped:Connect(function()
    local mousePos = uis:GetMouseLocation()
    Circle.Position = Vector2.new(mousePos.X, mousePos.Y)
    if AimbotActive then
        local target = GetClosestPlayer()
        if target then
            local headPos = camera:WorldToViewportPoint(target.Character.Head.Position)
            mousemoverel((headPos.X - mousePos.X) / 2, (headPos.Y - mousePos.Y) / 2)
        end
    end
end)

uis.InputBegan:Connect(function(key, gpe)
    if not gpe then
        if key.KeyCode == Enum.KeyCode.X then
            AimbotActive = not AimbotActive
        end
    end
end)
