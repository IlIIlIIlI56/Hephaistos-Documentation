<table align="center">
  <tr>
    <td align="center"><img src="Media/Figure 1.png" alt="Figure 1"/></td>
    <td align="center"><img src="Media/Figure 2.png" alt="Figure 2"/></td>
  </tr>
</table>

### Technical Specifications<br>& General Information:
---
Processor, Graphics & Shared Memory (APU):

> Semi-custom AMD APU derived from PlayStation 5 silicon, combining a Zen 2 CPU cluster with an RDNA 2 GPU on a shared GDDR6 memory pool — delivering console-grade compute in an embedded, fanless-capable form factor.

```text
- AMD Oberon 6 cores [Zen2] / 12 threads 3.5GHz
- AMD Cyan Skillfish 36 CU's RDNA2 1.5GHz
- 16GB GDDR6 V/RAM Bus:256bit/Bandwidth:448 GB/s
```

Internal Electrical Routing Scheme:

> Maps all internal power and data paths between components. The left column represents combined power+data connections (USB, DisplayPort, Ethernet); the right column shows dedicated power rails distributed through Molex-to-SATA tap adapters.

```text
[Power + Data]          [Device/Conector]         [Pure Power]
USB 2 ->                LoRaWAN                              |
Display Port -> HDMI -> Arzopa A1S 14" <- USB <- Molex <- SATA
Ethernet ->             RJ-45 Extensor Port                  |
USB 3 ->                2.5" SATA SSD                 <- USB 2
USB 3 ->                USB Hub        <- USB <- Molex <- SATA
```
