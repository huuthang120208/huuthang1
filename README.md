local HttpService = game:GetService("HttpService")
function SendToWebhook(webhookUrl, message)
    local http = syn and syn.request or http_request or request or nil
    local payload = {
        content = message, 
        username = "Hữu Thắng hiện lên và nói"
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
end
function CheckRace()
    local v227 = nil
    local v228 = nil
    local v229 = nil
    v229, v228, v227 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
    local statusMessage = ""
    if v229 == 1 then
     statusMessage = "Required Train More , ( gear 1 )"
    elseif v229 == 0 then 
     statusMessage = "Ready for Trial"
    elseif v229 == 2 or v229 == 4 or v229 == 7 then
     statusMessage = "Can Buy Gear With " .. v227 .. " Fragments (Status 2 : gear 1 , Status 4 : gear 2 , Status 7 : full gear  )"
    elseif v229 == 3 then
     statusMessage = "Required Train More ( gear 2 )"
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
    local fragment = game.Players.LocalPlayer.Data.Fragments.Value
    if fragment < 20000 then
        thongbao = "số fragment : " .. tostring(fragment) .. "  ( chưa đủ 20k fragment ) @everyone"
    else
        thongbao = "số fragment : " .. tostring(fragment) .. "  ( Đủ 20k fragment )"
    end
    if game.Players.LocalPlayer.Character:FindFirstChild("RaceTransformed") then
        local v4Status = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
        local raceInfo = race .. " V4 (Trạng thái: " .. tostring(v4Status) .. ")"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650928821768212/5nx2ScEE--inMxNOrk2RpAKsPKGR8YCLdrkN8C7JZT6xQkGfHmUQTY7hz1ftLeeepwqW",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo .. "\nTrạng thái V4: " .. tostring(v4Status) .. "\nTrạng thái ancient quests : " ..  statusMessage .. "\n".. thongbao
        )
        return raceInfo
    elseif v113 == -2 then
        local raceInfo = race .. " V3"
        SendToWebhook(
            "https://discord.com/api/webhooks/1313208538041946233/JZ8xcremwnzrrefPC7xTi9H0f45dM6qQ74ScolrBt6dJFHyai2pRYi27YclHIQHgFprl",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo .. "\n" .. thongbao
        )
        return raceInfo
    elseif v111 == -2 then
        local raceInfo = race .. " V2"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650557642768402/6jcRUy6tLXRLyo54I7QqtowCx8oU1VuLfDHGo1uF2BNAGa3-5Sm8I4XdV-TW_Yt_ZfR5",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo .. "\n" .. thongbao .. "\n@everyone"
        )
        return raceInfo
    else
        local raceInfo = race .. " V1"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650557642768402/6jcRUy6tLXRLyo54I7QqtowCx8oU1VuLfDHGo1uF2BNAGa3-5Sm8I4XdV-TW_Yt_ZfR5",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo .. "\n" .. thongbao .. "\n@everyone"
        )
        return raceInfo
    end
end

while true do     
    CheckRace()
    wait(500)
end
