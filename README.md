local oldGui = game.Players.LocalPlayer.PlayerGui:FindFirstChild("FarmMoneyGui")
if oldGui then
    oldGui:Destroy()
end

_G.FarmActive = false

local sg = Instance.new("ScreenGui")
sg.Name = "FarmMoneyGui"
sg.Parent = game.Players.LocalPlayer.PlayerGui
sg.ResetOnSpawn = false

local mf = Instance.new("Frame")
mf.Parent = sg
mf.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
mf.Position = UDim2.new(0.05, 0, 0.4, 0)
mf.Size = UDim2.new(0, 180, 0, 100)
mf.Active = true

local tt = Instance.new("TextLabel")
tt.Parent = mf
tt.BackgroundTransparency = 1
tt.Size = UDim2.new(1, 0, 0.4, 0)
tt.Text = "Farm Money"
tt.TextColor3 = Color3.fromRGB(255, 255, 255)
tt.TextSize = 18

local tb = Instance.new("TextButton")
tb.Parent = mf
tb.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
tb.Position = UDim2.new(0.1, 0, 0.45, 0)
tb.Size = UDim2.new(0.8, 0, 0.45, 0)
tb.Text = "Status: OFF"
tb.TextColor3 = Color3.fromRGB(255, 255, 255)
tb.TextSize = 16

task.spawn(function()
    while true do
        task.wait(0.5)
        if _G.FarmActive == true then
            local part = game.Workspace:FindFirstChild("TeleportOut", true)
            local plyr = game.Players.LocalPlayer
            if part and plyr.Character and plyr.Character:FindFirstChild("HumanoidRootPart") then
                local prompt = part:FindFirstChildOfClass("ProximityPrompt")
                if prompt then
                    plyr.Character.HumanoidRootPart.CFrame = part.CFrame * CFrame.new(0, 2, 0)
                    task.wait(0.1)
                    
                    if fireproximityprompt then
                        fireproximityprompt(prompt)
                    else
                        prompt:InputHoldBegin()
                        task.wait(0.65)
                        prompt:InputHoldEnd()
                    end
                end
            end
        end
    end
end)

tb.MouseButton1Click:Connect(function()
    _G.FarmActive = not _G.FarmActive
    if _G.FarmActive then
        tb.Text = "Status: ON"
        tb.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
    else
        tb.Text = "Status: OFF"
        tb.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    end
end)
