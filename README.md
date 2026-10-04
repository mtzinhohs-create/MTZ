--============================================================--
--                       FRIEREN MENU
--                    ROBLOX STUDIO / LOCAL
--============================================================--

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local VirtualUser = game:GetService("VirtualUser")

local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

--============================================================--
-- CONFIGURAÇÕES
--============================================================--

local Config = {
	-- ID da imagem do botão flutuante
	ICON_IMAGE = "rbxassetid://0000000000",

	-- Cores
	ThemeColor = Color3.fromRGB(170, 120, 255),
	ESPColor = Color3.fromRGB(255, 90, 120),

	-- Escala inicial menor
	MenuScale = 0.82,

	-- Movimento
	WalkSpeed = 16,
	InfiniteJump = false,
	Noclip = false,
	Fly = false,

	-- Combat
	Aimbot = false,
	AimbotFOV = 150,
	AimbotSmooth = 0.18,
	ShowFOV = true,

	-- Visual
	ESPBox = false,
	ESPLine = false,
	ESPSkeleton = false,

	-- Outros
	FullBright = false,
	AntiAFK = false,
	ClickTeleport = false,
}

--============================================================--
-- GUI PRINCIPAL
--============================================================--

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FrierenGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

--============================================================--
-- ESCALA DO MENU
--============================================================--

local MenuScale = Instance.new("UIScale")
MenuScale.Scale = Config.MenuScale

-- será colocado no Main posteriormente

--============================================================--
-- BOTÃO FLUTUANTE
--============================================================--

local FloatingButton = Instance.new("ImageButton")
FloatingButton.Name = "FrierenButton"
FloatingButton.Size = UDim2.fromOffset(58, 58)
FloatingButton.Position = UDim2.new(0, 18, 0.5, -29)
FloatingButton.BackgroundColor3 = Config.ThemeColor
FloatingButton.BorderSizePixel = 0
FloatingButton.Image = Config.ICON_IMAGE
FloatingButton.AutoButtonColor = false
FloatingButton.ZIndex = 50
FloatingButton.Parent = ScreenGui

local FloatingCorner = Instance.new("UICorner")
FloatingCorner.CornerRadius = UDim.new(1, 0)
FloatingCorner.Parent = FloatingButton

local FloatingStroke = Instance.new("UIStroke")
FloatingStroke.Thickness = 2
FloatingStroke.Color = Color3.new(1, 1, 1)
FloatingStroke.Transparency = 0.15
FloatingStroke.Parent = FloatingButton

--============================================================--
-- FOV CIRCLE
--============================================================--

local FOVCircle = Instance.new("Frame")
FOVCircle.Name = "AimbotFOV"
FOVCircle.AnchorPoint = Vector2.new(0.5, 0.5)
FOVCircle.Position = UDim2.fromScale(0.5, 0.5)
FOVCircle.Size = UDim2.fromOffset(
	Config.AimbotFOV * 2,
	Config.AimbotFOV * 2
)
FOVCircle.BackgroundTransparency = 1
FOVCircle.BorderSizePixel = 0
FOVCircle.Visible = false
FOVCircle.ZIndex = 5
FOVCircle.Parent = ScreenGui

local FOVCorner = Instance.new("UICorner")
FOVCorner.CornerRadius = UDim.new(1, 0)
FOVCorner.Parent = FOVCircle

local FOVStroke = Instance.new("UIStroke")
FOVStroke.Thickness = 2
FOVStroke.Transparency = 0.15
FOVStroke.Color = Config.ThemeColor
FOVStroke.Parent = FOVCircle

--============================================================--
-- PONTO CENTRAL
--============================================================--

local CenterDot = Instance.new("Frame")
CenterDot.Size = UDim2.fromOffset(4, 4)
CenterDot.AnchorPoint = Vector2.new(0.5, 0.5)
CenterDot.Position = UDim2.fromScale(0.5, 0.5)
CenterDot.BackgroundColor3 = Config.ThemeColor
CenterDot.BorderSizePixel = 0
CenterDot.Visible = false
CenterDot.ZIndex = 6
CenterDot.Parent = ScreenGui

local CenterCorner = Instance.new("UICorner")
CenterCorner.CornerRadius = UDim.new(1, 0)
CenterCorner.Parent = CenterDot

--============================================================--
-- MENU
--============================================================--

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.fromOffset(390, 455)
Main.Position = UDim2.new(0.5, -195, 0.5, -227)
Main.BackgroundColor3 = Color3.fromRGB(17, 17, 23)
Main.BorderSizePixel = 0
Main.Visible = false
Main.ZIndex = 20
Main.Parent = ScreenGui

MenuScale.Parent = Main

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 15)
MainCorner.Parent = Main

local MainStroke = Instance.new("UIStroke")
MainStroke.Thickness = 1.5
MainStroke.Color = Config.ThemeColor
MainStroke.Transparency = 0.15
MainStroke.Parent = Main

--============================================================--
-- HEADER
--============================================================--

local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, 0, 0, 62)
Header.BackgroundColor3 = Color3.fromRGB(24, 24, 32)
Header.BorderSizePixel = 0
Header.ZIndex = 21
Header.Parent = Main

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 15)
HeaderCorner.Parent = Header

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -70, 0, 30)
Title.Position = UDim2.fromOffset(18, 8)
Title.BackgroundTransparency = 1
Title.Text = "FRIEREN"
Title.TextColor3 = Color3.fromRGB(245, 240, 255)
Title.TextSize = 20
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.ZIndex = 22
Title.Parent = Header

local Subtitle = Instance.new("TextLabel")
Subtitle.Size = UDim2.new(1, -70, 0, 20)
Subtitle.Position = UDim2.fromOffset(19, 34)
Subtitle.BackgroundTransparency = 1
Subtitle.Text = "PvP • TEST MENU"
Subtitle.TextColor3 = Config.ThemeColor
Subtitle.TextSize = 10
Subtitle.Font = Enum.Font.GothamMedium
Subtitle.TextXAlignment = Enum.TextXAlignment.Left
Subtitle.ZIndex = 22
Subtitle.Parent = Header

local Close = Instance.new("TextButton")
Close.Size = UDim2.fromOffset(34, 34)
Close.Position = UDim2.new(1, -44, 0, 14)
Close.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
Close.Text = "×"
Close.TextColor3 = Color3.fromRGB(230, 230, 235)
Close.TextSize = 22
Close.Font = Enum.Font.GothamBold
Close.AutoButtonColor = false
Close.ZIndex = 22
Close.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 9)
CloseCorner.Parent = Close

--============================================================--
-- ABAS
--============================================================--

local TabBar = Instance.new("Frame")
TabBar.Size = UDim2.new(1, -20, 0, 40)
TabBar.Position = UDim2.fromOffset(10, 70)
TabBar.BackgroundColor3 = Color3.fromRGB(24, 24, 31)
TabBar.BorderSizePixel = 0
TabBar.ZIndex = 21
TabBar.Parent = Main

local TabBarCorner = Instance.new("UICorner")
TabBarCorner.CornerRadius = UDim.new(0, 10)
TabBarCorner.Parent = TabBar

local TabLayout = Instance.new("UIListLayout")
TabLayout.FillDirection = Enum.FillDirection.Horizontal
TabLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabLayout.Padding = UDim.new(0, 3)
TabLayout.Parent = TabBar

--============================================================--
-- ÁREA DAS ABAS
--============================================================--

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, -20, 1, -122)
Content.Position = UDim2.fromOffset(10, 117)
Content.BackgroundTransparency = 1
Content.ZIndex = 21
Content.Parent = Main

--============================================================--
-- FUNÇÕES DE UI
--============================================================--

local Tabs = {}

local function CreateTab(Name)
	local Frame = Instance.new("ScrollingFrame")
	Frame.Name = Name
	Frame.Size = UDim2.fromScale(1, 1)
	Frame.BackgroundTransparency = 1
	Frame.BorderSizePixel = 0
	Frame.ScrollBarThickness = 3
	Frame.Visible = false
	Frame.ZIndex = 21
	Frame.Parent = Content

	local Layout = Instance.new("UIListLayout")
	Layout.Padding = UDim.new(0, 7)
	Layout.Parent = Frame

	Layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
		Frame.CanvasSize = UDim2.fromOffset(
			0,
			Layout.AbsoluteContentSize.Y + 10
		)
	end)

	Tabs[Name] = Frame

	return Frame
end

local function CreateTabButton(Name)
	local Button = Instance.new("TextButton")
	Button.Name = Name .. "Button"
	Button.Size = UDim2.new(0.25, -3, 1, 0)
	Button.BackgroundTransparency = 1
	Button.Text = Name
	Button.TextColor3 = Color3.fromRGB(155, 155, 165)
	Button.TextSize = 11
	Button.Font = Enum.Font.GothamMedium
	Button.AutoButtonColor = false
	Button.ZIndex = 22
	Button.Parent = TabBar

	Button.MouseButton1Click:Connect(function()

		for TabName, Frame in pairs(Tabs) do
			Frame.Visible = TabName == Name
		end

		for _, Obj in ipairs(TabBar:GetChildren()) do
			if Obj:IsA("TextButton") then
				Obj.BackgroundColor3 = Color3.fromRGB(24, 24, 31)
				Obj.TextColor3 = Color3.fromRGB(155, 155, 165)
			end
		end

		Button.BackgroundColor3 = Config.ThemeColor
		Button.TextColor3 = Color3.new(1, 1, 1)

	end)

	local Corner = Instance.new("UICorner")
	Corner.CornerRadius = UDim.new(0, 8)
	Corner.Parent = Button

	return Button
end

local Combat = CreateTab("Combat")
local Visual = CreateTab("Visual")
local Movement = CreateTab("Movement")
local Settings = CreateTab("Settings")

CreateTabButton("Combat")
CreateTabButton("Visual")
CreateTabButton("Movement")
CreateTabButton("Settings")

Tabs.Combat.Visible = true

for _, Obj in ipairs(TabBar:GetChildren()) do
	if Obj:IsA("TextButton") and Obj.Name == "CombatButton" then
		Obj.BackgroundColor3 = Config.ThemeColor
		Obj.TextColor3 = Color3.new(1, 1, 1)
	end
end

--============================================================--
-- CONTROLES
--============================================================--

local function Toggle(Parent, Text, Default, Callback)

	local Button = Instance.new("TextButton")
	Button.Size = UDim2.new(1, -4, 0, 39)
	Button.BackgroundColor3 = Color3.fromRGB(29, 29, 38)
	Button.BorderSizePixel = 0
	Button.Text = ""
	Button.AutoButtonColor = false
	Button.ZIndex = 22
	Button.Parent = Parent

	local Corner = Instance.new("UICorner")
	Corner.CornerRadius = UDim.new(0, 9)
	Corner.Parent = Button

	local Label = Instance.new("TextLabel")
	Label.Size = UDim2.new(1, -65, 1, 0)
	Label.Position = UDim2.fromOffset(13, 0)
	Label.BackgroundTransparency = 1
	Label.Text = Text
	Label.TextColor3 = Color3.fromRGB(225, 225, 232)
	Label.TextSize = 12
	Label.Font = Enum.Font.GothamMedium
	Label.TextXAlignment = Enum.TextXAlignment.Left
	Label.ZIndex = 23
	Label.Parent = Button

	local Indicator = Instance.new("Frame")
	Indicator.Size = UDim2.fromOffset(34, 18)
	Indicator.Position = UDim2.new(1, -46, 0.5, -9)
	Indicator.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
	Indicator.BorderSizePixel = 0
	Indicator.ZIndex = 23
	Indicator.Parent = Button

	local IndicatorCorner = Instance.new("UICorner")
	IndicatorCorner.CornerRadius = UDim.new(1, 0)
	IndicatorCorner.Parent = Indicator

	local Dot = Instance.new("Frame")
	Dot.Size = UDim2.fromOffset(12, 12)
	Dot.Position = UDim2.fromOffset(3, 3)
	Dot.BackgroundColor3 = Color3.fromRGB(190, 190, 200)
	Dot.BorderSizePixel = 0
	Dot.ZIndex = 24
	Dot.Parent = Indicator

	local DotCorner = Instance.new("UICorner")
	DotCorner.CornerRadius = UDim.new(1, 0)
	DotCorner.Parent = Dot

	local State = Default

	local function Update()
		if State then
			Indicator.BackgroundColor3 = Config.ThemeColor
			Dot.Position = UDim2.new(1, -15, 0, 3)
			Dot.BackgroundColor3 = Color3.new(1, 1, 1)
		else
			Indicator.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
			Dot.Position = UDim2.fromOffset(3, 3)
			Dot.BackgroundColor3 = Color3.fromRGB(190, 190, 200)
		end
	end

	Button.MouseButton1Click:Connect(function()
		State = not State
		Update()
		Callback(State)
	end)

	Update()

	return Button
end

local function Slider(Parent, Text, Minimum, Maximum, Default, Callback)

	local Holder = Instance.new("Frame")
	Holder.Size = UDim2.new(1, -4, 0, 58)
	Holder.BackgroundColor3 = Color3.fromRGB(29, 29, 38)
	Holder.BorderSizePixel = 0
	Holder.ZIndex = 22
	Holder.Parent = Parent

	local Corner = Instance.new("UICorner")
	Corner.CornerRadius = UDim.new(0, 9)
	Corner.Parent = Holder

	local Label = Instance.new("TextLabel")
	Label.Size = UDim2.new(1, -25, 0, 23)
	Label.Position = UDim2.fromOffset(12, 5)
	Label.BackgroundTransparency = 1
	Label.TextColor3 = Color3.fromRGB(225, 225, 232)
	Label.TextSize = 11
	Label.Font = Enum.Font.GothamMedium
	Label.TextXAlignment = Enum.TextXAlignment.Left
	Label.ZIndex = 23
	Label.Parent = Holder

	local Bar = Instance.new("TextButton")
	Bar.Size = UDim2.new(1, -25, 0, 7)
	Bar.Position = UDim2.fromOffset(12, 39)
	Bar.BackgroundColor3 = Color3.fromRGB(55, 55, 65)
	Bar.BorderSizePixel = 0
	Bar.Text = ""
	Bar.ZIndex = 23
	Bar.Parent = Holder

	local BarCorner = Instance.new("UICorner")
	BarCorner.CornerRadius = UDim.new(1, 0)
	BarCorner.Parent = Bar

	local Fill = Instance.new("Frame")
	Fill.BorderSizePixel = 0
	Fill.BackgroundColor3 = Config.ThemeColor
	Fill.ZIndex = 24
	Fill.Parent = Bar

	local FillCorner = Instance.new("UICorner")
	FillCorner.CornerRadius = UDim.new(1, 0)
	FillCorner.Parent = Fill

	local Dragging = false

	local function SetValue(X)

		local Percent = math.clamp(
			(X - Bar.AbsolutePosition.X) /
			Bar.AbsoluteSize.X,
			0,
			1
		)

		local Value = Minimum +
			((Maximum - Minimum) * Percent)

		Value = math.floor(Value)

		local Normalized =
			(Value - Minimum) /
			(Maximum - Minimum)

		Fill.Size = UDim2.new(
			Normalized,
			0,
			1,
			0
		)

		Label.Text = Text .. "  " .. tostring(Value)

		Callback(Value)

	end

	Bar.InputBegan:Connect(function(Input)

		if Input.UserInputType == Enum.UserInputType.MouseButton1
			or Input.UserInputType == Enum.UserInputType.Touch then

			Dragging = true
			SetValue(Input.Position.X)

		end

	end)

	UserInputService.InputChanged:Connect(function(Input)

		if Dragging then

			if Input.UserInputType == Enum.UserInputType.MouseMovement
				or Input.UserInputType == Enum.UserInputType.Touch then

				SetValue(Input.Position.X)

			end

		end

	end)

	UserInputService.InputEnded:Connect(function(Input)

		if Input.UserInputType == Enum.UserInputType.MouseButton1
			or Input.UserInputType == Enum.UserInputType.Touch then

			Dragging = false

		end

	end)

	local Start =
		(Default - Minimum) /
		(Maximum - Minimum)

	Fill.Size = UDim2.new(Start, 0, 1, 0)
	Label.Text = Text .. "  " .. tostring(Default)

	return Holder
end

local function Section(Parent, Text)

	local Label = Instance.new("TextLabel")
	Label.Size = UDim2.new(1, -5, 0, 25)
	Label.BackgroundTransparency = 1
	Label.Text = Text
	Label.TextColor3 = Config.ThemeColor
	Label.TextSize = 11
	Label.Font = Enum.Font.GothamBold
	Label.TextXAlignment = Enum.TextXAlignment.Left
	Label.ZIndex = 22
	Label.Parent = Parent

	return Label
end

--============================================================--
-- CHARACTER
--============================================================--

local function Character()

	return LocalPlayer.Character
		or LocalPlayer.CharacterAdded:Wait()

end

local function Humanoid()

	return Character():FindFirstChildOfClass("Humanoid")

end

local function Root()

	return Character():FindFirstChild("HumanoidRootPart")

end

--============================================================--
-- COMBAT
--============================================================--

Section(Combat, "AIM ASSIST")

Toggle(
	Combat,
	"Aimbot",
	false,
	function(Value)

		Config.Aimbot = Value

		FOVCircle.Visible =
			Value and Config.ShowFOV

		CenterDot.Visible =
			Value and Config.ShowFOV

	end
)

Toggle(
	Combat,
	"Mostrar círculo do FOV",
	true,
	function(Value)

		Config.ShowFOV = Value

		FOVCircle.Visible =
			Value and Config.Aimbot

		CenterDot.Visible =
			Value and Config.Aimbot

	end
)

Slider(
	Combat,
	"FOV",
	30,
	500,
	Config.AimbotFOV,
	function(Value)

		Config.AimbotFOV = Value

		FOVCircle.Size =
			UDim2.fromOffset(
				Value * 2,
				Value * 2
			)

	end
)

Slider(
	Combat,
	"Suavidade",
	1,
	100,
	18,
	function(Value)

		Config.AimbotSmooth =
			Value / 100

	end
)

--============================================================--
-- AIMBOT ESTÁVEL
--============================================================--

local function GetBestTarget()

	local BestTarget = nil
	local BestDistance = Config.AimbotFOV

	local Viewport = Camera.ViewportSize

	local Center = Vector2.new(
		Viewport.X / 2,
		Viewport.Y / 2
	)

	for _, Player in ipairs(Players:GetPlayers()) do

		if Player ~= LocalPlayer then

			local CharacterModel =
				Player.Character

			if CharacterModel then

				local TargetHumanoid =
					CharacterModel:FindFirstChildOfClass("Humanoid")

				local Head =
					CharacterModel:FindFirstChild("Head")

				if TargetHumanoid
					and Head
					and TargetHumanoid.Health > 0 then

					local Position,
						OnScreen =
						Camera:WorldToViewportPoint(
							Head.Position
						)

					if OnScreen
						and Position.Z > 0 then

						local ScreenPosition =
							Vector2.new(
								Position.X,
								Position.Y
							)

						local Distance =
							(
								ScreenPosition -
								Center
							).Magnitude

						if Distance <
							BestDistance then

							BestDistance =
								Distance

							BestTarget =
								Head

						end

					end

				end

			end

		end

	end

	return BestTarget
end

RunService.RenderStepped:Connect(function()

	if not Config.Aimbot then
		return
	end

	local Target = GetBestTarget()

	if not Target then
		return
	end

	local CameraPosition =
		Camera.CFrame.Position

	local Desired =
		CFrame.lookAt(
			CameraPosition,
			Target.Position
		)

	-- Suavização evita a câmera "pular"
	local Smooth =
		math.clamp(
			Config.AimbotSmooth,
			0.01,
			1
		)

	Camera.CFrame =
		Camera.CFrame:Lerp(
			Desired,
			Smooth
		)

end)

--============================================================--
-- VISUAL
--============================================================--

Section(Visual, "PLAYER ESP")

Toggle(
	Visual,
	"ESP Box",
	false,
	function(Value)

		Config.ESPBox = Value

	end
)

Toggle(
	Visual,
	"ESP Line",
	false,
	function(Value)

		Config.ESPLine = Value

	end
)

Toggle(
	Visual,
	"ESP Skeleton",
	false,
	function(Value)

		Config.ESPSkeleton = Value

	end
)

--============================================================--
-- ESP HIGHLIGHT
--============================================================--

local Highlights = {}

local function UpdateHighlights()

	for Player, Object in pairs(Highlights) do

		if Object then
			Object:Destroy()
		end

		Highlights[Player] = nil

	end

	if not Config.ESPBox then
		return
	end

	for _, Player in ipairs(Players:GetPlayers()) do

		if Player ~= LocalPlayer
			and Player.Character then

			local Highlight =
				Instance.new("Highlight")

			Highlight.Name =
				"FrierenESP"

			Highlight.Adornee =
				Player.Character

			Highlight.FillColor =
				Config.ESPColor

			Highlight.OutlineColor =
				Config.ESPColor

			Highlight.FillTransparency = 0.82
			Highlight.OutlineTransparency = 0

			Highlight.Parent =
				Player.Character

			Highlights[Player] =
				Highlight

		end

	end
end

--============================================================--
-- ESP WORLD LINES
--============================================================--

local Lines = {}

local function ClearLines()

	for _, Data in pairs(Lines) do

		for _, Object in pairs(Data) do

			if Object
				and Object.Parent then

				Object:Destroy()

			end

		end

	end

	table.clear(Lines)

end

local function UpdateLines()

	ClearLines()

	if not Config.ESPLine then
		return
	end

	local LocalRoot = Root()

	if not LocalRoot then
		return
	end

	for _, Player in ipairs(Players:GetPlayers()) do

		if Player ~= LocalPlayer
			and Player.Character then

			local TargetRoot =
				Player.Character:FindFirstChild(
					"HumanoidRootPart"
				)

			if TargetRoot then

				local A0 =
					Instance.new("Attachment")

				A0.Parent = LocalRoot

				local A1 =
					Instance.new("Attachment")

				A1.Parent = TargetRoot

				local Beam =
					Instance.new("Beam")

				Beam.Attachment0 = A0
				Beam.Attachment1 = A1

				Beam.Color =
					ColorSequence.new(
						Config.ESPColor
					)

				Beam.Width0 = 0.035
				Beam.Width1 = 0.035

				Beam.FaceCamera = true
				Beam.LightEmission = 1
				Beam.Parent = LocalRoot

				Lines[Player] = {
					A0,
					A1,
					Beam
				}

			end

		end

	end
end

--============================================================--
-- SKELETON
--============================================================--

local Skeletons = {}

local Bones = {
	{"Head", "UpperTorso"},
	{"UpperTorso", "LowerTorso"},

	{"UpperTorso", "LeftUpperArm"},
	{"LeftUpperArm", "LeftLowerArm"},
	{"LeftLowerArm", "LeftHand"},

	{"UpperTorso", "RightUpperArm"},
	{"RightUpperArm", "RightLowerArm"},
	{"RightLowerArm", "RightHand"},

	{"LowerTorso", "LeftUpperLeg"},
	{"LeftUpperLeg", "LeftLowerLeg"},
	{"LeftLowerLeg", "LeftFoot"},

	{"LowerTorso", "RightUpperLeg"},
	{"RightUpperLeg", "RightLowerLeg"},
	{"RightLowerLeg", "RightFoot"},
}

local function ClearSkeleton()

	for _, Data in pairs(Skeletons) do

		for _, Object in ipairs(Data) do

			if Object
				and Object.Parent then

				Object:Destroy()

			end

		end

	end

	table.clear(Skeletons)

end

local function UpdateSkeleton()

	ClearSkeleton()

	if not Config.ESPSkeleton then
		return
	end

	for _, Player in ipairs(Players:GetPlayers()) do

		if Player ~= LocalPlayer
			and Player.Character then

			local Created = {}

			for _, Bone in ipairs(Bones) do

				local PartA =
					Player.Character:FindFirstChild(
						Bone[1]
					)

				local PartB =
					Player.Character:FindFirstChild(
						Bone[2]
					)

				if PartA and PartB then

					local A0 =
						Instance.new("Attachment")

					A0.Parent = PartA

					local A1 =
						Instance.new("Attachment")

					A1.Parent = PartB

					local Beam =
						Instance.new("Beam")

					Beam.Attachment0 = A0
					Beam.Attachment1 = A1

					Beam.Color =
						ColorSequence.new(
							Config.ESPColor
						)

					Beam.Width0 = 0.035
					Beam.Width1 = 0.035
					Beam.FaceCamera = true
					Beam.LightEmission = 1
					Beam.Parent = PartA

					table.insert(Created, A0)
					table.insert(Created, A1)
					table.insert(Created, Beam)

				end

			end

			Skeletons[Player] = Created

		end

	end
end

-- Atualização periódica do ESP
