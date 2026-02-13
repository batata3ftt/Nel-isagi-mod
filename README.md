local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FlowGui"
screenGui.Parent = playerGui

-- BOTÃO INVISÍVEL
local button = Instance.new("TextButton")
button.Parent = screenGui
button.Position = UDim2.fromScale(0.091, 0.109)
button.Size = UDim2.fromOffset(47, 47)
button.BackgroundTransparency = 1
button.Text = ""
button.AutoButtonColor = false

-- IMAGEM GIGANTE
local effectImage = Instance.new("ImageLabel")
effectImage.Parent = screenGui
effectImage.AnchorPoint = Vector2.new(0.5, 0.5)
effectImage.Position = UDim2.fromScale(0.5, 0.5)
effectImage.Size = UDim2.fromScale(1.2, 1.2)
effectImage.BackgroundTransparency = 1
effectImage.Image = "rbxassetid://119651352706278"
effectImage.ImageTransparency = 1
effectImage.ZIndex = 10

-- SOM (volume um pouco menor)
local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://90146075044849"
sound.Volume = 3 -- antes estava 5
sound.Parent = screenGui

-- COOLDOWN
local COOLDOWN_TIME = 60
local onCooldown = false

-- Função ativar Flow
local function activateFlow()

	local character = player.Character or player.CharacterAdded:Wait()
	local humanoid = character:WaitForChild("Humanoid")

	if humanoid.FloorMaterial == Enum.Material.Air then
		warn("Você está no ar.")
		return
	end

	ReplicatedStorage:WaitForChild("Flow_RE"):FireServer()
	print("Flow ativado.")
end

-- Função efeito visual
local function playEffect()

	task.wait(2)

	local fadeIn = TweenService:Create(
		effectImage,
		TweenInfo.new(1),
		{ImageTransparency = 0}
	)
	fadeIn:Play()
	fadeIn.Completed:Wait()

	task.wait(2)

	local fadeOut = TweenService:Create(
		effectImage,
		TweenInfo.new(1),
		{ImageTransparency = 1}
	)
	fadeOut:Play()
end

-- Clique
button.MouseButton1Click:Connect(function()

	if onCooldown then return end
	onCooldown = true

	activateFlow()

	task.delay(2, function()
		sound:Play()
	end)

	playEffect()

	-- Libera após 60 segundos
	task.delay(COOLDOWN_TIME, function()
		onCooldown = false
	end)

end)local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- CONFIG
local COOLDOWN_TIME = 30
local SOUND_VOLUME = 8

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "IsagiSkillGui"
screenGui.Parent = playerGui

-- BOTÃO QUADRADO INVISÍVEL
local button = Instance.new("TextButton")
button.Parent = screenGui
button.Position = UDim2.fromScale(0.405, 0.766)
button.Size = UDim2.fromOffset(36, 36)
button.BackgroundTransparency = 1
button.Text = ""
button.AutoButtonColor = false

-- IMAGEM TAMANHO MÁXIMO
local effectImage = Instance.new("ImageLabel")
effectImage.Parent = screenGui
effectImage.AnchorPoint = Vector2.new(0.5, 0.5)
effectImage.Position = UDim2.fromScale(0.498, 0.400)
effectImage.Size = UDim2.fromScale(1.2, 1.2)
effectImage.BackgroundTransparency = 1
effectImage.Image = "rbxassetid://107857818158099"
effectImage.ImageTransparency = 1 -- começa invisível
effectImage.ZIndex = 10

-- SOM BEM ALTO
local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://109521522241610"
sound.Volume = SOUND_VOLUME
sound.Parent = screenGui

-- Cooldown
local onCooldown = false

-- Função ativar Skill
local function activateSkill()

	local args = {
		"ADAPTABILITY GENIUS",
		"Skill1",
		{
			pitch = -0.13090118962765515,
			lookVector = Vector3.new(
				-0.966915488243103,
				-0.13052767515182495,
				-0.21917332708835602
			)
		}
	}

	ReplicatedStorage:WaitForChild("SkillActivated"):FireServer(unpack(args))
end

-- Função efeito visual
local function playEffect()

	-- Fade In até 60% visível (0.4 transparency)
	local fadeIn = TweenService:Create(
		effectImage,
		TweenInfo.new(0.4),
		{ImageTransparency = 0.4}
	)
	fadeIn:Play()
	fadeIn.Completed:Wait()

	task.wait(0.4)

	-- Fade Out rápido
	local fadeOut = TweenService:Create(
		effectImage,
		TweenInfo.new(0.25),
		{ImageTransparency = 1}
	)
	fadeOut:Play()
end

-- Clique
button.MouseButton1Click:Connect(function()

	if onCooldown then return end
	onCooldown = true

	activateSkill()

	-- Som sai na hora
	sound:Play()

	-- Imagem sai na hora
	playEffect()

	task.delay(COOLDOWN_TIME, function()
		onCooldown = false
	end)

end)local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local playerGui = player:WaitForChild("PlayerGui")

-- CONFIGURAÇÃO DO BOTÃO
local BUTTON_POSITION = UDim2.fromScale(0.503, 0.766)
local BUTTON_SIZE = UDim2.fromOffset(36, 36)
local COOLDOWN_TIME = 30

-- CONFIGURAÇÃO DA IMAGEM
local IMAGE_ID = "rbxassetid://121063191543922"
local IMAGE_POSITION = UDim2.fromScale(0.014, -0.113)
local IMAGE_OPACITY = 0.6
local FADE_TIME = 0.3 -- entrada e saída

-- CONFIGURAÇÃO DO ÁUDIO
local SOUND_ID = "rbxassetid://103345832831059"
local SOUND_VOLUME = 10 -- bem mais alto

-- ANIMAÇÃO A MONITORAR
local ANIMATION_ID = "rbxassetid://79799457000931"

-- CRIA GUI
local gui = Instance.new("ScreenGui")
gui.Name = "Skill3GUI"
gui.ResetOnSpawn = false
gui.Parent = playerGui

-- IMAGEM
local image = Instance.new("ImageLabel")
image.Parent = gui
image.BackgroundTransparency = 1
image.Image = IMAGE_ID
image.Position = IMAGE_POSITION
image.Size = UDim2.fromScale(1,1) -- tamanho máximo
image.Visible = false
image.ImageTransparency = 1

-- ÁUDIO
local sound = Instance.new("Sound")
sound.Parent = gui
sound.SoundId = SOUND_ID
sound.Volume = SOUND_VOLUME
sound.PlayOnRemove = false

-- BOTÃO INVISÍVEL
local button = Instance.new("TextButton")
button.Parent = gui
button.Position = BUTTON_POSITION
button.Size = BUTTON_SIZE
button.BackgroundTransparency = 1
button.Text = ""
button.AutoButtonColor = false

-- COOLDOWN
local onCooldown = false

-- FUNÇÃO DE MONITORAMENTO DA ANIMAÇÃO
local function monitorAnimation(animId, callback)
    local connection
    connection = RunService.Heartbeat:Connect(function()
        for _, track in pairs(humanoid:GetPlayingAnimationTracks()) do
            if track.Animation.AnimationId == animId then
                connection:Disconnect()
                callback(track)
                break
            end
        end
    end)
end

-- FUNÇÃO PARA FADE DA IMAGEM
local function showImage()
    image.Visible = true
    TweenService:Create(image, TweenInfo.new(FADE_TIME), {ImageTransparency = 1 - IMAGE_OPACITY}):Play()
    -- sumir rápido
    task.delay(0.5, function()
        TweenService:Create(image, TweenInfo.new(FADE_TIME), {ImageTransparency = 1}):Play()
        task.delay(FADE_TIME, function()
            image.Visible = false
        end)
    end)
end

-- FUNÇÃO PARA TOCAR ÁUDIO
local function playSound()
    sound:Play()
end

-- CLIQUE NO BOTÃO
button.MouseButton1Click:Connect(function()
    if onCooldown then return end
    onCooldown = true

    -- Executa o remote event da Skill 3
    local args = {
        "ADAPTABILITY GENIUS",
        "Skill3",
        {
            pitch = -0.5899257557404678,
            lookVector = Vector3.new(-0.30410847067832947, -0.5562993288040161, 0.7733363509178162)
        }
    }
    ReplicatedStorage:WaitForChild("SkillActivated"):FireServer(unpack(args))

    -- Monitora a animação
    monitorAnimation(ANIMATION_ID, function()
        showImage()
        playSound()
    end)

    -- Cooldown
    task.delay(COOLDOWN_TIME, function()
        onCooldown = false
    end)
end)
