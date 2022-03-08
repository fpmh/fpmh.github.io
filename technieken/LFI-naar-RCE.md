---
layout: default
permalink: /technieken/LFI-naar-RCE
permalink_name: /technieken/LFI-naar-RCE
title: hmpf's notitieblok
---


# LFI naar RCI middels log poisoning
Local File Inclusion is te escaleren naar Remote Code Execution middels log poisoning. 

Hoewel je met een LFI vulnerability niet direct kan schrijven in bestanden, wordt wellicht de data die jij genereert wel uitgeschreven in logbestanden. Een voorbeeld hiervan is het Apache2 access.log bestand. Hierin worden webrequests die gemaakt zijn naar de server **inclusief headers** gelogd. Door een PHP payload in bijvoorbeeld de User-Agent header te verwerken, kunnen we zo onze LFI escaleren naar RCE, door het logbestand in de browser te laden.

Met behulp van tools zoals [wfuzz](https://github.com/xmendez/wfuzz) of [ffuf](https://github.com/ffuf/ffuf) en [een wordlist](https://raw.githubusercontent.com/drtychai/wordlists/master/intruder/lfi.txt) fuzzen naar logbestanden op je target. Zodra je toegang hebt tot een logbestand kun je - afhankelijk van wat er gelogd wordt - de log poisonen. 

![image]()

In onderstaande voorbeeld wordt de Apache2 access.log (/var/log/apache2/access.log) gebruikt. Omdat onder andere de User-Agent header van de webrequests naar de server hierin worden gelogd, wordt dit de point of entry. Met behulp van bijvoorbeeld BurpSuite, cURL of de networktab in de browser devtools kun je een nieuw request maken met een PHP payload in de User-Agent header. 

```php
<?php system($_GET['cmd']); ?>
```

Zodra je nu via LFI /var/log/apache2/access.log opvraagt, zal de PHP code uitgevoerd worden. De PHP payload verwacht een GET-request, en zal de waarde van de parameter 'cmd' uit gaan voeren. 

