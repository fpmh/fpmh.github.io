---
layout: default
permalink: /technieken/LFI-naar-RCE
permalink_name: /technieken/LFI-naar-RCE
title: hmpf's notitieblok
---


# LFI naar RCI middels log poisoning
Local File Inclusion is te escaleren naar Remote Code Execution middels log poisoning. 

Hoewel je met een LFI vulnerability niet direct kan schrijven in bestanden, wordt wellicht de data die jij genereerd wel opgeslagen in logbestanden. Een simpel voorbeeld hiervan is het /var/log/apache2/access.log bestand. Dit bestand logt de requests die gemaakt zijn naar de webserver, **inclusief headers**. Door een simpele PHP shell in bijvoorbeeld de User-Agent header te verwerken, kunnen we zo onze LFI escaleren naar RCE.

Met behulp van tools zoals wfuzz of ffuf en [deze wordlist](https://raw.githubusercontent.com/drtychai/wordlists/master/intruder/lfi.txt) kun je op zoek gaan naar logbestanden. 

![image]()

