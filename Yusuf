local P,R,U=game:GetService("Players"),game:GetService("RunService"),game:GetService("UserInputService")
local PFS=game:GetService("PathfindingService")
local LP=P.LocalPlayer 

-- Из конфигурации полностью удален Rock
local C={
    Farm=false,
    AntiAdmin=false,
    Esp=false,
    AntiPlayer=false,
    PlayerDist=100,
    Speed=24,
    Tree=false,
    Iron=false,
    Sulfur=false
}

local curRes,curDist=nil,math.huge 
local path=PFS:CreatePath({AgentRadius=2,AgentHeight=5,AgentCanJump=false,WaypointSpacing=3})
local waypoints,currentWaypointIndex={},1 
local espLabels = {}

local SG=Instance.new("ScreenGui",LP:FindFirstChild("PlayerGui") or game:GetService("CoreGui"))
SG.Name,SG.ResetOnSpawn="DeltaFarmMenu",false 
local F=Instance.new("Frame",SG)
F.Size,F.Position,F.BackgroundColor3,F.BorderSizePixel,F.Visible=UDim2.new(0,165,0,385),UDim2.new(0.4,0,0.3,0),Color3.fromRGB(15, 15, 20),0,false 
Instance.new("UICorner",F).CornerRadius = UDim.new(0, 8)
local TB=Instance.new("TextButton",SG)
TB.Size,TB.Position,TB.BackgroundColor3,TB.Text,TB.TextColor3,TB.TextSize,TB.Font=UDim2.new(0,65,0,30),UDim2.new(0,10,0.3,0),Color3.fromRGB(25, 25, 30),"ФАРМ",Color3.fromRGB(255,50,100),14,5 
Instance.new("UICorner",TB).CornerRadius = UDim.new(0, 6)
local function Drag(g,b)
    local t,s,p,m=false,nil,nil,false 
    g.InputBegan:Connect(function(i)
        if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then t,s,p,m=true,i.Position,g.Position,false end 
    end)
    g.InputChanged:Connect(function(i)
        if(i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch)and t then 
            local d=i.Position-s if d.Magnitude>5 then m=true end 
            g.Position=UDim2.new(p.X.Scale,p.X.Offset+d.X,p.Y.Scale,p.Y.Offset+d.Y)
        end 
    end)
    g.InputEnded:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then t=false end end)
    if b then g.MouseButton1Click:Connect(function() if not m then F.Visible=not F.Visible end end) end 
end 
Drag(F,false) Drag(TB,true)

local LL=Instance.new("UIListLayout",F) LL.Padding=UDim.new(0,5)
Instance.new("UIPadding",F).PaddingLeft,Instance.new("UIPadding",F).PaddingTop=UDim.new(0,10),UDim.new(0,10)
local function Btn(t)
    local b=Instance.new("TextButton",F)
    b.Size,b.BackgroundColor3,b.TextColor3,b.Font,b.TextSize,b.Text=UDim2.new(0,145,0,26),Color3.fromRGB(25, 25, 30),Color3.fromRGB(230, 230, 235),5,11,t 
    Instance.new("UICorner",b).CornerRadius = UDim.new(0, 5) return b 
end 

local B1=Btn("Фарм: ВЫКЛ")
local B3=Btn("Защита от админов: ВЫКЛ")
local B4=Btn("Esp рудов: ВЫКЛ")
local B5=Btn("Защита от игроков: ВЫКЛ")

local SFrame=Instance.new("Frame",F) SFrame.Size,SFrame.BackgroundColor3,SFrame.BorderSizePixel=UDim2.new(0,145,0,18),Color3.fromRGB(30, 30,35),0
Instance.new("UICorner",SFrame).CornerRadius = UDim.new(0, 4)
local SBar=Instance.new("Frame",SFrame) SBar.Size,SBar.BackgroundColor3,SBar.BorderSizePixel=UDim2.new(0.3,0,1,0),Color3.fromRGB(255,50,100),0
Instance.new("UICorner",SBar).CornerRadius = UDim.new(0, 4)
local SText=Instance.new("TextLabel",SFrame) SText.Size,SText.BackgroundTransparency,SText.TextColor3,SText.TextSize,SText.Font=UDim2.new(1,0,1,0),1,Color3.fromRGB(255,255,255),10,5

local BT=Btn("Дерево: ВЫКЛ")

-- Заменили кнопку Камня на красивую информационную текстовую табличку
local BR=Instance.new("TextLabel",F)
BR.Size,BR.BackgroundColor3,BR.TextColor3,BR.Font,BR.TextSize,BR.Text=UDim2.new(0,145,0,26),Color3.fromRGB(35, 20, 25),Color3.fromRGB(255, 100, 100),5,10,"фарм камня пока что не работает"
Instance.new("UICorner",BR).CornerRadius = UDim.new(0, 5)

local BI=Btn("Железо: ВЫКЛ")
local BS=Btn("Сера: ВЫКЛ")
local TX=Instance.new("TextLabel",F) TX.Size,TX.BackgroundTransparency,TX.Text,TX.TextColor3,TX.TextSize,TX.Font=UDim2.new(0,145,0,14),1,"создан был Юсуфом",Color3.fromRGB(130,130,140),9,5 
local function UI() 
    B1.Text, B1.BackgroundColor3 = "Фарм: "..(C.Farm and "ВКЛ" or "ВЫКЛ"), C.Farm and Color3.fromRGB(255,50,100) or Color3.fromRGB(25, 25, 30)
    B3.Text, B3.BackgroundColor3 = "Защита от админов: "..(C.AntiAdmin and "ВКЛ" or "ВЫКЛ"), C.AntiAdmin and Color3.fromRGB(255,50,100) or Color3.fromRGB(25, 25, 30)
    B4.Text, B4.BackgroundColor3 = "Esp рудов: "..(C.Esp and "ВКЛ" or "ВЫКЛ"), C.Esp and Color3.fromRGB(255,50,100) or Color3.fromRGB(25, 25, 30)
    B5.Text, B5.BackgroundColor3 = "Защита от игроков: "..(C.AntiPlayer and "ВКЛ" or "ВЫКЛ"), C.AntiPlayer and Color3.fromRGB(255,50,100) or Color3.fromRGB(25, 25, 30)
    
    BT.Text, BT.BackgroundColor3 = "Дерево: "..(C.Tree and "ВКЛ" or "ВЫКЛ"), C.Tree and Color3.fromRGB(50, 180, 90) or Color3.fromRGB(25, 25, 30)
    BI.Text, BI.BackgroundColor3 = "Железо: "..(C.Iron and "ВКЛ" or "ВЫКЛ"), C.Iron and Color3.fromRGB(210, 100, 40) or Color3.fromRGB(25, 25, 30)
    BS.Text, BS.BackgroundColor3 = "Сера: "..(C.Sulfur and "ВКЛ" or "ВЫКЛ"), C.Sulfur and Color3.fromRGB(210, 210, 30) or Color3.fromRGB(25, 25, 30)
    
    SText.Text="Дистанция: "..math.floor(C.PlayerDist).."м"
    SBar.Size=UDim2.new(C.PlayerDist / 700, 0, 1, 0)
end 

local moveSlider = false
local function updateSlider(input)
    local percentage = math.clamp((input.Position.X - SFrame.AbsolutePosition.X) / SFrame.AbsoluteSize.X, 0, 1)
    C.PlayerDist = percentage * 700 UI()
end
SFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then moveSlider = true updateSlider(input) end
end)
U.InputChanged:Connect(function(input)
    if moveSlider and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then updateSlider(input) end
end)
U.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then moveSlider = false end
end)

local function clearEsp() for _, gui in pairs(espLabels) do if gui then pcall(function() gui:Destroy() end) end end espLabels = {} end
local function stopClick() pcall(function() local vim=game:GetService("VirtualInputManager") vim:SendMouseButtonEvent(0,0,0,false,game,0) vim:SendTouchEvent(1,Enum.UserInputState.End,100,100) end) end 

B1.MouseButton1Click:Connect(function()
    C.Farm=not C.Farm UI()
    if not C.Farm and LP.Character and LP.Character:FindFirstChildOfClass("Humanoid") then 
        local hum=LP.Character:FindFirstChildOfClass("Humanoid")
        hum:Move(Vector3.new(),false) hum.Sit,hum.WalkSpeed=false,16 stopClick() curRes,waypoints=nil,{}
    end 
end)

BT.MouseButton1Click:Connect(function() C.Tree = not C.Tree UI() curRes,waypoints = nil,{} end)
BI.MouseButton1Click:Connect(function() C.Iron = not C.Iron UI() curRes,waypoints = nil,{} end)
BS.MouseButton1Click:Connect(function() C.Sulfur = not C.Sulfur UI() curRes,waypoints = nil,{} end)
B3.MouseButton1Click:Connect(function() C.AntiAdmin = not C.AntiAdmin UI() end)
B4.MouseButton1Click:Connect(function() C.Esp = not C.Esp UI() if not C.Esp then clearEsp() end end)
B5.MouseButton1Click:Connect(function() C.AntiPlayer = not C.AntiPlayer UI() end)
UI()
local function check(n)
    local s = string.lower(n)
    if C.Sulfur and (string.find(s, "sulf") or string.find(s, "brim")) then return "sulfur" end
    if C.Iron and (string.find(s, "iron") or string.find(s, "ore")) then return "iron" end
    if C.Tree and (string.find(s, "tree") or string.find(s, "wood")) then return "tree" end
    -- Поиск камня полностью убран из проверок
    return nil
end 

local dangerTargetPos = nil

task.spawn(function()
    while task.wait(0.1) do
        dangerTargetPos = nil
        if C.Farm then
            local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
            for _, player in pairs(P:GetPlayers()) do
                if player ~= LP and player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
                    local hum = player.Character:FindFirstChildOfClass("Humanoid")
                    local pHrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if C.AntiAdmin and hum.Health > 101 and pHrp then dangerTargetPos = pHrp.Position break end
                    if C.AntiPlayer and hrp and pHrp then
                        if (hrp.Position - pHrp.Position).Magnitude <= C.PlayerDist then dangerTargetPos = pHrp.Position break end
                    end
                end
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.5) do
        if C.Esp and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
            local myPos = LP.Character.HumanoidRootPart.Position
            for _, v in pairs(workspace:GetDescendants()) do
                if (v:IsA("Model") or v:IsA("BasePart")) then
                    local oreType = check(v.Name)
                    local p = v:IsA("Model") and (v.PrimaryPart or v:FindFirstChildWhichIsA("BasePart") or v:FindFirstChildOfClass("MeshPart")) or v
                    if oreType and p then
                        local dist = (myPos - p.Position).Magnitude
                        if dist <= 200 then
                            if not espLabels[p] then
                                local bgui = Instance.new("BillboardGui", p) bgui.Size = UDim2.new(0, 80, 0, 24) bgui.AlwaysOnTop = true
                                local txt = Instance.new("TextLabel", bgui) txt.Size, txt.BackgroundTransparency, txt.TextSize, txt.Font = UDim2.new(1, 0, 1, 0), 1, 9, Enum.Font.SourceSansBold
                                if oreType == "sulfur" then txt.Text, txt.TextColor3 = "[ Сера ]", Color3.fromRGB(210, 210, 30)
                                elseif oreType == "iron" then txt.Text, txt.TextColor3 = "[ Железо ]", Color3.fromRGB(240, 110, 50)
                                else txt.Text, txt.TextColor3 = "[ Дерево ]", Color3.fromRGB(60, 230, 60) end
                                espLabels[p] = bgui
                            end
                        else
                            if espLabels[p] then pcall(function() espLabels[p]:Destroy() end) espLabels[p] = nil end
                        end
                    end
                end
            end
            for part, gui in pairs(espLabels) do if not part or not part.Parent or not part:IsDescendantOf(workspace) then if gui then pcall(function() gui:Destroy() end) end espLabels[part] = nil end end
        end
    end
end)
task.spawn(function()
    while task.wait(0.4) do 
        if C.Farm and not dangerTargetPos and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then 
            local hrp = LP.Character.HumanoidRootPart local res, dist = nil, math.huge 
            for _, v in pairs(workspace:GetDescendants()) do 
                if (v:IsA("Model") or v:IsA("BasePart")) and check(v.Name) then 
                    local p = v:IsA("Model") and (v.PrimaryPart or v:FindFirstChildWhichIsA("BasePart") or v:FindFirstChildOfClass("MeshPart")) or v 
                    if p then local d = (hrp.Position - p.Position).Magnitude if d < dist then res, dist = p, d end end 
                end 
            end 
            if res and res ~= curRes then 
                curRes = res 
                pcall(function() 
                    path:ComputeAsync(hrp.Position, curRes.Position)
                    if path.Status == Enum.PathStatus.Success then waypoints = path:GetWaypoints() currentWaypointIndex = 1 else waypoints = {} end 
                end) 
            end 
        end 
    end 
end) 

task.spawn(function()
    while task.wait(0.08) do
        if C.Farm and not dangerTargetPos and curRes and curRes.Parent and curDist <= 6.5 then
            pcall(function()
                local t = LP.Character:FindFirstChildWhichIsA("Tool") or LP.Backpack:FindFirstChildWhichIsA("Tool")
                if t then
                    if t.Parent == LP.Backpack then t.Parent = LP.Character end
                    local vim = game:GetService("VirtualInputManager") local rx, ry = math.random(150, 250), math.random(150, 250)
                    vim:SendMouseButtonEvent(0, 0, 0, true, game, 0) vim:SendTouchEvent(1, Enum.UserInputState.Begin, rx, ry)
                    task.wait(0.02) vim:SendTouchEvent(1, Enum.UserInputState.End, rx, ry)
                end
            end)
        end
    end
end)

R.RenderStepped:Connect(function()
    if C.Farm and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") and LP.Character:FindFirstChildOfClass("Humanoid") then 
        local hum, hrp = LP.Character:FindFirstChildOfClass("Humanoid"), LP.Character.HumanoidRootPart 
        
        if dangerTargetPos then
            stopClick()
            if hum.Sit then hum.Sit = false end 
            hum.WalkSpeed = C.Speed 
            local runDirection = (hrp.Position - dangerTargetPos).Unit
            local targetRunPos = hrp.Position + (runDirection * 15) 
            hum:MoveTo(targetRunPos) 
            return
        end
        
        if curRes and curRes.Parent then 
            curDist = (hrp.Position - curRes.Position).Magnitude 
            if curDist <= 8 then workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, curRes.Position) end
            
            if curDist > 5.5 then 
                if hum.Sit then hum.Sit = false end hum.WalkSpeed = C.Speed 
                if curDist > 10 and #waypoints > 0 and currentWaypointIndex <= #waypoints then 
                    local wp = waypoints[currentWaypointIndex] hum:MoveTo(wp.Position) 
                    if (hrp.Position - wp.Position).Magnitude < 3 then currentWaypointIndex = currentWaypointIndex + 1 end 
                else hum:MoveTo(curRes.Position) end 
            elseif curDist > 3.0 then
                hum.WalkSpeed = 6 hum:MoveTo(curRes.Position)
                hrp.CFrame=CFrame.lookAt(hrp.Position,Vector3.new(curRes.Position.X,hrp.Position.Y,curRes.Position.Z))
                if not hum.Sit then hum.Sit = true end
            else 
                hum:Move(Vector3.new(), false) 
                hrp.CFrame=CFrame.lookAt(hrp.Position,Vector3.new(curRes.Position.X,hrp.Position.Y,curRes.Position.Z))
                if not hum.Sit then hum.Sit = true end 
            end 
        else 
            stopClick() if hum.Sit then hum.Sit = false end hum.WalkSpeed = 16 curRes, waypoints = nil, {} 
        end 
    end 
end)
