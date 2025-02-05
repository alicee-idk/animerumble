local ScreenGui = Instance.new("ScreenGui")
local ImageButton = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")

-- Propriedades
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

ImageButton.Parent = ScreenGui
ImageButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ImageButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
ImageButton.BorderSizePixel = 0
ImageButton.Position = UDim2.new(0.005, 0, 0.010, 0)
ImageButton.Size = UDim2.new(0, 45, 0, 45)
ImageButton.Image = "rbxassetid://70519405275916" -- Id da icon aqui

UICorner.Parent = ImageButton

-- FunÃ§Ã£o para tornar o botÃ£o arrastÃ¡vel
local UIS = game:GetService("UserInputService")
local dragging, dragInput, startPos, startMousePos
local hasMoved = false

ImageButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        hasMoved = false
        startPos = ImageButton.Position
        startMousePos = UIS:GetMouseLocation()

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

ImageButton.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UIS.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = UIS:GetMouseLocation() - startMousePos
        if math.abs(delta.X) > 5 or math.abs(delta.Y) > 5 then 
            hasMoved = true
        end
        ImageButton.Position = UDim2.new(
            startPos.X.Scale, startPos.X.Offset + delta.X,
            startPos.Y.Scale, startPos.Y.Offset + delta.Y
        )
    end
end)

ImageButton.MouseButton1Down:Connect(function()
    task.wait(0.1) -- Isso serve pro icone nao abrir quando a pessoa for arrastar ele, mexe aqui nÃ£o
    if not hasMoved then
        game:GetService("VirtualInputManager"):SendKeyEvent(true, Enum.KeyCode.LeftControl, false, game)
    end
end)








local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "with ❤ frusteddy",
    SubTitle = "by Frusteddy",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = false,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local Tabs = {
    Main = Window:AddTab({ Title = "World", Icon = "house" }),
    Dungeon = Window:AddTab({ Title = "AutoRaid", Icon = "skull" }),
	OPTHINGS = Window:AddTab({ Title = "OP THINGS", Icon = "swords" }),
}

local Options = Fluent.Options

Fluent:Notify({
    Title = "Frusteddy Hub",
    Content = "bug report in discord",
    Duration = 5
})

Tabs.Main:AddParagraph({
    Title = "UPDATE 1.0",
    Content = "LOL BAD GAME"
})


Tabs.Main:AddParagraph({
    Title = "KILL AURA DON'T WORK WITHOUT DAMAGE SKILLS!!!!!!!!!!",
    Content = "KILL AURA DON'T WORK WITHOUT DAMAGE SKILLS!!!!!!!!!!"
})


local Toggle = Tabs.Dungeon:AddToggle("MyToggle", {Title = "Kill aura(DUNGEON)", Default = false })

Toggle:OnChanged(function(state)
    _G.auto = state 
    
    if _G.auto then
        spawn(function()
            while _G.auto do
                local args = {
                    [1] = "Skill"
                }
                
                game:GetService("ReplicatedStorage"):WaitForChild("Events")
                    :WaitForChild("RemoteEvents"):WaitForChild("DungeonEvent")
                    :FireServer(unpack(args))
                
                wait()
            end
        end)
    end
end)

Options.MyToggle:SetValue(false)



local Toggle = Tabs.Dungeon:AddToggle("MyToggle", {Title = "Kill aura(Normal Game)", Default = false })

Toggle:OnChanged(function(state)
    _G.auto = state -- Define o estado de _G.auto conforme o toggle
    
    if _G.auto then
        spawn(function() -- Executa em uma nova thread para evitar travamentos
            while _G.auto do
                local args = {
                    [1] = "Combat"
                }
                
                game:GetService("ReplicatedStorage"):WaitForChild("Events")
                    :WaitForChild("RemoteEvents"):WaitForChild("CombatEvent")
                    :FireServer(unpack(args))
                
                wait() -- Pequeno delay para evitar sobrecarga
            end
        end)
    end
end)

Options.MyToggle:SetValue(false)


local Input = Tabs.OPTHINGS:AddInput("Input", {
    Title = "Unit Giver",
    Default = "Default",
    Placeholder = "Digite o nome da unidade", -- Adicionando uma mensagem mais clara
    Numeric = false, -- Apenas permite números
    Finished = false, -- Apenas chama o callback quando pressionar Enter
    Callback = function(Value)
        print("Input alterado:", Value)
        
        -- Aqui, o nome digitado será passado diretamente para a função
        local args = {
            [1] = "AddUnit",
            [2] = {
                ["UnitName"] = Value -- Utilizando o valor digitado diretamente
            }
        }

        -- Enviando a requisição para o servidor
        game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("RemoteEvents"):WaitForChild("InventoryEvent"):FireServer(unpack(args))
    end
})

-- Callback para quando o usuário mudar o input
Input:OnChanged(function()
    print("Input atualizado:", Input.Value)
end)




local Toggle = Tabs.OPTHINGS:AddToggle("MyToggle", {Title = "SpawnRewardBoos(Madara)", Default = false })

Toggle:OnChanged(function(state)
    _G.auto = state -- Define o estado de _G.auto conforme o toggle
    
    if _G.auto then
        spawn(function() -- Executa em uma nova thread para evitar travamentos
            while _G.auto do
                local args = {
                    [1] = "Rewards",
                    [2] = {
                        ["BossName"] = "Madara"
                    }
                }

                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("RemoteEvents"):WaitForChild("BossFightEvent"):FireServer(unpack(args))
                wait() -- Espera 1 segundo antes de enviar outro comando
            end
        end)
    end
end)

Options.MyToggle:SetValue(false)


local Toggle = Tabs.Dungeon:AddToggle("MyToggle", {Title = "SpawnRewardBoos(SSJGOKU)", Default = false })

Toggle:OnChanged(function(state)
    _G.auto = state -- Define o estado de _G.auto conforme o toggle
    
    if _G.auto then
        spawn(function() -- Executa em uma nova thread para evitar travamentos
            while _G.auto do
                local args = {
                    [1] = "Rewards",
                    [2] = {
                        ["BossName"] = "SSJGoku"
                    }
                }

                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("RemoteEvents"):WaitForChild("BossFightEvent"):FireServer(unpack(args))
                wait() -- Espera 1 segundo antes de enviar outro comando
            end
        end)
    end
end)

Options.MyToggle:SetValue(false)

local Toggle = Tabs.Dungeon:AddToggle("MyToggle", {Title = "SpawnRewardBoos(Diavolo)", Default = false })

Toggle:OnChanged(function(state)
    _G.auto = state -- Define o estado de _G.auto conforme o toggle
    
    if _G.auto then
        spawn(function() -- Executa em uma nova thread para evitar travamentos
            while _G.auto do
                local args = {
                    [1] = "Rewards",
                    [2] = {
                        ["BossName"] = "Diavolo"
                    }
                }

                game:GetService("ReplicatedStorage"):WaitForChild("Events"):WaitForChild("RemoteEvents"):WaitForChild("BossFightEvent"):FireServer(unpack(args))
                wait() -- Espera 1 segundo antes de enviar outro comando
            end
        end)
    end
end)

Options.MyToggle:SetValue(false)

Options.MyToggle:SetValue(false)

Options.MyToggle:SetValue(false)

SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({})
InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)

Window:SelectTab(1)

Fluent:Notify({
    Title = "Fluent",
    Content = "O script foi carregado!",
    Duration = 8
})

SaveManager:LoadAutoloadConfig()

return
