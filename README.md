-- ===================================================
-- FERLONN HUB - TEMA VERMELHO E ÍCONE CUSTOMIZADO
-- ===================================================

-- Executa o script base
loadstring(game:HttpGet("https://raw.githubusercontent.com/lennonxscripts/lennonhubv2/refs/heads/main/stealaneggv2"))()

local COR_VERMELHA = Color3.fromRGB(180, 20, 20)
-- ID do ativo da imagem (L de Death Note / Ferlonn Hub)
local ID_ICONE_FERLONN = "rbxassetid://10723374271" 

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local function customizarUI(obj)
    pcall(function()
        -- 1. Substituição de Textos (Lennon Hub / Miranda -> Ferlonn Hub)
        if obj:IsA("TextLabel") or obj:IsA("TextButton") or obj:IsA("TextBox") then
            if obj.Text and obj.Text ~= "" then
                local txt = string.lower(obj.Text)
                if string.find(txt, "lennon") or string.find(txt, "miranda") or string.find(txt, "emorfz") then
                    obj.Text = "Ferlonn Hub"
                end
            end
        end

        -- 2. Troca forçada da imagem nos Ícones
        if obj:IsA("ImageLabel") or obj:IsA("ImageButton") then
            local nome = string.lower(obj.Name)
            local parentNome = obj.Parent and string.lower(obj.Parent.Name) or ""

            -- Localiza os elementos do botão flutuante e do topo da janela
            if string.find(nome, "icon") or string.find(nome, "logo") or string.find(nome, "lennon") or 
               string.find(nome, "image") or string.find(parentNome, "toggle") or string.find(parentNome, "header") then
                
                obj.Image = ID_ICONE_FERLONN
                obj.ImageTransparency = 0
                obj.ImageColor3 = Color3.fromRGB(255, 255, 255)
            end
        end

        -- 3. Aplicação da cor vermelha nas seções da interface
        if obj:IsA("GuiObject") then
            -- Altera barras e detalhes roxos/azuis para vermelho
            if obj.BackgroundColor3.R > 0.2 and obj.BackgroundColor3.B > 0.3 then
                obj.BackgroundColor3 = COR_VERMELHA
            end
            
            -- Pinta o botão flutuante mantendo o fundo do painel escuro
            local nome = string.lower(obj.Name)
            if string.find(nome, "toggle") then
                obj.BackgroundColor3 = COR_VERMELHA
            end
        end
    end)
end

-- Varredura contínua focada apenas na UI (Roda a cada 0.5s sem gerar lag)
task.spawn(function()
    while task.wait(0.5) do
        local alvos = {CoreGui}
        if LocalPlayer and LocalPlayer:FindFirstChildOfClass("PlayerGui") then
            table.insert(alvos, LocalPlayer:FindFirstChildOfClass("PlayerGui"))
        end

        for _, gui in ipairs(alvos) do
            for _, desc in ipairs(gui:GetDescendants()) do
                customizarUI(desc)
            end
        end
    end
end)

print("Ferlonn Hub: Ícone e textos atualizados!")