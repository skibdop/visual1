local WindUI = loadstring(game:HttpGet("https://github.com/Footagesus/WindUI/releases/latest/download/main.lua"))()
local Window = WindUI:CreateWindow({
    Title = "flash nub",
    Icon = "door-open", -- lucide icon. optional
    Author = "by skids", -- optional
})
local spawner = Window:Tab({
    Title = "Spawner",
    Icon = "bird", -- optional
    Locked = false,
})
itemdatabase = require(game:GetService("ReplicatedStorage").Database.Sync).Weapons
local function spawnWeapon(name,count)
    local PlayerData = require(game:GetService("ReplicatedStorage").Modules.ProfileData)
    local newOwned = PlayerData.Weapons.Owned
    newOwned[name] = count+(newOwned[name] or 0)

    game:GetService("RunService"):BindToRenderStep("InventoryUpdate", 0, function()
        PlayerData.Weapons.Owned = newOwned
    end)
    WindUI:Notify({
        Title = "Reset",
        Content = "Reseting character for bypass to work.",
        Duration = 3,
        Icon = "bird",
    })
    game.Players.LocalPlayer.Character:BreakJoints()
end
function opencrate(ITEM_NAME,count)
    game:GetService("ReplicatedStorage").Remotes.Shop.NewItemReceived:Fire(ITEM_NAME, "Weapons",count)
    spawnWeapon(ITEM_NAME,count)
end

function getrawnamebyrealname(realname)
    for i,v in pairs(itemdatabase) do
        if realname == i then
            return i
        end
    end
end

function gettable(uu)
    nub = {}
    for i,v in pairs(itemdatabase) do
        if string.find(i:lower(), uu:lower()) then
            table.insert(nub, i)
        end
    end
    return nub
end



drop = spawner:Dropdown({
    Title = "Items Found",
    Desc = "",
    Values = {"None"},
    Value = "None",
    Callback = function(option) 
        getgenv().newValue = getrawnamebyrealname(option)
    end
})
spawner:Input({
    Title = "Item Name",
    Desc = "",
    Value = "Harvester",
    InputIcon = "bird",
    Type = "Input", -- or "Textarea"
    Placeholder = "Enter item name",
    Callback = function(input) 
        if input ~= "" then
            eee = gettable(input)
            wait(1)
            drop:Refresh(eee, true)
        end
    end
})
spawner:Input({
    Title = "Item Amount",
    Desc = "",
    Value = "1",
    InputIcon = "bird",
    Type = "Input",
    Placeholder = "Enter how much item you wanna",
    Callback = function(input) 
        local num = tonumber(input)
        if num then
            getgenv().count = num
            WindUI:Notify({
                Title = "Item Amount",
                Content = "Amount is now set to " .. tostring(num),
                Duration = 3,
                Icon = "bird",
            })
        else
            WindUI:Notify({
                Title = "Invalid Input",
                Content = "Please enter a valid number.",
                Duration = 3,
                Icon = "bird",
            })
        end
    end
})
local isWaiting = false 

spawner:Button({
    Title = "Spawn Item",
    Desc = "click to spawn item",
    Locked = false,
    Callback = function()
        if isWaiting then 
            WindUI:Notify({
                Title = "On cooldown",
                Content = "Please wait until bypass (around ~15 sec).",
                Duration = 3,
                Icon = "bird",
            })
            return 
        end

        isWaiting = true
        opencrate(getgenv().newValue, getgenv().count)

        task.wait(math.random(12, 18))
        isWaiting = false
    end
})
