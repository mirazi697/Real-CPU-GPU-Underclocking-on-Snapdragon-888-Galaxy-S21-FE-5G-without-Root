# Real-CPU-GPU-Underclocking-on-Snapdragon-888-Galaxy-S21-FE-5G-without-Root
​A technical guide explaining how to achieve genuine underclocking of the CPU and GPU on the Samsung Galaxy S21 FE 5G (Snapdragon 888) to mitigate overheating and battery drain without needing root privileges or spoofing the battery percentage.

​# The Problem
​The Snapdragon 888 chipset is notorious for thermal throttling and overheating under load. Standard power-saving workarounds usually fall short:
​Stock Power Saving Mode: Fails to aggressively cap maximum frequencies during heavy workloads.
​Fake Battery Level Spoofing: Executed via ADB, this causes incorrect system battery readings and potential background conflicts.
​# The Solution & Methodology
​By reverse-engineering framework.jar and decompiling the IPowerManager$Stub.smali interface within the Android framework, the specific Binder Transaction Code for power management was identified:
.field static final blacklist TRANSACTION_setPowerModeChecked:I = 0x7
Using this transaction code via the ADB shell Binder interface forces the Kernel Governor directly into a low-power/battery-saver state without altering the actual system battery percentage.
# ​⚙️ How to Use (ADB / LADB)
​Enable Wireless Debugging (via LADB) or connect your device to a PC via ADB, then execute the following commands:
# ​1. Enable Underclocking Mode
service call power 7 i32 2
# 2. Disable Underclocking (Return to Normal)
# 📊 Verification & Results
​After applying the command (service call power 7 i32 2), you can verify the capped frequencies via shell:
CPU Max Frequency:
cat /sys/devices/system/cpu/cpu4/cpufreq/scaling_max_freq
# Output: 1555200 (Capped at 1.55 GHz)
GPU Max Frequency:
cat /sys/class/kgsl/kgsl-3d0/max_gpuclk
# Output: 443000000 (Capped at 443 MHz)
# 📱 Test Environment
​Tested and verified on the following hardware and software configuration:
​Device: Samsung Galaxy S21 FE 5G (SM-G990B2)
​Chipset: Qualcomm Snapdragon 888
​OS Version: One UI 8.0 / Android 16
​Kernel Version: 5.4.289-qgki
​# 👤 Credit
​Discovered and documented by Mirazi Malek via Android Framework Smali reverse-engineering.
