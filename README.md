local players = game:GetService("Players")
local rs = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local lp = players.LocalPlayer

local MenuVisivel = true
local JogadorSelecionado = nil

-- [FUNÇÕES DE LÓGICA E BYPASS EXTRAÍDAS DO SEU ROTEIRO]
local function uh()
	local e = rs:FindFirstChild("Events")
	if e then
		local r = e:FindFirstChild("CatalogGuiRemote")
		if r then return r end
	end
	local r = rs:FindFirstChild("CatalogGuiRemote")
	if r then return r end
	for _, d in ipairs(rs:GetDescendants()) do
		if d.Name == "CatalogGuiRemote" and (d:IsA("RemoteFunction") or d:IsA("RemoteEvent")) then
			return d
		end
	end
end

local function rgb(c)
	if typeof(c) ~= "Color3" then
		return {r = 255, g = 255, b = 255, IsRGBTable = true}
	end
	return {r = c.R * 255, g = c.G * 255, b = c.B * 255, IsRGBTable = true}
end

local function dict(d)
	local p = {
		HatAccessory = d.HatAccessory or "",
		HairAccessory = d.HairAccessory or "",
		FaceAccessory = d.FaceAccessory or "",
		BackAccessory = d.BackAccessory or "",
		FrontAccessory = d.FrontAccessory or "",
		NeckAccessory = d.NeckAccessory or "",
		ShouldersAccessory = d.ShouldersAccessory or "",
		WaistAccessory = d.WaistAccessory or "",
		Face = d.Face or 0,
		Head = d.Head or 0,
		Torso = d.Torso or 0,
		LeftArm = d.LeftArm or 0,
		RightArm = d.RightArm or 0,
		LeftLeg = d.LeftLeg or 0,
		RightLeg = d.RightLeg or 0,
		Shirt = d.Shirt or 0,
		Pants = d.Pants or 0,
		GraphicTShirt = d.GraphicTShirt or 0,
		HeadColor = rgb(d.HeadColor),
		TorsoColor = rgb(d.TorsoColor),
		LeftArmColor = rgb(d.LeftArmColor),
		RightArmColor = rgb(d.RightArmColor),
		LeftLegColor = rgb(d.LeftLegColor),
		RightLegColor = rgb(d.RightLegColor),
		HeightScale = d.HeightScale or 1,
		WidthScale = d.WidthScale or 1,
		DepthScale = d.DepthScale or 1,
		HeadScale = d.HeadScale or 1,
		BodyTypeScale = d.BodyTypeScale or 0,
		ProportionScale = d.ProportionScale or 0,
		ClimbAnimation = d.ClimbAnimation or 0,
		FallAnimation = d.FallAnimation or 0,
		IdleAnimation = d.IdleAnimation or 0,
		JumpAnimation = d.JumpAnimation or 0,
		MoodAnimation = d.MoodAnimation or 0,
		RunAnimation = d.RunAnimation or 0,
		SwimAnimation = d.SwimAnimation or 0,
		WalkAnimation = d.WalkAnimation or 0,
		MakeupItems = {},
		AccessoryRefinements = {},
		LayeredAccessories = {},
		StaticFacialAnimation = false,
	}
	pcall(function()
		local a = d:GetAccessories(true)
		if type(a) ~= "table" then return end
		local o = {}
		for _, x in ipairs(a) do
			if type(x) == "table" and x.AssetId then
				o[#o + 1] = {
					AssetId = x.AssetId,
					AccessoryType = tostring(x.AccessoryType or ""),
					Order = x.Order,
					Puffiness = x.Puffiness,
				}
			end
		end
		p.LayeredAccessories = o
	end)
	return p
end

local function ser(m, d)
	if type(m) == "table" and type(m.ToDictionary) == "function" then
		local ok, t = pcall(m.ToDictionary, m, d)
		if ok and type(t) == "table" then return t end
	end
	return dict(d)
end

local function descfrom(plr)
	if not plr then return end
	local h = plr.Character and plr.Character:FindFirstChildOfClass("Humanoid")
	if h then
		local ok, live = pcall(function()
			return h:GetAppliedDescription()
		end)
		if ok and live and live:IsA("HumanoidDescription") then
			return live, h.RigType
		end
	end
	local ok, d = pcall(function()
		return players:GetHumanoidDescriptionFromUserId(plr.UserId)
	end)
	if ok and d and d:IsA("HumanoidDescription") then
		return d, Enum.HumanoidRigType.R15
	end
end

local function executarInjecaoCompleta(plr)
	if not plr then return end
	local live, lrig = descfrom(plr)
	if not live then return end

	local rem = uh()
	if not rem then return end

	local p = {
		Action = "CreateAndWearHumanoidDescription",
		Properties = ser(nil, live),
		RigType = lrig or Enum.HumanoidRigType.R15,
	}
	pcall(function()
		if rem:IsA("RemoteFunction") then
			rem:InvokeServer(p)
		else
			rem:FireServer(p)
		end
	end)
end

-- [ESTRUTURA VISUAL INTERATIVA DO SEU PAINEL]
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "L_Lawliet_Advanced_Stealer"
ScreenGui.ResetOnSpawn = false

local successGui, _ = pcall(function() ScreenGui.Parent = CoreGui end)
if not successGui or not ScreenGui.Parent then 
    ScreenGui.Parent = lp:WaitForChild("PlayerGui", 10) 
end

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 350, 0, 450) 
MainFrame.Position = UDim2.new(0.5, -175, 0.5, -225)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true 
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, 0, 0, 45)
TitleText.BackgroundTransparency = 1
TitleText.Text = " CAC STEALER - ADVANCED LOGIC"
TitleText.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleText.TextXAlignment = Enum.TextXAlignment.Center
TitleText.Font = Enum.Font.SpecialElite 
TitleText.TextSize = 14
TitleText.Parent = MainFrame

local StatusLabel = Instance.new("TextLabel")
StatusLabel.Size = UDim2.new(1, -30, 0, 30)
StatusLabel.Position = UDim2.new(0, 15, 0, 45)
StatusLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
StatusLabel.Text = "ALVO: NENHUM (SELECIONE NA LISTA)"
StatusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
StatusLabel.Font = Enum.Font.SourceSansBold
StatusLabel.TextSize = 13
StatusLabel.Parent = MainFrame

local StatusCorner = Instance.new("UICorner")
StatusCorner.CornerRadius = UDim.new(0, 6)
StatusCorner.Parent = StatusLabel

local PlayerListFrame = Instance.new("ScrollingFrame")
PlayerListFrame.Size = UDim2.new(1, -30, 1, -120)
PlayerListFrame.Position = UDim2.new(0, 15, 0, 85)
PlayerListFrame.BackgroundTransparency = 0.95
PlayerListFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
PlayerListFrame.BorderSizePixel = 0
PlayerListFrame.ScrollBarThickness = 6
PlayerListFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)
PlayerListFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
PlayerListFrame.Parent = MainFrame

local ListLayout = Instance.new("UIListLayout")
ListLayout.Parent = PlayerListFrame
ListLayout.SortOrder = Enum.SortOrder.Name
ListLayout.Padding = UDim.new(0, 6)

ListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    PlayerListFrame.CanvasSize = UDim2.new(0, 0, 0, ListLayout.AbsoluteContentSize.Y + 10)
end)

-- Sistema dinâmico de botões por jogador
local function atualizarListaJogadores()
    for _, filho in pairs(PlayerListFrame:GetChildren()) do
        if filho:IsA("TextButton") then
            filho:Destroy()
        end
    end
    
    for _, player in pairs(players:GetPlayers()) do
        if player ~= lp then
            local PlayerBtn = Instance.new("TextButton")
            PlayerBtn.Size = UDim2.new(1, -10, 0, 35)
            PlayerBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            PlayerBtn.BackgroundTransparency = 0.2
            PlayerBtn.Text = "  " .. player.DisplayName .. " (@" .. player.Name .. ")"
            PlayerBtn.TextColor3 = Color3.fromRGB(230, 230, 230)
            PlayerBtn.Font = Enum.Font.SourceSansBold
            PlayerBtn.TextSize = 13
            PlayerBtn.TextXAlignment = Enum.TextXAlignment.Left
            PlayerBtn.Parent = PlayerListFrame
            
            local BtnCorner = Instance.new("UICorner")
            BtnCorner.CornerRadius = UDim.new(0, 6)
            BtnCorner.Parent = PlayerBtn
            
            PlayerBtn.MouseButton1Click:Connect(function()
                JogadorSelecionado = player
                StatusLabel.Text = "SELECIONADO: " .. player.DisplayName:upper() .. " | [G] PARA COPIAR"
                StatusLabel.TextColor3 = Color3.fromRGB(100, 255, 100)
            end)
        end
    end
end

-- Conexões de Rede (Entrada/Saída que limpam e listam de forma automática)
players.PlayerAdded:Connect(function()
    task.wait(1)
    atualizarListaJogadores()
end)

players.PlayerRemoving:Connect(function(player)
    if JogadorSelecionado == player then
        JogadorSelecionado = nil
        StatusLabel.Text = "ALVO: NENHUM (SELECIONE NA LISTA)"
        StatusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
    end
    atualizarListaJogadores()
end)

atualizarListaJogadores()

-- Ouvinte de Teclado (G para Clonar, CTRL para minimizar, Delete para fechar)
local conexaoTeclado
conexaoTeclado = UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.G then
        if JogadorSelecionado then
            executarInjecaoCompleta(JogadorSelecionado)
            StatusLabel.Text = "SKIN COPIADA COM SUCESSO!"
            StatusLabel.TextColor3 = Color3.fromRGB(50, 255, 50)
            task.wait(1)
            if JogadorSelecionado then
                StatusLabel.Text = "SELECIONADO: " .. JogadorSelecionado.DisplayName:upper() .. " | [G] PARA COPIAR"
                StatusLabel.TextColor3 = Color3.fromRGB(100, 255, 100)
            end
        end
    elseif input.KeyCode == Enum.KeyCode.LeftControl then
        MenuVisivel = not MenuVisivel
        MainFrame.Visible = MenuVisivel
    elseif input.KeyCode == Enum.KeyCode.Delete then
        if conexaoTeclado then conexaoTeclado:Disconnect() end
        ScreenGui:Destroy()
    end
end)

