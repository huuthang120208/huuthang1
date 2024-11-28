local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local PlaceId = 2753915549 
local function GetServerList(cursor)
    local url = "https://games.roblox.com/v1/games/" .. PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"
    if cursor then
        url = url .. "&cursor=" .. cursor
    end

    local success, result = pcall(function()
        return HttpService:JSONDecode(game:HttpGet(url))
    end)

    if success then
        return result
    else
        warn("Không thể lấy danh sách server:", result)
        return nil
    end
end
local function CheckServerPlayerCount(serverId, initialPlayerCount)
    local url = "https://games.roblox.com/v1/servers/" .. serverId
    local success, result = pcall(function()
        return HttpService:JSONDecode(game:HttpGet(url))
    end)

    if success and result.data then
        return result.data.playing == initialPlayerCount
    else
        return false
    end
end
local function JoinLowPlayerServer()
    local cursor = nil
    local serverFound = false
    while not serverFound do
        local servers = GetServerList(cursor)
        if servers and servers.data then
            for _, server in ipairs(servers.data) do
                if server.playing <= 2 then
                    local initialPlayerCount = server.playing
                    print("Đang kiểm tra server ID: " .. server.id .. " với số người chơi ban đầu: " .. initialPlayerCount)
                    local timePassed = 0
                    while timePassed < 10 do
                        if CheckServerPlayerCount(server.id, initialPlayerCount) then
                            wait(1)
                            timePassed = timePassed + 1
                        else
                            print("Số người chơi đã thay đổi, bỏ qua server này.")
                            break
                        end
                    end
                    if timePassed >= 10 then
                        print("Đang tham gia vào server ID: " .. server.id)
                        TeleportService:TeleportToPlaceInstance(PlaceId, server.id, Players.LocalPlayer)
                        serverFound = true
                        break
                    end
                end
            end
        end
        if servers and servers.nextPageCursor then
            cursor = servers.nextPageCursor
        else
            break
        end
        wait(1) 
    end

    if not serverFound then
        print("Không tìm thấy server có ít người chơi.")
    end
end
JoinLowPlayerServer()
