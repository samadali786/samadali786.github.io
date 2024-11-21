---

title: "How to create and track simple Fileless Malware in C++ in Windows OS"
date: "2024-06-04 00:00:00"
categories: [Malware Analysis Projects]
tags: [Malware Analysis]

---

# Overview

Fileless Malware is a type of malicious code that is attached or inject to any of the legimate program process to infect a computer. It's hard to detect and often evades detection by most of the security solutions. It is differnt then most of the traditional virus because it operates in memory and not stored in the file. filess infections go straight to the computer memory without touching the drives and as a result it often undected by antivirus, whitelisting, and other endpoint security solutions.


# Processes in Windows OS

If we open the task manager in Windows OS we see many different types of processes each process is belong to the program that are running on the computer. These processes divided into user mode processes (Background process) and System processes (Windows Process) which is the part of the Windows OS and have a highest privilages. there are also other GUI tool like Process Hacker which gives us more GUI feature to do in depth analysis of the process like PID(Process Identifier of each process).


# Processes from the Task manager tool in Window OS

image 
![image](C:\Users\samad\Desktop\task_manager_process.png)

# Process from the Process Hacker tool with PID in Windows OS


image
![image](C:\Users\samad\Desktop\process_hacher_process.png)



# Use of PID (Process Identifier) in FileLess attack

Each process has it's own uniquie process indentifier number that is associated with it. Threat actor or attacker uses these PID number to perform Fileless attack to get the handle to a process by doing that they can perform allocating memmory, modifying the privillages in certain pages in the memory basically they can manipulate the process however they want


# Basic Layout of the Memory  

Memory consists of multiple blocks and these blocks are called pages and each page has certain properties like size, range of address, security flags permissions like RWX(read, write, and execute), and 
"committed" or "reserved" 


# images of memory here

memory layout
![image](C:\Users\samad\Desktop\memory_layout_pages.png)

memmory layout types
![image](C:\Users\samad\Desktop\memory_layout_with_types.png)

# Memory with 32 and 64 bit Addresses

For 32 bit address all memory addresses have a max size of 4 bytes or a "DWORD(double word)". A word is 2 Byte so DWORD is double byte which is equal to 4 bytes. A "QWORD" as it equal to 8 bytes. 

For 64 bit everything is same except the size of the addressses is a maximum size of "QWORD" 8 bytes.  


# memory Address images

memory address image
![image](C:\Users\samad\Desktop\memory_address.png)

