-- ✅ King Legacy Auto Aim - Có bật/tắt, giảm lag, có thông báo
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local VIM = game:GetService("VirtualInputManager")
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")

-- ⚙️ Cài đặt
local FOV_RADIUS = 150
local SKILL_KEYS = { Enum.KeyCode.Z, Enum.KeyCode.X, Enum.KeyCode.C, Enum.KeyCode.V }
local TOGGLE_KEY = Enum.KeyCode.Equals -- Nút "+" trên bàn phím

-- 🔘 Trạng thái Auto Aim
local AutoAimEnabled = true

-- 🎯 Vẽ vòng FOV
local circle = Drawing.new("Circle")
circle.Color = Color3.fromRGB(0, 255, 0)
circle.Radius = FOV_RADIUS
circle.Thickness = 2
circle.Filled = false
circle.Visible = AutoAimEnabled

RunService.RenderStepped:Connect(function()
	circle.Position = UIS:GetMouseLocation()
	circle.Visible = AutoAimEnabled
end)

-- 📢 Thông báo trên màn hình
local function notify(msg)
	pcall(function()
		StarterGui:SetCore("SendNotification", {
			Title = "🎯 Auto Aim",
			Text = msg,
			Duration = 2
		})
	end)
end

-- 🔍 Tìm mục tiêu gần nhất trong FOV
local function getClosestTarget()
	local mouse = UIS:GetMouseLocation()
	local closest, minDist = nil, FOV_RADIUS

	for _, obj in pairs(workspace:GetDescendants()) do
		if obj:IsA("Humanoid") and obj.Health > 0 then
			local char = obj.Parent
			if char and char:IsA("Model") and char ~= LocalPlayer.Character and char:FindFirstChild("HumanoidRootPart") then
				local hrp = char.HumanoidRootPart
				local screenPos, visible = Camera:WorldToViewportPoint(hrp.Position)
				if visible then
					local dist = (Vector2.new(screenPos.X, screenPos.Y) - mouse).Magnitude
					if dist < minDist then
						minDist = dist
						closest = hrp
					end
				end
			end
		end
	end

	return closest
end

-- 🧠 Bắt phím dùng skill => Aim vào mục tiêu gần nhất
UIS.InputBegan:Connect(function(input, processed)
	if processed then return end

	-- 🔄 Toggle Auto Aim
	if input.KeyCode == TOGGLE_KEY then
		AutoAimEnabled = not AutoAimEnabled
		circle.Visible = AutoAimEnabled
		notify("Auto Aim: " .. (AutoAimEnabled and "✅ Bật" or "❌ Tắt"))
	end

	-- 🛠 Auto Aim khi bấm skill
	if AutoAimEnabled and table.find(SKILL_KEYS, input.KeyCode) then
		local target = getClosestTarget()
		if target then
			local pos, visible = Camera:WorldToViewportPoint(target.Position)
			if visible then
				task.spawn(function()
					VIM:SendMouseMoveEvent(pos.X, pos.Y, game)
				end)
			end
		end
	end
end)
