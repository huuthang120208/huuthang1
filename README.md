local HttpService = game:GetService("HttpService")

-- Hàm gửi tin nhắn đến webhook
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

    if success then
        print("Tin nhắn đã được gửi thành công!")
    else
        warn("Không thể gửi tin nhắn. Lỗi: ", response)
    end
end

-- Hàm kiểm tra race
function CheckRace()
    local v113 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("Wenlocktoad", "1")
    local v111 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("Alchemist", "1")
    local playerName = game.Players.LocalPlayer.Name
    local race = game.Players.LocalPlayer.Data.Race.Value

    if game.Players.LocalPlayer.Character:FindFirstChild("RaceTransformed") then
        -- Kiểm tra trạng thái của V4
        local v4Status = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
        local raceInfo = race .. " V4 (Trạng thái: " .. tostring(v4Status) .. ")"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650928821768212/5nx2ScEE--inMxNOrk2RpAKsPKGR8YCLdrkN8C7JZT6xQkGfHmUQTY7hz1ftLeeepwqW",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo .. "\nTrạng thái V4: " .. tostring(v4Status) 
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
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo
        )
        return raceInfo
    else
        local raceInfo = race .. " V1"
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650557642768402/6jcRUy6tLXRLyo54I7QqtowCx8oU1VuLfDHGo1uF2BNAGa3-5Sm8I4XdV-TW_Yt_ZfR5",
            "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo
        )
        return raceInfo
    end
end

-- Thực thi hàm kiểm tra
CheckRace()
