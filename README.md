# OneThunder - One Thunderbolt cable connects everything. 
A project for me to practice high-speed PCB layout, and firmware security studies. \
Aims to build a ITX-sized Thunderbolt 5 docking station

## TLDR; Don't waste time try to DIY, Trust me
#### It's more cost-effective to buy a branded one (If there is one that meets your needs)

8 layers PCB, 0201 SMD, lots of BGA components\
PCB trace impedance, length matching\
Firmware and driver issues, \
No contract, No NDA, No FAE support\
Everything is a mess, it's a nightmare

## Key Specification
### Form factor
Standard Mini-ITX form factor (170 mm × 170 mm)

### Chips onboard
* JHL9480, The heart of this project, Thunderbolt 5 Accessory Controller.
* Prom21 aka. B650, PCIe switch chip supports PCIe Gen 4.
* Prom19 aka. B550, PCIe switch chip supports PCIe Gen 3.
* AQC113CS, 10Gbps ethernet controller.
* VMM6210, Video interface protocol converter.
* PMG1-S3, USB-C Port controller MCU.

### Connectivity
<details><summary>External</summary>
<ul>
  <li>Display Interface<ul>
    <li>1× HDMI 2.1 Port (8k30)</li>
    <li>1× DisplayPort 1.4 Port</li>
  </ul></li>
  <li>Thunderbolt 5<ul>
    <li>1× UFP, With 120W PD </li>
    <li>2× DFP, With 35W PD </li>
  </ul></li>
  <li>USB<ul>
    <li>4× USB TYPE-A (Gen 2x1 10Gbps)</li>
    <li>1× USB TYPE-E (Gen 2x2 20Gbps)</li>
    <li>1× USB 20 Pin Header (Gen 2x1 (10Gbps))</li>
    <li>1× USB 9 Pin Header (2.0 480Mbps)</li>
  </ul></li>
  <li>Ethernet<ul>
    <li>1× 10Gbps RJ-45</li>
  </ul></li>
</ul>
</details>

<details><summary>Internal</summary>
<ul>
  <li>NVMe<ul>
    <li>2× M.2 M-Key Gen 4x4 Slots</li>
    <li>1× M.2 M-Key Gen 3x4 Slots (Share lane with PCIe)</li>
  </ul></li>
  <li>SATA<ul>
    <li>4× SATA 3.0 6Gbps Ports </li>
  </ul></li>
  <li>PCIe<ul>
    <li>1× PCIe Gen 3x4 open-ended slot (Share lane with NVMe)</li>
  </ul></li>
  <li>ATX Power<ul>
    <li>1× 24 Pin ATX PSU Header</li>
    <li>1× 8 Pin ATX PSU CPU Header, Required for USB PD</li>
  </ul></li>
  <li>MISC<ul>
    <li>4× 4 Pin 12v Fan Header</li>
    <li>1× 2 Pin 3v Power LED Header</li>
    <li>1× 2 Pin 3v HDD LED Header</li>
  </ul></li>
  </ul>
</details>


## Getting Started
>[!TIP]
>The project is still in the research phase. \
>Nothing was done yet. \
>Schematic/PCB files is not available.

### Current Progress
Tested on MAC mini M4 Pro, \
The concept works, Performance reaches Thunderbolt 5 PCIe Gen 4x4 (64Gbps) tunnel speed \
Connection: MAC Mini ---TBT5---> JHL9480 ---PCIe 4x4---> Prom21 ---> NVMe SSDs, SATA HDDs \
![POC](/images/prom21_m4mac.jpg?raw=true)

## Project Phases (aka. TODO List)
* Alpha(α)
  * Creating chip symbols and packages in EDA[^1]
    - [X] JHL9480   | Library done, but unverified
    - [X] Prom21    | Library done, Chip works as expected 
    - [ ] Prom19
    - [ ] AQC113CS
    - [ ] VMM6210
    - [ ] PMG1-S3
  * Create "DataSheep"[^2] for each chip
    - [X] JHL9480   | "DataSheep" is partially completed, With unverified parameters
    - [X] Prom21    | "DataSheep" is completed, Chip works as expected
    - [ ] Prom19    | Spec is available, "DataSheep" TBD
    - [X] AQC113CS  | "DataSheep" is partially completed, With unverified parameters
    - [X] VMM6210   | Datasheet is available, No "DataSheep" is needed
    - [X] PMG1-S3   | Datasheet is available, No "DataSheep" is needed
  * Firmware reverse engineering
    - [ ] JHL9480
    - [X] Prom21    | Tested, Everything works, SerDes cfg can be improved
    - [ ] Prom19
    - [ ] AQC113CS
    - [ ] VMM6210
    - [ ] PMG1-S3
* Beta(β) 
  * Separated hardware design prototypes
    - [ ] JHL9480
    - [X] Prom21  | PCIe AIC Done, Works on MAC(ARM) and Intel(X86) Based PCs 
    - [ ] Prom19
    - [ ] AQC113CS
    - [ ] VMM6210
    - [ ] PMG1-S3
  * Firmware testbench
    - [ ] JHL9480
    - [X] Prom21  | Tested, PCIe, AHCI, xHCI Works, Performance normal
    - [ ] Prom19
    - [ ] AQC113CS
    - [ ] VMM6210
    - [ ] PMG1-S3
* Final(Press F to Pay Respects)
  * Use some magic to glue everything together.
    - [ ] Some magic

### About EDA
Hardware design are planned to be done in KiCad.[^1]

### About Firmware
The PCIe switch, SATA(AHCI), USB(xHCI) controller (Prom 21, 19), \
Thunderbolt controller (JHL9480), \
Ethernet controller (AQC113CS), \
Video interface protocol converter (VMM6210), \
The firmware for the above chips will be released in binary format. 

The USB-C PD (PMG1-S3) Firmware will be released in source code and binary, \
ModusToolbox software from Infineon is required to customize the firmware.

## Author
Rei (霊) (rei@kolabo.dev)\
GitHub / Discord [@himko9] 

## Phase History
* αlpha
  * α1 (Current Phase) 
    * Project started, Creation of this Readme.md file
    * The POC works on my M4 Mac mini (M4 PRO)

## License
This project is licensed under the MIT License - see the LICENSE file for details



[^1]: I'm currently converting from Fusion360 to KiCad. While the project is still in alpha/beta, PCB/schematic/library design will be done in Fusion360 (Converted back to Eagle format for KiCad).
[^2]: "DataSheep" is a simplified version of the "Datasheet", contains the key specification, but without the programming (register) guide