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
        
        -- Kiểm tra và xử lý v4Status
        if v4Status then
            -- Tách 2 số đầu tiên từ v4Status, bất kể ký tự gì ngăn cách
            local gear, tier = string.match(v4Status, "([%d]+)[^%d]+([%d]+)")  -- Tìm 2 số liền nhau
            
            if gear and tier then
                local raceInfo = race .. " V4 (Gear: " .. gear .. ", Tier: " .. tier .. ")"
                SendToWebhook(
                    "https://discord.com/api/webhooks/1312650928821768212/5nx2ScEE--inMxNOrk2RpAKsPKGR8YCLdrkN8C7JZT6xQkGfHmUQTY7hz1ftLeeepwqW",
                    "Tên người chơi: " .. playerName .. "\nThông tin: " .. raceInfo
                )
                return raceInfo
            else
                warn("Lỗi: v4Status không có định dạng hợp lệ.")
            end
        else
            warn("Lỗi: Không thể lấy v4Status.")
        end
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
