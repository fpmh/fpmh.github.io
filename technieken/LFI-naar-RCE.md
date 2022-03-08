---
layout: default
permalink: /technieken/LFI-naar-RCE
permalink_name: /technieken/LFI-naar-RCE
title: hmpf's notitieblok
---


# LFI naar RCI middels log poisoning
Local File Inclusion is te escaleren naar Remote Code Execution middels log poisoning. 

Hoewel je met een LFI vulnerability niet direct kan schrijven in bestanden, wordt wellicht de data die jij genereert wel uitgeschreven in logbestanden. Het meestvoorkomende voorbeeld hiervan is het /var/log/apache2/access.log bestand. Dit bestand logt de webrequests die gemaakt zijn naar de server, **inclusief headers**. Door een PHP payload in bijvoorbeeld de User-Agent header te verwerken, kunnen we zo onze LFI escaleren naar RCE, door het logbestand in de browser te laden.

Ter best practice kun je met behulp van tools zoals [wfuzz](https://github.com/xmendez/wfuzz) of [ffuf](https://github.com/ffuf/ffuf) en [een wordlist](https://raw.githubusercontent.com/drtychai/wordlists/master/intruder/lfi.txt) fuzzen naar logbestanden op je target. Zodra je toegang hebt tot een logbestand kun je - afhankelijk van wat er gelogd wordt - de log poisonen. 

![image]()

In onderstaande voorbeeld wordt de Apache2 access.log (/var/log/apache2/access.log) gebruikt. Omdat onder andere de User-Agent header van de webrequests naar de server hierin worden gelogd, wordt dit de point of entry. Met behulp van bijvoorbeeld BurpSuite, cURL of de networktab in de browser devtools kun je een nieuw request maken met je PHP payload in de User-Agent header. 

```<?php system($_GET['cmd']);```



