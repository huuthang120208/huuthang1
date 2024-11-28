local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local PlaceId = 2753915549  -- PlaceId của game bạn cần

-- Hàm lấy danh sách server
local function GetServers(cursor)
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
        warn("Không thể lấy server:", result)
        if tostring(result):find("429") then
            print("Gặp lỗi 429 - Quá nhiều yêu cầu. Đợi lâu hơn rồi thử lại...")
            task.wait(10)  -- Tăng thời gian chờ khi gặp lỗi 429
        end
        return nil
    end
end

-- Hàm tìm và join vào server lâu đời nhất
local function JoinOldServer()
    local cursor = nil
    local oldestServerId = nil

    while true do
        local servers = GetServers(cursor)
        if not servers or not servers.data then
            print("Không thể lấy danh sách server, thử lại sau...")
            task.wait(5)  -- Chờ thêm thời gian để tránh spam yêu cầu
            cursor = nil  -- Quay lại từ đầu
            continue
        end

        -- Lưu lại server cuối cùng trong danh sách
        for _, server in ipairs(servers.data) do
            oldestServerId = server.id
        end

        if servers.nextPageCursor then
            cursor = servers.nextPageCursor  -- Chuyển sang trang kế tiếp
        else
            break  -- Hết trang, dừng lặp
        end

        task.wait(5)  -- Delay giữa các yêu cầu để tránh bị rate limit
    end

    -- Nếu tìm thấy server, tham gia vào server đó
    if oldestServerId then
        print("Đang kết nối tới server đã lâu có ID:", oldestServerId)
        TeleportService:TeleportToPlaceInstance(PlaceId, oldestServerId)
    else
        warn("Không tìm thấy server nào phù hợp.")
    end
end

-- Thực hiện join server lâu đời nhất
JoinOldServer()

