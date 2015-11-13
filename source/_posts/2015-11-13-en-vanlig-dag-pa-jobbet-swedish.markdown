---
layout: post
title: "En vanlig dag på jobbet (SWEDISH)"
date: 2015-11-13 20:57:29 +0100
comments: true
categories:
- work
- starwars
- dns
- darkside
- sith
- jedi
---

I vanlig ordning bashar vi DNS på jobbet, pga ofungerar *hårt* över VPN
för de flesta. (Ja vi kör alla Linux, utom cheferna som envisas med att
använda något ur gamla testamentet.) Här följer ett utdrag från vår IRC:

	14:32 <n00b> Success! Äntligen fick jag ordning på DNS via guest
		  wifi -> vpn -> office network. Firar med att skapa lite irc noise. :D
	14:32 < rooth>n00b: Du har väl fått den distribuerade /etc/hosts filen?
	14:33 < rooth> !dns paperboy
	14:33 < |master|> rooth: server028.intranet.example.com. 192.168.130.72 lazyboy lazyboy.intranet.example.com
	14:33 < rbot> rooth: lazyboy: 192.168.130.72
	14:33 < rooth> bottarna hjälper dig annars =)
	14:34 <n00b> hosts file is so... "unclean" ;)
	14:35 <n00b> Men bra att veta att bottar kan hjälpa. :)
	14:37 < rooth>n00b: Förstår inte alls vad du menar... =)
	14:37 < rooth>n00b: Det finns två skolor på bygget, dns vs hosts.
	14:38 <n00b> Oh! Så jag har tydligen valt sida nu då. :D
	14:40 < rooth>n00b: dns = dark-side.
	14:50 < lazzer>n00b: ;-)
	14:52 < rooth>n00b: s#local dns cache#/etc/hosts# =)
	14:52 < moggen> hehe HOSTS.TXT revival
	14:52 < moggen> var så det började en gång i tiden
	14:52 < moggen> innan man uppfann DNS
	14:53 < lazzer> Vi måste komma på ett bra system för att dela våra hosts-filer.
	14:53 <n00b> A long time ago, in a network far far away...
	14:53 <n00b> git repo for shared hosts fil, annan conf?
	14:57 <n00b> Finns det någon slags share-mekanism vad gäller grundläggande test
				 setup btw? Eller är det för svårt att få till generell grund conf
				 för lokal test setup?
	15:38 < troglobit> Åh, har ni öppnat DNS-lådan! LOL =)
	15:38  * troglobit kör med hosts-fil, såklart ;)
	15:40 < lazzer> troglobit: bra val ;-=
	15:44 < rooth> troglobit: Jag tyckte det var dags =)
	16:00 < moggen> troglobit: upptäckte när jag jobbade hemma att man får "fel" IP på
					web.example.com om man inte sitter på kontors-IP... hosts ftw.
	16:22 < troglobit> moggen: Härligt att se att du sållat dig till den enda sanna läran ;)
	16:59 < rooth> =)
	17:06 < wkz> moggen,troglobit : another one bites the dust... :)
	18:12 < n00b2> Helgen är räddad vpn-dosan återfunnen
	20:19 < troglobit> wkz: DNS är en kvarleva av ett döende system. Kom, anslut dig
				   till oss istället, vi kämpar för rättvisa, säkerhet, och fria
				   statiska /etc/hosts-uppdateringar i galaxen!
	20:19 < wkz> troglobit: NEVER!
	20:20 < troglobit> wkz: *Feel* the power of the dark side!
	20:20 < wkz> troglobit: LOL
	20:20 < troglobit> wkz: :-D

Some names have been changed to protect the innocent, the rest are Sith
Lords or Jedi Knights.
