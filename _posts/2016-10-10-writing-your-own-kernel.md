---
layout: "writing your own kernel"
date:   2016-10-11
desc: "writing your own simple kernel"
keywords: "linux,kernel,I/O"
categories: [Linux]
tags: [Linux,kernelnewbies]
icon: icon-html
---

## **About**

This tutorial will take you through a walk-through on the assembly code and c-programming language that will build up to a minimal kernel which boots up with the grub2 boot-loader. This being said keeping in mind that I am using a Ubuntu 16.04(LTS) operating system. But this will work smoothly with any older version or any other distro for that matter. This is simply because the entire process right from building the kernel to loading it from the boot-loader is independent of the operating system you use, for instance you will have a Grub boot-loader instead of Grub2\. I will write separate terminal commands for both of these down below.

## **Pre-Requisites**

1] A distro based on Linux 2] NASM assembler 3] [Id(GNU LINKER)](https://linux.die.net/man/1/ld)

## An Introduction

The entire code we will be writing for the custom kernel will be based on three files. The first one is written completely in assembly language, so if you are an intermediate or novice it would be great if you spend more time on understanding what it roughly means by Googling ofcourse! An attempt to fully cover the assembly code and it's interpretation is out of the scope of this blog.

## Assembly code (part 1/3 of kernel code)

> <pre class="prettyprint lang-asm prettyprinted"><span class="pln">bits</span> <span class="lit">32</span> <span class="pln">section</span> <span class="pun">.</span><span class="pln">text</span> <span class="pun">;</span><span class="pln">multiboot spec
>         align</span> <span class="lit">4</span> <span class="pln">dd</span> <span class="lit">0x1BADB002</span>            <span class="pln">dd</span> <span class="lit">0x00</span>                  <span class="pln">dd</span> <span class="pun">-</span> <span class="pun">(</span><span class="lit">0x1BADB002</span> <span class="pun">+</span> <span class="lit">0x00</span><span class="pun">)</span>
>
> <span class="kwd">global</span> <span class="pln">start</span> <span class="kwd">extern</span> <span class="pln">kmain</span> <span class="pun">;</span><span class="pln">kmain</span> <span class="kwd">is</span> <span class="kwd">defined</span> <span class="kwd">in</span> <span class="pln">the c file
>
> start</span><span class="pun">:</span> <span class="pln">cli</span> <span class="pun">;</span><span class="pln">block interrupts
>   mov esp</span><span class="pun">,</span> <span class="pln">stack_space</span> <span class="pun">;</span><span class="kwd">set</span> <span class="pln">stack pointer
>   call kmain
>   hlt</span> <span class="pun">;</span><span class="pln">halt the CPU
>
> section</span> <span class="pun">.</span><span class="pln">bss
> resb</span> <span class="lit">8192</span>		<span class="pun">;</span><span class="lit">8KB</span> <span class="kwd">for</span> <span class="pln">stack
> stack_space</span><span class="pun">:</span></pre>
>
> <pre class="prettyprint lang-asm prettyprinted"> <span class="pun">(Open your favourite text editor,say vim or gedit and save this file
> as kernel.asm)</span> </pre>

**_(Refer this link to know more [About asm extension)](http://www.reviversoft.com/file-extensions/asm)_** The content after the ";"(semi-colon) are the comments similar to those found in the modern high level languages. The initial line of assembly code starts with bits 32 indicating it's a directive for a 32 bit system, but my post claims to work even for a 64bit system, this is because they are backwards compatible to a lower bit version. we also invoke a kmain function in line four, this will be declared in the C-programming part of the code.

## C-Programme (Part 2/3 of kernel code)

Now we start the C part of the kernel by declaring the kmain() function.

> <pre class="prettyprint lang-c prettyprinted"><span class="kwd">void</span> <span class="pln">kmain</span><span class="pun">(</span><span class="kwd">void</span><span class="pun">)</span>
> <span class="pun">{</span>
> 	<span class="kwd">const</span> <span class="kwd">char</span> <span class="pun">*</span><span class="pln">str</span> <span class="pun">=</span> <span class="str">"your message"</span><span class="pun">;</span>
> 	<span class="kwd">char</span> <span class="pun">*</span><span class="pln">vidptr</span> <span class="pun">=</span> <span class="pun">(</span><span class="kwd">char</span><span class="pun">*)</span><span class="lit">0xb8000</span><span class="pun">;</span> 	
> 	<span class="kwd">unsigned</span> <span class="typ">int</span> <span class="pln">i</span> <span class="pun">=</span> <span class="lit">0</span><span class="pun">;</span>
> 	<span class="kwd">unsigned</span> <span class="typ">int</span> <span class="pln">j</span> <span class="pun">=</span> <span class="lit">0</span><span class="pun">;</span>
>
> 	
> 	<span class="kwd">while</span><span class="pun">(</span><span class="pln">j</span> <span class="pun"><</span> <span class="lit">80</span> <span class="pun">*</span> <span class="lit">25</span> <span class="pun">*</span> <span class="lit">2</span><span class="pun">)</span> <span class="pun">{</span>
> 		<span class="com">/* blank character */</span> <span class="pln">vidptr</span><span class="pun">[</span><span class="pln">j</span><span class="pun">]</span> <span class="pun">=</span> <span class="str">' '</span><span class="pun">;</span>
> 		<span class="com">/* attribute-byte - light grey on black screen */</span> <span class="pln">vidptr</span><span class="pun">[</span><span class="pln">j</span><span class="pun">+</span><span class="lit">1</span><span class="pun">]</span> <span class="pun">=</span> <span class="lit">0x07</span><span class="pun">;</span> <span class="pln">j</span> <span class="pun">=</span> <span class="pln">j</span> <span class="pun">+</span> <span class="lit">2</span><span class="pun">;</span>
> 	<span class="pun">}</span> <span class="pln">j</span> <span class="pun">=</span> <span class="lit">0</span><span class="pun">;</span>
>
> 	<span class="com">/* this loop writes the string to video memory */</span>
> 	<span class="kwd">while</span><span class="pun">(</span><span class="pln">str</span><span class="pun">[</span><span class="pln">j</span><span class="pun">]</span> <span class="pun">!=</span> <span class="str">'\0'</span><span class="pun">)</span> <span class="pun">{</span>
> 		<span class="com">/* the character's ascii */</span> <span class="pln">vidptr</span><span class="pun">[</span><span class="pln">i</span><span class="pun">]</span> <span class="pun">=</span> <span class="pln">str</span><span class="pun">[</span><span class="pln">j</span><span class="pun">];</span>
> 		 <span class="pln">vidptr</span><span class="pun">[</span><span class="pln">i</span><span class="pun">+</span><span class="lit">1</span><span class="pun">]</span> <span class="pun">=</span> <span class="lit">0x07</span><span class="pun">;</span>
> 		<span class="pun">++</span><span class="pln">j</span><span class="pun">;</span> <span class="pln">i</span> <span class="pun">=</span> <span class="pln">i</span> <span class="pun">+</span> <span class="lit">2</span><span class="pun">;</span>
> 	<span class="pun">}</span>
> 	<span class="kwd">return</span><span class="pun">;</span>
> <span class="pun">}</span> </pre>
>
> This essentially prints your message to a black screen when your kernel boots up.

## The linkage (part 3/3 of kernel code)

Now to the most interesting part of the entire project, the end! Yes the last part of building your kernel is to link both the assembly code and the C-code that we wrote earlier. This is were NASM and gcc come into the picture(gcc is a c/c++ compiler) . First using NASM we compile the kernel.asm file to a object file and again using gcc we compile kernel.c to another object file. Once that is done it is time to link both of the files together using the following linker script.

> <pre class="prettyprint lang-ld prettyprinted"><span class="pln">OUTPUT_FORMAT</span><span class="pun">(</span><span class="pln">elf32</span><span class="pun">-</span><span class="pln">i386</span><span class="pun">)</span> <span class="pln">ENTRY</span><span class="pun">(</span><span class="pln">start</span><span class="pun">)</span> <span class="pln">SECTIONS</span> <span class="pun">{</span>
>    <span class="pun">.</span> <span class="pun">=</span> <span class="lit">0x100000</span><span class="pun">;</span>
>    <span class="pun">.</span><span class="pln">text</span> <span class="pun">:</span> <span class="pun">{</span> <span class="pun">*(.</span><span class="pln">text</span><span class="pun">)</span> <span class="pun">}</span>
>    <span class="pun">.</span><span class="pln">data</span> <span class="pun">:</span> <span class="pun">{</span> <span class="pun">*(.</span><span class="pln">data</span><span class="pun">)</span> <span class="pun">}</span>
>    <span class="pun">.</span><span class="pln">bss</span> <span class="pun">:</span> <span class="pun">{</span> <span class="pun">*(.</span><span class="pln">bss</span><span class="pun">)</span>  <span class="pun">}</span>
>  <span class="pun">}
>
> (We save this as link.id)</span></pre>

Now that we have the requisite files coded and ready we are left with only the bootloader issue. Next we take the kernel we created( the object files ) and make up a executable in our **/boot** directory and write the code with appropriate permissions to show in the Grub menu.

## Building and running the Kernel

Open your command line and type in the following commands to create the object files as mentioned in the latter sections.

> <pre class="prettyprint lang-sh prettyprinted"><span class="pln">nasm</span> <span class="pun">-</span><span class="pln">f elf32 kernel</span><span class="pun">.</span><span class="pln">asm</span> <span class="pun">-</span><span class="pln">o kasm</span><span class="pun">.</span><span class="pln">o</span> </pre>

The above code uses NASM to compile the assembly code whereas gcc does the same for the C-code.

> <pre class="prettyprint lang-sh prettyprinted"><span class="pln">gcc</span> <span class="pun">-</span><span class="pln">m32</span> <span class="pun">-</span><span class="pln">c kernel</span><span class="pun">.</span><span class="pln">c</span> <span class="pun">-</span><span class="pln">o kc</span><span class="pun">.</span><span class="pln">o</span></pre>

As already mentioned the linker script we listed last will link the two by the following command.

> <pre class="prettyprint lang-sh prettyprinted"><span class="pln">ld</span> <span class="pun">-</span><span class="pln">m elf_i386</span> <span class="pun">-</span><span class="pln">T link</span><span class="pun">.</span><span class="pln">ld</span> <span class="pun">-</span><span class="pln">o kernel kasm</span><span class="pun">.</span><span class="pln">o kc</span><span class="pun">.</span><span class="pln">o</span></pre>

## Configuring Grub/Grub2 menu

Remember when I first wrote about a subtle change in code right at the end for older versions to that of the newer versions? Well, its the different line of code which pops up our entire kernel in the boot-loader menu. **Grub** users please type in the following into the last part of your grub.cfg file. You can find the grub.cfg file in the following path [caption id="attachment_media-2" align="aligncenter" width="1359"]![g.png](https://rahulkrishnanlive.files.wordpress.com/2016/11/g.png) _**/boot/grub/grub.cfg**_[/caption]

> <pre>title myKernel
> 	root (hd0,0)
> 	kernel /boot/kernel-701 ro</pre>

Grub2 users make the following changes, grub.cfg file locations will be the same as above.

> <pre class="prettyprint prettyprinted"><span class="pln">menuentry</span> <span class="str">'kernel 7001'</span> <span class="pun">{</span>
> 	<span class="kwd">set</span> <span class="pln">root</span><span class="pun">=</span><span class="str">'hd0,msdos1'</span> <span class="pln">multiboot</span> <span class="pun">/</span><span class="pln">boot</span><span class="pun">/</span><span class="pln">kernel</span><span class="pun">-</span><span class="lit">7001</span> <span class="pln">ro</span> <span class="pun">}</span></pre>

<pre>    (We can access the grub menu at startup by pressing
                  either _shift_ or _esc_ key)</pre>

## The Verdict

Now we have completely build a raw kernel which loads at boot-up. To test successfully you must get a black screen with the message that you typed in the kernel.c file appearing on it. We have not coded any I/O functions or responsive commands into it, so the kernel hangs after it. This marks the end of your first kernel build. Stay tuned to this blog for more content, until next time, PEACE!
