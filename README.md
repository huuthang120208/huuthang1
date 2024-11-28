local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")

local PlaceId = 2753915549  -- PlaceId của Blox Fruits

-- Hàm lấy danh sách server và lọc theo thời gian tồn tại
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

-- Hàm kiểm tra xem số người chơi có thay đổi không
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

-- Hàm tham gia server
local function JoinLowPlayerServer()
    local cursor = nil
    local serverFound = false
    local checkedServers = 0  -- Đếm số lượng server đã kiểm tra

    while not serverFound and checkedServers < 100 do  -- Giới hạn số server được kiểm tra
        local servers = GetServerList(cursor)

        if servers and servers.data then
            for _, server in ipairs(servers.data) do
                -- Kiểm tra server có ít người chơi (1 hoặc 2 người)
                if server.playing <= 2 then
                    local initialPlayerCount = server.playing
                    print("Đang kiểm tra server ID: " .. server.id .. " với số người chơi ban đầu: " .. initialPlayerCount)

                    -- Kiểm tra trong 2 giây nếu số người chơi không thay đổi (giảm thời gian kiểm tra)
                    local timePassed = 0
                    while timePassed < 2 do
                        if CheckServerPlayerCount(server.id, initialPlayerCount) then
                            wait(0.1)  -- Giảm thời gian chờ giữa các lần kiểm tra
                            timePassed = timePassed + 0.1
                        else
                            print("Số người chơi đã thay đổi, bỏ qua server này.")
                            break
                        end
                    end

                    -- Nếu số người chơi không thay đổi sau 2 giây, tham gia vào server
                    if timePassed >= 2 then
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

        checkedServers = checkedServers + 1  -- Tăng số lượng server đã kiểm tra
        wait(0.5)  -- Giảm delay giữa các lần kiểm tra server
    end

    if not serverFound then
        print("Không tìm thấy server có ít người chơi.")
    end
end

-- Gọi hàm để tham gia server
JoinLowPlayerServer()
