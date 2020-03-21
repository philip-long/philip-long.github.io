---
layout: default
title: Dual booting windows 10 and ubuntu 18.04 Dell Latitude
date:   2020-03-20 
category: [ ubuntu, dell, windows10,bitlocker]
---



   <p>
    Steps to install ubuntu alongside windows 10. Dual booting seems to be becoming more and more difficult. It goes without saying, you should backup everything before trying this because things can go wrong (very wrong)!  
    </p>
     
     <br>     
     
<h3> Partitioning windows </h3>

   <p> If you have bitlocker you need to save or print out your bitlocker key, this is very important, you will probably need to enter this at least once  </p>
   
    <li>. Manage bitlocker </li>
   
    <li> Backup your recovery key </li>  
   
    <li> Print the recovery key , Save to a USB flash drive </li>
 
 <br>     
 
   <p> Now shrink the disk </p>
    <li>  In Control panel, search disk and go to “create and format hard disk partitions </li>  
    <li> Right click the drive you want to shrink, and press shrink volume </li>   
    <li> Assign at least 40gb to the free space. </li>

<br>     

<h3> Disable Secure boot </h3>

 <li> Go to advanced start up in windows settings and click restart now </li>
 <li> Access UEFI settings </li>
 <li> Click trouble shoot options </li>
 <li> Click advanced options </li>
 <li> UEFI Firmware Settings </li>
 <li> Click restart. </li>
 <li> Move to boot tab in bios </li>
 <li> Disable secure boot and confirm setting ( you may be asked for bitlocker key) </li>
    
    <br>     
    
<h3> Switching from RAID to AHCI </h3>
 <li> Run MSCONFIG </li>
 <li>  Enable Safe Boot (minimal) </li>
 <li>  Reboot into UEFI/BIOS and change to AHCI </li>
 <li>  Boot up into safe mode </li>
 <li>  Run MSCONFIG and disable Safe Boot. </li>
 <li>  Reboot </li>

<br>     

<h3> Installing Ubuntu </h3>

   <p> I followed this [link](https://itsfoss.com/install-ubuntu-1404-dual-boot-mode-windows-8-81-uefi/) for the partitioning, except,</p>
   
<li> All partitions were set to logical rather than primary</li>
<li> Swap size set to 3GB</li>

<h3> After installation </h3>
  <li> So when you boot into back into windows, it will ask you for the bitlocker key </li>
  <li> Enter it and <b> all will be well </b> </li>
  
 <h3> What happens when all is not well </h3>

<p>
 In total, with the exact same windows 10 installation, we've tried this 5 times.  We've dual booted two lenovas and one dell latitude and after entering the bitlocker key once after installation you'll never need to enter it again. However, we carried out the exact same steps with 2 dell xps models and each boot into windows requries the bitlocker key to be entered once again. This is not practical, but we're required to keep bitlocker enabled. The least worst solution we've found is using a usb key with the bitlocker key saved to it. This means that at each startup user requries the usb to be in the laptop. Awkward but less awkward than a 48 digit key...
    </p>


