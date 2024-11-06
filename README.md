repeat task.wait(5) until game:IsLoaded(3)
repeat task.wait() until game.Players
repeat task.wait() until game.Players.LocalPlayer
repeat task.wait() until game.Players.LocalPlayer:FindFirstChild("PlayerGui")
repeat task.wait() until game.Players.LocalPlayer.PlayerGui:FindFirstChild("Main");
UserSettings():GetService('UserGameSettings').MasterVolume = 0;
settings().Rendering.QualityLevel = 1;
game:GetService("StarterGui"):SetCoreGuiEnabled(Enum.CoreGuiType.Chat,false)
game:GetService("StarterGui"):SetCoreGuiEnabled(Enum.CoreGuiType.PlayerList,false)
game:GetService("Lighting").GlobalShadows = false
for key, object in pairs(workspace:GetDescendants()) do
    if object:IsA("Part") or object:IsA("UnionOperation") or object:IsA("MeshPart") then
        object.Material = Enum.Material.SmoothPlastic
    elseif  (object:IsA("Texture") or object:IsA("Explosion") or object:IsA("ColorCorrectionEffect") or 
                object:IsA("Atmosphere") or object:IsA("SunRaysEffect") or object:IsA("BlurEffect") or 
                object:IsA("RainyStone") or object:IsA("Weather")  or object:IsA("BloomEffect")
                or object:IsA("Lighting") or object:IsA("FogEnd") or object:IsA("DepthOfFieldEffect")) then
        object:Destroy()
    end
end
_G.WebHook = {
    ["Enabled"] = false, -- เปิดการใช้งาน
    ["Url"] = "", -- ลิ้งค์เว็บฮุก
    ["Delay"] = 60 -- วินาที
}
_G.Team = "Pirate" -- Marine / Pirate
_G.MainSettings = {
        ["EnabledHOP"] = true, -- เปิด HOP ( มันไม่มีอยู่ละใส่มาเท่ๆ )
        ['FPSBOOST'] = true, -- ภาพกาก
        ["FPSLOCKAMOUNT"] = 10, -- จำนวน FPS
        ['WhiteScreen'] = true, -- จอขาว
        ['CloseUI'] = true, -- ปิด Ui
        ["NotifycationExPRemove"] = true, -- ลบ ExP ที่เด้งตอนฆ่ามอน
        ['AFKCheck'] = 540, -- ถ้ายืนนิ่งเกินวิที่ตั้งมันจะรีเกม
        ["LockFragments"] = 20000, -- ล็อคเงินม่วง
        ["LockFruitsRaid"] = { -- ล็อคผลที่ไม่เอาไปลงดัน
            [1] = "Dough-Dough",
            [2] = "Dragon-Dragon",
            [3] = "Kitsune-Kitsune",
            [4] = "Leopard-Leopard",
        }
    }
_G.Fruits_Settings = { -- ตั้งค่าผล
    ['Main_Fruits'] = {}, -- ผลหลัก ถ้ายังไม่ใช่ค่าที่ตั้งมันจะกินจนกว่าจะใช่หรือซื้อ
    ['Select_Fruits'] = {} -- กินหรือซื้อตอนไม่มีผล
}
_G.Quests_Settings = { -- ตั้งค่าเควสหลักๆ
    ['Rainbow_Haki'] = false,
    ["MusketeerHat"] = false,
    ["PullLever"] = true,
    ['DoughQuests_Mirror'] = {
        ['Enabled'] = true,
        ['UseFruits'] = false,
    }        
}
_G.Races_Settings = { -- ตั้งค่าเผ่า
    ['Race'] = {
        ['EnabledEvo'] = true,
        ["v2"] = true,
        ["v3"] = true,
        ["Races_Lock"] = {
            ["Races"] = { -- Select Races U want
                ["Mink"] = true,
                ["Human"] = true,
                ["Fishman"] = false,
            },
            ["RerollsWhenFragments"] = 25000 -- Random Races When Your Fragments is >= Settings
        }
    }
}
_G.Settings_Melee = { -- หมัดที่จะทำ
    ['Superhuman'] = true,
    ['DeathStep'] = true,
    ['SharkmanKarate'] = true,
    ['ElectricClaw'] = true,
    ['DragonTalon'] = true,
    ['Godhuman'] = true,
}
_G.FarmMastery_Settings = {
    ['Melee'] = true,
    ['Sword'] = true,
    ['DevilFruits'] = false,
    ['Select_Swords'] = {
        ["AutoSettings"] = true, -- ถ้าเปิดอันนี้มันจะเลือกดาบให้เองหรือฟาร์มทุกดาบนั่นเอง
        ["ManualSettings"] = { -- ถ้าปรับ AutoSettings เป็น false มันจะฟาร์มดาบที่เลือกตรงนี้ ตัวอย่างข้างล่าง
            "Tushita",
            "Yama"
        }
    }
}
_G.SwordSettings = { -- ดาบที่จะทำ
    ['Saber'] = false,
    ["Pole"] = false,
    ['MidnightBlade'] = false,
    ['Shisui'] = false,
    ['Saddi'] = false,
    ['Wando'] = false,
    ['Yama'] = true,
    ['Rengoku'] = false,
    ['Canvander'] = false,
    ['BuddySword'] = false,
    ['TwinHooks'] = false,
    ['HallowScryte'] = false,
    ['TrueTripleKatana'] = false,
    ['CursedDualKatana'] = true,
}
_G.SharkAnchor_Settings = {
    ["Enabled_Farm"] = false,
}
_G.GunSettings = { -- ปืนที่จะทำ
    ['Kabucha'] = false,
    ['SerpentBow'] = false,
    ['SoulGuitar'] = true,
}
getgenv().Key = "MARU-PSR6D-KW6B2-5ZST-WCT0Z-7F8B"
getgenv().id = "1084122060307050586"
getgenv().Script_Mode = "Kaitun_Script"
loadstring(game:HttpGet("https://raw.githubusercontent.com/xshiba/MaruBitkub/main/Mobile.lua"))()
