--// Enhanced FPS Boost Script for Roblox Rivals

local players = game:GetService("Players")
local localPlayer = players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- FPS Boost Logic (remove textures, reduce laggy aspects)
local function toggleFPSBoost(enabled)
    if enabled then
        -- Disable shadows, simplify lighting
        game.Lighting.GlobalShadows = false
        game.Lighting.OutdoorAmbient = Color3.fromRGB(255, 255, 255)  -- Bright light
        game.Lighting.FogEnd = 1000  -- Increase the fog distance
        game.Lighting.Ambient = Color3.fromRGB(255, 255, 255)  -- Simplify lighting
        game.Lighting.Brightness = 2  -- Reduce brightness
        
        -- Remove unnecessary particles and effects
        for _, effect in pairs(workspace:GetChildren()) do
            if effect:IsA("ParticleEmitter") or effect:IsA("Trail") or effect:IsA("Smoke") then
                effect.Enabled = false
            end
        end

        -- Remove reflections and complex materials
        for _, part in pairs(workspace:GetDescendants()) do
            if part:IsA("BasePart") then
                part.Reflectance = 0  -- Remove reflections
                part.Material = Enum.Material.SmoothPlastic  -- Set materials to SmoothPlastic
            end
        end
    else
        -- Restore normal settings
        game.Lighting.GlobalShadows = true
        game.Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
        game.Lighting.FogEnd = 100000
        game.Lighting.Ambient = Color3.fromRGB(128, 128, 128)
        game.Lighting.Brightness = 1
        
        -- Re-enable particle effects
        for _, effect in pairs(workspace:GetChildren()) do
            if effect:IsA("ParticleEmitter") or effect:IsA("Trail") or effect:IsA("Smoke") then
                effect.Enabled = true
            end
        end

        -- Restore original materials
        for _, part in pairs(workspace:GetDescendants()) do
            if part:IsA("BasePart") then
               
                part.Material = Enum.Material.Plastic  -- Restore to default material
            end
        end
    end
end

-- FPS unlock logic (set FPS value)
local function setFPSLimit(value)
    if value >= 1 and value <= 300 then
        -- Set FPS limit (Roblox's max FPS is 60, so overriding is limited, but we can tweak it)
        setfpscap(value)
    else
        warn("Invalid FPS value! Must be between 1 and 300.")
    end
end

-- UI to toggle FPS boost and input FPS value
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DogeFPSBoostIndicator"
screenGui.ResetOnSpawn = false
screenGui.Parent = localPlayer:WaitForChild("PlayerGui")

-- Main FPS Boost Text Label
local textLabel = Instance.new("TextLabel")
textLabel.Parent = screenGui
textLabel.BackgroundColor3 = Color3.new(0, 0, 0)  -- Black background
textLabel.BackgroundTransparency = 0.5  -- Semi-transparent
textLabel.Size = UDim2.new(0, 200, 0, 50)  -- Width: 200px, Height: 50px
textLabel.Position = UDim2.new(0, 10, 1, -110)  -- 10px from left, 110px from bottom
textLabel.TextColor3 = Color3.new(1, 1, 1)  -- White text
textLabel.TextSize = 20
textLabel.Font = Enum.Font.SourceSansBold
textLabel.Text = "Using Doge FPS Booster 🐶"
textLabel.TextWrapped = true

-- Subtext Label (Credits)
local creditLabel = Instance.new("TextLabel")
creditLabel.Parent = screenGui
creditLabel.BackgroundTransparency = 1  -- Fully transparent background
creditLabel.Size = UDim2.new(0, 200, 0, 30)  -- Width: 200px, Height: 30px
creditLabel.Position = UDim2.new(0, 10, 1, -80)  -- Positioned just below the main text
creditLabel.TextColor3 = Color3.fromRGB(200, 200, 200)  -- Light grey text for contrast
creditLabel.TextSize = 16
creditLabel.Font = Enum.Font.SourceSansItalic
creditLabel.Text = "Created by dogegod himself!"
creditLabel.TextWrapped = true

-- Toggle Text for FPS Boost
local toggleTextLabel = Instance.new("TextLabel")
toggleTextLabel.Parent = screenGui
toggleTextLabel.BackgroundTransparency = 1
toggleTextLabel.Size = UDim2.new(0, 200, 0, 30)
toggleTextLabel.Position = UDim2.new(0, 10, 1, -50)
toggleTextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleTextLabel.TextSize = 14
toggleTextLabel.Font = Enum.Font.SourceSans
toggleTextLabel.Text = "Press 2 to Toggle FPS Boost"

-- FPS Value Input Box
local inputBox = Instance.new("TextBox")
inputBox.Parent = screenGui
inputBox.BackgroundColor3 = Color3.fromRGB(0, 0, 0)  -- Black background
inputBox.BackgroundTransparency = 0.5  -- Semi-transparent
inputBox.Size = UDim2.new(0, 200, 0, 30)  -- Width: 200px, Height: 30px
inputBox.Position = UDim2.new(0, 10, 1, -20)  -- Positioned just below the toggle text
inputBox.TextColor3 = Color3.fromRGB(255, 255, 255)
inputBox.TextSize = 16
inputBox.Font = Enum.Font.SourceSans
inputBox.PlaceholderText = "Set FPS (1-300)"
inputBox.Text = ""  -- Default text
inputBox.TextWrapped = true

-- When the user submits FPS value
inputBox.FocusLost:Connect(function()
    local fpsValue = tonumber(inputBox.Text)
    if fpsValue then
        setFPSLimit(fpsValue)  -- Set the FPS value
    else
        warn("Invalid FPS input! Enter a number between 1 and 300.")
    end
end)

-- Toggle FPS boost on key press (Press 2 to toggle)
local isFPSBoostEnabled = false
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed then
        if input.KeyCode == Enum.KeyCode.Two then
            isFPSBoostEnabled = not isFPSBoostEnabled  -- Toggle the FPS Boost
            toggleFPSBoost(isFPSBoostEnabled)  -- Apply the FPS boost
        end
    end
end)

-- Initial FPS boost setup (ensure FPS boost is disabled by default)
toggleFPSBoost(isFPSBoostEnabled)
