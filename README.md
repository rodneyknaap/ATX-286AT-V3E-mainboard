# ATX-286AT-V3E-mainboard
This project features the REV3E design for a 80286 ATX mainboard based on the IBM 5170 AT PC

## 286 PC/AT CPLD ATX mainboard revision 3E

This project is REV3E of my open source 286 AT ATX PC mainboard design using CPLD technology.
As with the first revision, the design is based on the original IBM 5170 PC/AT concept.

The project consists of an ATX mainboard supporting XMS and EMS according to the EMS drivers created by sqpat here on GitHub.

SRAM ICs can be added as a set to each low and high byte module which will result in 2MB of SRAM per set of a high byte and low byte SRAM chip.
The memory layout is printed on the mainboard. The sets are numbered 1 to 8, 1 to 4 are 8MB of XMS, 5 to 8 are 8MB of which at the moment the first 4MB double as XMS or EMS RAM.
So a builder who wants to have XMS and EMS should minimally populate RAM sets 5 and 6 to have 4MB of EMS. The sets 5 and 6 can then be added to what is populated in sets 1 to 4 to increase that XMS capacity if EMS is not in use. Minimum to populate would be set 1 only which would lead to 2MB of XMS only. For example sets 1, 5 and 6 could be populated which results in 6MB of XMS of which 4MB can be used as EMS when EMS in use. Usually XMS and EMS are not combined at the same time because EMS is used in real mode of the 286 CPU.

The mainboard supports the 80286 16 bit CPU, bus driving is now completely done by CPLD logic.
A Harris 286 rated at 20MHz is recommended, certain older manufacturing dated chips are able to run at much higher clock speeds in other systems.
A good manufacturing year is 1992 for these Harris 20MHz chips. Later ones may not clock as high as earlier ones.

Basically for composing the core PC/AT system based on IBM 5170 technology all logic is now contained within 5 CPLD ICs.
A few TTL chips are added for the printer port output enabled parts. When using the printer port, make sure to check the pins of the header in the schematic and make sure your cable matches those.
Printer port has not been tested yet in the current edition, however has been verified in other revisions using TTL ICs to comprise a printer port.

## Purpose and permitted use, cautions for a potential builder of this design
This project was created for historical purposes out of love for historical computing designs and for the purpose of enabling computing enthousiasts with a sufficient level of building and troubleshooting expertise to be able to experience the technology by building and troubleshooting the hardware described in this project. Due to the level of this project, it may be suitable as a project for students to get into. If there are any questions from teachers who like to teach about this technology I would be happy to answer them. It may be really interesting to analyse the elaborate and complex CPU timing and 8 bit to 16 bit data byte translation and DMA mechanisms in an educational setting.

Besides the GPL3 license there are a few warnings and usage restrictions applicable:
No guarantees of function or fitness for any particular or useful purpose is given, building and using this design is at the sole responsibility of the builder.

Do not attempt this project unless you have the necessary electronics assembly expertise and experience, and know how to observe all electronics safety guidelines which are applicable.

It is not permitted to use the computer built from this design without the assumption of the possibility of loss of data or malfunction of the connected device. To be used strictly for personal hobby and experimental purposes only. No applications are permitted where failure of the device could result in damage or injury of any kind.

If you plan to use this design or any part of it in new designs, the acknowledgement of the designer and the design sources and inspirations, historical and modern, of all subparts contained within this design should be included and respected in your publication, to accredit the hard work, time and effort dedicated by the people before you who contributed to make your project possible.

No guarantee for any proper operation or suitability for any possible use or purpose is given, using the resulting hardware from this design is purely educational and experimental and not intended for serious applications. Loss of data is likely and to be expected when connecting any storage device or storage media to the resulting system from this design, or when configuring or operating any storage device or media with the system of this design.

When connecting this system to a computer network which contains stored information on it, it is at the sole responsibility and risk of the person making the connection, no guarantee is given against data loss or data corruption, malfunctions or failure of the whole computer network and/or any information contained inside it on other devices and media which are connected to the same network.

When building this project, the builder assumes personal responsibility for troubleshooting it and using the necessary care and expertise to make it function properly as defined by the design. You can email me with questions, but I will reply only if I have time and if I find the question to be valid. Which will probably also lead to an update here. I want to primarily dedicate my time to new project development, I am not able to do any user support, so that's why I provide the elaborate info here which will be expanded if needed.

These disclaimers and conditions may seem unfriendly but remember that they are by no means meant to reflect on you as a reader personally or individually, just imagine that all possible people and unwise use and situations still need to be covered since this project is openly published on the internet, which means any person on the planet is able to find the information, thus also the comments are meant for every possible person who wants to use the information. I am reasonably assuming that 99% of people will be civilized enough to observe respect and common sense.

# REV3E design of a PC/AT mainboard based on CPLD technology  
For background information and previous acknowledgements, please first see the Rev3(D) design repository. 
The information provided here is purely meant to describe the differences and changes in the new REV3E design.
For clarity, only the files relevant to REV3E are featured here.
For completeness, please read the information in the REV3 repository first to understand the background of the project.  

The design features SRAM footprints on the mainboard rather than featuring modules. This makes construction of the board more simple and straight forward.
The SRAMs need to be populated in sets, one for the low and high byte of a set. Starting with RAM set 1, this is the minimum SRAM for the project and would amount to 2MB to be used as conventional and XMS RAM. If you want EMS RAM, then add sets of SRAMs in positions 5 and 6, which would create 4MB of EMS. With this, for example, the system can run the RealDOOM project as under development by sqpat here on GitHub. When populating set 1, 5 and 6, the EMS sets 5 and 6 can be dual purposed as additional XMS while EMS is not in use. So that could be made to amount as 6MB XMS sharing 4MB with the EMS system when loading the EMS driver. The system needs a RESET to disable the EMS system after loading the EMS driver, and the full 6MB of XMS will become available again after a RESET or power cycle.

The System controller contains cycle control logic which switches the 286 on a lower clock speed for speed sensitive memory and I/O operations. When these areas are detected, an alternate clock mode is implemented dynamically in specific points of the CPU clock cycles in order not to disrupt the clock cycle transitions while the CPU is executing cycles.

The Address driver/decoder CPLD in addition contains DMA specific logic in order not only to generate the system address bus from the CPU but also from the DMACs and a bus Master if present on the slot connector. The Address driver/decoder is also decoding all XMS and EMS memory in the system. For this purpose the Address driver/decoder communicates with the EMS controller in order to determine the selection of XMS or EMS memory for each area of system memory. By default, each page block of system memory is initially programmed to be a default page which is located in the XMS memory chips. When a certain block is reprogrammed in the EMS page registers, it will be remapped into any programmable block inside the EMS memory pool. By programming a memory block back to default, the original memory contents are mapped back into that 16KB page block.

# Status of the project  
The REV3D project PCB layout and CPLD chipset projects have been created and awaiting to transfer the components to a REV3E board for further testing.

Initial quartus projects have been pin verified against the mainboard schematic and layout.

Kind regards,

Rodney

Last updated april 6th, 2026.
