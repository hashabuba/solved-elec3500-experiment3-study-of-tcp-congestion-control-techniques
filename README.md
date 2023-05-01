Download Link: https://assignmentchef.com/product/solved-elec3500-experiment3-study-of-tcp-congestion-control-techniques
<br>
<strong>Experiment: </strong>Study of TCP Congestion Control Techniques.

<strong>Required Reading Materials:  </strong>

<ol>

 <li>Text book, Chapter 3, section 3.7, page 297-306.</li>

 <li>Lecture slides, slide set 14.</li>

</ol>

<strong>Objective: </strong>In this simulation experiment the basic TCP (Transmission Control Protocol) congestion control technique will be examined. TCP use the AIMD (Additive-Increase, Multiplicative Decrease) process to throttle TCP connection rates to avoid the congestion on the transmission link. This experiment will examine the AIMD technique for different traffic load conditions. This experiment will also study the effect traffic load on the RTT (Round Trip Time) and the router queue length. The simulation model use a client server architecture.

<strong>Installation Steps: </strong>

This laboratory is designed based on the TCP congestion control technique lecture materials. The model simulates the TCP Reno protocol. The standard TCP/IP model has been modified to support the experiment. You’ll need the TCPIP folder, which can be downloaded from here:

https://www.dropbox.com/s/m79p0fh7w0ps380/TCPIP.zip?dl=1 Or if you use git, clone it from here:

https://github.com/UON-ELEC3500-SAM/TCPIP.git

You need to extract it into the in the OMNeT++ samples folder. You can run either one of these commands to get the TCPIP folder automatically. DON’T include the quotations when copy and pasting into the command line, and make sure that everything is entered as a single line into the command line (i.e. don’t include the line breaks/returns). You can open a terminal in the VM by pressing Ctrl + Alt + T

“cd /tmp/ramdisk/omnetpp-5.6.2/samples &amp;&amp; wget https://www.dropbox.com/s/m79p0fh7w0ps380/TCPIP.zip &amp;&amp; unzip TCPIP.zip -d /tmp/ramdisk/omnetpp-5.6.2/samples”

OR

“cd /tmp/ramdisk/omnetpp-5.6.2/samples &amp;&amp; git clone https://github.com/UON-ELEC3500-SAM/TCPIP.git” e.g. the directory structure should look like: /tmp/ramdisk/omnetpp-5.6.2/samples/TCPIP

(i.e. DON’T extract it into the ELEC3500 directory, extract it into the directory directly ABOVE the ELEC3500 directory)




You will also need to import the TCPIP folder into OMNeT++:

<ol>

 <li>In OMNeT++ click File-&gt;Import</li>

 <li>The “Existing projects into workspace” option should be selected by default. Click the “Next” button</li>

 <li>In the “Select root directory” text box, edit the text to say “/tmp/ramdisk/omnetpp-5.6.2/samples” (without the quotation marks), then click the “Finish” button.</li>

</ol>

Also note that you need to have version 4.1.2 of the inet framework installed, the simulation WILL NOT work with the inet framework version 4.2.1. To automatically download inet 4.1.2, you can copy and paste the following commands into the command line (this time you can copy and paste each line individually into the command line i.e. enter the commands one line at a time!)

cd /tmp/ramdisk/omnetpp-5.6.2/samples

wget <a href="https://github.com/inet-framework/inet/releases/download/v4.1.2/inet-4.1.2-src.tgz">https://github.com/inet</a><a href="https://github.com/inet-framework/inet/releases/download/v4.1.2/inet-4.1.2-src.tgz">–</a><a href="https://github.com/inet-framework/inet/releases/download/v4.1.2/inet-4.1.2-src.tgz">framework/inet/releases/download/v4.1.2/inet</a><a href="https://github.com/inet-framework/inet/releases/download/v4.1.2/inet-4.1.2-src.tgz">–</a><a href="https://github.com/inet-framework/inet/releases/download/v4.1.2/inet-4.1.2-src.tgz">4.1.2</a><a href="https://github.com/inet-framework/inet/releases/download/v4.1.2/inet-4.1.2-src.tgz">–</a><a href="https://github.com/inet-framework/inet/releases/download/v4.1.2/inet-4.1.2-src.tgz">src.tgz</a>

tar xfzv inet-4.1.2-src.tgz

rm -rf inet

mv inet4 inet

rm -rf inet-4.1.2-src.tgz <strong>Simulations Procedure:</strong>

Check the omnetpp.ini file, you will see that several parameters are listed under the heading “<em>Parameters that needs to be changed for ELEC3500”</em>. The default simulation parameter values are listed in table-1. Not necessarily all parameters need to be changed. Use simulation time of 200 sec. <strong>Table: 1 Simulation Parameter Values </strong>

<table width="595">

 <tbody>

  <tr>

   <td width="235"><strong>Parameter </strong></td>

   <td width="247"><strong>Variable </strong></td>

   <td width="113"><strong>Default Value </strong></td>

  </tr>

  <tr>

   <td width="235">Client No.</td>

   <td width="247">Cwindow.numClients</td>

   <td width="113">1</td>

  </tr>

  <tr>

   <td width="235">Simulation time</td>

   <td width="247">sim-time-limit</td>

   <td width="113">200.00s</td>

  </tr>

  <tr>

   <td width="235">Advertised window</td>

   <td width="247">tcp.advertised window</td>

   <td width="113"><strong><u>65535</u></strong>*10 B</td>

  </tr>

  <tr>

   <td width="235">Maximum transmission unit</td>

   <td width="247">ppp.mtu</td>

   <td width="113">4470B</td>

  </tr>

  <tr>

   <td width="235">Maximum segment size</td>

   <td width="247">tcp.mss</td>

   <td width="113">1000B</td>

  </tr>

  <tr>

   <td width="235">Router queue size</td>

   <td width="247">router.ppp[*].queue.frameCapacity</td>

   <td width="113">10 MTU</td>

  </tr>

  <tr>

   <td width="235">Client file Size</td>

   <td width="247">.Client*.app[*].sendBytes</td>

   <td width="113">1000 MiB</td>

  </tr>

  <tr>

   <td width="235">Transmission link speed<sup>** </sup></td>

   <td width="247">router.pppg++</td>

   <td width="113">10Mbps</td>

  </tr>

 </tbody>

</table>

**: this parameter is in .ned file.

<strong><u>Part 1: </u></strong>In this part of the lab, we will examine the effect of traffic load generated by multiple users on the TCP connection throughput. The OMNeT++ program labels the clients starting with an index of zero: e.g. when running with a single client, the client will be named client #0.




Initially, run the simulation model three times: varying the number clients from one client (i.e. only client #0), to two clients for the second simulation (i.e. clients #0, #1), and finally the third simulation with three clients (i.e. clients #0, #1, #2) for a total of three different simulations to be performed for part 1. Use default values for other parameters in the omnet.ini file.




Collect following plots after the simulation. To obtain following simulation plots right click on the vector value then click on the plot then save the plot. Save plots for your report.

<ol>

 <li>Simulation time vs the congestion window size (cwnd) for Client #0 for all three simulations.</li>

 <li>Simulation time vs RTT (measured RTT) for client #0 for all three simulations.</li>

 <li>Simulation time vs number of duplicated ACK received by Client #0 (rcvd dupAcks), only for the final simulation (i.e. the simulation with clients #0, #1, #2)</li>

 <li>Simulation time vs Router queue lengths (queueLength:vector) for clients #0 and #1 for the second simulation (i.e. the simulation with only clients #0 and #1)</li>

 <li>Simulation time vs congestion window size (cwnd) for client #2, only for the third simulation (i.e. the simulation with clients #0, #1, #2)</li>

</ol>

<strong><u>Part 2:  </u></strong>Change the MSS (Maximum Segment Size) to 2000B and obtain following simulation results

<ol>

 <li>Simulation time vs the congestion window size for Client #0 for all three simulations</li>

 <li>Simulation time vs number of duplicated ACKs for Client #0, only for the second simulation</li>

 <li>Simulation time vs congestion window size for client #2, only for the final simulation</li>

</ol>