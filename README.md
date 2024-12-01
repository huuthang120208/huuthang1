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

-- Hàm kiểm tra trạng thái của Ancient One
function CheckAcientOneStatus()
    if not game.Players.LocalPlayer.Character:FindFirstChild("RaceTransformed") then
        return "You have yet to achieve greatness"
    end
    local v227, v228, v229 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
    if v229 == 1 then
        return "Required Train More"
    elseif v229 == 2 or v229 == 4 or v229 == 7 then
        return "Can Buy Gear With " .. v227 .. " Fragments"
    elseif v229 == 3 then
        return "Required Train More"
    elseif v229 == 5 then
        return "You Are Done Your Race."
    elseif v229 == 6 then
        return "Upgrades completed: " .. v228 - 2 .. "/3, Need Trains More"
    elseif v229 == 8 then
        return "Remaining " .. 10 - v228 .. " training sessions."
    else
        return "You have yet to achieve greatness"
    end
end

-- Hàm kiểm tra trạng thái race và gửi thông tin vào webhook
function CheckRace()
    local v113 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("Wenlocktoad", "1")
    local v111 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("Alchemist", "1")
    local playerName = game.Players.LocalPlayer.Name
    local race = game.Players.LocalPlayer.Data.Race.Value

    -- Kiểm tra trạng thái V4
    local v4Status = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
    local v4Gear, v4Tier = string.match(v4Status, "([0-9]+) ([0-9]+)")

    -- Lấy trạng thái của Ancient One
    local ancientStatus = CheckAcientOneStatus()

    -- Xử lý thông tin V4
    local raceInfo
    if tonumber(v4Gear) and tonumber(v4Tier) then
        raceInfo = race .. " V4, Gear: " .. v4Gear .. ", Tier: " .. v4Tier .. " (" .. ancientStatus .. ")"
        -- Gửi thông tin V4 cùng với trạng thái Ancient One vào webhook
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650928821768212/5nx2ScEE--inMxNOrk2RpAKsPKGR8YCLdrkN8C7JZT6xQkGfHmUQTY7hz1ftLeeepwqW", 
            "Player: " .. playerName .. "\n" .. raceInfo
        )
    elseif v113 == -2 then
        raceInfo = race .. " V3"
        -- Gửi thông tin V3 vào webhook
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650732901765120/JrwPBSLg2kj9NCl1GjYyGt5C8xgZqX5rzN_eXzkiWTtexoxfDTJ31dXquMc1NT6bfimA", 
            "Player: " .. playerName .. "\n" .. raceInfo
        )
    elseif v111 == -2 then
        raceInfo = race .. " V2"
        -- Gửi thông tin V2 vào webhook
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650557642768402/6jcRUy6tLXRLyo54I7QqtowCx8oU1VuLfDHGo1uF2BNAGa3-5Sm8I4XdV-TW_Yt_ZfR5", 
            "Player: " .. playerName .. "\n" .. raceInfo
        )
    else
        raceInfo = race .. " V1"
        -- Gửi thông tin V1 vào webhook
        SendToWebhook(
            "https://discord.com/api/webhooks/1312650557642768402/6jcRUy6tLXRLyo54I7QqtowCx8oU1VuLfDHGo1uF2BNAGa3-5Sm8I4XdV-TW_Yt_ZfR5", 
            "Player: " .. playerName .. "\n" .. raceInfo
        )
    end
end

-- Gọi hàm kiểm tra và gửi thông tin
CheckRace()
