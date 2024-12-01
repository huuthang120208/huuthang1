
function scriptautov4()
    _G.Team = "Marine" -- Marine / Pirate
    _G.Settings_V4 = {
        ["LockTiers"] = 6, 
        ["Lever"] = true, 
        ["InVIPServ"] = true, 
        ["HelperNameList"] = { 
            "huynhbahuuthang1",
            "InezHarrellbdys38751"
        },
        ["V4FarmList"] = { 
            "elednDaB_665",
            "bunt_4zg",
            "jitte5iv",
            "pater_33d",
            "worst3mw",
            "causi8xz",
            "uptow2ih",
            "excre_87g",
            "busil9kk",
            "burly_6no",
            "sever_1xb",
            "gorge_18k",
            "dubio_85a",
            "juven8ka",
            "stink8ix",
            "owenXufFN8734",
            "backb5mm",
            "anagr1sz",
            "hut3xd",
            "relen89d"
        }
    }
    getgenv().Key = "MARU_V4-KRVC0Z7XJB7VYNW"
    getgenv().id = "1084122060307050586"
    loadstring(game:HttpGet("https://raw.githubusercontent.com/xshiba/MasterPClient/main/Loader.lua"))()  
end
function jointeam()
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
end
jointeam()
while true do
    if game.PlaceId == 7449423635 then
        print("Đã tới PlaceId 7449423635, dừng lại.")
        break
    elseif game.PlaceId == 2753915549 or game.PlaceId == 4442272183 then

        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelZou")
    end
    wait(1) -- Đợi 1 giây trước khi kiểm tra lại
end
if game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("CommF_"):InvokeServer("CheckTempleDoor") == false then
    function HyperCahaya(Pos)
        Distance = (Pos.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude 
        Speed = Distance < 10 and 20 or Distance < 25 and 5050 or Distance < 50 and 2040 or Distance < 150 and 830 or Distance < 250 and 620 or Distance < 500 and 410 or Distance < 750 and 370 or 320 
        game:GetService("TweenService"):Create(
            game:GetService("Players").LocalPlayer.Character.HumanoidRootPart, 
            TweenInfo.new(Distance / Speed, Enum.EasingStyle.Linear), 
            {CFrame = Pos} -- Escape dấu ngoặc nhọn
        ):Play() 
    end 
    HyperCahaya(CFrame.new(-15852.91796875, 485.5301818847656, 452.25537109375))
    wait(50)
    scriptautov4()
else
    scriptautov4()
end
local HttpService = game:GetService("HttpService")
-- Hàm gửi tin nhắn đến webhook
function SendToWebhook(webhookUrl, message)
    local http = syn and syn.request or http_request or request or nil
    if not http then
        warn("Executor không hỗ trợ HTTP requests.")
        return
    end
    local payload = {
        content = message, -- Nội dung tin nhắn
        username = "Race Info Bot"
    }
    local success, response = pcall(function()
        return http({
            Url = webhookUrl,
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json"
            },
            Body = HttpService:JSONEncode(payload)
        })
    end)
function CheckRace()
    local v227 = nil
    local v228 = nil
    local v229 = nil
    v229, v228, v227 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
    local statusMessage = ""
    if v229 == 1 then
     statusMessage = "Required Train More , ( gear ??? need more test )"
    elseif v229 == 2 or v229 == 4 or v229 == 7 then
     statusMessage = "Can Buy Gear With " .. v227 .. " Fragments ( gear 2 )"
    elseif v229 == 3 then
     statusMessage = "Required Train More ( gear ??? need more test )"
    elseif v229 == 5 then
     statusMessage = "You Are Done Your Race. ( full gear t10 )"
    elseif v229 == 6 then
     statusMessage = "Upgrades completed: " .. v228 - 2 .. "/3, Need Trains More ( gear 3 )"
    elseif v229 == 8 then
     statusMessage = "Remaining " .. 10 - v228 .. " training sessions. ( full gear )" 
    else
     statusMessage = "(Đéo đủ trình độ)" 
    end
    local v113 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("Wenlocktoad", "1")
    local v111 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("Alchemist", "1")
    local playerName = game.Players.LocalPlayer.Name
    local race = game.Players.LocalPlayer.Data.Race.Value

    if game.Players.LocalPlayer.Character:FindFirstChild("RaceTransformed") then
        local v4Status = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
        local raceInfo = race .. " V4 (Trạng thái: " .. tostring(v4Status) .. ")"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650928821768212/5nx2ScEE--inMxNOrk2RpAKsPKGR8YCLdrkN8C7JZT6xQkGfHmUQTY7hz1ftLeeepwqW",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo .. "\nTrạng thái V4: " .. tostring(v4Status) .. "\nTrạng thái ancient quests : " ..  statusMessage
        )
        return raceInfo
    elseif v113 == -2 then
        local raceInfo = race .. " V3"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650732901765120/JrwPBSLg2kj9NCl1GjYyGt5C8xgZqX5rzN_eXzkiWTtexoxfDTJ31dXquMc1NT6bfimA",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo 
        )
        return raceInfo
    elseif v111 == -2 then
        local raceInfo = race .. " V2"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650557642768402/6jcRUy6tLXRLyo54I7QqtowCx8oU1VuLfDHGo1uF2BNAGa3-5Sm8I4XdV-TW_Yt_ZfR5",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo .. "\n@everyone"
        )
        return raceInfo
    else
        local raceInfo = race .. " V1"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650557642768402/6jcRUy6tLXRLyo54I7QqtowCx8oU1VuLfDHGo1uF2BNAGa3-5Sm8I4XdV-TW_Yt_ZfR5",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo .. "\n@everyone"
        )
        return raceInfo
    end
end

while true do     
    CheckRace()
    wait(500)
end
