-- | by xXh team |

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")

-- GUI
local gui = Instance.new("ScreenGui")
gui.Parent = game.CoreGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0,340,0,420)
frame.Position = UDim2.new(0,20,0.5,-200)
frame.BackgroundColor3 = Color3.fromRGB(0,0,0)
frame.BorderColor3 = Color3.fromRGB(0,255,0)
frame.Parent = gui

-- TITLE
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1,-40,0,30)
title.BackgroundColor3 = Color3.fromRGB(0,0,0)
title.TextColor3 = Color3.fromRGB(0,255,0)
title.Font = Enum.Font.Code
title.TextSize = 18
title.Parent = frame

-- PLAYER COUNT FUNCTION
local function updatePlayerCount()
	title.Text = "COPY USERNAME PLAYER BY XIANDI ⚠ | PLAYERS : "..#Players:GetPlayers()
end

updatePlayerCount()

-- MINIMIZE
local toggle = Instance.new("TextButton")
toggle.Size = UDim2.new(0,30,0,30)
toggle.Position = UDim2.new(1,-30,0,0)
toggle.Text = "-"
toggle.BackgroundColor3 = Color3.fromRGB(0,0,0)
toggle.TextColor3 = Color3.fromRGB(0,255,0)
toggle.Font = Enum.Font.Code
toggle.Parent = frame

-- SEARCH
local search = Instance.new("TextBox")
search.Size = UDim2.new(1,0,0,25)
search.Position = UDim2.new(0,0,0,30)
search.PlaceholderText = "search username..."
search.BackgroundColor3 = Color3.fromRGB(0,0,0)
search.TextColor3 = Color3.fromRGB(0,255,0)
search.Font = Enum.Font.Code
search.Parent = frame

-- SCAN TEXT
local scan = Instance.new("TextLabel")
scan.Size = UDim2.new(1,0,0,25)
scan.Position = UDim2.new(0,0,0,55)
scan.BackgroundTransparency = 1
scan.TextColor3 = Color3.fromRGB(0,255,0)
scan.Font = Enum.Font.Code
scan.Text = "SCANNING PLAYERS..."
scan.Parent = frame

-- PLAYER LIST
local scrolling = Instance.new("ScrollingFrame")
scrolling.Size = UDim2.new(1,0,1,-130)
scrolling.Position = UDim2.new(0,0,0,80)
scrolling.BackgroundColor3 = Color3.fromRGB(0,0,0)
scrolling.BorderSizePixel = 0
scrolling.Parent = frame

local layout = Instance.new("UIListLayout")
layout.Parent = scrolling

-- COPY ALL
local copyAll = Instance.new("TextButton")
copyAll.Size = UDim2.new(1,0,0,25)
copyAll.Position = UDim2.new(0,0,1,-50)
copyAll.Text = "COPY ALL PLAYERS"
copyAll.BackgroundColor3 = Color3.fromRGB(0,0,0)
copyAll.TextColor3 = Color3.fromRGB(0,255,0)
copyAll.Font = Enum.Font.Code
copyAll.Parent = frame

-- TEXT BOX
local box = Instance.new("TextBox")
box.Size = UDim2.new(1,0,0,25)
box.Position = UDim2.new(0,0,1,-25)
box.BackgroundColor3 = Color3.fromRGB(0,0,0)
box.TextColor3 = Color3.fromRGB(0,255,0)
box.Font = Enum.Font.Code
box.Parent = frame

-- COPY FUNCTION
local function copy(text)
	if setclipboard then
		setclipboard(text)
	else
		box.Text = text
		box:CaptureFocus()
	end
end

-- PLAYER BUTTON
local buttons = {}

local function addPlayer(player)

	local button = Instance.new("TextButton")
	button.Size = UDim2.new(1,0,0,25)
	button.BackgroundColor3 = Color3.fromRGB(0,0,0)
	button.TextColor3 = Color3.fromRGB(0,255,0)
	button.Font = Enum.Font.Code
	button.Text = player.Name.." | "..player.DisplayName.." | "..player.UserId
	button.Parent = scrolling

	buttons[player.Name] = button

	button.MouseButton1Click:Connect(function()
		copy(player.Name.." | "..player.DisplayName.." | "..player.UserId)
	end)

	updatePlayerCount()

end

-- REMOVE PLAYER
local function removePlayer(player)
	if buttons[player.Name] then
		buttons[player.Name]:Destroy()
		buttons[player.Name] = nil
	end
	updatePlayerCount()
end

for _,p in pairs(Players:GetPlayers()) do
	addPlayer(p)
end

Players.PlayerAdded:Connect(addPlayer)
Players.PlayerRemoving:Connect(removePlayer)

-- SEARCH (USERNAME + DISPLAYNAME + USERID)
search:GetPropertyChangedSignal("Text"):Connect(function()

	local text = string.lower(search.Text)

	for _,player in pairs(Players:GetPlayers()) do

		local button = buttons[player.Name]

		if button then

			local username = string.lower(player.Name)
			local display = string.lower(player.DisplayName)
			local userid = tostring(player.UserId)

			if string.find(username,text)
			or string.find(display,text)
			or string.find(userid,text) then
				button.Visible = true
			else
				button.Visible = false
			end

		end

	end

end)

-- COPY ALL
copyAll.MouseButton1Click:Connect(function()

	local list = {}

	for _,p in pairs(Players:GetPlayers()) do
		table.insert(list,p.Name.." | "..p.DisplayName.." | "..p.UserId)
	end

	copy(table.concat(list,", "))

end)

-- BINARY FRAME
local binaryFrame = Instance.new("Frame")
binaryFrame.Size = UDim2.new(1,0,1,0)
binaryFrame.BackgroundTransparency = 1
binaryFrame.Parent = frame

-- BINARY RAIN
for i = 1,15 do

	local label = Instance.new("TextLabel")
	label.BackgroundTransparency = 1
	label.TextColor3 = Color3.fromRGB(0,255,0)
	label.Font = Enum.Font.Code
	label.TextSize = 14
	label.Size = UDim2.new(0,20,0,400)
	label.Position = UDim2.new(0,math.random(0,300),0,-400)

	local text = ""
	for j = 1,40 do
		text = text .. tostring(math.random(0,1)) .. "\n"
	end

	label.Text = text
	label.Parent = binaryFrame

	task.spawn(function()
		while true do
			label.Position = UDim2.new(0,label.Position.X.Offset,0,-400)

			for y = -400,400,3 do
				label.Position = UDim2.new(0,label.Position.X.Offset,0,y)
				task.wait(0.02)
			end
		end
	end)

end

-- MINIMIZE
local minimized = false

toggle.MouseButton1Click:Connect(function()

	minimized = not minimized

	if minimized then
		toggle.Text = "+"
		search.Visible = false
		scan.Visible = false
		scrolling.Visible = false
		copyAll.Visible = false
		box.Visible = false
		binaryFrame.Visible = false
		frame.Size = UDim2.new(0,340,0,30)
	else
		toggle.Text = "-"
		search.Visible = true
		scrolling.Visible = true
		copyAll.Visible = true
		box.Visible = true
		binaryFrame.Visible = true
		frame.Size = UDim2.new(0,340,0,420)
	end

end)

-- DRAG (PC + MOBILE)
local dragging = false
local dragStart
local startPos

title.InputBegan:Connect(function(input)

	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then

		dragging = true
		dragStart = input.Position
		startPos = frame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)

	end

end)

UIS.InputChanged:Connect(function(input)

	if dragging then

		local delta = input.Position - dragStart

		frame.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)

	end

end)

-- SCANNING ANIMATION
task.spawn(function()

	local dots = ""

	for i = 1,20 do

		dots = dots .. "."
		scan.Text = "SCANNING PLAYERS" .. dots

		task.wait(0.15)

		if #dots >= 3 then
			dots = ""
		end

	end

	scan:Destroy()

end)
