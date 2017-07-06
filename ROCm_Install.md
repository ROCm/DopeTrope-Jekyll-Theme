---
layout: default
title: ROCm Install
---

<h2>ROCm GPU Server Driver Installation Guide for Linux</h2>

<h3>Introduction</h3>

<p>The ROCm Platform brings a rich foundation to advanced computing by seamlessly integrating the CPU and GPU with the goal of solving real-world problems.</p>

<p>This support starts with AMD’s FIJI Family of dGPUs. ROCm 1.3 further extends support to include the Polaris Family of ASICs. With ROCm 1.6 we add Vega Family of products. </p>

<h3>System Requirements</h3>

<p>To use ROCm on your system you need the following: </p>

<ul>
<li>ROCm Capable CPU and GPU 
<ul>
<li>PCIe Gen 3 Enabled CPU with PCIe Platform Atomics </li>
<li>ROCm enabled GPU&#39;s 
<ul>
<li>Radeon Instinct Family MI25, MI8, MI6 </li>
<li>Radeon Vega Frontier Edition </li>
<li><a href="hardware.md">Broader Set of Tested Hardware</a></li>
</ul></li>
</ul></li>
<li>Supported Version of Linux with a specified GCC Compiler and ToolChain </li>
</ul>

<p>Table 1. Native Linux Distribution Support in ROCm  1.6</p>

<table>
<thead>
<tr>
<th style="text-align: left">Distribution</th>
<th style="text-align: left">Kernel</th>
<th style="text-align: left">GCC</th>
<th style="text-align: left">GLIBC</th>
</tr>
</thead>

<tbody>
<tr>
<td style="text-align: left">x86_64</td>
<td style="text-align: left"></td>
<td style="text-align: left">.</td>
<td style="text-align: left"></td>
</tr>
<tr>
<td style="text-align: left">Fedora 24</td>
<td style="text-align: left">4.9</td>
<td style="text-align: left">5.40</td>
<td style="text-align: left">2.23  </td>
</tr>
<tr>
<td style="text-align: left">Ubuntu 16.04</td>
<td style="text-align: left">4.9</td>
<td style="text-align: left">5.40</td>
<td style="text-align: left">2.23</td>
</tr>
</tbody>
</table>

<h3>Pre Install Directions</h3>

<h5>Verify You Have ROCm Capable GPU Installed int the System</h5>

<pre><code class="language-shell">lspci | grep -i AMD
</code></pre>

<h6>You will see list of AMD GPU&#39;s</h6>

<h5>Verify You Have a Supported Version of Linux</h5>

<pre><code class="language-shell">uname -m &amp;&amp; cat /etc/*release
</code></pre>

<h6>You will see some thing like for Ubuntu</h6>

<pre><code class="language-shell">x86_64
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION=&quot;Ubuntu 16.04.2 LTS&quot;
</code></pre>

<h5>Verify version of GCC</h5>

<pre><code class="language-shell">gcc --version 
</code></pre>

<h6>You will see</h6>

<pre><code class="language-shell">gcc (Ubuntu 5.4.0-6ubuntu1~16.04.4) 5.4.0 20160609 
</code></pre>

<h3>Choose an Installation Method</h3>

<p>Package manager Based Install 
A Package Manager Based Installation use your Linux Distro system&#39;s package management service.<br>
Ubuntu uses Debian  and Fedora RPM Packages</p>

<ul>
<li>Ubuntu </li>
<li>Fedora </li>
</ul>

<h4>Ubuntu Install</h4>

<h5>Add the Repo Server</h5>

<p>For Debian based systems, like Ubuntu, configure the Debian ROCm repository as
follows:</p>

<pre><code class="language-shell">wget -qO - http://repo.radeon.com/rocm/apt/debian/rocm.gpg.key | sudo apt-key add -
sudo sh -c &#39;echo deb [arch=amd64] http://repo.radeon.com/rocm/apt/debian/ xenial main &gt; /etc/apt/sources.list.d/rocm.list&#39;
</code></pre>

<p>The gpg key might change, so it may need to be updated when installing a new 
release. The current rocm.gpg.key is not avialable in a standard key ring distribution,
but has the following sha1sum hash:</p>

<p>f0d739836a9094004b0a39058d046349aacc1178  rocm.gpg.key</p>

<h5>Install or update ROCm</h5>

<pre><code class="language-shell">sudo apt-get update
sudo apt-get install rocm rocm-opencl-dev
</code></pre>

<p>Then, make the ROCm kernel your default kernel. If using grub2 as your bootloader, you can edit the GRUB_DEFAULT variable in the following file:</p>

<pre><code class="language-shell">sudo nano /etc/default/grub
</code></pre>

<p>set the GRUB_Default 
Edit: GRUB_DEFAULT=<q>Advanced options for Ubuntu&gt;Ubuntu, with Linux 4.9.0-kfd-compute-rocm-rel-1.6-77</q>
```shell
sudo update-grub</p>

<h2>```</h2>

<h4>Fedora Install</h4>

<p>Use the  dnf (yum) repository for installation of rpm packages.
To configure a system to use the ROCm rpm directory create the file
/etc/yum.repos.d/rocm.repo with the following contents:</p>

<pre><code class="language-shell">[remote]

name=ROCm Repo

baseurl=http://repo.radeon.com/rocm/yum/rpm/

enabled=1

gpgcheck=0
</code></pre>

<p>Execute the following commands:
<code>shell
sudo dnf clean all
sudo dnf install rocm rocm-opencl-dev</code>
Just like Ubuntu installs, the ROCm kernel must be the default kernel used at boot time.</p>

<p>Post Install Manual installation steps for Fedora to support HCC compiler </p>

<p>A fully functional Fedora installation requires a few manual steps to properly 
setup, including:
 * <a href="https://github.com/RadeonOpenCompute/hcc/wiki#fedora">Building compatible libc++ and libc++abi libraries for Fedora</a></p>

<h5>Post install verification</h5>

<p>Verify you have the correct Kernel Post install </p>

<pre><code class="language-shell">uname -r
4.9.0-kfd-compute-rocm-rel-1.6-77
</code></pre>

<p>Test if OpenCL is working based on default ROCm OpenCL include and library locations:
<code>shell
g++ -I /opt/rocm/opencl/include/ ./HelloWorld.cpp -o HelloWorld -L/opt/rocm/opencl/lib/x86_64 -lOpenCL</code></p>

<p>Run it:
<code>shell
./HelloWorld</code></p>

<h3>To Uninstall the a Package</h3>

<ul>
<li>Ubuntu 
<code>shell
sudo apt-get purge libhsakmt
sudo apt-get purge radeon-firmware
sudo apt-get purge $(dpkg -l | grep &#39;kfd\|rocm&#39; | grep linux | grep -v libc | awk &#39;{print $2}&#39;)</code></li>
<li>Fedora 
<code>shell
sudo dnf remove ROCm</code> 
<a href="ROCmLinuxpackages.md">List of ROCm Packages for Ubuntu and Fedora</a></li>
</ul>

<h3>Installing development packages for cross compilation</h3>

<p>It is often useful to develop and test on different systems. In this scenario, you may prefer to avoid installing the ROCm Kernel to your development system.</p>

<p>In this case, install the development subset of packages:</p>

<pre><code class="language-shell">sudo apt-get update
sudo apt-get install rocm-dev
</code></pre>
