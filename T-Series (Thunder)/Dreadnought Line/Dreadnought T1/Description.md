### Technical Specifications<br>& General Information:
---
Processor, Graphics & Shared Memory (APU):

> Semi-custom AMD APU derived from PlayStation 5 silicon, combining a Zen 2 CPU cluster with an RDNA 2 GPU on a shared GDDR6 memory pool — delivering console-grade performance in an embedded, graphics card form factor, perfect for extra customization.

```text
- AMD Oberon 6 cores [Zen2] / 12 threads 3.5GHz
- AMD Cyan Skillfish 36 CU's RDNA2 1.5GHz
- 16GB GDDR6 V/RAM Bus:256bit/Bandwidth:448 GB/s
```

<table align="center" width="100%">
  <tr>
    <td width="60.5%" align="left">
      <img src="Media/AMD BC 250 (Main Board) Prototype.jpg" alt="Figure 1" width="100%"/>
    </td>
    <td width="39.5%" align="left">
      <img src="Media/Cyberdeck Overview.jpg" alt="Figure 2" width="100%"/>
    </td>
  </tr>
</table>

Internal Electrical Routing Scheme:

> Basic map of internal power and data paths between components. The left column represents combined power+data connections (USB, DisplayPort, Ethernet); the right column shows dedicated power distributed through USB, SATA-to-Molex and Molex-to-USB adapters.

```text
[Power + Data]          [Device/Conector]         [Pure Power]
USB 2 ->                LoRaWAN                              |
Display Port -> HDMI -> Arzopa A1S 14" <- USB <- Molex <- SATA
Ethernet ->             RJ-45 Extensor Port                  |
USB 3 ->                2.5" SATA SSD                 <- USB 2
USB 3 ->                USB Hub        <- USB <- Molex <- SATA
```

Gaming & Overall Performance:

> The BC-250 delivers solid performance, suitable for LLM's up to 10GB/12GB in size depending on configuration and, in gaming, excelling at 1080p, comparable to an RX 6600 or GTX 1660 Ti, with basic Ray Tracing capabilities and even FSR Frame Generation.
-  Cyberpunk 2077<br>
  ``1080p High + FSR 3.1 Quality + RT (lighting only): 60-70 FPS``
-  Counter-Strike 2<br>
  ``Works Smoothly at 1080p: 100+ FPS in most confisgurations``
-  The Last of Us Part I<br>
  ``1080p Medium-High + FSR 3.1 Quality (RT Off): 60 FPS``
