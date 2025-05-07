
# ARM SystemReady IR *"just works"* U-boot and TF-A Firmware Binaries for RZ/G
## Overview
This repository contains the **binaries** used for SystemReady IR
certification on the following RZ platfoms:
- RZ/G2L
- RZ/G2LC
- RZ/G2UL
- RZ/V2L
## Licenses 
| Binary file |License folder|License files|
|--|--|--|
|bl2_bp-smarc<-platform><_pmic>.srec|trusted-firmware-a|generic_MIT, LICENSE, license.rst|
|bl2_bp-smarc<-platform><_pmic>.bin|trusted-firmware-a|generic_MIT, LICENSE, license.rst|
|fip-smarc<-platform><_pmic>.srec|u-boot|Exceptions,gpl-2.0.txt,README|
|fip-smarc<-platform><_pmic>.bin|u-boot|Exceptions,gpl-2.0.txt,README|
|capsule<-platform><_pmic>.bin|u-boot|Exceptions,gpl-2.0.txt,README|
|Flash_Writer_SCIF<_PLATFORM>_SMARC<_PMIC>_DDR4_2GB_1PCS.mot|u-flash-writer|LICENSE.md|
## Source code
| Source Code |Git repository|
|--|--|
|u-boot|https://github.com/renesas-rz/renesas-u-boot-cip/|
|trusted-firmware-a|https://github.com/renesas-rz/rzg_trusted-firmware-a|
|Yocto build |https://github.com/renesas-rz/meta-renesas|

## ARM SystemReady Certification
Please refer to the ARM website to find the latest list of certified Renesas RZ platform certificates.
The details can be found at the following link:
https://armkeil.blob.core.windows.net/developer/Files/pdf/certificate-list/arm-systemready-ir-renesas.pdf

The following RZ platforms have been awarded with SystemReady IR certificates, or are pending final review and publication of certificates by ARM and/or ARM Certification Partner

| MPU | Platform |SystemReady IR Version |ARM Certified/Certification in Review|
|--|--|--|--|
|RZ/G2M|...to add|ASR-v1.1|Certified|
|RZ/G2L|...to add|ASR-v1.2|Certified|
|RZ/G2L|...to add|ASR-v2.1|*Certification in Review*|
|RZ/G2LC|...to add|ASR-v1.2|Certified|
|RZ/G2UL|...to add|ASR-v1.2|Certified|
|RZ/G2UL|...to add|ASR-v2.1|*Certification in Review*|
|RZ/V2L|...to add|ASR-v1.2|Certified|
|RZ/V2L|...to add|ASR-v2.1|*Certification in Review*|


## Key Changes From the Standard BSP 
The following are the key differences between the standard BSP firmware and this ARM system ready firmware
| Subject            | Standard BSP Firmware| ASR SystemReady Firmware    |
|--------------------|--------------|------------------|
| u-boot version     | v2021.10/rz  | v2023.10/rz-asr  |
| FIP Base address   | 0x1D200      | 0x20000          |
| Boot Sequence      | CIP Linux    | Distros OS       |

## Programming Instructions - SCIF mode
Please refer to Renesas Quick Start Guide provide withn the BSP for the platform specific details
- The key difference for this procedure is the FIP Base address change to 0x20000
- The below section shows the example of SCIF mode using new FIP address
### SCIF Mode Programming Steps (With Changed FIP Address )
- Change dip switch SW11 to SCIF dowload mode ( 1=OFF 2=ON 3=OFF 4=ON)
- Reset the board. 
  - The following message will be displayed
<pre> 
SCIF Download mode
(C) Renesas Electronics Corp.
-- Load Program to SystemRAM ---------------
please send !
</pre>
- Send the Flash_Writer utility .mot file 
    - RZG2L:  Flash_Writer_SCIF_RZG2L_SMARC_PMIC_DDR4_2GB_1PCS.mot
    - RZG2LC: Flash_Writer_SCIF_RZG2LC_SMARC_DDR4_1GB_1PCS.mot
    - RZG2UL: Flash_Writer_SCIF_RZG2UL_SMARC_DDR4_1GB_1PCS.mot
    - RZV2L:  Flash_Writer_SCIF_RZV2L_SMARC_PMIC_DDR4_2GB_1PCS.mot

- Once the flash writer send successfully the command line will be displayed
<pre> > </pre>
- Enter XLS2, followed by the addresses of both the RAM and Qspi flash for BL2
<pre> 
>XLS2
> Please Input Program top address : H'11E00
> Please Input Qspi Save Address   : H'00E00
</pre>
- Send BL2 srec file
    - RZG2L:  bl2_bp-smarc-rzg2l_pmic.srec
    - RZG2LC: bl2_bp-smarc-rzg2lc.srec
    - RZG2UL: bl2_bp-smarc-rzg2ul.srec
    - RZV2L:  bl2_bp-smarc-rzv2l_pmic.srec
- Wait for the BL2 write to complete 
- Enter XLS2, followed by the addresses of both the RAM and Qspi flash for the FIP
<pre> 
>XLS2
> Please Input Program top address : H'00000
> Please Input Qspi Save Address   : H'20000
</pre>
- Send the FIP .srec file
    - RZG2L:  fip-smarc-rzg2l_pmic.srec
    - RZG2LC: fip-smarc-rzg2lc.srec
    - RZG2UL: fip-smarc-rzg2ul.srec
    - RZV2L:  fip-smarc-rzv2l_pmic.srec
- Change dip switch SW11 to QSPI mode ( 1=OFF 2=OFF 3=OFF 4=ON)
- Reset the board


