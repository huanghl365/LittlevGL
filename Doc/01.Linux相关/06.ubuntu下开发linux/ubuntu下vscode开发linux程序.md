# ubuntu下vscode搭建嵌入式Linux开发环境

## 安装vscode及插件

1. 官网下载deb安装包后执行 sudo dpkg -i XXX.deb 或者从文件夹双击.下载链接:https://code.visualstudio.com/Download ；

2. 安装以下插件

   1.C/C++

   2.C/C++ CLang command adaper

   3.C++ intellisense

   4.GBKtoUTF8

   5.include Autocomplete

   6.rainbow Brackets

3. 在vscode欢迎界面点击【Add workspace folder】，然后添加自己的linux内核源码根目录。

4. 新建应用程序目录（里面有你的程序源码和makefile文件），该目录可以存放在你想存放的任何路径下，然后 在vscode软件中点击【File】->【add folder to workspace】选项，选择刚刚建立的自己的程序目录，如图是我本人的内核源码目录和应用程序目录。

![](media/19.kernel_filename.png)内核文件路径

1. 【File】->【save workspace as …】保存工作空间,命名vscode_workspace。注:工作空间的名字与路径随意。

## 添加内核include目录

修改vscode配置文件，使其能找到头文件

在vscode 选择inux内核路径，然后 按快捷键”Ctrl + Shift + P”, 然后搜索>Edit configurations ， 单击后，会打开一个c_cpp_properties.json文件，该文件在.vscode里面，也可以手动打开。添加自己的内核目录

```
"includePath": [
                "${workspaceFolder}/**",
                "/home/book/NUC970/NUC970_Linux_Kernel/include",
                "/home/book/NUC970/NUC970_Linux_Kernel/arch/arm/include",
                "/home/book/NUC970/NUC970_Linux_Kernel/arch/arm/mach-nuc970/include"
            ],
```

## 添加宏定义

找到内核autoconf.h 文件，位于内核源码的/include/generated/路径下。

```
/home/book/NUC970/NUC970_Linux_Kernel/include/generated/autoconf.h 
```

autoconf.h 文件是C格式的，如下

```
#define CONFIG_HAVE_ARCH_SECCOMP_FILTER 1 
#define CONFIG_SCSI_DMA 1                 
#define CONFIG_KERNEL_GZIP 1              
#define CONFIG_ATAGS 1                    
#define CONFIG_CRC32 1                    
#define CONFIG_I2C_BOARDINFO 1            
#define CONFIG_AEABI 1     
………………………………………………………………
~~~~~~~~~~~~~~~~~~~~~~~~~
    等等等等
```

而我们配置文件c_cpp_properties.json里面宏定义必须去掉#define，且需要加双引号，比如

```
#define CONFIG_HAVE_ARCH_SECCOMP_FILTER 1 
需要改为
"CONFIG_HAVE_ARCH_SECCOMP_FILTER 1"

#define CONFIG_UEVENT_HELPER_PATH "/sbin/hotplug"
需要改为
"CONFIG_UEVENT_HELPER_PATH \"/sbin/hotplug\" "

#define CONFIG_DEFAULT_HOSTNAME "(none)"
需要改为
"CONFIG_DEFAULT_HOSTNAME \"(none)\" "

#define CONFIG_CMDLINE "noinitrd root=/dev/mtdblock2 rootfstype=yaffs2 rootflags=inband-tags console=ttyS0, 115200n8 rdinit=/sbin/init mem=64"

需要改为
"CONFIG_CMDLINE \"noinitrd root=/dev/mtdblock2 rootfstype=yaffs2 rootflags=inband-tags console=ttyS0, 115200n8 rdinit=/sbin/init mem=64\""

等等，上面对于宏定义自己带"的情况下需要加转义字符\
```

最终改过后的如下，还需在最前面加上`"__KERNEL__",` 总之不能让c_cpp_properties.json文件报错

```
"defines": [
                "__KERNEL__",
                "CONFIG_HAVE_ARCH_SECCOMP_FILTER 1                  ",
                "CONFIG_SCSI_DMA 1                                  ",
                "CONFIG_KERNEL_GZIP 1                               ",
                "CONFIG_ATAGS 1                                     ",
                "CONFIG_CRC32 1                                     ",
                "CONFIG_I2C_BOARDINFO 1                             ",
                "CONFIG_AEABI 1                                     ",
                "CONFIG_NETWORK_FILESYSTEMS 1                       ",
                "CONFIG_COMMON_CLK_DEBUG 1                          ",
                "CONFIG_CGROUP_DEVICE 1                             ",
                "CONFIG_ARCH_SUSPEND_POSSIBLE 1                     ",
                "CONFIG_SSB_POSSIBLE 1                              ",
                "CONFIG_MTD_CMDLINE_PARTS 1                         ",
                "CONFIG_USB_OHCI_LITTLE_ENDIAN 1                    ",
                "CONFIG_CRYPTO_MANAGER_DISABLE_TESTS 1              ",
                "CONFIG_HAVE_KERNEL_LZMA 1                          ",
                "CONFIG_ARCH_WANT_IPC_PARSE_VERSION 1               ",
                "CONFIG_UNIX_DIAG 1                                 ",
                "CONFIG_GENERIC_SMP_IDLE_THREAD 1                   ",
                "CONFIG_DEFAULT_SECURITY_DAC 1                      ",
                "CONFIG_RT_GROUP_SCHED 1                            ",
                "CONFIG_KTIME_SCALAR 1                              ",
                "CONFIG_HAVE_IRQ_TIME_ACCOUNTING 1                  ",
                "CONFIG_BQL 1                                       ",
                "CONFIG_DEFAULT_TCP_CONG \"cubic\"                    ",
                "CONFIG_UEVENT_HELPER_PATH \"/sbin/hotplug\" "         ,
                "CONFIG_DEVTMPFS 1                                  ",
                "CONFIG_PWM_NUC970 1                                ",
                "CONFIG_NAMESPACES 1                                ",
                "CONFIG_DEFAULT_MESSAGE_LOGLEVEL 4                  ",
                "CONFIG_BLK_DEV_BSG 1                               ",
                "CONFIG_ATAGS_PROC 1                                ",
                "CONFIG_CRYPTO_RNG2 1                               ",
                "CONFIG_MSDOS_FS 1                                  ",
                "CONFIG_IIO_KFIFO_BUF 1                             ",
                "CONFIG_SAMPLE_NUC970ADC 200                        ",
                "CONFIG_NUC970_UART10_PB1 1                         ",
                "CONFIG_CFG80211 1                                  ",
                "CONFIG_TREE_PREEMPT_RCU 1                          ",
                "CONFIG_HAVE_PROC_CPU 1                             ",
                "CONFIG_IOMMU_SUPPORT 1                             ",
                "CONFIG_ROMFS_BACKED_BY_BLOCK 1                     ",
                "CONFIG_USB 1                                       ",
                "CONFIG_ETHERNET 1                                  ",
                "CONFIG_HAVE_DMA_CONTIGUOUS 1                       ",
                "CONFIG_DQL 1                                       ",
                "CONFIG_COREDUMP 1                                  ",
                "CONFIG_BCMA_POSSIBLE 1                             ",
                "CONFIG_FORCE_MAX_ZONEORDER 11                      ",
                "CONFIG_MODULES_USE_ELF_REL 1                       ",
                "CONFIG_PRINTK 1                                    ",
                "CONFIG_TIMERFD 1                                   ",
                "CONFIG_MTD_CFI_I2 1                                ",
                "CONFIG_SHMEM 1                                     ",
                "CONFIG_MTD 1                                       ",
                "CONFIG_MIGRATION 1                                 ",
                "CONFIG_HAVE_ARCH_JUMP_LABEL 1                      ",
                "CONFIG_DEVTMPFS_MOUNT 1                            ",
                "CONFIG_NUC970_PWM1_NONE 1                          ",
                "CONFIG_NLS_CODEPAGE_437 1                          ",
                "CONFIG_MTD_NAND_IDS 1                              ",
                "CONFIG_EXPORTFS 1                                  ",
                "CONFIG_OLD_SIGSUSPEND3 1                           ",
                "CONFIG_HAVE_BPF_JIT 1                              ",
                "CONFIG_MTD_NAND_NUC970 1                           ",
                "CONFIG_IP_PNP 1                                    ",
                "CONFIG_NUC970_UART5 1                              ",
                "CONFIG_BOARD_NUC972 1                              ",
                "CONFIG_STACKTRACE_SUPPORT 1                        ",
                "CONFIG_LOCKD 1                                     ",
                "CONFIG_ARM 1                                       ",
                "CONFIG_NET_VENDOR_MICROCHIP 1                      ",
                "CONFIG_ARM_L1_CACHE_SHIFT 5                        ",
                "CONFIG_CPU_TLB_V4WBI 1                             ",
                "CONFIG_NUC970_PWM0_PB2 1                           ",
                "CONFIG_BSD_PROCESS_ACCT 1                          ",
                "CONFIG_CPU_COPY_V4WB 1                             ",
                "CONFIG_USB_STORAGE 1                               ",
                "CONFIG_BLOCK 1                                     ",
                "CONFIG_INIT_ENV_ARG_LIMIT 32                       ",
                "CONFIG_ROOT_NFS 1                                  ",
                "CONFIG_TMPFS_POSIX_ACL 1                           ",
                "CONFIG_BUG 1                                       ",
                "CONFIG_NUC970_PWM3_NONE 1                          ",
                "CONFIG_SPI 1                                       ",
                "CONFIG_NUC970_UART8 1                              ",
                "CONFIG_DEVKMEM 1                                   ",
                "CONFIG_VT 1                                        ",
                "CONFIG_SPLIT_PTLOCK_CPUS 999999                    ",
                "CONFIG_POWER_SUPPLY 1                              ",
                "CONFIG_WEXT_CORE 1                                 ",
                "CONFIG_NLS 1                                       ",
                "CONFIG_SPI_SPIDEV 1                                ",
                "CONFIG_IRQ_WORK 1                                  ",
                "CONFIG_SPI_BITBANG 1                               ",
                "CONFIG_USB_COMMON 1                                ",
                "CONFIG_NUC970_UART1_FC_PE 1                        ",
                "CONFIG_NLS_ISO8859_1 1                             ",
                "CONFIG_CRYPTO_WORKQUEUE 1                          ",
                "CONFIG_NUC970_ETH0 1                               ",
                "CONFIG_USB_EHCI_HCD 1                              ",
                "CONFIG_NETDEVICES 1                                ",
                "CONFIG_HAVE_CONTEXT_TRACKING 1                     ",
                "CONFIG_IOSCHED_DEADLINE 1                          ",
                "CONFIG_CGROUP_FREEZER 1                            ",
                "CONFIG_EVENTFD 1                                   ",
                "CONFIG_FS_POSIX_ACL 1",
                "CONFIG_DEFCONFIG_LIST \"/lib/modules/$UNAME_RELEASE/.config\"",
                "CONFIG_PROC_PAGE_MONITOR 1",
                "CONFIG_RCU_FANOUT_LEAF 16              ",
                "CONFIG_GPIO_NUC970_EINT_WKUP 1         ",
                "CONFIG_CPU_CACHE_VIVT 1                ",
                "CONFIG_NUC970_USBH_PWR_NONE 1          ",
                "CONFIG_SERIAL_NUC970_CONSOLE 1         ",
                "CONFIG_HAVE_ARCH_PFN_VALID 1           ",
                "CONFIG_IIO_NUC970ADC 1                 ",
                "CONFIG_GENERIC_STRNLEN_USER 1          ",
                "CONFIG_HAVE_DYNAMIC_FTRACE 1           ",
                "CONFIG_FMI_NUC970 1                    ",
                "CONFIG_CPUSETS 1                       ",
                "CONFIG_NEED_MACH_GPIO_H 1              ",
                "CONFIG_DEFAULT_CFQ 1                   ",
                "CONFIG_RCU_STALL_COMMON 1              ",
                "CONFIG_DEBUG_BUGVERBOSE 1              ",
                "CONFIG_FAT_FS 1                        ",
                "CONFIG_PINCONF 1                       ",
                "CONFIG_GENERIC_CLOCKEVENTS 1           ",
                "CONFIG_ROMFS_FS 1                      ",
                "CONFIG_IOSCHED_CFQ 1                   ",
                "CONFIG_HAVE_KERNEL_XZ 1                ",
                "CONFIG_CPU_CP15_MMU 1                  ",
                "CONFIG_CONSOLE_TRANSLATIONS 1          ",
                "CONFIG_ARCH_SUPPORTS_ATOMIC_RMW 1      ",
                "CONFIG_USB_OHCI_HCD 1                  ",
                "CONFIG_DUMMY_CONSOLE 1                 ",
                "CONFIG_TRACE_IRQFLAGS_SUPPORT 1        ",
                "CONFIG_NUC970ADC_I33V 1                ",
                "CONFIG_ENABLE_UART6_CTS_WAKEUP 1       ",
                "CONFIG_HAVE_REGS_AND_STACK_ACCESS_API 1",
                "CONFIG_PWM_SYSFS 1                     ",
                "CONFIG_LBDAF 1                         ",
                "CONFIG_CPU_ARM926T 1                   ",
                "CONFIG_NUC970_ADC 1                    ",
                "CONFIG_OABI_COMPAT 1                   ",
                "CONFIG_HAVE_GENERIC_HARDIRQS 1         ",
                "CONFIG_BINFMT_ELF 1                    ",
                "CONFIG_IIO_TRIGGER 1                   ",
                "CONFIG_HOTPLUG 1                       ",
                "CONFIG_CPU_CP15 1                      ",
                "CONFIG_NUC970_UART8_FC_PE 1            ",
                "CONFIG_NUC970_PWM2_NONE 1              ",
                "CONFIG_RESOURCE_COUNTERS 1             ",
                "CONFIG_SLABINFO 1                      ",
                "CONFIG_HARDIRQS_SW_RESEND 1            ",
                "CONFIG_SPI_MASTER 1                    ",
                "CONFIG_VT_HW_CONSOLE_BINDING 1         ",
                "CONFIG_USB_NUC970_OHCI 1               ",
                "CONFIG_GENERIC_CALIBRATE_DELAY 1       ",
                "CONFIG_HZ_PERIODIC 1                   ",
                "CONFIG_BROKEN_ON_SMP 1                 ",
                "CONFIG_ARCH_REQUIRE_GPIOLIB 1          ",
                "CONFIG_TMPFS 1                         ",
                "CONFIG_ANON_INODES 1                   ",
                "CONFIG_FUTEX 1                         ",
                "CONFIG_SPI_NUC970_P1_PI 1              ",
                "CONFIG_VMSPLIT_3G 1                    ",
                "CONFIG_SERIAL_CORE_CONSOLE 1           ",
                "CONFIG_SLUB_DEBUG 1                    ",
                "CONFIG_PINCTRL 1                       ",
                "CONFIG_NUC970_UART10 1                 ",
                "CONFIG_CGROUP_SCHED 1                  ",
                "CONFIG_SYSVIPC 1                       ",
                "CONFIG_CRYPTO_PCOMP2 1                 ",
                "CONFIG_HAVE_DEBUG_KMEMLEAK 1           ",
                "CONFIG_CPU_32v5 1                      ",
                "CONFIG_MODULES 1                       ",
                "CONFIG_UNIX 1                          ",
                "CONFIG_YAFFS_YAFFS1 1                  ",
                "CONFIG_HAVE_CLK 1                      ",
                "CONFIG_CRYPTO_HASH2 1                  ",
                "CONFIG_DEFAULT_HOSTNAME \"(none)\"       ",
                "CONFIG_NFS_FS 1                        ",
                "CONFIG_CRYPTO_ALGAPI 1                 ",
                "CONFIG_BSD_PROCESS_ACCT_V3 1           ",
                "CONFIG_MTD_CFI_I1 1                    ",
                "CONFIG_NFS_COMMON 1                    ",
                "CONFIG_FAIR_GROUP_SCHED 1              ",
                "CONFIG_ARCH_HAS_ATOMIC64_DEC_IF_POSITIVE 1    ",
                "CONFIG_CRYPTO_HASH 1                          ",
                "CONFIG_EFI_PARTITION 1                        ",
                "CONFIG_LOG_BUF_SHIFT 17                       ",
                "CONFIG_EXTRA_FIRMWARE \"\"                      ",
                "CONFIG_VFAT_FS 1                              ",
                "CONFIG_PID_NS 1                               ",
                "CONFIG_KEXEC 1                                ",
                "CONFIG_CRC32_SLICEBY8 1                       ",
                "CONFIG_ROMFS_ON_BLOCK 1                       ",
                "CONFIG_MTD_NAND_ECC 1                         ",
                "CONFIG_HAVE_LATENCYTOP_SUPPORT 1              ",
                "CONFIG_TMPFS_XATTR 1                          ",
                "CONFIG_NET_VENDOR_NUVOTON 1                   ",
                "CONFIG_YAFFS_AUTO_YAFFS2 1                    ",
                "CONFIG_HAVE_FUNCTION_TRACER 1                 ",
                "CONFIG_NUC970_UART6 1                         ",
                "CONFIG_CRYPTO_MANAGER2 1                      ",
                "CONFIG_GENERIC_PCI_IOMAP 1                    ",
                "CONFIG_SLUB 1                                 ",
                "CONFIG_I2C 1                                  ",
                "CONFIG_BINFMT_SCRIPT 1                        ",
                "CONFIG_FRAME_POINTER 1                        ",
                "CONFIG_TICK_CPU_ACCOUNTING 1                  ",
                "CONFIG_VM_EVENT_COUNTERS 1                    ",
                "CONFIG_CRYPTO_ECB 1                           ",
                "CONFIG_DEBUG_FS 1                             ",
                "CONFIG_BASE_FULL 1                            ",
                "CONFIG_SUNRPC 1                               ",
                "CONFIG_YAFFS_FS 1                             ",
                "CONFIG_USB_NUC970_EHCI 1                      ",
                "CONFIG_IIO_BUFFER 1                           ",
                "CONFIG_GPIO_SYSFS 1                           ",
                "CONFIG_FW_LOADER 1                            ",
                "CONFIG_KALLSYMS 1                             ",
                "CONFIG_COMMON_CLK 1                           ",
                "CONFIG_GENERIC_ATOMIC64 1                     ",
                "CONFIG_PWM 1                                  ",
                "CONFIG_MII 1                                  ",
                "CONFIG_SIGNALFD 1                             ",
                "CONFIG_NET_CORE 1                             ",
                "CONFIG_UIDGID_CONVERTED 1                     ",
                "CONFIG_UNINLINE_SPIN_UNLOCK 1                 ",
                "CONFIG_XZ_DEC 1                               ",
                "CONFIG_LOCKD_V4 1                             ",
                "CONFIG_DUMMY 1                                ",
                "CONFIG_HAS_IOMEM 1                            ",
                "CONFIG_GPIO_DEVRES 1                          ",
                "CONFIG_GENERIC_IRQ_PROBE 1                    ",
                "CONFIG_MTD_MAP_BANK_WIDTH_1 1                 ",
                "CONFIG_EPOLL 1                                ",
                "CONFIG_ICPLUS_PHY 1                           ",
                "CONFIG_HAVE_NET_DSA 1                         ",
                "CONFIG_YAFFS_XATTR 1                          ",
                "CONFIG_NET 1                                  ",
                "CONFIG_PINMUX 1                               ",
                "CONFIG_PACKET 1                               ",
                "CONFIG_ARCH_BINFMT_ELF_RANDOMIZE_PIE 1        ",
                "CONFIG_HAVE_CLK_PREPARE 1                     ",
                "CONFIG_NFS_V3 1                               ",
                "CONFIG_INET 1                                 ",
                "CONFIG_NUC970_I2C1_PI 1                       ",
                "CONFIG_FREEZER 1                              ",
                "CONFIG_HAVE_MACH_CLKDEV 1                     ",
                "CONFIG_NEED_KUSER_HELPERS 1                   ",
                "CONFIG_RTC_LIB 1                              ",
                "CONFIG_HAVE_KPROBES 1                         ",
                "CONFIG_CRYPTO_AES 1                           ",
                "CONFIG_GPIOLIB 1                              ",
                "CONFIG_CLKSRC_MMIO 1                          ",
                "CONFIG_SPI_NUC970_P1 1                        ",
                "CONFIG_CLONE_BACKWARDS 1                      ",
                "CONFIG_BLK_DEV_RAM_COUNT 16                   ",
                "CONFIG_PREEMPT_RCU 1                          ",
                "CONFIG_LOCKDEP_SUPPORT 1                      ",
                "CONFIG_USB_ARCH_HAS_EHCI 1                    ",
                "CONFIG_GENERIC_STRNCPY_FROM_USER 1            ",
                "CONFIG_MTD_BLKDEVS 1                          ",
                "CONFIG_NEED_DMA_MAP_STATE 1                   ",
                "CONFIG_IIO 1                                  ",
                "CONFIG_PAGE_OFFSET 0xC0000000                 ",
                "CONFIG_ZBOOT_ROM_BSS 0x0                      ",
                "CONFIG_CFG80211_DEFAULT_PS 1                  ",
                "CONFIG_CPU_NUC970 1                           ",
                "CONFIG_TTY 1                                  ",
                "CONFIG_HAVE_KERNEL_GZIP 1                     ",
                "CONFIG_NEED_PER_CPU_KM 1                      ",
                "CONFIG_ARM_NR_BANKS 8                         ",
                "CONFIG_GENERIC_IO 1                           ",
                "CONFIG_ARCH_NR_GPIO 0                         ",
                "CONFIG_GENERIC_BUG 1                          ",
                "CONFIG_HAVE_FTRACE_MCOUNT_RECORD 1            ",
                "CONFIG_HW_CONSOLE 1                           ",
                "CONFIG_IOSCHED_NOOP 1                         ",
                "CONFIG_HAVE_UID16 1                           ",
                "CONFIG_GENERIC_ACL 1                          ",
                "CONFIG_LOCALVERSION \"\"                        ",
                "CONFIG_CPU_PABRT_LEGACY 1                     ",
                "CONFIG_CRYPTO 1                               ",
                "CONFIG_DEFAULT_MMAP_MIN_ADDR 4096             ",
                "CONFIG_CMDLINE \"noinitrd root=/dev/mtdblock2 rootfstype=yaffs2 rootflags=inband-tags console=ttyS0, 115200n8 rdinit=/sbin/init mem=64\"",
                "CONFIG_HAVE_DMA_API_DEBUG 1                              ",
                "CONFIG_USB_ARCH_HAS_HCD 1                                ",
                "CONFIG_STRICT_DEVMEM 1                                   ",
                "CONFIG_GENERIC_IRQ_SHOW 1                                ",
                "CONFIG_PANIC_ON_OOPS_VALUE 0                             ",
                "CONFIG_ALIGNMENT_TRAP 1                                  ",
                "CONFIG_SCSI_MOD 1                                        ",
                "CONFIG_SERIAL_CORE 1                                     ",
                "CONFIG_BUILDTIME_EXTABLE_SORT 1                          ",
                "CONFIG_UID16 1                                           ",
                "CONFIG_SERIAL_NUC970 1                                   ",
                "CONFIG_HAVE_KRETPROBES 1                                 ",
                "CONFIG_HAS_DMA 1                                         ",
                "CONFIG_SCSI 1                                            ",
                "CONFIG_CLKDEV_LOOKUP 1                                   ",
                "CONFIG_ARCH_USES_GETTIMEOFFSET 1                         ",
                "CONFIG_NUC970_FMI_MTD_NAND 1                             ",
                "CONFIG_PHYLIB 1                                          ",
                "CONFIG_SPI_NUC970_P1_NORMAL_MODE 1                       ",
                "CONFIG_UNCOMPRESS_INCLUDE \"mach/uncompress.h\"            ",
                "CONFIG_IPC_NS 1                                          ",
                "CONFIG_MISC_FILESYSTEMS 1                                ",
                "CONFIG_DEBUG_LL_INCLUDE \"mach/debug-macro.S\"             ",
                "CONFIG_RCU_CPU_STALL_TIMEOUT 21                          ",
                "CONFIG_CFG80211_DEBUGFS 1                                ",
                "CONFIG_YAFFS_YAFFS2 1                                    ",
                "CONFIG_CRYPTO_ARC4 1                                     ",
                "CONFIG_CRYPTO_MANAGER 1                                  ",
                "CONFIG_MTD_NAND 1                                        ",
                "CONFIG_RT_MUTEXES 1                                      ",
                "CONFIG_VECTORS_BASE 0xffff0000                           ",
                "CONFIG_WIRELESS 1                                        ",
                "CONFIG_WEXT_PROC 1                                       ",
                "CONFIG_PERF_USE_VMALLOC 1                                ",
                "CONFIG_FAT_DEFAULT_IOCHARSET \"iso8859-1\"                 ",
                "CONFIG_I2C_BUS_NUC970_P1 1                               ",
                "CONFIG_FRAME_WARN 1024                                   ",
                "CONFIG_RCU_CPU_STALL_VERBOSE 1                           ",
                "CONFIG_GENERIC_HWEIGHT 1                                 ",
                "CONFIG_CGROUPS 1                                         ",
                "CONFIG_HAS_IOPORT 1                                      ",
                "CONFIG_CGROUP_CPUACCT 1                                  ",
                "CONFIG_HZ 100                                            ",
                "CONFIG_NUC970_CLK_TIMER 1                                ",
                "CONFIG_I2C_HELPER_AUTO 1                                 ",
                "CONFIG_ARM_PATCH_PHYS_VIRT 1                             ",
                "CONFIG_DEFAULT_IOSCHED \"cfq\"                             ",
                "CONFIG_CGROUP_PERF 1                                     ",
                "CONFIG_NLATTR 1                                          ",
                "CONFIG_TCP_CONG_CUBIC 1                                  ",
                "CONFIG_SYSFS 1                                           ",
                "CONFIG_ARM_THUMB 1                                       ",
                "CONFIG_HAVE_SYSCALL_TRACEPOINTS 1                        ",
                "CONFIG_BATTREY_NUC970ADC 1                               ",
                "CONFIG_I2C_COMPAT 1                                      ",
                "CONFIG_NUC970_UART6_FC_PG 1                              ",
                "CONFIG_GPIO_NUC970 1                                     ",
                "CONFIG_MSDOS_PARTITION 1                                 ",
                "CONFIG_HAVE_OPROFILE 1                                   ",
                "CONFIG_HAVE_GENERIC_DMA_COHERENT 1                       ",
                "CONFIG_ARCH_HAVE_CUSTOM_GPIO_H 1                         ",
                "CONFIG_OLD_SIGACTION 1                                   ",
                "CONFIG_HAVE_ARCH_KGDB 1                                  ",
                "CONFIG_USB_ARCH_HAS_OHCI 1                               ",
                "CONFIG_ZONE_DMA_FLAG 0                                   ",
                "CONFIG_PACKET_DIAG 1                                     ",
                "CONFIG_PROC_PID_CPUSET 1                                 ",
                "CONFIG_MTD_MAP_BANK_WIDTH_2 1                            ",
                "CONFIG_GENERIC_IDLE_POLL_SETUP 1                         ",
                "CONFIG_IP_MULTICAST 1                                    ",
                "CONFIG_DEFAULT_SECURITY \"\"                               ",
                "CONFIG_RWSEM_GENERIC_SPINLOCK 1                          ",
                "CONFIG_HAVE_DMA_ATTRS 1                                  ",
                "CONFIG_HAVE_FUNCTION_GRAPH_TRACER 1                      ",
                "CONFIG_ARCH_NUC970 1                                     ",
                "CONFIG_BASE_SMALL 0                                      ",
                "CONFIG_CRYPTO_BLKCIPHER2 1                               ",
                "CONFIG_COMPACTION 1                                      ",
                "CONFIG_PROC_FS 1                                         ",
                "CONFIG_MTD_BLOCK 1                                       ",
                "CONFIG_FLATMEM 1                                         ",
                "CONFIG_PAGEFLAGS_EXTENDED 1                              ",
                "CONFIG_SYSCTL 1                                          ",
                "CONFIG_SPI_NUC970_P0 1                                   ",
                "CONFIG_HAVE_C_RECORDMCOUNT 1                             ",
                "CONFIG_HAVE_ARCH_TRACEHOOK 1                             ",
                "CONFIG_SPI_NUC970_P0_NORMAL 1                            ",
                "CONFIG_NET_NS 1                                          ",
                "CONFIG_HAVE_PERF_EVENTS 1                                ",
                "CONFIG_DEBUG_MEMORY_INIT 1                               ",
                "CONFIG_SYS_SUPPORTS_APM_EMULATION 1                      ",
                "CONFIG_FAT_DEFAULT_CODEPAGE 437                          ",
                "CONFIG_BLK_DEV 1                                         ",
                "CONFIG_TRACING_SUPPORT 1                                 ",
                "CONFIG_UNIX98_PTYS 1                                     ",
                "CONFIG_CRYPTO_MICHAEL_MIC 1                              ",
                "CONFIG_HAVE_KERNEL_LZO 1                                 ",
                "CONFIG_IIO_CONSUMERS_PER_TRIGGER 2                       ",
                "CONFIG_ELF_CORE 1                                        ",
                "CONFIG_USB_SUPPORT 1                                     ",
                "CONFIG_FLAT_NODE_MEM_MAP 1                               ",
                "CONFIG_VT_CONSOLE 1                                      ",
                "CONFIG_CFG80211_WEXT 1                                   ",
                "CONFIG_FPE_NWFPE 1                                       ",
                "CONFIG_BLK_DEV_RAM 1                                     ",
                "CONFIG_IIO_TRIGGERED_BUFFER 1                            ",
                "CONFIG_PREEMPT 1                                         ",
                "CONFIG_GENERIC_CLOCKEVENTS_BUILD 1                       ",
                "CONFIG_SYSVIPC_SYSCTL 1                                  ",
                "CONFIG_MACH_NUC970 1                                     ",
                "CONFIG_CPU_USE_DOMAINS 1                                 ",
                "CONFIG_NUC970_UART1 1                                    ",
                "CONFIG_PINCTRL_NUC970 1                                  ",
                "CONFIG_I2C_CHARDEV 1                                     ",
                "CONFIG_CROSS_COMPILE \"\"                                  ",
                "CONFIG_CPU_ABRT_EV5TJ 1                                  ",
                "CONFIG_FHANDLE 1                                         ",
                "CONFIG_SWAP 1                                            ",
                "CONFIG_BLK_DEV_SD 1                                      ",
                "CONFIG_CMDLINE_FROM_BOOTLOADER 1                         ",
                "CONFIG_MODULE_UNLOAD 1                                   ",
                "CONFIG_PREEMPT_COUNT 1                                   ",
                "CONFIG_RCU_FANOUT 32                                     ",
                "CONFIG_BITREVERSE 1                                      ",
                "CONFIG_BLK_DEV_RAM_SIZE 16384                            ",
                "CONFIG_CRYPTO_BLKCIPHER 1                                ",
                "CONFIG_FILE_LOCKING 1                                    ",
                "CONFIG_AIO 1                                             ",
                "CONFIG_PERF_EVENTS 1                                     ",
                "CONFIG_GENERIC_HARDIRQS 1                                ",
                "CONFIG_NUC970_NAND_PC 1                                  ",
                "CONFIG_MTD_MAP_BANK_WIDTH_4 1                            ",
                "CONFIG_NLS_DEFAULT \"iso8859-1\"                           ",
                "CONFIG_UTS_NS 1                                          ",
                "CONFIG_CRYPTO_AEAD2 1                                    ",
                "CONFIG_CRYPTO_ALGAPI2 1                                  ",
                "CONFIG_ZBOOT_ROM_TEXT 0x0                                ",
                "CONFIG_HAVE_MEMBLOCK 1                                   ",
                "CONFIG_INPUT 1                                           ",
                "CONFIG_PROC_SYSCTL 1                                     ",
                "CONFIG_MMU 1                                             ",
                "CONFIG_KUSER_HELPERS 1                                   "
            ],
```

然后拷贝刚修改的 “includePath”: [] 和 “defines”: []，到自己应用程序目录下.vscode/c_cpp_properties.json 里面去，即确保内核和自己的应用程序都能找得到。

![](media/18.vscode_cfg.png)vscode配置界面

## 设置左侧目录不自动展开

左侧目录中包含了linux源码，默认打开一个文件，默认会自动展开并定位到该文件。

在驱动开发中关闭该功能会有更好的体验，方式如下：

a.按Ctrl+Shift+P快捷键，然后输入setting，从下拉选择中找到“Open settings(JSON)”

b.在打开的文件中输入 “explorer.autoReveal”: false

如下

```
{
    "explorer.autoReveal": false
}
```

下面就可以愉快的玩耍了。