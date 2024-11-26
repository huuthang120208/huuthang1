
function scriptautov4()
    _G.Team = "Marine" -- Marine / Pirate
    _G.Settings_V4 = {
        ["LockTiers"] = 6, 
        ["Lever"] = true, 
        ["InVIPServ"] = true, 
        ["HelperNameList"] = { 
            "VN5ByLx5",
            "InezHarrellbdys38751"
        },
        ["V4FarmList"] = { 
            "gulli9bx",
            "downr94t",
            "subur9wz",
            "shini7xx",
            "poise5a2",
            "regul_7mt",
            "rival7x4",
            "ammon7ds",
            "unrea4zs",
            "blink2ci",
            "shame4ax",
            "negle8xs",
            "anthe4hx",
            "sank3dm",
            "reeme4us8",
            "after3co",
            "certi7kz",
            "handb70g",
            "mobst1mm",
            "money3xv"
        }
    }
    getgenv().Key = "MARU_V4-KRVC0Z7XJB7VYNW"
    getgenv().id = "1084122060307050586"
    loadstring(game:HttpGet("https://raw.githubusercontent.com/xshiba/MasterPClient/main/Loader.lua"))()  
end
if game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("CommF_"):InvokeServer("CheckTempleDoor") == false then
    do -- Team Script
        repeat 
            ChooseTeam = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("ChooseTeam",true)
            UIController = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("UIController",true)
            if UIController and ChooseTeam then
                if ChooseTeam.Visible then
                    for i,v in pairs(getgc()) do
                        if type(v) == "function" and getfenv(v).script == UIController then
                            local constant = getconstants(v)
                            pcall(function()
                                if constant[1] == "Marines" and #constant == 1 then
                                    v(shared.Team or "Marines")
                                end
                            end)
                        end
                    end
                end
            end
            wait(1)
        until game.Players.LocalPlayer.Team
        repeat wait() until game.Players.LocalPlayer.Character
    end
    wait(10)
    function HyperCahaya(Pos)
        Distance = (Pos.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude 
        Speed = Distance < 10 and 20 or Distance < 25 and 5050 or Distance < 50 and 2040 or Distance < 150 and 830 or Distance < 250 and 620 or Distance < 500 and 410 or Distance < 750 and 370 or 320 
        game:GetService("TweenService"):Create(
            game:GetService("Players").LocalPlayer.Character.HumanoidRootPart, 
            TweenInfo.new(Distance / Speed, Enum.EasingStyle.Linear), 
            {CFrame = Pos}
        ):Play() 
    end 
    HyperCahaya(CFrame.new(-15852.91796875, 485.5301818847656, 452.25537109375))
    wait(100)
    scriptautov4()
else
    scriptautov4()
end
