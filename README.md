local Pedrin Hub = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Players = game:GetService("Players")
local VirtualInputManager = game:GetService("VirtualInputManager")
local TweenService = game:GetService("TweenService")
local wrk = game:GetService("Workspace")
local rs = game:GetService("ReplicatedStorage")
local plr = Players.LocalPlayer
local hrp = plr.Character:FindFirstChild("HumanoidRootPart")

local sinks = workspace.World.Areas["Starter Resaurant"].Sinks
local selectedItem

local circleHandler = "Players." .. plr.Name .. ".PlayerGui.gameui.skill test.skillTestScript.circleHandler"
local secondCircleHandler = "Players." .. plr.Name .. ".PlayerGui.gameui.skill test.skillTestScript.secondCircleHandler"

local Window = Rayfield:CreateWindow({
    Name = "▶ Pedrin Hub ◀",
    Icon = 0,
    LoadingTitle = "Loading...",
    LoadingSubtitle = "by Pedrin031 🥵",
    Theme = "DarkBlue",
})

local Main = Window:CreateTab("Main")
local Misc = Window:CreateTab("Misc")
local Player = Window:CreateTab("Player")


local circleHandlerClick = nil
local secondCircleHandlerClick = nil

function troll(text, textColor, strokeColor)
    local player = Players.LocalPlayer
    if not player then return end
    
    local gui = player:FindFirstChild("PlayerGui")
    if not gui then return end
    
    local passiveGui = gui:FindFirstChild("passivegui")
    if not passiveGui then return end
    
    local infoPop = passiveGui:FindFirstChild("infopop")
    if not infoPop then return end
    
    local textTemp = infoPop:FindFirstChild("textTemp")
    if not textTemp then return end
    
    if not text then return end
    
    textColor = textColor or Color3.new(1, 1, 1)
    strokeColor = strokeColor or Color3.new(0, 0, 0)
    
    local newText = textTemp:Clone()
    newText.Text = text
    newText.TextColor3 = textColor
    newText.Size = UDim2.new(1, 0, 0, 0)
    newText.Visible = true
    newText.Parent = infoPop
    
    if newText:FindFirstChild("UIStroke") then
        newText.UIStroke.Color = strokeColor
    end
    
    TweenService:Create(newText, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.new(1, 0, 0.15, 0)
    }):Play()
    
    task.delay(4, function()
        TweenService:Create(newText, TweenInfo.new(1), { TextTransparency = 1 }):Play()
        if newText:FindFirstChild("UIStroke") then
            TweenService:Create(newText.UIStroke, TweenInfo.new(1), { Transparency = 1 }):Play()
        end
        game:GetService("Debris"):AddItem(newText, 1)
    end)
end

for _, func in next, getgc(true) do
    if typeof(func) == "function" then
        local funcName = debug.info(func, "n")
        local funcParent = debug.info(func, "s")

        if funcName == "inputBegan" and funcParent == circleHandler then
            circleHandlerClick = func
        elseif funcName == "inputBegan" and funcParent == secondCircleHandler then
            secondCircleHandlerClick = func
            break
        end
    end
end

local Asdaasdadsad = Main:CreateSection("Autofarm")

local AutoClick = Main:CreateToggle({
    Name = "Auto Click",
    CurrentValue = false,
    Callback = function(Value)
        _G.autoclick = Value
        if Value then
            local fakeInput = {
                UserInputType = Enum.UserInputType.MouseButton1,
                KeyCode = Enum.KeyCode.Unknown,
            }

            if not circleHandlerClick or not secondCircleHandlerClick then troll("Something went wrong...", Color3.new(1,0,0)) return end
            troll("Enabled Auto Clicker!", Color3.new(0,1,0))
            while _G.autoclick do
                if plr.PlayerGui.gameui["skill test"].Visible then
                    circleHandlerClick(fakeInput, false)
                    for i = 1, 3 do
                        secondCircleHandlerClick(fakeInput, false)
                    end
                end
                task.wait()
            end
            troll("Disabled Auto Clicker", Color3.new(1,0,0))
        end
    end,
})

local AutoSink = Main:CreateToggle({
    Name = "Auto Dish/Fish/Swish",
    CurrentValue = false,
    Callback = function(Value)
        _G.autosink = Value
        if Value then
            troll("Enabled Auto Dish!", Color3.new(0,1,0))
            while _G.autosink do
                VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
                task.wait(0.05)
                VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
                task.wait(0.1)
            end
            troll("Disabled Auto Dish", Color3.new(1,0,0))
        end
    end,
})

local AutoSell = Main:CreateToggle({
    Name = "Auto Sell",
    CurrentValue = false,
    Callback = function(Value)
        _G.autosell = Value
        
        if Value then
            troll("Enabled Auto Sell!", Color3.new(0,1,0))
            while _G.autosell do
                rs.events.sell:InvokeServer(true)
                task.wait()
            end
            troll("Disabled Auto Sell", Color3.new(1,0,0))
        end
    end,
})

local AntiFail = Main:CreateButton({
    Name = "Never Fail",
    Callback = function()
        for _, func in next, getgc(true) do
            if typeof(func) == "function" then
                local funcName = debug.info(func, "n")
                if funcName == "checkValidClick" then
                    troll("Never Fail has been Enabled", Color3.new(0,1,0))
                    hookfunction(func, function()
                        return true
                    end)
                end
            end
        end
    end,
})

local Asdaasdadsad = Main:CreateSection("Other shit")

local InfMoney = Main:CreateToggle({
    Name = "Infinite Money",
    CurrentValue = false,
    Callback = function(Value)
        _G.infmoney = Value
        
        if Value then
            local rewardGUI = plr:WaitForChild("PlayerGui"):WaitForChild("passivegui"):WaitForChild("dailyRewards"):WaitForChild("page")

            local bahaska = 1

            for _, reward in ipairs(rewardGUI:GetChildren()) do
                if reward:IsA("ImageButton") then
                    local claimed = reward:FindFirstChild("claim")
                    if claimed and claimed.Text == "CLAIMED" then
                        bahaska = bahaska + 1
                    elseif claimed and claimed.Text == "CLAIM" then
                        bahaska = bahaska - 1
                        break
                    end
                end
            end

            local rewards = {
                [1] = "$100",
                [2] = "$5,000",
                [3] = "$10,000",
                [4] = "$20,000",
                [5] = "$30,000",
                [6] = "$50,000",
                [7] = "$75,000"
            }

            local reward = rewards[bahaska]

            if reward == nil then troll("Try opening the daily rewards menu!", Color3.new(1,0,0)) return end

            troll("Enabled Infinite Money!", Color3.new(0,1,0))
            troll("Your daily reward is: " .. reward, Color3.new(0,1,0))
            while _G.infmoney do
                rs.events.dailyReward:InvokeServer(reward)
                task.wait()
            end
            troll("Disabled Infinite Money", Color3.new(1,0,0))
        end
    end,
})

local infoPopup = plr:WaitForChild("PlayerGui"):WaitForChild("passivegui"):WaitForChild("infopop")

infoPopup.ChildAdded:Connect(function(child)
    if child:IsA("TextLabel") or child:IsA("TextButton") then
        if child.Text:lower():sub(1, 6) == "reward" and _G.infmoney then
            child.Visible = false
        end
    end
end)

local AutoClaim = Main:CreateToggle({
    Name = "Auto Claim Playtime",
    CurrentValue = false,
    Callback = function(Value)
        _G.autoclaim = Value

        if Value then
            troll("Enabled Auto Claim Playtime!", Color3.new(0,1,0))
            while _G.autoclaim do
                local rewardsGui = plr.PlayerGui:FindFirstChild("passivegui")
                if rewardsGui then
                    local playtimeRewards = rewardsGui:FindFirstChild("playtimeRewards")
                    if playtimeRewards then
                        for _, button in ipairs(playtimeRewards.page:GetChildren()) do
                            if button:IsA("GuiButton") and button.time.Text == "CLAIM" then
                                rs.events.playtimeReward:InvokeServer(string.lower(button.reward.Text))
                            end
                        end
                    end
                end
                task.wait(1)
            end
            troll("Disabled Auto Claim Playtime", Color3.new(1,0,0))
        end
    end,
})

local AppraiseItem = Main:CreateButton({
    Name = "Appraise Item",
    Callback = function()
        local heldTool = plr.Character:FindFirstChildOfClass("Tool")
        if not heldTool then print("Hold an item to appraise!") return end
        
        rs.events.appraiseItem:InvokeServer(heldTool)
    end,
})

local Section = Main:CreateSection("NPC")

local AutoFeedFilly = Main:CreateToggle({
    Name = "Auto Feed Filly",
    CurrentValue = false,
    Callback = function(Value)
        _G.autofeedfilly = Value
        
        if Value then
            troll("Enabled Auto Feed Filly!", Color3.new(0,1,0))
            while _G.autofeedfilly do
                rs.events.npcEvents.eat:InvokeServer(true)
                task.wait()
            end
            troll("Disabled Auto Feed Filly", Color3.new(1,0,0))
        end
    end,
})

local AutoFeedNeedySteve = Main:CreateToggle({
    Name = "Auto Feed Needy Steve",
    CurrentValue = false,
    Callback = function(Value)
        _G.autofeedneedysteve = Value
        
        if Value then
            troll("Enabled Auto Feed Needy Steve!", Color3.new(0,1,0))
            while _G.autofeedneedysteve do
                rs.events.npcEvents.giveGoldenSpork:InvokeServer(true)
                task.wait()
            end
            troll("Disabled Auto Feed Needy Steve", Color3.new(1,0,0))
        end
    end,
})

local AutoFeedJoey = Main:CreateToggle({
    Name = "Auto Feed Joey",
    CurrentValue = false,
    Callback = function(Value)
        _G.autofeedjoey = Value
        
        if Value then
            troll("Enabled Auto Feed Joey!", Color3.new(0,1,0))
            while _G.autofeedjoey do
                rs.events.npcEvents.largejoeycheck:InvokeServer(true)
                task.wait()
            end
            troll("Disabled Auto Feed Joey", Color3.new(1,0,0))
        end
    end,
})

local Section = Misc:CreateSection("Misc")

local AntiAfk = Misc:CreateButton({
    Name = "Anti Afk",
    Callback = function()
        local vu = game:GetService("VirtualUser")
        game:GetService("Players").LocalPlayer.Idled:connect(function()
        vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        wait(1)
        vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        end)
        troll("Enabled Anti Afk!", Color3.new(0,1,0))
    end,
})

local ForceSwimToggle = Misc:CreateToggle({
    Name = "Force Swim",
    CurrentValue = false,
    Callback = function(Value)
        local swimFolder = wrk:FindFirstChild("World"):FindFirstChild("Map"):FindFirstChild("Nature"):FindFirstChild("Water"):FindFirstChild("Swim")

        local existingPart = swimFolder:FindFirstChild("FunnyPart")

        if Value then
            if not hrp then return end
            if existingPart then return end

            troll("Enabled Force Swim!", Color3.new(0,1,0))

            local part = Instance.new("Part")
            part.Name = "FunnyPart"
            part.Size = Vector3.new(1, 1, 1)
            part.Transparency = 1
            part.Anchored = false
            part.CanCollide = false
            part.Massless = true

            local weld = Instance.new("WeldConstraint")
            weld.Part0 = part
            weld.Part1 = hrp
            weld.Parent = part

            part.CFrame = hrp.CFrame

            part.Parent = swimFolder

        else
            if existingPart then
                troll("Disabled Force Swim", Color3.new(1,0,0))
                existingPart:Destroy()
            end
        end
    end,
})

local SwimmingScriptToggle = Misc:CreateToggle({
    Name = "Swimming Script",
    CurrentValue = true,
    Callback = function(Value)
        plr.Character:FindFirstChild("swimmingScript").Enabled = Value
    end,
})

local RemovePopups = Misc:CreateButton({
    Name = "Remove Popups",
    Callback = function()
        troll("Removed Advertise Popups!", Color3.new(0,1,0))
        plr.PlayerGui.passivegui:FindFirstChild("doubleLuckPopup"):Destroy()
        plr.PlayerGui.passivegui:FindFirstChild("fastDishingPopup"):Destroy()
    end,
})

local BarrierRemove = Misc:CreateButton({
    Name = "Remove Barriers",
    Callback = function()
        local barriers = wrk:FindFirstChild("World"):FindFirstChild("Walls")
        if not barriers then return end
        troll("Removed Barriers", Color3.new(0,1,0))
        barriers:Destroy()
    end,
})

local Section = Misc:CreateSection("Animations")

local emoteAnim = plr.Character.Humanoid.Animator.digin
local danceSpeed = 1
local currentTrack

local SpeedSlider = Misc:CreateSlider({
    Name = "Dance Speed",
    Range = {0.5, 3},
    Increment = 0.1,
    CurrentValue = 1,
    Callback = function(Value)
        danceSpeed = Value
        if currentTrack then
            currentTrack:AdjustSpeed(danceSpeed)
        end
    end,
})

local AnimButton = Misc:CreateButton({
    Name = "[FE] Play Anim",
    Callback = function()
        if currentTrack then
            currentTrack:Stop()
            currentTrack = nil
            return
        end

        if emoteAnim then
            local humanoid = plr.Character and plr.Character:FindFirstChild("Humanoid")
            if humanoid then
                local animator = humanoid:FindFirstChild("Animator") or humanoid:FindFirstChildOfClass("Animator")
                if not animator then
                    animator = Instance.new("Animator")
                    animator.Parent = humanoid
                end

                local animation = Instance.new("Animation")
                animation.AnimationId = emoteAnim.AnimationId

                currentTrack = animator:LoadAnimation(animation)
                currentTrack.Looped = true
                currentTrack:Play(1, 9999999, danceSpeed)
                currentTrack:AdjustSpeed(danceSpeed)
            end
        else
            Rayfield:Notify({Title = "Error", Content = "Emote not found!"})
        end
    end,
})

local AnimDropdown = Misc:CreateDropdown({
    Name = "Select Anim",
    Options = {"digin", "keepdiggin"},
    CurrentOption = "digin",
    MultipleOptions = false,
    Callback = function(Option)
        local emote = typeof(Option) == "table" and Option[1] or Option
        emoteAnim = plr.Character.Humanoid.Animator:FindFirstChild(emote)

        if currentTrack and emoteAnim then
            local humanoid = plr.Character and plr.Character:FindFirstChild("Humanoid")
            if humanoid then
                local animator = humanoid:FindFirstChild("Animator") or humanoid:FindFirstChildOfClass("Animator")
                if animator then
                    local animation = Instance.new("Animation")
                    animation.AnimationId = emoteAnim.AnimationId

                    currentTrack:Stop()
                    currentTrack = animator:LoadAnimation(animation)
                    currentTrack.Looped = true
                    currentTrack:Play(0.1, 9999999, danceSpeed)
                end
            end
        end
    end,
})

local Section1 = Player:CreateSection("Player")

local walkSpeedSlider = Player:CreateSlider({
    Name = "WalkSpeed",
    Range = {16, 320},
    Increment = 1,
    CurrentValue = 16,
    Callback = function(Value)
        plr.Character.Humanoid.WalkSpeed = Value
    end,
})

local jumpPowerSlider = Player:CreateSlider({
    Name = "JumpPower",
    Range = {50, 1000},
    Increment = 10,
    CurrentValue = 50,
    Callback = function(Value)
        plr.Character.Humanoid.JumpPower = Value
        plr.Character.Humanoid.UseJumpPower = true
    end,
})

local sitToggle = Player:CreateToggle({
    Name = "Sit",
    CurrentValue = false,
    Callback = function(Value)
        plr.Character.Humanoid.Sit = Value
    end,
})
