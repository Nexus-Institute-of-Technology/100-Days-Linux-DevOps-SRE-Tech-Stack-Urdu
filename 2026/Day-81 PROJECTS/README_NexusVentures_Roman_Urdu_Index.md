# NexusVentures RHCSA Mini-Project Series — Roman Urdu

Yeh package RHCSA 9 ke 20 numbered tasks ko 20 alag, qadam-ba-qadam Roman Urdu practical projects mein tabdeel karta hai.

## Aham Lab Usool

- Students apni assigned Rocky Linux 9 VM par `root` account se kaam karenge.
- SELinux ko enforcing aur firewalld ko enabled rakhein.
- Jo configurations mustaqil honi chahiye unhein reboot ke baad dobara validate karein.
- Network, boot recovery, disk, LVM, VDO aur container changes se pehle Xen Orchestra snapshot banayein.
- Kai student VMs ko kabhi ek hi static IP address assign na karein.
- Disk erase karne wali commands sirf instructor ki assigned khali disk par chalayein.

## Project Index

1. [Network ki Bunyad aur Mustaqil Shanakht](NexusVentures_Project_01_network-foundation_Roman_Urdu.md)
2. [Control Shuda Software Repository Configuration](NexusVentures_Project_02_software-repositories_Roman_Urdu.md)
3. [TCP Port 82 par Mehfooz Apache Service](NexusVentures_Project_03_web-port-82-selinux_Roman_Urdu.md)
4. [User Shanakht aur Administrative Group ki Tayyari](NexusVentures_Project_04_users-groups_Roman_Urdu.md)
5. [Mehfooz Mushtarka Administration Workspace](NexusVentures_Project_05_collaborative-directory_Roman_Urdu.md)
6. [AutoFS ke Sath Zaroorat par NFS Access](NexusVentures_Project_06_autofs-nfs_Roman_Urdu.md)
7. [Schedule Shuda User Kaam aur Cron Access Control](NexusVentures_Project_07_cron-access-control_Roman_Urdu.md)
8. [ACLs ke Sath Bareek Satah ka File Access](NexusVentures_Project_08_acl-permissions_Roman_Urdu.md)
9. [Chrony ke Sath Bharosemand Waqt ki Ham-Ahangi](NexusVentures_Project_09_chrony-ntp_Roman_Urdu.md)
10. [Bari Configuration Files ki Talaash aur Jama Karna](NexusVentures_Project_10_large-file-discovery_Roman_Urdu.md)
11. [Shanakht, Archive aur Private Default Permissions](NexusVentures_Project_11_identity-archive-umask_Roman_Urdu.md)
12. [Password Aging aur Tafweez Shuda Administration](NexusVentures_Project_12_password-aging-sudo_Roman_Urdu.md)
13. [Mazboot Bash File Collection Automation](NexusVentures_Project_13_bash-file-collector_Roman_Urdu.md)
14. [GRUB ke Zariye Root Password Recovery](NexusVentures_Project_14_root-password-recovery_Roman_Urdu.md)
15. [Swap aur LVM Database Storage](NexusVentures_Project_15_swap-lvm-storage_Roman_Urdu.md)
16. [Thin-Provisioned VDO Storage Volume](NexusVentures_Project_16_vdo-storage_Roman_Urdu.md)
17. [Online LVM aur Filesystem ko Barhana](NexusVentures_Project_17_lvm-expansion_Roman_Urdu.md)
18. [TuneD ke Sath Khudkar Performance Profile ka Intikhab](NexusVentures_Project_18_tuned-profile_Roman_Urdu.md)
19. [Rootless Rsyslog Container ko Startup Service Banana](NexusVentures_Project_19_rootless-container-service_Roman_Urdu.md)
20. [Logserver Container ke Liye Mustaqil Journal Storage](NexusVentures_Project_20_container-persistent-journal_Roman_Urdu.md)

## Project Dependencies

- Project 03, Projects 01 aur 02 ke network aur repository kaam ko istemal karta hai.
- Projects 05, 07, 08, 11 aur 12, Project 04 mein banaye gaye accounts ko istemal karte hain.
- Project 17, Project 15 mein banaye gaye LVM ko barhata hai.
- Project 20, Project 19 mein banaye gaye rootless container ko aage barhata hai.

## Teaching ka Tareeqa

Har project mein students commands ka output, commands ki wazahat, validation, zaroorat par reboot ka saboot aur rollback procedure jama karein. Instructor projects ko alag alag bhi assign kar sakta hai ya inhein ek connected NexusVentures infrastructure program ke taur par chala sakta hai.
