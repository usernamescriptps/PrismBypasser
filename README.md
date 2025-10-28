-- =============================================
-- FULL CHAT BYPASS + KEYLESS + 9-TAB UI (EXPANDED & CATEGORIZED)
-- Tabs:
--   [Auto Bypass]  → Input + Send + Favorites + Import + Spam
--   [Swear Words]  → 50+ Hard Bypassed Swears
--   [Phrases]      → 35+ Toxic & Swear-Filled
--   [Info]         → Executor + Discord
--   [SEXUAL]       → 15 Sexual Phrases
--   [RACIST]       → 15 Racist Slurs & Phrases
--   [WEBSITES]     → 15 Adult Websites (Bypassed)
--   [HOMOPHOBIC]   → 15 Homophobic Insults
--   [Custom 5]     → 15 Editable Phrases
-- Works on: Krnl, Synapse, Fluxus, Delta, Comet, JJSploit, etc.
-- =============================================

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local HttpService = game:GetService("HttpService")
local TextChatService = game:GetService("TextChatService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- === CONFIGURATION ===
local DISCORD_LINK = "https://discord.gg/emqFFrn2RN"  -- CHANGE THIS

-- === UNICODE BYPASS MAP (f = f, q = q, t = t, r = r) ===
local bypassMap = {
    ["A"]="Α",["B"]="В",["C"]="С",["D"]="ᗪ",["E"]="Ε",["F"]="Ғ",["G"]="Ꮐ",["H"]="Н",["I"]="І",["J"]="Ј",
    ["K"]="Κ",["L"]="ᒪ",["M"]="М",["N"]="N",["O"]="О",["P"]="Ρ",["Q"]="Ԛ",["R"]="Ꭱ",["S"]="Ѕ",["T"]="T",
    ["U"]="∪",["V"]="Ѵ",["W"]="Ꮃ",["X"]="Χ",["Y"]="Υ",["Z"]="Ζ",
    ["a"]="а",["b"]="Ƅ",["c"]="с",["d"]="ԁ",["e"]="е",["f"]="f",["g"]="ɡ",["h"]="һ",["i"]="і",["j"]="ј",
    ["k"]="κ",["l"]="ӏ",["m"]="м",["n"]="n",["o"]="о",["p"]="р",["q"]="q",["r"]="r",["s"]="ѕ",["t"]="t",
    ["u"]="υ",["v"]="ѵ",["w"]="ԝ",["x"]="х",["y"]="у",["z"]="ᴢ"
}

local function bypassText(text)
    local result = ""
    for i = 1, #text do
        local char = text:sub(i,i)
        result = result .. (bypassMap[char] or char)
    end
    return result
end

-- === AUTO SEND FUNCTION ===
local function autoSend(message)
    local bypassed = bypassText(message)
    if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
        local TextChannels = TextChatService:FindFirstChild("TextChannels")
        if TextChannels then
            local RBXGeneral = TextChannels:FindFirstChild("RBXGeneral")
            if RBXGeneral then
                RBXGeneral:SendAsync(bypassed)
                return true
            end
        end
    end
    local SayMessageRequest = ReplicatedStorage:FindFirstChild("SayMessageRequest", true)
    if SayMessageRequest then
        SayMessageRequest:FireServer(bypassed, "All")
        return true
    end
    for _, v in pairs(ReplicatedStorage:GetDescendants()) do
        if v:IsA("RemoteEvent") and v.Name:lower():find("chat") then
            pcall(function() v:FireServer(bypassed) end)
            return true
        end
    end
    return false
end

-- === MAIN UI ===
local MainWindow = Rayfield:CreateWindow({
    Name = "Auto Chat Bypass",
    LoadingTitle = "Bypass Active",
    LoadingSubtitle = "Keyless • 9 Tabs Loaded",
    ConfigurationSaving = { Enabled = false }
})

-- === TAB 1: AUTO BYPASS ===
local MainTab = MainWindow:CreateTab("Auto Bypass", 4483362458)
local inputText = ""
MainTab:CreateInput({Name="Enter Message",PlaceholderText="Type anything to bypass...",RemoveTextAfterFocusLost=false,Callback=function(t)inputText=t end})
MainTab:CreateButton({Name="AUTO BYPASS & SEND",Callback=function()
    if inputText=="" or inputText:match("^%s*$") then Rayfield:Notify({Title="Empty",Content="Type a message!",Duration=3,Image=7412345678}) return end
    local s=autoSend(inputText)
    Rayfield:Notify({Title=s and "Sent!"or"Failed",Content=s and("Bypassed: "..bypassText(inputText))or"Send failed.",Duration=4,Image=s and 4483362458 or 7412345678})
end})

-- Favorites
MainTab:CreateParagraph({Title="Favorites",Content="Click to send your saved phrases"})
local favorites,favoriteButtons={},{}
local function refreshFavorites()
    for _,b in pairs(favoriteButtons) do if b and b.Destroy then pcall(b.Destroy,b) end end
    favoriteButtons={}
    for i,fav in ipairs(favorites) do
        local btn=MainTab:CreateButton({Name="★ "..fav,Callback=function()autoSend(fav)Rayfield:Notify({Title="Sent",Content="Bypassed: "..bypassText(fav),Duration=3,Image=4483362458})end})
        table.insert(favoriteButtons,btn)
    end
end
MainTab:CreateButton({Name="Add to Favorites",Callback=function()
    if inputText=="" then Rayfield:Notify({Title="Empty",Content="Type something first!",Duration=3,Image=7412345678}) return end
    table.insert(favorites,inputText) refreshFavorites()
    Rayfield:Notify({Title="Added!",Content="'"..inputText.."' saved.",Duration=3,Image=4483362458})
end})
MainTab:CreateButton({Name="Clear Favorites",Callback=function()favorites={}refreshFavorites()Rayfield:Notify({Title="Cleared",Content="Favorites reset.",Duration=3,Image=4483362458})end})

-- Import
MainTab:CreateParagraph({Title="Import Phrases",Content="Paste multiple (one per line)"})
local importText=""
MainTab:CreateInput({Name="Paste Here",PlaceholderText="Line 1\nLine 2...",MultiLine=true,Callback=function(t)importText=t end})
MainTab:CreateButton({Name="Import to Favorites",Callback=function()
    if importText=="" then Rayfield:Notify({Title="Empty",Content="Paste phrases!",Duration=3,Image=7412345678}) return end
    local lines={}
    for l in importText:gmatch("[^\r\n]+") do local tr=l:match("^%s*(.-)%s*$") if tr~="" then table.insert(lines,tr) end end
    for _,l in ipairs(lines) do table.insert(favorites,l) end
    refreshFavorites()
    Rayfield:Notify({Title="Imported!",Content=#lines.." added.",Duration=3,Image=4483362458})
end})

-- Spam
MainTab:CreateParagraph({Title="Spam Mode",Content="Auto-send with delay"})
local spamOn,spamDelay,spamMsg=false,1,""
MainTab:CreateToggle({Name="Enable Spam",CurrentValue=false,Callback=function(v)
    spamOn=v
    if v then
        spamMsg=inputText
        if spamMsg=="" then Rayfield:Notify({Title="No Msg",Content="Type to spam!",Duration=3,Image=7412345678}) spamOn=false return end
        spawn(function() while spamOn do autoSend(spamMsg) wait(spamDelay) end end)
        Rayfield:Notify({Title="Spamming",Content="Every "..spamDelay.."s",Duration=3,Image=4483362458})
    else
        Rayfield:Notify({Title="Stopped",Content="Spam disabled.",Duration=3,Image=7412345678})
    end
end})
MainTab:CreateSlider({Name="Delay (s)",Range={0.1,10},Increment=0.1,CurrentValue=1,Callback=function(v)spamDelay=v end})

-- === TAB 2: SWEAR WORDS ===
local SwearTab = MainWindow:CreateTab("Swear Words", 7412345678)
local swears = {"fuck","shit","nigger","bitch","asshole","cunt","faggot","retard","motherfucker","cock","pussy","dick","tits","whore","slut","bastard","nigga","fucker","cocksucker","twat","wanker","knob","arse","jizz","cum","semen","vagina","penis","anus","rape","kill","die","suicide","gay","lesbian","tranny","chink","spic","kike","gook","ass","nazi","hitler","terrorist","drugs","heroin","meth","cocaine","weed"}
for _,w in ipairs(swears) do SwearTab:CreateButton({Name=w,Callback=function()autoSend(w)Rayfield:Notify({Title="Sent",Content="Bypassed: "..bypassText(w),Duration=3,Image=4483362458})end}) end

-- === TAB 3: PHRASES ===
local PhraseTab = MainWindow:CreateTab("Phrases", 4483362458)
‌local phrases = {"fuck you","suck my dick","eat shit","go fuck yourself","cunt face","nigger lover","faggot ass","retard kid","your mom sucks","dickhead","pussy boy","bitch ass nigga","kill yourself","cut yourself bitch","hang yourself","rape your mom","I am a nazi!","I am part of the KKK!","ISIS terrorist","I bet you have a bomb vest","Hang yourself you fucking nigger","heil hitler","Be a nazi you fucking jew","tranny freak","lesbian dyke","gay homo","heroin addict","meth addict","cocaine whore","weed junkie","cocaine snorter","acid freak","nazi salute","hitler did nothing wrong","cocaine"}
for _,p in ipairs(phrases) do PhraseTab:CreateButton({Name=p,Callback=function()autoSend(p)Rayfield:Notify({Title="Sent",Content="Bypassed: "..bypassText(p),Duration=3,Image=4483362458})end}) end

-- === TAB 4: INFO ===
local InfoTab = MainWindow:CreateTab("Info", 4483362458)
InfoTab:CreateLabel("Status: Keyless Access")
InfoTab:CreateLabel("Discord: "..DISCORD_LINK)
local exec = syn and "Synapse X" or KRNL_LOADED and "Krnl" or Fluxus and "Fluxus" or getexecutorname and getexecutorname() or "Unknown"
InfoTab:CreateLabel("Executor: "..exec)

-- === TAB 5: SEXUAL ===
local SexualTab = MainWindow:CreateTab("SEXUAL", 4483362458)
local sexual = {"I want to fuck you hard","Suck my cock deep","Your pussy feels so wet",Suck my cock","Lick my dick","I love big tits","Fuck me in the ass","I’m horny as fuck","Send nudes now","I masturbate to you","Let’s have sex","I want your dick","Drink my cum","Ride my cock","I’m cumming!","Who wants to be raped?"," Let me Rape you"}
for _,s in ipairs(sexual) do SexualTab:CreateButton({Name=s,Callback=function()autoSend(s)Rayfield:Notify({Title="Sent",Content="Bypassed: "..bypassText(s),Duration=3,Image=4483362458})end}) end

-- === TAB 6: RACIST ===
local RacistTab = MainWindow:CreateTab("RACIST", 7412345678)
local racist = {"Niggers belong in chains","All spics are criminals","Chinks eat dogs","Kikes control the banks","Gooks are subhuman","All Niggers are slaves","Nigger music is trash","Niggers burn in hell","Chink virus maker","Kike nose","Gook eyes","Black rapist","Niggers can’t read","Spics breed like rats","Chinks copy everything"}
for _,r in ipairs(racist) do RacistTab:CreateButton({Name=r,Callback=function()autoSend(r)Rayfield:Notify({Title="Sent",Content="Bypassed: "..bypassText(r),Duration=3,Image=4483362458})end}) end

-- === TAB 7: WEBSITES ===
local WebsitesTab = MainWindow:CreateTab("WEBSITES", 4483362458)
local sites = {"pornhub.com","xvideos.com","xnxx.com","youporn.com","redtube.com","tube8.com","spankbang.com","xhamster.com","brazzers.com","bangbros.com","naughtyamerica.com","realitykings.com","onlyfans.com","chaturbate.com","livejasmin.com"}
for _,s in ipairs(sites) do WebsitesTab:CreateButton({Name=s,Callback=function()autoSend(s)Rayfield:Notify({Title="Sent",Content="Bypassed: "..bypassText(s),Duration=3,Image=4483362458})end}) end

-- === TAB 8: HOMOPHOBIC ===
local HomophobicTab = MainWindow:CreateTab("HOMOPHOBIC", 7412345678)
local homo = {"Faggots burn in hell","Gays are pedophiles","Dykes are ugly","Tranny freak","Homo cocksucker","Gay men spread AIDS","Lesbians hate men","Faggot ass licker","Tranny rapist","Fuck All Faggots","Trans bitch","Homo marriage is fake","Faggots can’t fight","Tranny delusion","Gay pride is sin"}
for _,h in ipairs(homo) do HomophobicTab:CreateButton({Name=h,Callback=function()autoSend(h)Rayfield:Notify({Title="Sent",Content="Bypassed: "..bypassText(h),Duration=3,Image=4483362458})end}) end

-- === TAB 9: CUSTOM 5 ===
local Custom5Tab = MainWindow:CreateTab("Custom 5", 4483362458)
local custom = {}
for i=1,15 do table.insert(custom,"Phrase "..i.." - Edit Me") end
for _,c in ipairs(custom) do Custom5Tab:CreateButton({Name=c,Callback=function()autoSend(c)Rayfield:Notify({Title="Sent",Content="Bypassed: "..bypassText(c),Duration=3,Image=4483362458})end}) end
Custom5Tab:CreateParagraph({Title="Custom 5",Content="Edit the 'custom' table in script to add your own phrases."})

-- === DONE ===
Rayfield:Notify({Title="Bypass Loaded!",Content="Keyless • 9 tabs ready!",Duration=5,Image=4483362458})
