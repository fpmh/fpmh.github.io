---
layout: default
permalink: /technieken/LFI-naar-RCE
permalink_name: /technieken/LFI-naar-RCE
title: hmpf's notitieblok
---


# LFI naar RCI middels log poisoning
Local File Inclusion is te escaleren naar Remote Code Execution middels log poisoning. 

Hoewel je met een LFI vulnerability niet direct kan schrijven in bestanden, wordt wellicht de data die jij genereerd wel opgeslagen in logbestanden. Het meestvoorkomende voorbeeld hiervan is het /var/log/apache2/access.log bestand. Dit bestand logt de requests die gemaakt zijn naar de webserver, **inclusief headers**. Door een simpele PHP shell in bijvoorbeeld de User-Agent header te verwerken, kunnen we zo onze LFI escaleren naar RCE, door simpelweg het logbestand te laden in de browser.

Ter best practice kun je met behulp van tools zoals wfuzz of ffuf en [deze wordlist](https://raw.githubusercontent.com/drtychai/wordlists/master/intruder/lfi.txt) fuzzen naar logbestanden op je target. Zodra je toegang hebt tot een logbestand waar je (indirect) data in kan schrijven kun je de log poisonen. 

![image]()

Welke data je zal gaan manipuleren is afhankelijk van welke data opgeslagen wordt in de logs. In dit geval wordt de Apache2 access.log (/var/log/apache2/access.log) gebruikt. Omdat onder andere de User-Agent header van de webrequests naar de server hierin worden gelogd, wordt dit onze point of entry. Met behulp van BurpSuite, cURL of de network tab in je browsers devtools kun je een nieuw request maken met je PHP payload in de User-Agent header. 

```php
<?php system($_GET['cmd']);```

![image]()
