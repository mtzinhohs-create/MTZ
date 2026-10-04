--========================================================--
--              FLUXO PVP - TEST MENU
--              Roblox Studio / LocalScript
--========================================================--

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local VirtualUser = game:GetService("VirtualUser")

local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

--========================================================--
-- CONFIGURAÇÕES
--========================================================--

local CONFIG = {
	-- Coloque aqui o ID da imagem que você enviar para o Roblox
	ICON_IMAGE = "rbxassetid://0000000000",

	MenuColor = Color3.fromRGB(170, 100, 255),
	ESPColor = Color3.fromRGB(255, 70, 70),

	WalkSpeed = 16,
	FOV = 90,
	AimbotFOV = 150,

	InfiniteJump = false,
	Aimbot = false,
	Noclip = false,
	ESP = false,
	ESPLine = false,
	ESPSkeleton = false,
	Fly = false,
	FullBright = false,
	ClickTeleport = false,
	AntiAFK = false,
}

--========================================================--
-- GUI
--========================================================--

local Gui = Instance.new("ScreenGui")
Gui.Name = "FluxoTestMenu"
Gui.ResetOnSpawn = false
Gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
Gui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Botão flutuante
local FloatingButton = Instance.new("ImageButton")
FloatingButton.Name = "FloatingButton"
FloatingButton.Size = UDim2.fromOffset(65, 65)
FloatingButton.Position = UDim2.new(0, 20, 0.5, -30)
FloatingButton.BackgroundColor3 = CONFIG.MenuColor
FloatingButton.BorderSizePixel = 0
FloatingButton.Image = CONFIG.ICON_IMAGE
FloatingButton.Parent = Gui

local ButtonCorner = Instance.new("UICorner")
ButtonCorner.CornerRadius = UDim.new(1, 0)
ButtonCorner.Parent = FloatingButton

local ButtonStroke = Instance.new("UIStroke")
ButtonStroke.Thickness = 2
ButtonStroke.Color = Color3.new(1,1,1)
ButtonStroke.Parent = FloatingButton

--========================================================--
-- MENU PRINCIPAL
--========================================================--

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.fromOffset(430, 500)
Main.Position = UDim2.new(0.5, -215, 0.5, -250)
Main.BackgroundColor3 = Color3.fromRGB(20,20,25)
Main.BorderSizePixel = 0
Main.Visible = false
Main.Parent = Gui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 14)
MainCorner.Parent = Main

local MainStroke = Instance.new("UIStroke")
MainStroke.Thickness = 2
MainStroke.Color = CONFIG.MenuColor
MainStroke.Parent = Main

--========================================================--
-- TÍTULO
--========================================================--

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -60, 0, 55)
Title.Position = UDim2.fromOffset(20, 5)
Title.BackgroundTransparency = 1
Title.Text = "FLUXO PVP"
Title.TextColor3 = Color3.new(1,1,1)
Title.TextSize = 25
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Main

local Close = Instance.new("TextButton")
Close.Size = UDim2.fromOffset(40,40)
Close.Position = UDim2.new(1,-50,0,10)
Close.BackgroundColor3 = Color3.fromRGB(45,45,50)
Close.Text = "X"
Close.TextColor3 = Color3.new(1,1,1)
Close.TextSize = 18
Close.Font = Enum.Font.GothamBold
Close.Parent = Main

Instance.new("UICorner", Close).CornerRadius = UDim.new(0,10)

--========================================================--
-- SCROLL
--========================================================--

local Scroll = Instance.new("ScrollingFrame")
Scroll.Size = UDim2.new(1,-20,1,-75)
Scroll.Position = UDim2.fromOffset(10,65)
Scroll.BackgroundTransparency = 1
Scroll.BorderSizePixel = 0
Scroll.ScrollBarThickness = 4
Scroll.CanvasSize = UDim2.new()
Scroll.Parent = Main

local Layout = Instance.new("UIListLayout")
Layout.Padding = UDim.new(0,7)
Layout.Parent = Scroll

Layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	Scroll.CanvasSize = UDim2.fromOffset(
		0,
		Layout.AbsoluteContentSize.Y + 15
	)
end)

--========================================================--
-- FUNÇÕES DE UI
--========================================================--

local function CreateToggle(text, callback)
	local Button = Instance.new("TextButton")

	Button.Size = UDim2.new(1,-10,0,42)
	Button.BackgroundColor3 = Color3.fromRGB(35,35,42)
	Button.BorderSizePixel = 0
	Button.Text = text .. " : OFF"
	Button.TextColor3 = Color3.new(1,1,1)
	Button.TextSize = 15
	Button.Font = Enum.Font.GothamMedium
	Button.Parent = Scroll

	Instance.new("UICorner", Button).CornerRadius = UDim.new(0,8)

	local enabled = false

	Button.MouseButton1Click:Connect(function()
		enabled = not enabled

		Button.Text = text .. (enabled and " : ON" or " : OFF")
		Button.BackgroundColor3 =
			enabled and CONFIG.MenuColor or Color3.fromRGB(35,35,42)

		callback(enabled)
	end)

	return Button
end

local function CreateSlider(text, min, max, default, callback)
	local Holder = Instance.new("Frame")
	Holder.Size = UDim2.new(1,-10,0,65)
	Holder.BackgroundColor3 = Color3.fromRGB(35,35,42)
	Holder.BorderSizePixel = 0
	Holder.Parent = Scroll

	Instance.new("UICorner", Holder).CornerRadius = UDim.new(0,8)

	local Label = Instance.new("TextLabel")
	Label.Size = UDim2.new(1,-20,0,25)
	Label.Position = UDim2.fromOffset(10,3)
	Label.BackgroundTransparency = 1
	Label.Text = text .. ": " .. tostring(default)
	Label.TextColor3 = Color3.new(1,1,1)
	Label.TextSize = 14
	Label.Font = Enum.Font.GothamMedium
	Label.TextXAlignment = Enum.TextXAlignment.Left
	Label.Parent = Holder

	local Slider = Instance.new("TextButton")
	Slider.Size = UDim2.new(1,-20,0,10)
	Slider.Position = UDim2.fromOffset(10,40)
	Slider.BackgroundColor3 = Color3.fromRGB(60,60,65)
	Slider.Text = ""
	Slider.Parent = Holder

	Instance.new("UICorner", Slider).CornerRadius = UDim.new(1,0)

	local Fill = Instance.new("Frame")
	Fill.BackgroundColor3 = CONFIG.MenuColor
	Fill.BorderSizePixel = 0
	Fill.Size = UDim2.new((default-min)/(max-min),0,1,0)
	Fill.Parent = Slider

	Instance.new("UICorner", Fill).CornerRadius = UDim.new(1,0)

	local dragging = false

	local function Update(x)
		local percent = math.clamp(
			(x - Slider.AbsolutePosition.X) /
			Slider.AbsoluteSize.X,
			0,
			1
		)

		local value = math.floor(min + ((max-min)*percent))

		Fill.Size = UDim2.new(percent,0,1,0)
		Label.Text = text .. ": " .. tostring(value)

		callback(value)
	end

	Slider.MouseButton1Down:Connect(function(x)
		dragging = true
		Update(x)
	end)

	UserInputService.InputChanged:Connect(function(input)
		if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
			Update(input.Position.X)
		end
	end)

	UserInputService.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			dragging = false
		end
	end)

	return Holder
end

--========================================================--
-- CHARACTER
--========================================================--

local function GetCharacter()
	return LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
end

local function GetHumanoid()
	return GetCharacter():FindFirstChildOfClass("Humanoid")
end

--========================================================--
-- INFINITE JUMP
--========================================================--

UserInputService.JumpRequest:Connect(function()
	if CONFIG.InfiniteJump then
		local Humanoid = GetHumanoid()

		if Humanoid then
			Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
		end
	end
end)

--========================================================--
-- SPEED
--========================================================--

RunService.RenderStepped:Connect(function()
	local Humanoid = GetHumanoid()

	if Humanoid then
		Humanoid.WalkSpeed = CONFIG.WalkSpeed
	end
end)

--========================================================--
-- NOCLIP
--========================================================--

RunService.Stepped:Connect(function()
	if CONFIG.Noclip then
		local Character = GetCharacter()

		for _, obj in ipairs(Character:GetDescendants()) do
			if obj:IsA("BasePart") then
				obj.CanCollide = false
			end
		end
	end
end)

--========================================================--
-- AIMBOT
--========================================================--

local function GetClosestPlayer()
	local closest = nil
	local shortest = CONFIG.AimbotFOV

	for _, player in ipairs(Players:GetPlayers()) do

		if player ~= LocalPlayer and player.Character then

			local Humanoid = player.Character:FindFirstChildOfClass("Humanoid")
			local Head = player.Character:FindFirstChild("Head")

			if Humanoid and Head and Humanoid.Health > 0 then

				local screenPos, visible =
					Camera:WorldToViewportPoint(Head.Position)

				if visible then

					local center = Vector2.new(
						Camera.ViewportSize.X/2,
						Camera.ViewportSize.Y/2
					)

					local distance = (
						Vector2.new(screenPos.X,screenPos.Y)
						- center
					).Magnitude

					if distance < shortest then
						shortest = distance
						closest = Head
					end
				end
			end
		end
	end

	return closest
end

RunService.RenderStepped:Connect(function()

	if CONFIG.Aimbot then

		local target = GetClosestPlayer()

		if target then

			Camera.CFrame = CFrame.lookAt(
				Camera.CFrame.Position,
				target.Position
			)

		end
	end
end)

--========================================================--
-- ESP
--========================================================--

local ESPObjects = {}

local function RemoveESP(player)

	if ESPObjects[player] then

		for _, obj in pairs(ESPObjects[player]) do
			if obj and obj.Parent then
				obj:Destroy()
			end
		end

		ESPObjects[player] = nil
	end
end

local function CreateESP(player)

	if player == LocalPlayer then
		return
	end

	local character = player.Character

	if not character then
		return
	end

	RemoveESP(player)

	local objects = {}

	-- Highlight / Box
	local highlight = Instance.new("Highlight")
	highlight.Name = "FluxoESP"
	highlight.FillTransparency = 0.8
	highlight.OutlineTransparency = 0
	highlight.FillColor = CONFIG.ESPColor
	highlight.OutlineColor = CONFIG.ESPColor
	highlight.Adornee = character
	highlight.Parent = character

	table.insert(objects, highlight)

	-- Nome
	local head = character:FindFirstChild("Head")

	if head then

		local billboard = Instance.new("BillboardGui")
		billboard.Size = UDim2.fromOffset(150,40)
		billboard.StudsOffset = Vector3.new(0,3,0)
		billboard.AlwaysOnTop = true
		billboard.Adornee = head
		billboard.Parent = head

		local label = Instance.new("TextLabel")
		label.Size = UDim2.fromScale(1,1)
		label.BackgroundTransparency = 1
		label.Text = player.Name
		label.TextColor3 = CONFIG.ESPColor
		label.TextStrokeTransparency = 0
		label.TextSize = 14
		label.Font = Enum.Font.GothamBold
		label.Parent = billboard

		table.insert(objects,billboard)
	end

	ESPObjects[player] = objects
end

local function UpdateESP()

	for _, player in ipairs(Players:GetPlayers()) do

		if player ~= LocalPlayer then

			if CONFIG.ESP then
				CreateESP(player)
			else
				RemoveESP(player)
			end

		end
	end
end

Players.PlayerAdded:Connect(function(player)

	player.CharacterAdded:Connect(function()

		task.wait(1)

		if CONFIG.ESP then
			CreateESP(player)
		end

	end)

end)

--========================================================--
-- FLY
--========================================================--

local FlyVelocity

RunService.RenderStepped:Connect(function()

	if not CONFIG.Fly then

		if FlyVelocity then
			FlyVelocity:Destroy()
			FlyVelocity = nil
		end

		return
	end

	local Character = GetCharacter()
	local Root = Character:FindFirstChild("HumanoidRootPart")

	if not Root then return end

	if not FlyVelocity then

		FlyVelocity = Instance.new("BodyVelocity")
		FlyVelocity.MaxForce = Vector3.new(1e6,1e6,1e6)
		FlyVelocity.Velocity = Vector3.zero
		FlyVelocity.Parent = Root

	end

	FlyVelocity.Velocity =
		Camera.CFrame.LookVector * 60

end)

--========================================================--
-- FULLBRIGHT
--========================================================--

local OriginalLighting = {
	Brightness = Lighting.Brightness,
	ClockTime = Lighting.ClockTime,
	FogEnd = Lighting.FogEnd,
	GlobalShadows = Lighting.GlobalShadows
}

RunService.RenderStepped:Connect(function()

	if CONFIG.FullBright then

		Lighting.Brightness = 3
		Lighting.ClockTime = 14
		Lighting.FogEnd = 100000
		Lighting.GlobalShadows = false

	else

		Lighting.Brightness = OriginalLighting.Brightness
		Lighting.ClockTime = OriginalLighting.ClockTime
		Lighting.FogEnd = OriginalLighting.FogEnd
		Lighting.GlobalShadows = OriginalLighting.GlobalShadows

	end

end)

--========================================================--
-- CLICK TELEPORT
--========================================================--

local Mouse = LocalPlayer:GetMouse()

Mouse.Button1Down:Connect(function()

	if CONFIG.ClickTeleport then

		local Character = GetCharacter()
		local Root = Character:FindFirstChild("HumanoidRootPart")

		if Root and Mouse.Hit then
			Root.CFrame = Mouse.Hit + Vector3.new(0,3,0)
		end

	end

end)

--========================================================--
-- ANTI AFK
--========================================================--

LocalPlayer.Idled:Connect(function()

	if CONFIG.AntiAFK then

		VirtualUser:CaptureController()
		VirtualUser:ClickButton2(
			Vector2.new()
		)

	end

end)

--========================================================--
-- BOTÕES DO MENU
--========================================================--

CreateToggle("Infinite Jump", function(value)
	CONFIG.InfiniteJump = value
end)

CreateToggle("Aimbot", function(value)
	CONFIG.Aimbot = value
end)

CreateToggle("Noclip", function(value)
	CONFIG.Noclip = value
end)

CreateToggle("ESP Box", function(value)
	CONFIG.ESP = value
	UpdateESP()
end)

CreateToggle("ESP Line", function(value)
	CONFIG.ESPLine = value
end)

CreateToggle("ESP Skeleton", function(value)
	CONFIG.ESPSkeleton = value
end)

CreateToggle("Fly", function(value)
	CONFIG.Fly = value
end)

CreateToggle("FullBright", function(value)
	CONFIG.FullBright = value
end)

CreateToggle("Click Teleport", function(value)
	CONFIG.ClickTeleport = value
end)

CreateToggle("Anti AFK", function(value)
	CONFIG.AntiAFK = value
end)

--========================================================--
-- SLIDERS
--========================================================--

CreateSlider(
	"WalkSpeed",
	16,
	150,
	16,
	function(value)
		CONFIG.WalkSpeed = value
	end
)

CreateSlider(
	"Aimbot FOV",
	30,
	500,
	150,
	function(value)
		CONFIG.AimbotFOV = value
	end
)

CreateSlider(
	"Camera FOV",
	50,
	120,
	90,
	function(value)

		CONFIG.FOV = value
		Camera.FieldOfView = value

	end
)

--========================================================--
-- RESET
--========================================================--

local Reset = Instance.new("TextButton")
Reset.Size = UDim2.new(1,-10,0,42)
Reset.BackgroundColor3 = Color3.fromRGB(150,50,50)
Reset.Text = "RESET CHARACTER"
Reset.TextColor3 = Color3.new(1,1,1)
Reset.TextSize = 14
Reset.Font = Enum.Font.GothamBold
Reset.Parent = Scroll

Instance.new("UICorner",Reset).CornerRadius = UDim.new(0,8)

Reset.MouseButton1Click:Connect(function()

	local Humanoid = GetHumanoid()

	if Humanoid then
		Humanoid.Health = 0
	end

end)

--========================================================--
-- CORES DO MENU
--========================================================--

local ColorButton = Instance.new("TextButton")
ColorButton.Size = UDim2.new(1,-10,0,42)
ColorButton.BackgroundColor3 = CONFIG.MenuColor
ColorButton.Text = "Mudar cor do menu"
ColorButton.TextColor3 = Color3.new(1,1,1)
ColorButton.TextSize = 14
ColorButton.Font = Enum.Font.GothamBold
ColorButton.Parent = Scroll

Instance.new("UICorner",ColorButton).CornerRadius = UDim.new(0,8)

local Colors = {
	Color3.fromRGB(170,100,255),
	Color3.fromRGB(0,170,255),
	Color3.fromRGB(0,255,150),
	Color3.fromRGB(255,80,100),
	Color3.fromRGB(255,170,0),
	Color3.fromRGB(255,255,255)
}

local ColorIndex = 1

ColorButton.MouseButton1Click:Connect(function()

	ColorIndex += 1

	if ColorIndex > #Colors then
		ColorIndex = 1
	end

	CONFIG.MenuColor = Colors[ColorIndex]

	MainStroke.Color = CONFIG.MenuColor
	FloatingButton.BackgroundColor3 = CONFIG.MenuColor
	ColorButton.BackgroundColor3 = CONFIG.MenuColor

end)

--========================================================--
-- COR DO ESP
--========================================================--

local ESPColorButton = Instance.new("TextButton")
ESPColorButton.Size = UDim2.new(1,-10,0,42)
ESPColorButton.BackgroundColor3 = CONFIG.ESPColor
ESPColorButton.Text = "Mudar cor do ESP"
ESPColorButton.TextColor3 = Color3.new(1,1,1)
ESPColorButton.TextSize = 14
ESPColorButton.Font = Enum.Font.GothamBold
ESPColorButton.Parent = Scroll

Instance.new("UICorner",ESPColorButton).CornerRadius = UDim.new(0,8)

ESPColorButton.MouseButton1Click:Connect(function()

	CONFIG.ESPColor = Color3.fromHSV(
		math.random(),
		0.9,
		1
	)

	ESPColorButton.BackgroundColor3 = CONFIG.ESPColor

	UpdateESP()

end)

--========================================================--
-- ABRIR / FECHAR
--========================================================--

FloatingButton.MouseButton1Click:Connect(function()
	Main.Visible = not Main.Visible
end)

Close.MouseButton1Click:Connect(function()
	Main.Visible = false
end)

--========================================================--
-- ARRASTAR MENU
--========================================================--

local dragging = false
local dragStart
local startPos

Title.InputBegan:Connect(function(input)

	if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then

		dragging = true
		dragStart = input.Position
		startPos = Main.Position

	end

end)

UserInputService.InputChanged:Connect(function(input)

	if dragging and
		(input.UserInputType == Enum.UserInputType.MouseMovement
		or input.UserInputType == Enum.UserInputType.Touch) then

		local delta = input.Position - dragStart

		Main.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)

	end

end)

UserInputService.InputEnded:Connect(function(input)

	if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then

		dragging = false

	end

end)

print("FLUXO PVP TEST MENU carregado!")
