1782014.                                                            -       -----------------------------------------------
-- SERVICES
------------------------------------------------
local player = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local lighting = game:GetService("Lighting")

------------------------------------------------
-- GUI
------------------------------------------------
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.ResetOnSpawn = false

------------------------------------------------
-- MAIN
------------------------------------------------
local main = Instance.new("Frame")
main.Parent = gui
main.Size = UDim2.new(0,500,0,520)
main.Position = UDim2.new(0.32,0,0.18,0)
main.BackgroundColor3 = Color3.fromRGB(15,15,15)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
Instance.new("UICorner",main).CornerRadius = UDim.new(0,12)

local stroke = Instance.new("UIStroke",main)
stroke.Color = Color3.fromRGB(0,170,255)
stroke.Thickness = 2

------------------------------------------------
-- TOPBAR
------------------------------------------------
local top = Instance.new("Frame",main)
top.Size = UDim2.new(1,0,0,45)
top.BackgroundColor3 = Color3.fromRGB(20,20,20)
top.BorderSizePixel = 0
Instance.new("UICorner",top)

local title = Instance.new("TextLabel",top)
title.Size = UDim2.new(1,0,1,0)
title.BackgroundTransparency = 1
title.Text = "v 1.4"
title.TextColor3 = Color3.fromRGB(0,170,255)
title.Font = Enum.Font.GothamBold
title.TextSize = 18

------------------------------------------------
-- SIDEBAR
------------------------------------------------
local side = Instance.new("Frame",main)
side.Size = UDim2.new(0,130,1,-45)
side.Position = UDim2.new(0,0,0,45)
side.BackgroundColor3 = Color3.fromRGB(18,18,18)
side.BorderSizePixel = 0
Instance.new("UICorner",side)

------------------------------------------------
-- CONTENT
------------------------------------------------
local content = Instance.new("Frame",main)
content.Size = UDim2.new(1,-130,1,-45)
content.Position = UDim2.new(0,130,0,45)
content.BackgroundTransparency = 1

------------------------------------------------
-- PAGES
------------------------------------------------
local pages = {}

local function newPage()
	local p = Instance.new("Frame",content)
	p.Size = UDim2.new(1,0,1,0)
	p.BackgroundTransparency = 1
	p.Visible = false
	table.insert(pages,p)
	return p
end

local function showPage(i)
	for n,v in pairs(pages) do
		v.Visible = (n == i)
	end
end

local movement = newPage()
local visual = newPage()

showPage(1)

------------------------------------------------
-- TAB
------------------------------------------------
local function tab(text,icon,index,y)

	local b = Instance.new("TextButton",side)
	b.Size = UDim2.new(1,-10,0,40)
	b.Position = UDim2.new(0,5,0,y)

	b.Text = icon.." "..text
	b.Font = Enum.Font.GothamBold
	b.TextSize = 14
	b.TextColor3 = Color3.new(1,1,1)

	b.BackgroundColor3 = Color3.fromRGB(25,25,25)
	b.BorderSizePixel = 0

	Instance.new("UICorner",b)

	local s = Instance.new("UIStroke",b)
	s.Color = Color3.fromRGB(0,170,255)
	s.Thickness = 0

	b.MouseEnter:Connect(function()
		TweenService:Create(s,TweenInfo.new(0.2),{
			Thickness = 2
		}):Play()
	end)

	b.MouseLeave:Connect(function()
		TweenService:Create(s,TweenInfo.new(0.2),{
			Thickness = 0
		}):Play()
	end)

	b.MouseButton1Click:Connect(function()
		showPage(index)
	end)
end

tab("Di chuyển","",1,10)
tab("Hình ảnh","",2,60)

------------------------------------------------
-- TOGGLE
------------------------------------------------
local function toggle(parent,text,y,callback)

	local frame = Instance.new("Frame",parent)
	frame.Size = UDim2.new(1,-20,0,50)
	frame.Position = UDim2.new(0,10,0,y)

	frame.BackgroundColor3 = Color3.fromRGB(22,22,22)
	frame.BorderSizePixel = 0

	Instance.new("UICorner",frame)

	local label = Instance.new("TextLabel",frame)
	label.Size = UDim2.new(0.7,0,1,0)
	label.Position = UDim2.new(0,10,0,0)

	label.BackgroundTransparency = 1
	label.Text = text
	label.TextColor3 = Color3.new(1,1,1)
	label.Font = Enum.Font.GothamBold
	label.TextSize = 15
	label.TextXAlignment = Enum.TextXAlignment.Left

	local button = Instance.new("TextButton",frame)
	button.Size = UDim2.new(0,60,0,26)
	button.Position = UDim2.new(0.8,0,0.24,0)

	button.Text = ""
	button.BackgroundColor3 = Color3.fromRGB(60,60,60)
	button.BorderSizePixel = 0

	Instance.new("UICorner",button)

	local dot = Instance.new("Frame",button)
	dot.Size = UDim2.new(0,22,0,22)
	dot.Position = UDim2.new(0.05,0,0.08,0)
	dot.BackgroundColor3 = Color3.new(1,1,1)
	dot.BorderSizePixel = 0
	Instance.new("UICorner",dot)

	local enabled = false

	button.MouseButton1Click:Connect(function()

		enabled = not enabled

		if enabled then

			TweenService:Create(button,TweenInfo.new(0.2),{
				BackgroundColor3 = Color3.fromRGB(0,170,255)
			}):Play()

			TweenService:Create(dot,TweenInfo.new(0.2),{
				Position = UDim2.new(0.55,0,0.08,0)
			}):Play()

		else

			TweenService:Create(button,TweenInfo.new(0.2),{
				BackgroundColor3 = Color3.fromRGB(60,60,60)
			}):Play()

			TweenService:Create(dot,TweenInfo.new(0.2),{
				Position = UDim2.new(0.05,0,0.08,0)
			}):Play()

		end

		callback(enabled)

	end)
end

------------------------------------------------
-- SPEED
------------------------------------------------
local speedBox = Instance.new("TextBox",movement)
speedBox.Size = UDim2.new(0,120,0,32)
speedBox.Position = UDim2.new(0,15,0,10)
speedBox.Text = "50"
speedBox.PlaceholderText = "Speed"
speedBox.BackgroundColor3 = Color3.fromRGB(35,35,35)
speedBox.TextColor3 = Color3.new(1,1,1)
speedBox.BorderSizePixel = 0
Instance.new("UICorner",speedBox)

toggle(movement,"Tăng tốc🦵",60,function(state)

	local char = player.Character or player.CharacterAdded:Wait()
	local hum = char:WaitForChild("Humanoid")

	hum.WalkSpeed = state and tonumber(speedBox.Text) or 16
end)

------------------------------------------------
-- NOCLIP
------------------------------------------------
local noclip = false

toggle(movement,"Xuyên tường",125,function(state)
	noclip = state
end)

RunService.Stepped:Connect(function()

	if noclip then

		local char = player.Character

		if char then
			for _,v in pairs(char:GetDescendants()) do

				if v:IsA("BasePart") then
					v.CanCollide = false
				end
			end
		end
	end
end)

------------------------------------------------
-- FLY SPEED
------------------------------------------------
local flySpeed = 80

local flySpeedBox = Instance.new("TextBox",movement)
flySpeedBox.Size = UDim2.new(0,120,0,32)
flySpeedBox.Position = UDim2.new(0,15,0,250)

flySpeedBox.Text = "80"
flySpeedBox.PlaceholderText = "Tốc độ bay"

flySpeedBox.BackgroundColor3 = Color3.fromRGB(35,35,35)
flySpeedBox.TextColor3 = Color3.new(1,1,1)
flySpeedBox.BorderSizePixel = 0

Instance.new("UICorner",flySpeedBox)

------------------------------------------------
-- FLY
------------------------------------------------
local flying = false
local bv,bg,flyConn
local flyDir = Vector3.zero

------------------------------------------------
-- MOBILE BUTTONS
------------------------------------------------
local mobile = Instance.new("Frame",gui)
mobile.Size = UDim2.new(0,170,0,170)
mobile.Position = UDim2.new(0.03,0,0.7,0)
mobile.BackgroundTransparency = 1

local function makeBtn(txt,x,y)

	local b = Instance.new("TextButton",mobile)

	b.Size = UDim2.new(0,50,0,50)
	b.Position = UDim2.new(0,x,0,y)

	b.Text = txt
	b.TextScaled = true

	b.BackgroundColor3 = Color3.fromRGB(0,170,255)
	b.TextColor3 = Color3.new(1,1,1)

	b.BorderSizePixel = 0
	b.AutoButtonColor = false

	Instance.new("UICorner",b)

	local hold = false

	b.MouseButton1Down:Connect(function()
		hold = true
	end)

	b.MouseButton1Up:Connect(function()
		hold = false
		flyDir = Vector3.zero
	end)

	RunService.RenderStepped:Connect(function()

		if hold then

			if txt == "↑" then
				flyDir = Vector3.new(0,0,-1)

			elseif txt == "↓" then
				flyDir = Vector3.new(0,0,1)

			elseif txt == "←" then
				flyDir = Vector3.new(-1,0,0)

			elseif txt == "→" then
				flyDir = Vector3.new(1,0,0)
			end
		end
	end)
end

makeBtn("↑",55,0)
makeBtn("↓",55,110)
makeBtn("←",0,55)
makeBtn("→",110,55)

------------------------------------------------
-- START FLY
------------------------------------------------
local function startFly()

	local char = player.Character or player.CharacterAdded:Wait()
	local hrp = char:WaitForChild("HumanoidRootPart")
	local hum = char:WaitForChild("Humanoid")

	hum.PlatformStand = true

	bv = Instance.new("BodyVelocity",hrp)
	bv.MaxForce = Vector3.new(1e6,1e6,1e6)

	bg = Instance.new("BodyGyro",hrp)
	bg.MaxTorque = Vector3.new(1e6,1e6,1e6)
	bg.P = 5000

	flyConn = RunService.RenderStepped:Connect(function()

		local cam = workspace.CurrentCamera
		local move = Vector3.zero

		move += cam.CFrame.RightVector * flyDir.X
		move += cam.CFrame.LookVector * -flyDir.Z

		if UIS:IsKeyDown(Enum.KeyCode.W) then
			move += cam.CFrame.LookVector
		end

		if UIS:IsKeyDown(Enum.KeyCode.S) then
			move -= cam.CFrame.LookVector
		end

		if UIS:IsKeyDown(Enum.KeyCode.A) then
			move -= cam.CFrame.RightVector
		end

		if UIS:IsKeyDown(Enum.KeyCode.D) then
			move += cam.CFrame.RightVector
		end

		flySpeed = tonumber(flySpeedBox.Text) or 80

		bv.Velocity = bv.Velocity:Lerp(move * flySpeed,0.15)
		bg.CFrame = bg.CFrame:Lerp(cam.CFrame,0.2)

	end)
end

local function stopFly()

	if flyConn then
		flyConn:Disconnect()
	end

	if bv then
		bv:Destroy()
	end

	if bg then
		bg:Destroy()
	end

	local char = player.Character

	if char then
		local hum = char:FindFirstChild("Humanoid")

		if hum then
			hum.PlatformStand = false
		end
	end
end

toggle(movement,"Bay🕊",190,function(state)

	flying = state

	if state then
		startFly()
	else
		stopFly()
	end
end)

------------------------------------------------
-- VISUAL
------------------------------------------------
toggle(visual,"Tàng hình😶‍🌫️",20,function(state)

	local char = player.Character or player.CharacterAdded:Wait()

	for _,v in pairs(char:GetDescendants()) do

		if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" then
			v.Transparency = state and 1 or 0
		end
	end
end)

toggle(visual,"FullBright",85,function(state)

	lighting.Brightness = state and 5 or 2
	lighting.ClockTime = state and 14 or 12
	lighting.GlobalShadows = not state
end)

------------------------------------------------
-- ESP
------------------------------------------------
toggle(visual,"nhìn thấy player👀 ",150,function(state)

	if state then

		for _,p in pairs(game.Players:GetPlayers()) do

			if p ~= player and p.Character then

				local h = Instance.new("Highlight")
				h.Adornee = p.Character
				h.FillColor = Color3.fromRGB(0,170,255)
				h.OutlineColor = Color3.new(1,1,1)
				h.Parent = p.Character
			end
		end
	end
end)

------------------------------------------------
-- MENU ANIMATION
------------------------------------------------
local function openMenu(frame)

	frame.Visible = true
	frame.Size = UDim2.new(0,0,0,0)

	TweenService:Create(
		frame,
		TweenInfo.new(
			0.35,
			Enum.EasingStyle.Back,
			Enum.EasingDirection.Out
		),
		{
			Size = UDim2.new(0,500,0,520)
		}
	):Play()
end

local function closeMenu(frame)

	local tween = TweenService:Create(
		frame,
		TweenInfo.new(
			0.25,
			Enum.EasingStyle.Quad,
			Enum.EasingDirection.In
		),
		{
			Size = UDim2.new(0,0,0,0)
		}
	)

	tween:Play()

	tween.Completed:Connect(function()
		frame.Visible = false
	end)
end

------------------------------------------------
-- NÚT ẨN HIỆN
------------------------------------------------
local hide = Instance.new("TextButton",gui)

hide.Size = UDim2.new(0,50,0,50)
hide.Position = UDim2.new(0.02,0,0.4,0)

hide.Text = "D"
hide.TextScaled = true

hide.BackgroundColor3 = Color3.fromRGB(0,170,255)
hide.TextColor3 = Color3.new(1,1,1)

hide.BorderSizePixel = 0
hide.Active = true
hide.Draggable = true

Instance.new("UICorner",hide)

local showUI = true

hide.MouseButton1Click:Connect(function()

	showUI = not showUI

	if showUI then
		openMenu(main)
	else
		closeMenu(main)
	end
end)
