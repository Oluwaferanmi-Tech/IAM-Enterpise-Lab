# Module 0: Lab Setup

## What I did

Before touching any IAM concepts, I needed a working lab environment: a Windows Server 2025 domain controller, a Windows 11 client, and a Kali Linux box for security testing — all on one network. The path to get there wasn't the one I planned on.

## Steps

### Attempt 1: Local virtualization with UTM (Apple Silicon, x86 emulation)

My laptop is an M1 MacBook Pro, which meant the standard Windows Server 2025 evaluation ISO — an x86-64 build — couldn't run natively. UTM (free, open-source) supports x86 emulation on Apple Silicon via QEMU, so I started there.

![UTM emulation setup](images/01-utm-emulate-setup.png)

Getting the VM to boot at all required manually navigating a UEFI shell (`fs0:` / `fs1:`, `cd efi\boot`, `bootx64.efi`) since the firmware couldn't auto-detect the installer.

![UEFI shell navigation](images/02-uefi-shell.png)

Once past that, first-boot times under emulation were genuinely long — sometimes over an hour with no visual feedback, easy to mistake for a frozen VM.

### The product-key bug

After finally reaching Windows Setup, I hit a wall: `Setup has failed to validate the product key`, even after explicitly selecting "I don't have a product key." This turned out to be a known, documented bug in UTM's GitHub discussions affecting other users on the same emulation path — not something specific to my setup.

![Product key validation error](images/03-product-key-error.png)

### Attempt 2: Switching to native ARM64 virtualization

Rather than keep fighting emulation, I switched approaches entirely: instead of running the x86 build under emulation, I sought out an ARM64-native build of Windows Server 2025, which would run at full native speed via UTM's "Virtualize" mode instead of "Emulate."

This meant leaving the officially-supported Evaluation Center ISO behind, since Microsoft doesn't publish a stable ARM64 Server release — only Windows Insider Preview builds. Getting one required:

- Installing Homebrew and several command-line dependencies (`aria2`, `cabextract`, `wimlib`, and `chntpw` via a community tap)
- Using UUP dump to locate and download an ARM64 Insider Preview build
- Running a conversion script to assemble the downloaded files into a bootable ISO

![UUP dump build selection](images/04-uupdump-arm64-search.png)

The first build I tried failed with `EMPTY_FILELIST` — it had been removed from Microsoft's servers between being indexed and my attempt to download it, a known risk with fast-cycling Insider Canary builds. A second, more recently-indexed build succeeded.

![Successful ISO build](images/05-iso-build-success.png)

### Windows 11 and Kali Linux: the easy parts

By contrast, the other two VMs were straightforward:

- **Kali Linux** came from UTM's built-in gallery — a pre-built, official ARM64 image with no manual ISO work at all.
- **Windows 11** used a similar native-ARM approach via CrystalFetch, a tool purpose-built for fetching official Windows ARM64 images, and installed cleanly on the first real attempt.

![Kali and Windows 11 running](images/06-kali-and-win11.png)

### The pivot to Azure

Windows Server 2025 continued to be the sticking point even on the ARM64 path — build availability on UUP dump isn't guaranteed to stay stable, and I didn't want the reliability of my lab depending on which Insider build happened to still be hosted on a given day. At that point I made the call to stop fighting local virtualization for this one VM and move the environment to Azure instead.

This meant:
- Creating an Azure account and a single shared Virtual Network, so all three VMs could reach each other exactly as they would on one physical host
- Provisioning Windows Server 2025 (a genuine, officially-supported release — no emulation, no Insider builds) on that network
- Migrating Windows 11 and Kali Linux to Azure as well, to keep all three machines on one consistent, mutually-reachable network rather than splitting environments

![Azure VNet and three VMs](images/07-azure-vnet-three-vms.png)

Along the way, I hit an Azure-specific snag too — certain VM sizes returned `NotAvailableForSubscription` on a free trial account. Switching to a newer VM size series (rather than assuming it was a regional capacity issue) resolved it.

## Challenges

- **Architecture mismatch** was the root cause of almost everything here — Windows Server 2025's only stable release is x86, while Apple Silicon is ARM, and every workaround (emulation, Insider ARM builds) came with its own tradeoffs.
- **A genuine software bug** (the product-key validation failure) cost real time before I confirmed, via UTM's own GitHub discussions, that it wasn't something I was doing wrong.
- **Insider Preview build volatility** meant that even after solving the architecture problem, reliability wasn't guaranteed — Microsoft can pull a build between when it's indexed and when you try to use it.
- **Free trial cloud quotas** aren't uniform across VM sizes or series, and the error messages don't always make the actual cause obvious.

## What I learned

Troubleshooting infrastructure that doesn't cooperate is itself a core IAM/cloud administration skill, not a distraction from it. Diagnosing *why* something fails — an architecture mismatch, a known software bug, a licensing restriction, a quota limit — and knowing when to stop debugging and switch approaches is exactly the kind of judgment this field asks for. The pivot to Azure also meant I picked up real, hands-on platform experience (virtual networking, VM provisioning, quota troubleshooting) that wasn't part of the original plan but is directly relevant to SC-300.
