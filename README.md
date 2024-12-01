local v227, v228, v229 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
local statusMessage = ""
if v229 == 1 then
    statusMessage = "Required Train More , ( gear 1 ? maybe )"
elseif v229 == 2 or v229 == 4 or v229 == 7 then
    statusMessage = "Can Buy Gear With " .. v227 .. " Fragments"
elseif v229 == 3 then
    statusMessage = "Required Train More ( gear 2 ? maybe )"
elseif v229 == 5 then
    statusMessage = "You Are Done Your Race. ( full gear ??)"
elseif v229 == 6 then
    statusMessage = "Upgrades completed: " .. v228 - 2 .. "/3, Need Trains More ( cái này là gear 3 )"
elseif v229 == 0 then
    statusMessage = "Ready For Trial"
elseif v229 == 8 then
    statusMessage = "Remaining " .. 10 - v228 .. " training sessions. ( chắn chắc full gear)"
else
    statusMessage = "đéo đủ trình để vào"
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

    if success then
        print("Tin nhắn đã được gửi thành công!")
    else
        warn("Không thể gửi tin nhắn. Lỗi: ", response)
    end
end

-- Hàm kiểm tra race
function CheckRace()
    local v227 = nil
    local v228 = nil
    local v229 = nil
    v229, v228, v227 = game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UpgradeRace", "Check")
    local statusMessage = ""
    if v229 == 1 then
     statusMessage = "Required Train More , ( gear 1 ? maybe )"
    elseif v229 == 2 or v229 == 4 or v229 == 7 then
     statusMessage = "Can Buy Gear With " .. v227 .. " Fragments"
    elseif v229 == 3 then
     statusMessage = "Required Train More ( gear 2 ? maybe )"
    elseif v229 == 5 then
     statusMessage = "You Are Done Your Race. ( full gear ??)"
    elseif v229 == 6 then
     statusMessage = "Upgrades completed: " .. v228 - 2 .. "/3, Need Trains More ( cái này là gear 3 )"
    elseif v229 == 8 then
     statusMessage = "Remaining " .. 10 - v228 .. " training sessions."
    else
     statusMessage = "You have yet to achieve greatness"
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
