<td align="center" width="100%">
  <img src="Media/Electrichal Scheme.png" alt="Electrical Conection Scheme"/>
</td>

### Technical Specifications<br>& General Information:
---

Processor, Graphics & Shared Memory (APU):
```text
- AMD Oberon 6 cores [Zen2] / 12 threads 3.5GHz
- AMD Cyan Skillfish 36 CU's RDNA2 1.5GHz
- 16GB GDDR6 V/RAM Bus:256bit/Bandwidth:448 GB/s
```
Internal Electrical Routing Scheme:
```text
[Power + Data]          [Device/Conector]         [Pure Power]
USB 2 ->                LoRaWAN                              |
Display Port -> HDMI -> Arzopa A1S 14" <- USB <- Molex <- SATA
Ethernet ->             RJ-45 Extensor Port                  |
USB 3 ->                2.5" SATA SSD                 <- USB 2
USB 3 ->                USB Hub        <- USB <- Molex <- SATA
```
