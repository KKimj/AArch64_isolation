FVP v8 Foundation Platform 11.11 build 34
==============================================

Copyright (C) 2017,2018,2019 ARM Limited, All rights reserved

Contents:

* Model overview
* Documentation and support
* Known issues and limitations

Model overview
--------------

This v8 Foundation Platform provides a basic ARMv8 platform environment suitable
for running bare metal semi-hosted applications, and Linux OS booting. The
platform provides:

- An ARMv8-A processor cluster with 1-4 CPUs
- 8GB of RAM
- 4 PL011 UARTs exposed as host TCP sockets
- Platform peripherals including a Real-time clock, Watchdog timer,
  Read-time timer and Power controller
- Secure peripherals including a Trusted watchdog, Random number generator,
  Non-volatile counters and Root-key storage
- A network device model connected to host network resources
- A block storage device implemented as a file on the host
- A small system register block
- A CLCD that allows GUI visualization
- Debug capabilities by enabling a CADI server

The ARMv8-A processor model implements:

- AArch64 at all exception levels
- AArch32 support at EL0/EL1
- Little and big endian at all exception levels
- Generic timers
- Self-hosted debug
- GICv2 and optional GICv3 memory mapped CPU interfaces and distributor
- No support for Thumb2EE
- No support for Crypto extensions
- ARMv8 versions 8.0, 8.1, 8.2, 8.3, 8.4 and 8.5
- Scalable Vector Extension

The models runs as fast as possible unless all the CPUs in the cluster are
in WFI/WFE, in which case the model idles until an interrupt or external
event occurs.

Caches are modelled as stateless and there is no write buffer. This gives
the effect of perfect memory coherence on the data side. The instruction
side has a variable size prefetch buffer so requires correct barriers to
be used in target code to operate correctly.

There are two styles of network support:

NAT, IPv4 based networking provides limited IP connectivity by using user
level IP services. This requires no extra privileges to set up or use but
has inherent limitations. System level services, or services conflicting
with those on the host can be provided using port remapping.

Bridged networking requires the setup of an Ethernet bridge device. This
usually requires administrator privileges. See the documentation in the
Linux bridge-utils package for more details.

Documentation and support
-------------------------

All model configuration is provided by command line arguments. Run the model
with the --help command line option to get a summary of the available
commands.

Further model documentation including the modelled peripherals, system memory
maps, interrupt routing and semi-hosting support can be found in DUI0677, the
Foundation Platform User Guide.

ARM does not offer direct customer support for the Foundation Platform. For
technical support use the ARM support forums, https://community.arm.com/groups/tools.

Enhancements and changes in the 11.8 build 34 release
-----------------------------------------------------
- In addition to the previous architecture versions, a command line option for
  --arm-v8.5 has been added. ARMv8.5 is the new default for the platform, being
  the newest version for ARMv8.

Enhancements and changes in the 11.3 build 34 release
-----------------------------------------------------
- In addition to the previous architecture versions, a command line option for
  --arm-v8.4 has been added. ARMv8.4 is the new default for the platform, being
  the newest version for ARMv8.

Enhancements and changes in the 11.1 build 22 release
-----------------------------------------------------

- The following parameters have been added to the model that allow the use of
  semihosting features:

  --(no-)semihost                   enable/disable semihosting (default: enabled)
  --semihost-cmd=cmd                a string used as the semihosting cmd-line
  --semihosting-heap_base=address   Virtual address of heap base.
  --semihosting-heap_limit=address  Virtual address of top of heap.
  --semihosting-stack_base=address  Virtual address of base of descending stack.
  --semihosting-stack_limit=address Virtual address of stack limit.


Enhancements and changes in the 11.0 build 34 release
-----------------------------------------------------
- In addition to the previous architecture versions, a command line option for
  --arm-v8.3 has been added. ARMv8.3 is the new default for the platform, being
  the newest version for ARMv8.

- The Scalable Vector Extension has been added to the model. This is enabled
  by default but can be disabled with command line option --no-sve.
  SVE requires the architecture version selected to be ARM v8.2 or later.

- --gicv3 is selected by default. Previous versions of the model used the GICv2
  layout by default, which can be selected in this version using --no-gicv3

- The physical address size of the v8 CPU has been increased from 36 bits
  to 40 bits for compatibility with KVM software. (The address range of DRAM
  implemented in the platform has not changed).

Enhancements and changes in the 9.6 build 37 release
---------------------------------------------------------
- Three command line options have been added to control the ARMv8 version enabled,
  --arm-v8.0 --arm-v8.1 and --arm-v8.2 for all the current ARMv8 versions. ARMv8.0
  was the original platform as released until 9.5. ARMv8.1 enables some extra features
  in the cores. ARMv8.2 is the new default for the platform, being the newest version
  for ARMv8.

- Two command line options have been added to enable debug capabilities through
  use of the CADI server, --cadi-server and --print-port-number. The first option
  enables the cadi server while the second prints the port on the terminal in case
  several CADI servers are enabled for several platforms, to facilitate the connection
  to the model.

- A CLCD has been added to enable visualization in the model. This extends the
  capabilites of the Foundation Platform by allowing the user to run more complex
  software such as booting the latest Android image available at the Linaro page.

- The System ID Register Revision field has been updated to 3 to indicate
  version 9.6 of the platform.

Enhancements and changes in the 9.2 build 28 release
---------------------------------------------------------
- Two command line options have been added, --rate-limit and --no-rate-limit.
  Before this release the platform had rate limit enabled, which accounted for
  some development use cases, but limited performance unnecessarily for other
  users. By adding these parameters we offer a more configurable experience.
  The default is rate limit disabled.

- In order to offer a better out-of-the-box experience when testing the platform,
  the default number of cores running on the model has changed from 4 to 1.

Enhancements and changes in the 9.1 build 27 release
---------------------------------------------------------
- Changes in release naming: Foundation Model becomes Foundation Platform for
  consistency. Also the version number changes from 2.1 to 9.1 to jointly
  release the Foundation Platform with Fast Models.
  (SDDKW-29083/SDDKW-27924)

- The --quiet command line option caused the Foundation Platform to print a
  virtioblockdevice error and exit on startup. This has been fixed.
  (SDDKW-28056)

- Virtio-p9 functionality has been added to the Foundation Platform on this
  release. This is controlled by the --p9-root-dir option.
  (SDDKW-28947)

- AA64PFR0_EL1.GIC was reporting the incorrect value when --no-gicv3 was used.
  This has been corrected.
  (TA-603008/SDDKW-28608)

- The System ID Register Revision field has been updated to 2 to indicate
  version 9.1 of the platform.
  (SDDKW-29537)


Enhancements and changes in the 9.0 build 28 release
---------------------------------------------------------
- Significant performance improvements have been made to the code translation
  engine in the Foundation Model, particularly when running full OSes.

- The AP_REFCLK CNTCTL is no longer a secure device. It may now be accessed
  by Secure and Non-secure memory accesses.
  (SDDKW-25449/SDDKW-25785)

- The AP_REFCLK counter was not correctly connected to a reference clock
  or the CPU cluster's counter port. As a result AP_REFCLOCK failed to
  generate interrupts or timer event streams when programmed to do so.
  This has been fixed.
  (SDDKW-25785/SDDKW-27432)

- In GICv2 compatibility mode (--no-gicv3), the GIC did not correctly send
  wake requests to the Power Controller when it received interrupts for a
  powered off CPU core. This has been corrected.
  (SDDKW-23820/SDDKW-23854/SDDKW-25857)

- The Power Controller cluster power off has been modified so that powered
  on cores in STANDBYWFI are powered off either if the core has pending
  power off, or the cluster has a pending power off. This avoids a case
  where a cluster's L2 could be powered off, but a core within the cluster
  is remains powered on and is woken from STANDBYWFI without a powered L2.
  (SDDKW-25579)

- The Power Controller cluster affinity level was misconfigured causing
  PSCI firmware to incorrectly power off all cores. This has been fixed.
  (SDDKW-27433)

- A significant memory leak when repeatedly modifying page tables and
  accessing memory was introduced in Foundation Model 0.8 build 5206.
  This has been fixed.
  (TA-571741/SDDKW-25515/SDDKW-27242)

- A system reset function has been added to the system configuration control
  register. Writing the value 0xC0900000 to it asserts and then clears the
  reset pins on all components in the system. The contents of RAM are not
  affected.
  (SDDKW-26250)

- The Foundation Model only attempts to open terminal xterms if the DISPLAY
  environment is set and not empty.
  (SDDKW-25310)

- The System ID Register Revision field has been updated to 1 to indicate
  version 2.1 of the platform.
  (SDDKW-27624)

Enhancements and changes in the 0.8 build 5206 release
---------------------------------------------------------
- The Foundation Model has been revised to support the ARM Trusted Base
  System Architecture (TBSA) and Server Base System Architecture (SBSA).
  A number of peripherals have been added with corresponding changes in
  the memory map. These changes are documented in DUI0677 the Foundation
  Model User Guide, issue C and later.

- An optional GICv3 controlled by the --(no-)gicv3 command line options
  has been added to the Foundation v2 Model.

- The system register block has been updated to add an ID register. This
  allows target code to distinguish between Foundation v1 and Foundation v2
  Models; and between models using the GICv2 legacy memory map, and GICv3 map.

- Initialisation and reset issues with the virtio block device affecting
  Linux KVM have been fixed.
  (TA-566785/SDDKW-23448/SDDKW-24825)

Enhancements and changes in the 0.8 build 4850 release
---------------------------------------------------------

- To bring the Foundation Model in line with other Fast Models packages, the
  directory structure of the install has been changed. See section 2.1 of
  DUI0677, the Foundation Model User Guide for details.

- Stability and performance improvements to the virtio block device when
  used for I/O intensive workloads.
  (TA-536363/TA-543995/SDDKW-20727)

- Allow virtio block devices to use read-only images and add a --read-only
  option to force writeable images to be read-only.
  (SDDKW-21945)

- Add the --nsdata command line option to allow data to be loaded into
  non-secure memory when the --secure-memory is enabled.
  (SDDKW-21078)

- Add a --uart-start-port option to configure the TCP port range used by
  the Foundation Model UART to listen for connections. This allows
  many Foundation Models to be started on the same host.
  (SDDKW-21677)

- Allow the system global counter module to count using real time instead
  of simulated time when the --use-real-time option is provided.
  (SDDKW-20933)

Known issues and limitations
----------------------------

This version of the model has the following issues and limitations:


NAT (aka user mode) network support has known issues which cause networking
to intermittently slow down.

The processor reports an MIDR with 0x41 as the implementer code, 0xF as the
architecture code, 0xD00 as the part number, and the major and minor revision
of the model.

UARTs cannot be configured as enabled at reset. Bootloaders will need to do
this manually.

Foundation Platform 11.11 has been tested with Linaro 17.07 images.
Please, note that they require --arm-v8.0 option in order to boot successfully.


FVP v8 Foundation Platform 11.11 build 34

