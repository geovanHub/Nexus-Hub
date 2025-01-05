if game.PlaceId == 16896041188 then

    -- Load Orion
    local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

    -- Main Window
    local Window = OrionLib:MakeWindow({Name = "Nexus Hub", HidePremium = false, SaveConfig = true, ConfigFolder = "Zetra"})

    -- "Player" Tab
    local PlayerTab = Window:MakeTab({
        Name = "Player",
        Icon = "rbxassetid://4483345998", -- Player Icon
        PremiumOnly = false
    })

    -- Adding Buttons to Player Tab
    PlayerTab:AddButton({
        Name = "Esp (player's)",
        Callback = function()
            EspPlayers() -- Chamando a função EspPlayers
            print("ESP Activated!")
        end    
    })

    PlayerTab:AddButton({
        Name = "Noclip",
        Callback = function()
            ToggleNoclip() -- Chamando a função ToggleNoclip
            print("Noclip " .. (NoclipStatus and "Activated" or "Deactivated"))
        end    
    })

    PlayerTab:AddButton({
        Name = "Set Walk Speed",
        Callback = function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/Prexry/Fenx-X/main/speed.lua"))()
            print("Walk Speed Script Executed!")
        end
    })

    PlayerTab:AddButton({
        Name = "Shift Lock",
        Callback = function()
            loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Compilador-de-script-de-shift-lock-para-celular-e-mobilador-23855"))()
            print("Shift Lock Script Executed!")
        end
    })

    -- "Char • Outfit's" Tab
    local OutfitTab = Window:MakeTab({
        Name = "Char • Outfit's",
        Icon = "rbxassetid://4483345998", -- Ícone do botão (substitua se quiser)
        PremiumOnly = false
    })

    -- Adding Button to "Char • Outfit's" Tab
    OutfitTab:AddButton({
        Name = "Avatar Outfit's",
        Callback = function()
            loadstring(game:HttpGet('https://pastefy.app/S7xNJSXX/raw'))() -- Executa o script fornecido
            execute("Script18")
            print("Avatar Outfit's Script Executed!")
        end    
    })

    -- Adding Button "FE Free Emote"
    OutfitTab:AddButton({
        Name = "FE Free Emote",
        Callback = function()
            loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/Gi7331/scripts/main/Emote.lua"))()
            print("FE Free Emote Script Executed!")
        end    
    })

end

-- Noclip Function
local NoclipStatus = false
local function ToggleNoclip()
    NoclipStatus = not NoclipStatus
    local Character = game.Players.LocalPlayer.Character
    if Character and Character:FindFirstChild("HumanoidRootPart") then
        local humanoid = Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.PlatformStand = NoclipStatus
            for _, part in pairs(Character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = not NoclipStatus
                end
            end
        end
    end
end

-- ESP Function
function EspPlayers()
    -- Configurações
    local Settings = {
        ['Material'] = Enum.Material.Neon, 
        ['Color'] = Color3.fromRGB(0, 255, 255), 
        ['Transparency'] = 0.7
    }

    local ScreenGui = Instance.new('ScreenGui', game.CoreGui)
    ScreenGui.IgnoreGuiInset = true

    local ViewportFrame = Instance.new('ViewportFrame', ScreenGui)
    ViewportFrame.CurrentCamera = workspace.CurrentCamera
    ViewportFrame.Size = UDim2.new(1, 0, 1, 0)
    ViewportFrame.BackgroundTransparency = 1
    ViewportFrame.ImageTransparency = Settings.Transparency

    local Chasms = {}

    local function generateChasm(player)
        local Character = workspace:FindFirstChild(player.Name)

        if Character then
            for _, Part in pairs(Character:GetChildren()) do
                if Part:IsA('Part') or Part:IsA('MeshPart') then
                    local Chasm = Part:Clone()
                    for _, Child in pairs(Chasm:GetChildren()) do
                        if Child:IsA('Decal') then
                            Child:Destroy()
                        end
                    end

                    Chasm.Parent = ViewportFrame
                    Chasm.Material = Settings.Material
                    Chasm.Color = Settings.Color
                    Chasm.Anchored = true

                    table.insert(Chasms, Chasm)
                end
            end
        end
    end

    local function clearChasms()
        for _, Chasm in pairs(Chasms) do
            Chasm:Destroy()
        end
        Chasms = {}
    end

    while game:GetService('RunService').RenderStepped:Wait() do
        clearChasms()
        for _, Player in pairs(game:GetService('Players'):GetPlayers()) do
            if Player ~= game:GetService('Players').LocalPlayer then
                generateChasm(Player)
            end
        end
        wait(0.1)
    end
end
