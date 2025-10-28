-- =============================================
-- PRISM BYPASSER + KEYLESS + 9-TAB UI
-- Tabs: Auto | Swears | Phrases | Info | SEXUAL | RACIST | WEBSITES | HOMOPHOBIC | Custom
-- =============================================

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local TS = game:GetService("TextChatService")
local RS = game:GetService("ReplicatedStorage")

-- DISCORD
local DISCORD = "https://discord.gg/emqFFrn2RN"

-- BYPASS MAP
local m = {A="Α",B="В",C="С",D="ᗪ",E="Ε",F="Ғ",G="Ꮐ",H="Н",I="І",J="Ј",K="Κ",L="ᒪ",M="М",N="N",O="О",P="Ρ",Q="Ԛ",R="Ꭱ",S="Ѕ",T="T",U="∪",V="Ѵ",W="Ꮃ",X="Χ",Y="Υ",Z="Ζ",
           a="а",b="Ƅ",c="с",d="ԁ",e="е",f="f",g="ɡ",h="һ",i="і",j="ј",k="κ",l="ӏ",m="м",n="n",o="о",p="р",q="q",r="r",s="ѕ",t="t",u="υ",v="ѵ",w="ԝ",x="х",y="у",z="ᴢ"}
local function bypass(t) local s="" for i=1,#t do local c=t:sub(i,i) s=s..(m[c] or c) end return s end

-- SEND
local function send(msg)
    local b = bypass(msg)
    if TS.ChatVersion == Enum.ChatVersion.TextChatService then
        local ch = TS:FindFirstChild("TextChannels")
        if ch then local gen = ch:FindFirstChild("RBXGeneral") if gen then gen:SendAsync(b) return true end end
    end
    local rem = RS:FindFirstChild("SayMessageRequest", true)
    if rem then rem:FireServer(b, "All") return true end
    for _, v in pairs(RS:GetDescendants()) do
        if v:IsA("RemoteEvent") and v.Name:lower():find("chat") then pcall(v.FireServer, v, b) return true end
    end
    return false
end

-- UI
local W = Rayfield:CreateWindow({Name="Prism Bypasser",LoadingTitle="Loaded",LoadingSubtitle="9 Tabs",ConfigurationSaving={Enabled=false}})

-- AUTO BYPASS
local T1 = W:CreateTab("Auto Bypass",4483362458)
local txt = ""
T1:CreateInput({Name="Message",PlaceholderText="Type here...",Callback=function(v)txt=v end})
T1:CreateButton({Name="SEND",Callback=function()
    if txt=="" then Rayfield:Notify({Title="Empty",Content="Write something",Duration=3}) return end
    local ok = send(txt)
    Rayfield:Notify({Title=ok and "Sent" or "Failed",Content=ok and bypass(txt) or "Error",Duration=3,Image=ok and 4483362458 or 7412345678})
end})

-- FAVORITES
local favs, favBtns = {}, {}
local function refresh()
    for _,b in pairs(favBtns) do if b and b.Destroy then pcall(b.Destroy,b) end end
    favBtns = {}
    for i,f in ipairs(favs) do
        local btn = T1:CreateButton({Name="★ "..f,Callback=function()send(f)end})
        table.insert(favBtns, btn)
    end
end
T1:CreateButton({Name="Add to Favs",Callback=function()if txt~="" then table.insert(favs,txt) refresh() end end})
T1:CreateButton({Name="Clear Favs",Callback=function()favs={} refresh() end})

-- SPAM
local spamOn, delay = false, 1
T1:CreateToggle({Name="Spam Mode",CurrentValue=false,Callback=function(v)
    spamOn=v
    if v and txt~="" then spawn(function()while spamOn do send(txt) wait(delay) end end) end
end})
T1:CreateSlider({Name="Delay",Range={0.1,10},Increment=0.1,CurrentValue=1,Callback=function(v)delay=v end})

-- TABS (All your phrases included)
local tabs = {
    {"Swear Words",7412345678,{"fuck","shit","nigger","bitch","asshole","cunt","faggot","retard","motherfucker","cock","pussy","dick","tits","whore","slut","bastard","nigga","fucker","cocksucker","twat","wanker","knob","arse","jizz","cum","semen","vagina","penis","anus","rape","kill","die","suicide","gay","lesbian","tranny","chink","spic","kike","gook","ass","nazi","hitler","terrorist","drugs","heroin","meth","cocaine","weed"}},
    {"Phrases",4483362458,{"fuck you","suck my dick","eat shit","go fuck yourself","cunt face","nigger lover","faggot ass","retard kid","your mom sucks","dickhead","pussy boy","bitch ass nigga","kill yourself","cut yourself bitch","hang yourself","rape your mom","I am a nazi!","I am part of the KKK!","ISIS terrorist","I bet you have a bomb vest","Hang yourself you fucking nigger","heil hitler","Be a nazi you fucking jew","tranny freak","lesbian dyke","gay homo","heroin addict","meth addict","cocaine whore","weed junkie","cocaine snorter","acid freak","nazi salute","hitler did nothing wrong","cocaine"}},
    {"SEXUAL",4483362458,{"I want to fuck you hard","Suck my cock deep","Your pussy feels so wet","Suck my cock","Lick my dick","I love big tits","Fuck me in the ass","I’m horny as fuck","Send nudes now","I masturbate to you","Let’s have sex","I want your dick","Drink my cum","Ride my cock","I’m cumming!","Who wants to be raped?","Let me Rape you"}},
    {"RACIST",7412345678,{"Niggers belong in chains","All spics are criminals","Chinks eat dogs","Kikes control the banks","Gooks are subhuman","All Niggers are slaves","Nigger music is trash","Niggers burn in hell","Chink virus maker","Kike nose","Gook eyes","Black rapist","Niggers can’t read","Spics breed like rats","Chinks copy everything"}},
    {"WEBSITES",4483362458,{"pornhub.com","xvideos.com","xnxx.com","youporn.com","redtube.com","tube8.com","spankbang.com","xhamster.com","brazzers.com","bangbros.com","naughtyamerica.com","realitykings.com","onlyfans.com","chaturbate.com","livejasmin.com"}},
    {"HOMOPHOBIC",7412345678,{"Faggots burn in hell","Gays are pedophiles","Dykes are ugly","Tranny freak","Homo cocksucker","Gay men spread AIDS","Lesbians hate men","Faggot ass licker","Tranny rapist","Fuck All Faggots","Trans bitch","Homo marriage is fake","Faggots can’t fight","Tranny delusion","Gay pride is sin"}}
}
for _,t in ipairs(tabs) do
    local tab = W:CreateTab(t[1],t[2])
    for _,p in ipairs(t[3]) do tab:CreateButton({Name=p,Callback=function()send(p)end}) end
end

-- INFO
local T4 = W:CreateTab("Info",4483362458)
T4:CreateLabel("Status: Keyless")
T4:CreateLabel("Discord: "..DISCORD)
local exec = syn and "Synapse" or KRNL_LOADED and "Krnl" or Fluxus and "Fluxus" or getexecutorname and getexecutorname() or "Unknown"
T4:CreateLabel("Executor: "..exec)

-- CUSTOM
local T9 = W:CreateTab("Custom 5",4483362458)
local custom = {}
for i=1,15 do table.insert(custom,"Phrase "..i.." - Edit Me") end
for _,c in ipairs(custom) do T9:CreateButton({Name=c,Callback=function()send(c)end}) end

Rayfield:Notify({Title="Prism Bypasser",Content="9 Tabs Loaded • Keyless",Duration=5,Image=4483362458})
