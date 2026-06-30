-- XZ AIMBOT

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

local function getCam()
    return workspace.CurrentCamera
end

local Settings = {
    AimEnabled = false,
    FOV = 120,
    ShowFOV = true,
    ESPLine = false,
    ESPBox = false,
    ESPHealth = false,
    ESPSkeleton = false,
}

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "XZ_AIMBOT"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local s, err = pcall(function()
    ScreenGui.Parent = CoreGui
end)
if not s then
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
end

-- MAIN FRAME
local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.new(0, 290, 0, 175)
Main.Position = UDim2.new(0, 40, 0, 40)
Main.BackgroundColor3 = Color3.fromRGB(18, 18, 26)
Main.BackgroundTransparency = 0.08
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true
Main.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = Main

local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(60, 60, 85)
UIStroke.Thickness = 1
UIStroke.Transparency = 0.3
UIStroke.Parent = Main

-- TITLE BAR
local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 34)
TitleBar.BackgroundColor3 = Color3.fromRGB(28, 28, 40)
TitleBar.BackgroundTransparency = 0.2
TitleBar.BorderSizePixel = 0
TitleBar.Parent = Main

local TitleBarCorner = Instance.new("UICorner")
TitleBarCorner.CornerRadius = UDim.new(0, 8)
TitleBarCorner.Parent = TitleBar

local TitleBarCover = Instance.new("Frame")
TitleBarCover.Size = UDim2.new(1, 0, 0, 8)
TitleBarCover.Position = UDim2.new(0, 0, 1, -8)
TitleBarCover.BackgroundColor3 = Color3.fromRGB(28, 28, 40)
TitleBarCover.BackgroundTransparency = 0.2
TitleBarCover.BorderSizePixel = 0
TitleBarCover.Parent = TitleBar

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -40, 1, 0)
Title.Position = UDim2.new(0, 12, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "XZ AIMBOT"
Title.TextColor3 = Color3.fromRGB(190, 190, 230)
Title.TextSize = 14
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TitleBar

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 24, 0, 24)
CloseBtn.Position = UDim2.new(1, -30, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(220, 80, 80)
CloseBtn.TextSize = 13
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.BorderSizePixel = 0
CloseBtn.Parent = TitleBar

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 4)
CloseCorner.Parent = CloseBtn

CloseBtn.MouseButton1Click:Connect(function()
    Settings.AimEnabled = false
    Settings.ShowFOV = false
    Settings.ESPLine = false
    Settings.ESPBox = false
    Settings.ESPHealth = false
    Settings.ESPSkeleton = false
    FovCircle.Visible = false
    hideAllLines()
    hideAllBoxes()
    hideAllHealthBars()
    hideAllSkeletons()
    ScreenGui:Destroy()
end)

-- SIDEBAR
local function createTabBtn(y, txt)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 42, 0, 42)
    btn.Position = UDim2.new(0, 8, 0, y)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
    btn.BorderSizePixel = 0
    btn.Text = txt
    btn.TextColor3 = Color3.fromRGB(170, 170, 200)
    btn.TextSize = 20
    btn.Font = Enum.Font.GothamBold
    btn.Parent = Main

    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, 5)
    c.Parent = btn

    return btn
end

local activeTab = "aimbot"
local AimTabBtn = createTabBtn(44, "⊕")
local EspTabBtn = createTabBtn(94, "◉")

-- TAB 1 (AIMBOT)
local Tab1Frame = Instance.new("Frame")
Tab1Frame.Size = UDim2.new(0, 226, 0, 130)
Tab1Frame.Position = UDim2.new(0, 56, 0, 40)
Tab1Frame.BackgroundTransparency = 1
Tab1Frame.BorderSizePixel = 0
Tab1Frame.Parent = Main
Tab1Frame.Visible = true

local ToggleLabel = Instance.new("TextLabel")
ToggleLabel.Size = UDim2.new(0, 80, 0, 28)
ToggleLabel.Position = UDim2.new(0, 6, 0, 6)
ToggleLabel.BackgroundTransparency = 1
ToggleLabel.Text = "AIMBOT"
ToggleLabel.TextColor3 = Color3.fromRGB(170, 170, 200)
ToggleLabel.TextSize = 14
ToggleLabel.Font = Enum.Font.Gotham
ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
ToggleLabel.Parent = Tab1Frame

local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 44, 0, 24)
ToggleBtn.Position = UDim2.new(0, 172, 0, 6)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(55, 55, 75)
ToggleBtn.BorderSizePixel = 0
ToggleBtn.Text = "OFF"
ToggleBtn.TextColor3 = Color3.fromRGB(200, 90, 90)
ToggleBtn.TextSize = 11
ToggleBtn.Font = Enum.Font.GothamBold
ToggleBtn.Parent = Tab1Frame

local ToggleCorner = Instance.new("UICorner")
ToggleCorner.CornerRadius = UDim.new(0, 4)
ToggleCorner.Parent = ToggleBtn

ToggleBtn.MouseButton1Click:Connect(function()
    Settings.AimEnabled = not Settings.AimEnabled
    if Settings.AimEnabled then
        TweenService:Create(ToggleBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(70, 130, 80)}):Play()
        ToggleBtn.TextColor3 = Color3.fromRGB(100, 230, 110)
        ToggleBtn.Text = "ON"
    else
        TweenService:Create(ToggleBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(55, 55, 75)}):Play()
        ToggleBtn.TextColor3 = Color3.fromRGB(200, 90, 90)
        ToggleBtn.Text = "OFF"
    end
end)

local FovLabel = Instance.new("TextLabel")
FovLabel.Size = UDim2.new(0, 80, 0, 28)
FovLabel.Position = UDim2.new(0, 6, 0, 38)
FovLabel.BackgroundTransparency = 1
FovLabel.Text = "FOV: " .. Settings.FOV
FovLabel.TextColor3 = Color3.fromRGB(170, 170, 200)
FovLabel.TextSize = 14
FovLabel.Font = Enum.Font.Gotham
FovLabel.TextXAlignment = Enum.TextXAlignment.Left
FovLabel.Parent = Tab1Frame

local SliderBg = Instance.new("Frame")
SliderBg.Size = UDim2.new(0, 140, 0, 5)
SliderBg.Position = UDim2.new(0, 78, 0, 50)
SliderBg.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
SliderBg.BorderSizePixel = 0
SliderBg.Parent = Tab1Frame

local SliderBgCorner = Instance.new("UICorner")
SliderBgCorner.CornerRadius = UDim.new(1, 0)
SliderBgCorner.Parent = SliderBg

local SliderFill = Instance.new("Frame")
SliderFill.Size = UDim2.new(Settings.FOV / 400, 0, 1, 0)
SliderFill.BackgroundColor3 = Color3.fromRGB(100, 140, 220)
SliderFill.BorderSizePixel = 0
SliderFill.Parent = SliderBg

local SliderFillCorner = Instance.new("UICorner")
SliderFillCorner.CornerRadius = UDim.new(1, 0)
SliderFillCorner.Parent = SliderFill

local SliderBtn = Instance.new("TextButton")
SliderBtn.Size = UDim2.new(0, 14, 0, 14)
SliderBtn.Position = UDim2.new(Settings.FOV / 400, 0, 0, -4.5)
SliderBtn.BackgroundColor3 = Color3.fromRGB(120, 160, 240)
SliderBtn.BorderSizePixel = 0
SliderBtn.Text = ""
SliderBtn.Parent = SliderBg

local SliderBtnCorner = Instance.new("UICorner")
SliderBtnCorner.CornerRadius = UDim.new(1, 0)
SliderBtnCorner.Parent = SliderBtn

local ShowFovLabel = Instance.new("TextLabel")
ShowFovLabel.Size = UDim2.new(0, 100, 0, 28)
ShowFovLabel.Position = UDim2.new(0, 6, 0, 72)
ShowFovLabel.BackgroundTransparency = 1
ShowFovLabel.Text = "Mostrar FOV"
ShowFovLabel.TextColor3 = Color3.fromRGB(170, 170, 200)
ShowFovLabel.TextSize = 14
ShowFovLabel.Font = Enum.Font.Gotham
ShowFovLabel.TextXAlignment = Enum.TextXAlignment.Left
ShowFovLabel.Parent = Tab1Frame

local ShowFovBtn = Instance.new("TextButton")
ShowFovBtn.Size = UDim2.new(0, 44, 0, 24)
ShowFovBtn.Position = UDim2.new(0, 172, 0, 72)
ShowFovBtn.BackgroundColor3 = Color3.fromRGB(70, 130, 80)
ShowFovBtn.BorderSizePixel = 0
ShowFovBtn.Text = "ON"
ShowFovBtn.TextColor3 = Color3.fromRGB(100, 230, 110)
ShowFovBtn.TextSize = 11
ShowFovBtn.Font = Enum.Font.GothamBold
ShowFovBtn.Parent = Tab1Frame

local ShowFovCorner = Instance.new("UICorner")
ShowFovCorner.CornerRadius = UDim.new(0, 4)
ShowFovCorner.Parent = ShowFovBtn

ShowFovBtn.MouseButton1Click:Connect(function()
    Settings.ShowFOV = not Settings.ShowFOV
    if Settings.ShowFOV then
        TweenService:Create(ShowFovBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(70, 130, 80)}):Play()
        ShowFovBtn.TextColor3 = Color3.fromRGB(100, 230, 110)
        ShowFovBtn.Text = "ON"
    else
        TweenService:Create(ShowFovBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(55, 55, 75)}):Play()
        ShowFovBtn.TextColor3 = Color3.fromRGB(200, 90, 90)
        ShowFovBtn.Text = "OFF"
    end
end)

-- TAB 2 (ESP)
local Tab2Frame = Instance.new("Frame")
Tab2Frame.Size = UDim2.new(0, 226, 0, 130)
Tab2Frame.Position = UDim2.new(0, 56, 0, 40)
Tab2Frame.BackgroundTransparency = 1
Tab2Frame.BorderSizePixel = 0
Tab2Frame.Parent = Main
Tab2Frame.Visible = false

local espToggles = {"ESP Line", "ESP Box", "ESP Health", "ESP Skeleton"}
local espKeys = {"ESPLine", "ESPBox", "ESPHealth", "ESPSkeleton"}
local espButtons = {}

for i = 1, 4 do
    local yPos = 4 + (i - 1) * 31

    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(0, 110, 0, 26)
    lbl.Position = UDim2.new(0, 6, 0, yPos)
    lbl.BackgroundTransparency = 1
    lbl.Text = espToggles[i]
    lbl.TextColor3 = Color3.fromRGB(170, 170, 200)
    lbl.TextSize = 14
    lbl.Font = Enum.Font.Gotham
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Parent = Tab2Frame

    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 44, 0, 22)
    btn.Position = UDim2.new(0, 172, 0, yPos + 2)
    btn.BackgroundColor3 = Color3.fromRGB(55, 55, 75)
    btn.BorderSizePixel = 0
    btn.Text = "OFF"
    btn.TextColor3 = Color3.fromRGB(200, 90, 90)
    btn.TextSize = 11
    btn.Font = Enum.Font.GothamBold
    btn.Parent = Tab2Frame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 4)
    corner.Parent = btn

    local key = espKeys[i]
    espButtons[key] = btn

    btn.MouseButton1Click:Connect(function()
        Settings[key] = not Settings[key]
        if Settings[key] then
            TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(70, 130, 80)}):Play()
            btn.TextColor3 = Color3.fromRGB(100, 230, 110)
            btn.Text = "ON"
        else
            TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(55, 55, 75)}):Play()
            btn.TextColor3 = Color3.fromRGB(200, 90, 90)
            btn.Text = "OFF"
        end
    end)
end

-- TAB SELECTOR
local function selectTab(tab)
    activeTab = tab
    if tab == "aimbot" then
        TweenService:Create(AimTabBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(55, 65, 90)}):Play()
        TweenService:Create(EspTabBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(40, 40, 55)}):Play()
        Tab1Frame.Visible = true
        Tab2Frame.Visible = false
    else
        TweenService:Create(EspTabBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(55, 65, 90)}):Play()
        TweenService:Create(AimTabBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(40, 40, 55)}):Play()
        Tab1Frame.Visible = false
        Tab2Frame.Visible = true
    end
end

AimTabBtn.MouseButton1Click:Connect(function() selectTab("aimbot") end)
EspTabBtn.MouseButton1Click:Connect(function() selectTab("esp") end)

-- FOV CIRCLE
local FovCircle = Drawing.new("Circle")
FovCircle.Thickness = 1.5
FovCircle.NumSides = 72
FovCircle.Radius = Settings.FOV
FovCircle.Filled = false
FovCircle.Color = Color3.fromRGB(110, 150, 240)
FovCircle.Transparency = 0.6
FovCircle.Visible = Settings.ShowFOV
FovCircle.Position = getCam().ViewportSize / 2

-- SLIDER DRAG
local draggingSlider = false

SliderBtn.MouseButton1Down:Connect(function()
    draggingSlider = true
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        draggingSlider = false
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and draggingSlider then
        local relX = Mouse.X - SliderBg.AbsolutePosition.X
        local clamped = math.clamp(relX, 0, SliderBg.AbsoluteSize.X)
        local percent = clamped / SliderBg.AbsoluteSize.X
        Settings.FOV = math.floor(percent * 400)
        FovCircle.Radius = Settings.FOV
        FovLabel.Text = "FOV: " .. Settings.FOV
        SliderBtn.Position = UDim2.new(percent, 0, 0, -4.5)
        SliderFill.Size = UDim2.new(percent, 0, 1, 0)
    end
end)

-- HELPERS
local function findHead(char)
    if not char then return nil end
    local head = char:FindFirstChild("Head")
    if head and head:IsA("BasePart") then return head end
    for _, v in pairs(char:GetDescendants()) do
        if v.Name == "Head" and v:IsA("BasePart") then
            return v
        end
    end
    return nil
end

local function findFeet(char)
    if not char then return nil end
    local candidates = {"LeftFoot", "RightFoot", "LeftLowerLeg", "RightLowerLeg", "Left Leg", "Right Leg"}
    local lowest, lowY = nil, math.huge
    for _, n in pairs(candidates) do
        local p = char:FindFirstChild(n)
        if p and p:IsA("BasePart") and p.Position.Y < lowY then
            lowY = p.Position.Y
            lowest = p
        end
    end
    if not lowest then
        local hum = findHum(char)
        if hum and hum.RootPart then
            local off = hum.RootPart.Size.Y * 1.2
            return {Position = hum.RootPart.Position - Vector3.new(0, off, 0)}
        end
    end
    return lowest
end

local function findHum(char)
    if not char then return nil end
    return char:FindFirstChildOfClass("Humanoid")
end

local function findPart(char, name)
    local p = char:FindFirstChild(name)
    if p and p:IsA("BasePart") then return p end
    return nil
end

local function isSameTeam(plr)
    if not plr then return false end
    local mt = LocalPlayer.Team
    local tt = plr.Team
    return mt and tt and mt == tt
end

-- BOT CACHE
local botCache = {}
local function refreshBots()
    local new = {}
    for _, v in pairs(workspace:GetDescendants()) do
        if v:IsA("Model") and findHum(v) and findHead(v) and not Players:GetPlayerFromCharacter(v) then
            table.insert(new, v)
        end
    end
    botCache = new
end
refreshBots()

-- CHECK VISIBILITY
local function isVisible(targetPart)
    if not targetPart then return false end
    local cam = getCam()
    if not cam then return false end
    local origin = cam.CFrame.Position
    local dir = (targetPart.Position - origin).Unit * 1000
    local params = RaycastParams.new()
    params.FilterType = Enum.RaycastFilterType.Blacklist
    params.FilterDescendantsInstances = {LocalPlayer.Character or {}}
    local result = workspace:Raycast(origin, dir, params)
    if result then
        return result.Instance:IsDescendantOf(targetPart.Parent)
    end
    return true
end

-- FIND CLOSEST TARGET
local function getClosestTarget()
    local best, bestDist = nil, Settings.FOV
    local cam = getCam()
    if not cam then return nil end
    local center = cam.ViewportSize / 2

    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and not isSameTeam(plr) and plr.Character then
            local head = findHead(plr.Character)
            local hum = findHum(plr.Character)
            if head and hum and hum.Health > 0 then
                local pos, onScreen = cam:WorldToViewportPoint(head.Position)
                if onScreen then
                    local dist = (Vector2.new(pos.X, pos.Y) - center).Magnitude
                    if dist <= Settings.FOV and dist < bestDist and isVisible(head) then
                        bestDist = dist
                        best = {Character = plr.Character, Head = head}
                    end
                end
            end
        end
    end

    for _, bot in pairs(botCache) do
        if bot then
            local head = findHead(bot)
            local hum = findHum(bot)
            if head and hum and hum.Health > 0 then
                local pos, onScreen = cam:WorldToViewportPoint(head.Position)
                if onScreen then
                    local dist = (Vector2.new(pos.X, pos.Y) - center).Magnitude
                    if dist <= Settings.FOV and dist < bestDist and isVisible(head) then
                        bestDist = dist
                        best = {Character = bot, Head = head}
                    end
                end
            end
        end
    end
    return best
end

-- === ESP DRAWING POOLS ===

-- Lines (ESP Line)
local linePool = {}
local function hideAllLines()
    for _, l in pairs(linePool) do l.Visible = false end
end
local function getLine(i)
    if not linePool[i] then
        linePool[i] = Drawing.new("Line")
        linePool[i].Thickness = 1.5
        linePool[i].Color = Color3.fromRGB(255, 255, 255)
        linePool[i].Transparency = 0.35
    end
    return linePool[i]
end

-- Boxes (4 lines per target)
local boxPool = {}
local function hideAllBoxes()
    for _, l in pairs(boxPool) do l.Visible = false end
end
local function getBoxLine(i)
    if not boxPool[i] then
        boxPool[i] = Drawing.new("Line")
        boxPool[i].Thickness = 1.2
        boxPool[i].Color = Color3.fromRGB(255, 255, 255)
        boxPool[i].Transparency = 0.3
    end
    return boxPool[i]
end

-- Health bars (2 lines per target: bg + fill)
local healthPool = {}
local function hideAllHealthBars()
    for _, l in pairs(healthPool) do l.Visible = false end
end
local function getHealthLine(i)
    if not healthPool[i] then
        healthPool[i] = Drawing.new("Line")
    end
    return healthPool[i]
end

-- Skeleton lines
local skeletonPool = {}
local function hideAllSkeletons()
    for _, l in pairs(skeletonPool) do l.Visible = false end
end
local function getSkeletonLine(i)
    if not skeletonPool[i] then
        skeletonPool[i] = Drawing.new("Line")
        skeletonPool[i].Thickness = 1.5
        skeletonPool[i].Color = Color3.fromRGB(255, 255, 255)
        skeletonPool[i].Transparency = 0.25
    end
    return skeletonPool[i]
end

local skeletonBones = {
    {"Head", "UpperTorso"}, {"UpperTorso", "LowerTorso"},
    {"UpperTorso", "LeftUpperArm"}, {"LeftUpperArm", "LeftLowerArm"}, {"LeftLowerArm", "LeftHand"},
    {"UpperTorso", "RightUpperArm"}, {"RightUpperArm", "RightLowerArm"}, {"RightLowerArm", "RightHand"},
    {"LowerTorso", "LeftUpperLeg"}, {"LeftUpperLeg", "LeftLowerLeg"}, {"LeftLowerLeg", "LeftFoot"},
    {"LowerTorso", "RightUpperLeg"}, {"RightUpperLeg", "RightLowerLeg"}, {"RightLowerLeg", "RightFoot"},
    {"Head", "Torso"}, {"Torso", "Left Arm"}, {"Torso", "Right Arm"},
    {"Torso", "Left Leg"}, {"Torso", "Right Leg"},
}

-- === COLLECT TARGETS ===
local function getAliveTargets()
    local targets = {}
    local cam = getCam()
    if not cam then return targets end

    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and not isSameTeam(plr) and plr.Character then
            local head = findHead(plr.Character)
            local hum = findHum(plr.Character)
            if head and hum and hum.Health > 0 then
                local pos, onScr = cam:WorldToViewportPoint(head.Position)
                if onScr then
                    table.insert(targets, {Char = plr.Character, Head = head, HeadPos = pos})
                end
            end
        end
    end

    for _, bot in pairs(botCache) do
        if bot then
            local head = findHead(bot)
            local hum = findHum(bot)
            if head and hum and hum.Health > 0 then
                local pos, onScr = cam:WorldToViewportPoint(head.Position)
                if onScr then
                    table.insert(targets, {Char = bot, Head = head, HeadPos = pos})
                end
            end
        end
    end
    return targets
end

local function getHealthColor(hp, maxHp)
    local pct = hp / math.max(maxHp, 1)
    if pct > 0.7 then
        return Color3.fromRGB(80, 230, 80)
    elseif pct > 0.4 then
        return Color3.fromRGB(230, 220, 60)
    elseif pct > 0.2 then
        return Color3.fromRGB(240, 140, 40)
    else
        return Color3.fromRGB(220, 50, 50)
    end
end

-- MAIN LOOP
local function hideExcess(pool, start)
    local i = start + 1
    while pool[i] do
        pool[i].Visible = false
        i = i + 1
    end
end

local frameCount = 0
RunService.RenderStepped:Connect(function()
    frameCount = frameCount + 1
    if frameCount % 60 == 0 then refreshBots() end

    local cam = getCam()
    if not cam then return end
    local vp = cam.ViewportSize

    FovCircle.Position = vp / 2
    FovCircle.Visible = Settings.ShowFOV

    if Settings.AimEnabled then
        local t = getClosestTarget()
        if t and t.Head then
            cam.CFrame = CFrame.new(cam.CFrame.Position, t.Head.Position)
        end
    end

    -- Collect targets once for all ESP
    local doLine = Settings.ESPLine
    local doBox = Settings.ESPBox
    local doHealth = Settings.ESPHealth
    local doSkeleton = Settings.ESPSkeleton

    if doLine or doBox or doHealth or doSkeleton then
        local targets = getAliveTargets()
        local li, bi, hi, si = 0, 0, 0, 0

        for _, t in pairs(targets) do
            local char = t.Char
            local head = t.Head
            local hPos = t.HeadPos
            local headScr = Vector2.new(hPos.X, hPos.Y)

            local feet = findFeet(char)
            local fPos = nil
            if feet then
                local fp, onS = cam:WorldToViewportPoint(feet.Position)
                if onS then
                    fPos = Vector2.new(fp.X, fp.Y)
                end
            end

            local hasFeet = fPos ~= nil

            -- ESP Line (all converge to top-center)
            if doLine then
                li = li + 1
                local l = getLine(li)
                l.From = headScr
                l.To = Vector2.new(vp.X / 2, 0)
                l.Visible = true
            end

            if hasFeet then
                local bh = fPos.Y - headScr.Y
                if bh < 5 then bh = 5 end
                local bw = bh * 0.5
                if bw < 20 then bw = 20 end
                local cx = headScr.X

                -- ESP Box
                if doBox then
                    local tl = Vector2.new(cx - bw / 2, headScr.Y)
                    local tr = Vector2.new(cx + bw / 2, headScr.Y)
                    local bl = Vector2.new(cx - bw / 2, fPos.Y)
                    local br = Vector2.new(cx + bw / 2, fPos.Y)

                    bi = bi + 1; local l1 = getBoxLine(bi); l1.From = tl; l1.To = tr; l1.Visible = true
                    bi = bi + 1; local l2 = getBoxLine(bi); l2.From = bl; l2.To = br; l2.Visible = true
                    bi = bi + 1; local l3 = getBoxLine(bi); l3.From = tl; l3.To = bl; l3.Visible = true
                    bi = bi + 1; local l4 = getBoxLine(bi); l4.From = tr; l4.To = br; l4.Visible = true
                end

                -- ESP Health
                if doHealth then
                    local barW = 5
                    local barX = cx + bw / 2 + 4
                    local hum = findHum(char)
                    local hp, maxHp = 100, 100
                    if hum then
                        hp = hum.Health
                        maxHp = hum.MaxHealth
                    end
                    local pct = math.clamp(hp / math.max(maxHp, 1), 0, 1)
                    local fillH = bh * pct

                    hi = hi + 1; local bg = getHealthLine(hi)
                    bg.From = Vector2.new(barX, headScr.Y)
                    bg.To = Vector2.new(barX, fPos.Y)
                    bg.Color = Color3.fromRGB(15, 15, 25)
                    bg.Thickness = barW
                    bg.Transparency = 0.2
                    bg.Visible = true

                    hi = hi + 1; local fill = getHealthLine(hi)
                    fill.From = Vector2.new(barX, fPos.Y)
                    fill.To = Vector2.new(barX, fPos.Y - fillH)
                    fill.Color = getHealthColor(hp, maxHp)
                    fill.Thickness = barW
                    fill.Transparency = 0
                    fill.Visible = true
                end
            end

            -- ESP Skeleton
            if doSkeleton then
                for _, bone in pairs(skeletonBones) do
                    local a = findPart(char, bone[1])
                    local b = findPart(char, bone[2])
                    if a and b then
                        local pa, onA = cam:WorldToViewportPoint(a.Position)
                        local pb, onB = cam:WorldToViewportPoint(b.Position)
                        if onA and onB then
                            si = si + 1
                            local ln = getSkeletonLine(si)
                            ln.From = Vector2.new(pa.X, pa.Y)
                            ln.To = Vector2.new(pb.X, pb.Y)
                            ln.Visible = true
                        end
                    end
                end
            end
        end

        hideExcess(linePool, li)
        hideExcess(boxPool, bi)
        hideExcess(healthPool, hi)
        hideExcess(skeletonPool, si)
    else
        hideAllLines()
        hideAllBoxes()
        hideAllHealthBars()
        hideAllSkeletons()
    end
end)

game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "XZ AIMBOT",
    Text = "Carregado com sucesso!",
    Duration = 2,
})
